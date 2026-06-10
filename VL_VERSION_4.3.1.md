# VL Syntax Reference Complete Version (Rules & Widgets)

## Version Declaration

```vl
// VL_VERSION: version_number
```

This version declaration must be placed in the first line comment of all files to ensure the parser uses the correct version rules.

Current version: `// VL_VERSION:4.3.1`

Note: The version declaration is not a VL code section; please use `//` instead of `#`.

## File Types and Structure

### Project File Tree Structure:

```
Workspace/
├─ Services/              # ServiceDomain files (service domain definitions, callable from Section)
│   ├─ Catalog.vs
│   └─ Order.vs
├─ Database/              # Database files (database structure definitions)
│   └─ MyProject.vdb
├─ Sections/              # Section files (frontend view modules, can only be used in App)
│   ├─ NavHeader.sc
│   ├─ UserProfile.sc
│   ├─ ProductCard.sc
│   └─ OrderList.sc
├─ ExtComponents/         # Component and WebComponent files (project-local extension modules)
│   ├─ InputField.cp
│   ├─ DataAuth.cp
│   └─ RichChart.wc
├─ Process/               # Project generation auxiliary files (can be any file type such as .md, .json, etc.; do not participate in VL compilation, but are part of the project)
├─ Config/                # Project-level engineering config files; do not participate in VL compilation
│   ├─ project.state.json
│   ├─ project.settings.json
│   └─ secrets.json
├─ Theme/                 # Theme files (Point Slot Values only; one Theme file per project)
│   └─ MyProject.vth
└─ Apps/                  # App files (one file per application)
    ├─ ShopApp.vx
    └─ AdminApp.vx
```

VL source files under `Services/`, `Database/`, `Sections/`, `ExtComponents/`, `Theme/`, and `Apps/` participate in the final project code compilation. Files under `Process/` and `Config/` do not participate in VL compilation.

### `Config/` Directory

`Config/` is the standard project-level engineering configuration directory. It is used for project state, platform/project settings, and secrets. It does not define business VL syntax, does not replace `Services/Sections/ExtComponents/Apps`, and does not introduce an additional env-schema layer.

| File | Writable | Maintainer | Purpose |
| ---- | -------- | ---------- | ------- |
| `Config/project.state.json` | No | parser | Project state and app-binding metadata |
| `Config/project.settings.json` | Yes | user | User-editable deployment settings and latest deployment snapshot |
| `Config/secrets.json` | Yes | user | Project-private secrets |

**Standard responsibilities:**

- `project.state.json`: parser-maintained project state, including project `gid`, the single backend app binding, and each frontend app's stable `nid`
- `project.settings.json`: project/platform settings and deployment snapshot; its field schema and platform synchronization behavior are defined by Platform API / Project Config documentation, not by the VL language grammar
- `secrets.json`: project-private secrets; local development may keep plaintext, while production should rely on platform secret management

**App binding rules:**

- Each project has exactly one backend app
- The backend app name should follow `<ProjectName>BackendMainApp`
- Backend app naming is maintained by parser rather than freely edited by users
- `.vx` filenames under `Apps/` are stable frontend app identifiers
- Frontend apps do not support rename as a normal workflow; if replacement is needed, delete the old app and create a new one

### File Cross-References

VL code files support a certain degree of cross-referencing. Please strictly follow these rules:

|                              | App | Section | ServiceDomain | Component | WebComponent |
| ---------------------------- | --- | ------- | ------------- | --------- | ------------ |
| App can reference            | ❌  | ✅      | ❌            | ✅        | ✅           |
| Section can reference        | ❌  | ❌      | ✅            | ✅        | ✅           |
| Component can reference      | ❌  | ❌      | ❌            | ❌        | ✅           |
| WebComponent can reference   | ❌  | ❌      | ❌            | ❌        | ❌           |
| ServiceDomain can reference  | ❌  | ❌      | ❌            | ❌        | ❌           |

### File Sections and Structure Examples

VL files follow strict section division. **All sections must appear in order and only once**; otherwise compilation will fail.

#### 1. `.vx` — App (Application Entry)

**Core Responsibilities**: Route management, Section orchestration, cross-module coordination

**Required Sections**: `# SysConfig`, `# Frontend Tree`, `# Frontend Event Handlers`

```vl
// VL_VERSION:4.3.1
<App-ShopApp "root">

# SysConfig
DEVICE_TARGET:"PC"
SCREEN_RESOLUTION:"1920x1080"

# Frontend Global Vars
$currentKeyword(STRING) = ""
$selectedProductId(INT) = 0

# Frontend Tree
<Page-Home "homePage"> path:"home"
-<Row-Layout> gap:"16px"
--<Section-Sidebar "sidebar"> width:"240px"
--<Col-MainArea> flex:"1"
---<Section-SearchBar "searchBar"> width:100% height:"64px"
---<Section-ProductList "productList"> flex:"1" margin-top:"16px"

# Frontend Event Handlers
<Section-SearchBar "searchBar">.@searchSubmit(keyword)
-$currentKeyword = keyword
-<Section-ProductList "productList">.RefreshWithKeyword(keyword)

<Section-ProductList "productList">.@productSelected(productId)
-$selectedProductId = productId
-<ClientUtils>.switchRoute("product-detail")

# Frontend Internal Methods
# Frontend Pipeline Funcs
</App-ShopApp>
```

**Layout Control Examples**: Header `height:"64px"`, Sidebar `width:"240px"`, Main `flex:"1"`

#### 2. `.sc` — Section (Business Module)

**Core Responsibilities**: Business logic, data interaction, ServiceDomain calls

**Required Sections**: `# Frontend Tree`, `# Frontend Event Handlers`

```vl
// VL_VERSION:4.3.1
// Preview: width:100% min-height:400px
<Section-ProductList "root"> containerType:col padding:24px

# Frontend Public Props
$categoryId(INT) = 0

# Frontend Public Events
EVENT @productSelected(productId(INT))

# Frontend Public Methods
METHOD RefreshWithKeyword(keyword(STRING)); RETURN {success:BOOL}
-$searchKeyword = keyword
-loadProducts() -> _result
-RETURN {success:_result.success}

# Frontend Global Vars
$products([{id:INT,name:STRING,price:FLOAT,image:STRING}]) = []
$searchKeyword(STRING) = ""
$isLoading(BOOL) = false

# Frontend Tree
<ServiceDomain-Product "productService">
-<Service-GetProducts> params:(categoryId(INT),keyword(STRING)) returns:(success(BOOL),data([{}]))

<Col-ListContainer> gap:"16px"
-<If-Loading> conditions:$isLoading
--<Component-LoadingSpinner "spinner">
-<If-HasProducts> conditions:($products.length > 0)
--<For-Products> sourceArray:$products loopVar:[_item0,_index0]
---<Component-ProductCard "card"> product:_item0 width:"280px"
-<If-Empty> conditions:($products.length == 0 && !$isLoading)
--<Component-EmptyState "emptyState"> message:"No products found"

# Frontend Event Handlers
<Component-ProductCard "card">.@cardClick(productId)
-@productSelected(productId)

# Frontend Internal Methods
METHOD loadProducts(); RETURN {success:BOOL}
-$isLoading = true
-<ServiceDomain-Product "productService">.GetProducts($categoryId, $searchKeyword) -> _result
-$isLoading = false
-GUARD (!_result.success) "Load products failed"
-$products = _result.data
-RETURN {success:true}

# Frontend Pipeline Funcs
</Section-ProductList>
```

#### 3. `.cp` — Component (Pure UI Component)

**Core Responsibilities**: Pure UI display, reusable components, no business logic

**Required Sections**: `# Frontend Tree`, `# Frontend Event Handlers`

```vl
// VL_VERSION:4.3.1
// Preview: width:280px height:360px
<Component-ProductCard "root"> containerType:col padding:16px

# Frontend Public Props
$product({id:INT,name:STRING,price:FLOAT,image:STRING}) = {id:0,name:"",price:0,image:""}

# Frontend Public Events
EVENT @cardClick(productId(INT))

# Frontend Derived Vars
$formattedPrice(STRING) = ("¥" + $product.price.toFixed(2))

# Frontend Tree
<Col-CardContent> gap:"12px"
-<Image-ProductImage "thumb"> sourceUri:$product.image width:100% height:"200px"
-<Text-ProductName "title"> value:$product.name
-<Text-ProductPrice "price"> value:$formattedPrice

# Frontend Event Handlers
<Col-CardContent>.@click()
-@cardClick($product.id)

# Frontend Internal Methods
# Frontend Pipeline Funcs
</Component-ProductCard>
```

**Key Differences**:

- App controls Section position and size (`width:100%`, `flex:"1"`)
- Section handles business logic and data loading, using Components internally
- Component only handles UI display, receives data via Props, communicates via Events

#### 4. `.wc` — WebComponent (Project-Local Native Web Extension)

**Core Responsibilities**: Native browser rendering, third-party JS integration, canvas/chart/editor/map/custom element capabilities that are not expressible through ordinary VL component trees.

**Required Header Contract**: the header starts with first-line `// VL_VERSION`, followed by `// VL_WC_LEVEL`, optional `// Description`, `// Props`, `// Events`, `// Methods`, optional `// Dependencies`, and complete structured `// @meta component|prop|event|method ...` comments for every public interface item. `// Dependencies` is required only when the `.wc` implementation loads external JS or CSS dependencies.

`.wc` files are project-local frontend extension modules. A `.wc` file placed under `ExtComponents/` participates in project compilation and can be referenced by App, Section, and Component files through `<WebComponent-Name "id">`, where `Name` is the PascalCase file stem. For example, `ExtComponents/RichChart.wc` is referenced as `<WebComponent-RichChart "chart"> option:$chartOption width:"100%" height:"360px"`.

Rules:

1. `.wc` source does not use VL chapter sections such as `# Frontend Tree`, `# Frontend Public Props`, or `# Frontend Event Handlers`.
2. The public interface is declared only by the top header lines `Props:`, `Events:`, and `Methods:`, and each public declaration must have matching `@meta` with `name:"..."`.
3. The JavaScript body must define the browser-side implementation. It should register exactly one custom element class or equivalent mount target for the component body, and must avoid leaking additional global names beyond intentional dependency loaders. The custom element tag name does not have to be derived from the VL `WebComponent-*` import name; when exactly one custom element is registered by the `.wc`, the platform binds the VL module to that registered element.
4. Public props are passed from VL instance attributes into the WebComponent wrapper. Scalar values may be reflected as attributes when representable; `OBJECT`, `ARRAY`, and `ANY` values must be delivered through JavaScript property channels.
5. A dynamic public prop must have an incremental update path, such as a setter, `attributeChangedCallback`, or a dedicated internal apply method. Props that can only be read during initialization must declare `binding:"static"` in their `@meta prop`.
6. Public events declared as `@eventName(param(Type))` must be emitted by the implementation as DOM `CustomEvent` events named `eventName`; event parameters are carried in `event.detail` by parameter name.
7. Public methods declared in `Methods:` must map to callable JavaScript methods with the same public name. If a method returns asynchronously through callback, the callback parameter must be declared in `Methods:` and described in `@meta method`.
8. A `WebComponent-*` instance follows the same external size contract as other module component instances. If the parent writes `width`, `height`, `flex`, `min-*`, or `max-*`, the visible custom element body must consume the corresponding host frame instead of leaving only an empty wrapper resized.
9. A `.wc` implementation may load external JS or CSS dependencies, but dependency URLs and versions must be explicit in the header `Dependencies:` comment and deterministic in the source body.
10. Project-local `.wc` files are ordinary project source files. They do not require any external component catalog, registry, or group metadata to compile inside the current project.
11. If a `.wc` creates external resources such as library instances, timers, observers, subscriptions, sockets, or manually attached global listeners, it should release them in `disconnectedCallback` or an equivalent teardown path.

`VL_WC_LEVEL`:

- `// VL_WC_LEVEL:3` is the current standard authoring level for `VL_VERSION:4.3.1` `.wc` source.
- Level `3` means the `.wc` uses the standard header contract, structured `@meta`, VL-to-DOM prop bridge, DOM `CustomEvent` event bridge, and public method bridge defined in this section.
- Levels `1` and `2` are reserved for historical parser compatibility and must not be generated for new `VL_VERSION:4.3.1` source.
- Any value other than `3` is outside the current `.wc` authoring contract.

Header line syntax:

- Header declarations are single physical `//` comment lines in the top header block.
- Recommended header order is `VL_VERSION` -> `VL_WC_LEVEL` -> `Description` -> `Props` -> `Events` -> `Methods` -> `Dependencies` -> `@meta`. `VL_VERSION` must remain the first line; the rest of the order is the standard authoring style for predictable AI and tool output.
- `// Props:` is required. It contains a comma-separated list of `propName(Type)` entries, or an empty list when the component exposes no public props.
- `// Events:` is required. It contains a comma-separated list of `@eventName(paramName(Type),...)` entries, or an empty list when the component exposes no public events.
- `// Methods:` is required. It contains a comma-separated list of `methodName(paramName(Type),...)` entries, or an empty list when the component exposes no public methods.
- `// Dependencies:` is required only when external JS or CSS dependencies are loaded. It contains a comma-separated list of deterministic dependency identifiers such as `library@version` or explicit URLs.
- `// Description:` is optional. It is a single free-text summary line for human readability; structured machine-readable contract data still belongs in `// @meta ...`.
- Public prop, event, method, and parameter names use camelCase. Types use the normal VL public interface type names, including scalar types and structured object or array types.

VL-to-DOM bridge contract:

- The canonical DOM attribute name for a public prop is the lowercase form of the public prop name, without inserting hyphens. For example, `toolbarPreset` maps to `toolbarpreset`, and `customClass` maps to `customclass`.
- `observedAttributes`, `getAttribute`, `setAttribute`, `removeAttribute`, and `attributeChangedCallback` must use the canonical DOM attribute name.
- For a `BOOL` public prop sent through the DOM attribute channel, attribute presence means `true` and attribute absence means `false`. The host expresses `false` by removing the attribute, not by writing the string `"false"`.
- If a `BOOL` prop is delivered through a JavaScript property channel instead of an attribute channel, the property value must be treated as a real boolean value.
- `OBJECT`, `ARRAY`, and `ANY` values must not rely on stringified HTML attributes as their primary runtime channel; they must be accepted through JavaScript property setters or an equivalent property bridge.

Single-line examples:

- `// VL_VERSION:4.3.1`
- `// VL_WC_LEVEL:3`
- `// Props: option(OBJECT), disabled(BOOL), renderer(STRING)`
- `// Events: @ready(), @pointClick(name(STRING),value(ANY))`
- `// Methods: resize(), toBase64(callback(OBJECT))`
- `// Dependencies: chartlib@1.2.3`
- `// @meta prop name:"option" description:"Complete chart option object." required:true control:"json" example:({series:[{type:"bar",data:[1,2,3]}]})`
- `<WebComponent-RichChart "chart"> option:$chartOption renderer:"canvas" width:"100%" height:"360px"`
- JavaScript property channel for object props: `set option(v) { this._option = v; this._applyOption(); }`

Minimal complete `.wc` example:

```javascript
// VL_VERSION:4.3.1
// VL_WC_LEVEL:3
// Description: Minimal text input WebComponent.
// Props: value(STRING), disabled(BOOL), customClass(STRING), option(OBJECT)
// Events: @change(value(STRING))
// Methods: focus()
// @meta component summary:"Minimal text input WebComponent." keywords:(["input","web component"]) useCases:(["custom input bridge"]) notFor:(["rich text editing"])
// @meta prop name:"value" description:"Current input value." required:false example:"hello"
// @meta prop name:"disabled" description:"Whether the input is disabled. Attribute presence means true." required:false example:false
// @meta prop name:"customClass" description:"Custom CSS class applied to the input element." required:false example:"compact"
// @meta prop name:"option" description:"Optional structured configuration delivered through the JavaScript property channel." required:false control:"json" example:({maxLength:32})
// @meta event name:"change" description:"Fired when the user changes the value." params:({value:{description:"Latest input value.",required:true,example:"hello"}})
// @meta method name:"focus" description:"Move focus to the input."
(function() {
  'use strict';
  if (customElements.get('vl-mini-input')) return;
  class VLMiniInput extends HTMLElement {
    static get observedAttributes() { return ['value', 'disabled', 'customclass']; }
    constructor() { super(); this._input = null; this._option = {}; }
    set option(v) { this._option = v || {}; this._apply(); }
    get option() { return this._option; }
    connectedCallback() {
      this.innerHTML = '';
      this._input = document.createElement('input');
      this.appendChild(this._input);
      this._input.addEventListener('input', () => this.dispatchEvent(new CustomEvent('change', { detail: { value: this._input.value } })));
      this._apply();
    }
    disconnectedCallback() { this._input = null; }
    attributeChangedCallback() { this._apply(); }
    focus() { if (this._input) this._input.focus(); }
    _apply() { if (!this._input) return; this._input.value = this.getAttribute('value') || ''; this._input.disabled = this.hasAttribute('disabled'); this._input.className = this.getAttribute('customclass') || ''; }
  }
  customElements.define('vl-mini-input', VLMiniInput);
})();
```

#### 5. `.vs` — ServiceDomain (Backend Service Domain)

**Required Sections**: `# Backend Tree`, `# Services`. Optional section: `# Backend Environment Vars` (before `# Backend Tree`).

```vl
// VL_VERSION:4.3.1
<ServiceDomain-Doc>

# Backend Environment Vars
ENV SOME_API_KEY(STRING) "External API key"

# Backend Tree
<VirtualTable-DocList "docTable"> sourceTable:Documents
-<Field-title> type:STRING
-<Field-content> type:STRING

# Services
SERVICE GetDocList();RETURN {success:BOOL,data:[{}]}
-<VirtualTable-DocList "docTable">.select(null,[["_update","desc"]],[0,100]) -> _r
-RETURN {success:true,data:_r.dataArray}

# Backend Event Handlers
# Transactions
# Backend Internal Methods
# Backend Pipeline Funcs
</ServiceDomain-Doc>
```

#### 6. `.vdb` — Database (Database Structure)

```vl
// VL_VERSION:4.3.1
<Database-ProjectName>
<Table-Documents> data:[{"_id":1,"title":"Doc1","_create":"2024-01-15 09:30:00"}]
-<Field-title> type:STRING notNull:true
-<Field-content> type:STRING
-<Index-TitleIdx> type:UNIQUE fields:["title"]
<Relation-UserDocs> users._id<<documents._user
</Database-ProjectName>
```

#### 7. `.vth` — Theme (Theme Configuration)

**Rules**: Singleton file. File structure is two sections (fixed order): Meta (optional) and Point Slot Values.

```vl
// VL_VERSION:4.3.1
<Theme-ProjectName>

# Meta
mode:"light"

# Point Slot Values
intent.primary.intentBg:#0052D9
intent.primary.intentFg:#FFFFFF
intent.primary.intentBorder:#0052D9
intent.primary.intentOnBg:#FFFFFF
intent.primary.intentFocusRing:0 0 0 2px rgba(0,82,217,0.2)
intent.primary.intentSubtleBg:rgba(0,82,217,0.08)
intent.danger.intentBg:#F53F3F
intent.danger.intentFg:#FFFFFF
intent.danger.intentBorder:#F53F3F
intent.danger.intentOnBg:#FFFFFF
intent.danger.intentFocusRing:0 0 0 2px rgba(245,63,63,0.2)
intent.neutral.intentBg:#F5F5F5
intent.neutral.intentFg:#1D2129
intent.neutral.intentBorder:#E5E6EB
emphasis.filled.emphasisBg:@intent.intentBg
emphasis.filled.emphasisFg:@intent.intentOnBg
emphasis.filled.emphasisBorderColor:transparent
emphasis.filled.emphasisBorderWidth:0
emphasis.outlined.emphasisBg:transparent
emphasis.outlined.emphasisFg:@intent.intentFg
emphasis.outlined.emphasisBorderColor:@intent.intentBorder
emphasis.outlined.emphasisBorderWidth:1px
emphasis.ghost.emphasisBg:transparent
emphasis.ghost.emphasisFg:@intent.intentFg
emphasis.ghost.emphasisBorderColor:#E5E6EB
emphasis.ghost.emphasisBorderWidth:1px
emphasis.tonal.emphasisBg:@intent.intentSubtleBg
emphasis.tonal.emphasisFg:@intent.intentFg
emphasis.tonal.emphasisBorderWidth:0
shape.default.shapeRadius:4px
shape.pill.shapeRadius:9999px
shape.square.shapeRadius:0
surface.solid.surfaceBg:#FFFFFF
surface.elevated.surfaceBg:#FFFFFF
surface.elevated.surfaceShadow:0 4px 12px rgba(0,0,0,0.08)
surface.overlay.surfaceBg:#FFFFFF
surface.overlay.surfaceBackdrop:rgba(0,0,0,0.45)
affordance.passive.cursor:default
affordance.listitem.hoverOverlay:rgba(0,0,0,0.06)
affordance.listitem.activeOverlay:rgba(0,0,0,0.12)
affordance.navitem.focusRing:0 0 0 2px rgba(0,82,217,0.2)

</Theme-ProjectName>
```

### App, Section, Component Responsibility Division

| Dimension                    | App (.vx)                    | Section (.sc)                | Component (.cp)              |
| ---------------------------- | ---------------------------- | ---------------------------- | ---------------------------- |
| Core Responsibilities        | Route management, Section orchestration, global state | Business module, data interaction, complete business logic | Pure UI display, reusable, no business logic |
| Call ServiceDomain           | ❌                           | ✅                           | ❌                           |
| Contain Section              | ✅                           | ❌                           | ❌                           |
| Contain Component            | ✅                           | ✅                           | ❌                           |
| Use For/If                   | ❌ (no For/TreeFor)          | ✅                           | ✅                           |
| Can nest children when used  | N/A                          | ❌ (widgets only)            | ❌ (widgets only)            |
| External layout on root      | N/A                          | ❌ (set by App)              | ❌ (set by parent)           |
| Root container type          | Layout root                  | `containerType`, default `col` | `containerType`, default `col` |

**Key Rules:**

- Section and Component are **independent components** when used — strictly forbidden to nest other components under them (except widgets)
- App cannot directly add basic UI components or contain business logic
- Section cannot nest other Sections or handle route navigation
- Component cannot call ServiceDomain or nest other Components

#### Style Property Responsibility Division

**Core Principle:** Outer file controls "space and layer order", Section/Component definition file controls "appearance and content". (Outer file means App file, or if Section embeds Component, then relative to Component, this Section file is the outer file)

| Property Type              | Specific Properties                        | Outer | Section/Component Internal |
| -------------------------- | ------------------------------------------ | ----- | -------------------------- |
| **External Size**    | width, height                              | ✅    | ❌                         |
| **Flex/Grid**        | flex, grid-column, etc.                    | ✅    | ❌                         |
| **External Spacing** | margin, margin-\*                          | ✅    | ❌                         |
| **Layering**         | z-index                                    | ✅    | ❌                         |
| **Visibility**       | show                                       | ✅    | ❌                         |
| **Internal Container** | containerType (`col` / `row` / `grid`) | ❌    | ✅                         |
| **Internal Spacing** | padding, padding-\*                        | ❌    | ✅                         |
| **Skin Expression**  | `style` coordinate / `sk.*`                | ❌    | ✅                         |
| **Content Overflow** | overflow                                   | ❌    | ✅                         |
| **Size Constraints** | min-width / max-width / min-height / max-height | ⚠️  | ⚠️                         |

**Preview Size Annotation:** Section/Component root component does not define external layout properties. Preview values are comment-only IDE hints and MUST NOT become runtime layout props.

```vl
// Preview: width:240px height:100vh
<Section-Sidebar "root"> containerType:col padding:16px
```

#### Closed CSS Property Model (VL 4.2.5)

VL is not a general CSS authoring language. Component properties adopt a **closed whitelist** model.

Rules:

1. Raw CSS properties must fall into exactly one of these sets:
   - allowed direct-write CSS properties
   - restricted CSS properties
   - forbidden CSS properties
2. Non-CSS VL-specific attributes must fall into the VL structure-attribute list
3. Any property not explicitly listed in one of these categories is forbidden
4. Tools and AI agents MUST NOT infer extra legal CSS properties from general web/CSS knowledge

##### Allowed Direct-Write Skeleton CSS Properties (Complete List)

- `width`
- `height`
- `min-width`
- `max-width`
- `min-height`
- `max-height`
- `flex`
- `padding`
- `padding-top`
- `padding-right`
- `padding-bottom`
- `padding-left`
- `margin`
- `margin-top`
- `margin-right`
- `margin-bottom`
- `margin-left`
- `gap`
- `align-items`
- `justify-content`
- `flex-wrap`
- `overflow`
- `font-size`
- `font-style`
- `font-weight`
- `line-height`
- `letter-spacing`
- `text-align`
- `white-space`
- `text-overflow`
- `word-spacing`
- `word-break`
- `text-indent`
- `max-rows`
- `z-index`
- `grid-template-columns`

##### VL Structure Attributes (Complete List)

- `containerType`

`containerType` rules:

1. `containerType` is not a general CSS property
2. It is only valid on `.sc/.cp` root
3. Allowed values are `col`, `row`, `grid`

##### Fixed Skin Override Props (Complete List)

- `sk.bg`
- `sk.fg`
- `sk.bgImage`
- `sk.borderColor`
- `sk.borderWidth`
- `sk.borderTop`
- `sk.borderRight`
- `sk.borderBottom`
- `sk.borderLeft`
- `sk.shadow`
- `sk.opacity`
- `sk.radius`

##### Per-Node-Class Property Applicability

The closed whitelists above declare what properties exist. This sub-section declares **which node class each property may be written on**. Any property × node-class cell marked ❌ is invalid; tools and AI agents MUST NOT write properties outside their applicable cells.

**Node classes:**

- **Layout container (普通布局容器)**: `Row`, `Col`, `Grid`, `Block`. Pure containers carrying child nodes, area background, and layout structure.
- **Flex/grid container (flex/grid 排布容器)**: `Row`, `Col`, `Grid`. A narrower subset of layout containers that accept `gap`, `align-items`, `justify-content`, and `grid-template-columns`. `Row` additionally accepts `flex-wrap` for simple inline wrapping flows. `Block` is a layout container but is not a flex/grid container. `Button` is not a layout container, but it has a narrow child-layout exception for icon/text composition: `gap`, `align-items`, and `justify-content`.
- **Atomic component (基础组件)**: `Text`, `Icon`, `Image`, `Video`, `Button`, `Input`, `Textarea`, `Divider`. Atomic UI nodes whose internal rendering is platform-implemented; external code must not inject CSS authoring directives into their internals.
- **Module component instance (模块组件实例)**: `Section-*`, `Component-*`, `WebComponent-*` non-declaration-root usage sites. The module source may be project-local or imported; from the outside, the instance exposes only public props/events/methods plus an outer frame.

`App`, `Page`, and `Modal` are special boundaries handled in their own chapters; they do not enter this matrix and must not be inferred as layout containers or module component instances.

`.sc/.cp` files' own `<Section-... "root">` / `<Component-... "root">` real root is not a module component instance usage site; it follows the `.sc/.cp Real Root Model` rules.

**Skeleton CSS applicability:**

| Property | Layout container | Text/Icon | Button | Input/Textarea | Image/Video | Divider | Module instance |
|---|---|---|---|---|---|---|---|
| `width` `height` `min-*` `max-*` `flex` `margin` `margin-*` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `padding` `padding-*` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ |
| `gap` | ✅ Row/Col/Grid | ❌ | ✅ (children gap) | ❌ | ❌ | ❌ | ❌ |
| `align-items` `justify-content` | ✅ Row/Col/Grid | ❌ | ✅ (children alignment) | ❌ | ❌ | ❌ | ❌ |
| `flex-wrap` | ✅ Row only (`wrap` / `nowrap`) | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `overflow` | ✅ | ✅ Text ellipsis/clipping | ❌ | ✅ Textarea | ❌ | ❌ | ❌ |
| `z-index` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `font-size` `font-style` `font-weight` `line-height` `letter-spacing` | ❌ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| `text-align` | ❌ | ✅ Text/Icon | ✅ | ✅ | ❌ | ❌ | ❌ |
| `white-space` `text-overflow` `word-spacing` `word-break` `text-indent` `max-rows` | ❌ | ✅ Text | ❌ | ❌ | ❌ | ❌ | ❌ |
| `grid-template-columns` | ✅ Grid | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |

**sk.\* applicability:**

| Property | Layout container | Text/Icon | Button | Input/Textarea | Image/Video | Divider | Module instance |
|---|---|---|---|---|---|---|---|
| `sk.bg` `sk.bgImage` `sk.shadow` `sk.radius` `sk.opacity` | ✅ | ✅ | ✅ | ✅ | ✅ (radius mainly) | ❌ | ✅ |
| `sk.borderColor` `sk.borderWidth` `sk.borderTop/Right/Bottom/Left` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `sk.fg` | ❌ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |

**Design principles:**

(a) **Text-family skeleton properties** only apply to text-rendering nodes. Common text metrics (`font-size`, `font-style`, `font-weight`, `line-height`, `letter-spacing`) apply to `Text`, `Icon`, `Button`, `Input`, and `Textarea`; Text display-flow properties (`white-space`, `text-overflow`, `word-spacing`, `word-break`, `text-indent`, `max-rows`) apply to `Text`. VL deliberately differs from browser CSS here: text-related skeleton properties do not promise inheritance, and authors are not allowed to rely on CSS cascade to flow text styles from a container down to child `Text`. Text appearance must land on the node that actually renders text or on an explicit public semantic prop of the component.

(b) **Internal layout properties** (`padding`, `gap`, `align-items`, `justify-content`, `overflow`, `flex-wrap`) express "how this node arranges its children" and only apply to container-shaped nodes:

- `padding` is a box-model inset and is allowed on layout containers and all basic components; it remains forbidden on module component instances because external padding would pierce the component boundary
- `gap` is a flex/grid container feature, restricted to Row/Col/Grid (Button is an exception for its icon + text spacing)
- `align-items` / `justify-content` are flex/grid main/cross axis alignment, restricted to Row/Col/Grid (Button is an exception for icon/text child alignment); `Block` as a layout container does not accept these alignment properties
- `overflow` is a clipping decision restricted to layout containers, `Textarea`, and `Text` for ellipsis/clipping. `Text overflow` does not mean container clipping of children; it serves `white-space` / `text-overflow` text-rendering semantics
- `grid-template-columns` is restricted to `Grid`
- `flex-wrap` is a Row-only inline wrapping flag, restricted to `Row` with values `wrap` or `nowrap`

(c) **External size contract** (`width`, `height`, `min-*`, `max-*`, `flex`, `margin*`) applies to all nodes; this is the only stable external layout contract between sibling and parent.

(d) **Layering**: `z-index` is the public layer-order skeleton property. VL does not expose `position`, `top`, `right`, `bottom`, or `left`; fixed overlays and coordinate positioning must be expressed through dedicated components or component internals.

(e) **sk.\* skin overrides:**

- Surface family (`sk.bg`, `sk.bgImage`, `sk.shadow`, `sk.radius`, `sk.opacity`) acts on the node's host element and is allowed on every non-Divider node (Divider's body is a single line, no fillable surface)
- Border family (`sk.borderColor`, `sk.borderWidth`, `sk.border*`) is allowed on every node (Divider's border IS the divider)
- Foreground family (`sk.fg`) only carries meaning on direct text/icon-rendering nodes (`Text`, `Icon`, `Button`, `Input`, `Textarea`); written on a container or module instance it does not reliably penetrate to inner child nodes

(f) **Module instance specifics**: a module instance is opaque from the outside, so it accepts only the external size contract, `z-index`, and host-level container sk.* (surface + border families). Internal layout, typography, and foreground skin must be either consumed inside the component implementation or exposed as semantic public props (e.g. `density`, `itemPadding`, `contentInset`).

##### Forbidden CSS Properties (Complete List)

- `display`
- `position`
- `top`
- `right`
- `bottom`
- `left`
- `flex-direction`
- `flex-grow`
- `flex-shrink`
- `flex-basis`
- `flex-flow`
- `color`
- `background`
- `background-color`
- `background-image`
- `background-repeat`
- `background-position`
- `background-size`
- `border`
- `border-top`
- `border-right`
- `border-bottom`
- `border-left`
- `border-color`
- `border-width`
- `border-style`
- `box-shadow`
- `opacity`
- `border-radius`
- `text-transform`
- `cursor`

##### Restricted CSS Properties

- `flex`
- `flex-wrap`

`flex` rules:

1. `flex` allows positive numeric grow factors such as `flex:"1"`, `flex:1`, `flex:"2"`, and `flex:"0.5"`
2. `flex` in VL means "consume remaining parent-axis space" according to the numeric grow factor
3. `flex` is not the general CSS shorthand and does not support `flex:"1 1 auto"`, `flex:"auto"`, `flex:"none"`, or `flex:"0 0 240px"`
4. For complex two-dimensional ratio layouts, prefer `Grid` with `grid-template-columns`

`flex-wrap` rules:

1. `flex-wrap` only allows `flex-wrap:"wrap"` or `flex-wrap:"nowrap"`
2. `flex-wrap` is only valid on `Row`; on `Col`, `Grid`, `Block`, atomic components, or module instances it is not part of the node's public skeleton contract
3. `wrap-reverse` and other CSS shorthand values are not part of VL standard authoring

##### Mutually Exclusive Property Families

`padding` family:

- `padding`
- `padding-top`
- `padding-right`
- `padding-bottom`
- `padding-left`

Rules:

1. `padding` and any `padding-*` cannot be mixed on the same component
2. `padding-top/right/bottom/left` may be combined with each other

`margin` family:

- `margin`
- `margin-top`
- `margin-right`
- `margin-bottom`
- `margin-left`

Rules:

1. `margin` and any `margin-*` cannot be mixed on the same component
2. `margin-top/right/bottom/left` may be combined with each other

##### Section/Component Instance Main-Axis Contract

When a `Section` or `Component` instance is a direct child of parent `Row` or `Col`, it must declare exactly one main-axis contract.

Parent list:

- `Row`
- `Col`

Applicable child list:

- `Section`
- `Component`

Rules:

1. Under parent `Row`, a direct `Section` / `Component` child MUST declare exactly one of:
   - `width`
   - `flex`
2. Under parent `Col`, a direct `Section` / `Component` child MUST declare exactly one of:
   - `height`
   - `flex`
3. `width:"100%"` / `height:"100%"` are valid contracts; a contract is not limited to fixed pixel values

##### Automatic Shrink Behavior for `flex:"1"`

To keep flex children from being clipped by browser default shrink behavior:

1. Under parent `Row`, any direct child using `flex:"1"` automatically receives `min-width:"0"`
2. Under parent `Col`, any direct child using `flex:"1"` automatically receives `min-height:"0"`
3. If authors explicitly declare `min-width` or `min-height`, the explicit value wins

These two props are reserved override props rather than first-class standard authoring props.

##### Scroll Container Stable Size Carrier

When a `Col` or `Row` declares `overflow:"auto"` or `overflow:"scroll"`, it becomes a scroll container. A scroll container MUST NOT rely solely on content to determine its own size. It MUST obtain a stable size carrier through one of the following:

1. Explicit size declaration (e.g., `height:"400px"`, `height:"100vh"`, `width:"320px"`)
2. `flex:"1"`, provided its parent container already has a stable main-axis size carrier
3. A resolvable percentage size (e.g., `height:"100%"`), provided the parent node itself has a resolvable explicit size

The purpose of `overflow` is to host scrolling, not to let content push the container open. If a scroll container has no stable size carrier, runtime behavior degrades to content compression, scroll failure, or visual overlap.

##### Scroll List Item Must Not Use Same-Axis `flex:"1"` in Scroll Container

When a list area meets both of the following conditions:

1. The parent container hosts scrolling (`overflow:"auto"` or `overflow:"scroll"`)
2. Child items are repeated list items (e.g., `Row`/`Col`/`Grid`/`Component` items generated by `For`)

Then these list items SHOULD have a stable minimum readable height. They must not be infinitely compressed when the parent container shrinks.

Scroll intent and "same-axis continued compression to absorb overflow" intent are contradictory:

1. If the parent is a vertical scroll container (`Col overflow:"auto"` / `overflow:"scroll"`), repeated list items must not participate in vertical remaining-height distribution via `flex:"1"`
2. If the parent is a horizontal scroll container (`Row overflow:"auto"` / `overflow:"scroll"`), repeated list items must not participate in horizontal remaining-width distribution via `flex:"1"`

The reason is:

- Scrolling requires content to form real overflow on the scroll axis
- Same-axis `flex:"1"` attempts to continue participating in remaining-space distribution and compression
- When both are combined, runtime often absorbs the overflow that should trigger scrolling, resulting in "cards squished flat, stuck together, unable to scroll"

Therefore:

1. "Repeated list items inside a scroll container using same-axis `flex:"1"`" is an explicit anti-pattern
2. Whether list items declare `min-height` is an author-side layout detail; the spec does not set a separate warning for this heuristic risk

Recommended approaches:

1. Declare explicit `min-height` on list items
2. Or ensure list items naturally maintain stable height through clear internal layout

##### Container Host Main-Axis Contract for `flex:"1"` Children

If a `Row` or `Col` contains any direct child declaring positive numeric `flex`, the container itself SHOULD have an explicit host sizing intent for the axis consumed by that child. Otherwise the child's remaining-space semantics can be unstable, most commonly causing scroll areas to fall back to content height and lose intended scrolling.

Rules:

1. If a `Row` has any direct child declaring positive numeric `flex`, `width` is the explicit horizontal host contract.
2. If a `Col` has any direct child declaring positive numeric `flex`, `height` is the explicit vertical host contract.
3. `flex` conflicts with `width` / `height` only when it is a same-axis contract inherited from the parent container direction. A `Row` under a parent `Col` may declare both `width` and `flex`; a `Col` under a parent `Row` may declare both `height` and `flex`.
4. `width:"100%"` / `height:"100%"` are valid host main-axis contracts.
5. Missing host sizing intent is a lint warning. Same-axis duplicate contracts remain a lint error.
6. This rule applies to all direct children that declare positive numeric `flex`, not only container-type children.
7. This rule applies both to ordinary `Row` / `Col` nodes and to the first internal `Row` / `Col` under `.sc/.cp` root.

## Typical Error Examples

```vl
// ❌ Error: Section nested inside Section
<Section-MainLayout "root">
-<Section-Sidebar "sidebar">

// ❌ Error: Component calling ServiceDomain
<Component-UserCard "root">
<ServiceDomain-User "userService">

// ❌ Error: Nesting child components under Section
<Section-ProductList "productList">
-<Component-Pagination "pagination">

// ❌ Error: Section root component defining external layout properties
<Section-ProductCard "root"> width:"320px" margin:"16px" containerType:col padding:"24px"
```

## Core Syntax Constraints (Globally Applicable)

VL follows strict formatting rules to ensure code consistency and parsability. The following constraints apply globally unless specific components or syntax have explicit exceptions:

#### Single-Line Rule (Mandatory)

Many definitions and statements **must** be completed on a single line; line breaks are not allowed. This includes but is not limited to:

- Variable definitions (global `$var(...) = ...`, local `_var(...) = ...`, `_var({}) = {}`, `_var([]) = []`).
- Method, service, event, pipe definition lines (i.e., the first line, including all parameters and `RETURN` declarations).
- All `RETURN {...}` (for METHOD/SERVICE) or `RETURN value` (for PIPE) statements.
- All assignment statements (`$var = ...`, `_var = ...`).
- Object and array initialization assignments (even if structurally complex).
- Component property definitions on a single line (even if multiple properties separated by spaces, still on the same line).
- Component start declarations on a single line. A component declaration MUST NOT continue onto following lines, including `params:(...)` and `returns:(...)` when they belong to that same declaration.

Violating this rule will cause compilation errors. The following sections will not repeat "single-line rule" for each applicable case; developers should remember this global constraint.

#### No Leading Spaces and Flush Output Rule (Mandatory)

- All `-` (minus/hyphen) symbols representing hierarchical structure **must be flush left**; no spaces are allowed before them.
- In fact, in VL syntax, as long as a line has content, there should be **no spaces before any content** (except JSON code blocks or specific embedded languages).

#### Semicolon (;) Usage (Strict)

- Semicolons are also used as separators in `FOR` counting loop control parts (e.g., `FOR (_i(INT)=0; _i<10; _i++)`).
- **Forbidden** to use semicolons at the end of ordinary code lines (such as variable assignments, method calls, RETURN statements).
- **Forbidden** to use semicolons between style properties or after the last style property.

#### String Escaping

**Strictly forbidden to use any `\` escape character! Strictly forbidden to use `\` escape character anywhere! Check and remove all `\` escape characters before output!**

Important things said three times! Otherwise it will cause serious compilation errors!!

#### Indentation

VL uses `-` without spaces to represent indentation:

- **Top-level definitions (no indentation):** Root components (`<Section-..>`), section headings (`# Frontend Global Vars`, `# Services`, `# Frontend Tree`, etc.), and method/service/event/pipe definition lines (`METHOD...`, `SERVICE...`, `EVENT...`, `PIPE...`) are usually at the leftmost position of the code file, without leading `-`.
- **Frontend Tree:**

  - Components added directly under root are written without leading `-`.
  - Child components add one `-` at the beginning for each deeper level. For example:

    ```vl
    <Col-Header>
    -<Text-Title> value:"Title"
    <Col-Content>
    -<Button-Submit> value:"Submit"

    <VirtualTable-Users> ..
    -<Field-_id> ... // Field is VirtualTable child component
    ```
- **Inside methods/event handlers:**

  - Method/event handler body code blocks must all be indented at least one level (starting with `-`).
  - Control flow structures (`IF`, `FOR` blocks) introduce deeper indentation levels.

#### Section Markers and Comments

`#` represents standard section structure markers (such as `# Frontend Tree`, `# Frontend Default Styles`). Note: please strictly follow the preset section structure and order according to the current file development specification.

`//` represents inline comments and line comments.

`/*` and `*/` represent block comments.

Example:

```vl
// VL_VERSION:4.3.1

<Section-antUploadDragger>
/*
This module is mainly used to simulate the UploadDragger component in Ant Design.
*/

# Frontend Global Vars
$uploadedFiles([{uid:STRING,name:STRING,status:STRING,url:STRING,size:INT}]) = []

</Section-antUploadDragger>
```

#### Structured Source Meta Comments

VL source files declare structured source-comment metadata in two distinct namespaces, both of which do not change runtime behavior:

- `// @meta ...` — public contract metadata. Extracted, uploaded to the online component repository, and consumed by component catalog search, IDE panels, AI component understanding, and downloaded usage hints. Anything written under `@meta` IS the public contract.
- `// @contract ...` — source-internal engineering contracts. Read by parser, lint, authoring checks, and pre-upload preflight tools. NEVER extracted into `metadataJson`, NEVER shown in component catalog search, downloaded usage hints, or IDE public-interface panels. The first standardized `@contract` field is `@contract component sizeMode`, defined in `Component External Size Consumption Contract` below.

The remainder of this section governs `// @meta`. Source files define public interface metadata directly in structured `// @meta ...` comments. They are the extractable source-of-truth metadata for local preview tools, online component browsing, search, downloaded IDE usage hints, AI editing, refactoring, and project-level meta map construction.

Public interface meta is required for public interfaces. It is not an optional prose comment. A public declaration without the required source meta is incomplete source.

Rules:

- Do not add extra VL chapters for metadata. Existing `# Frontend Public Props`, `# Frontend Public Events`, `# Frontend Public Methods`, and `# Services` remain unchanged.
- Only `// @meta` comments participate in metadata extraction. Ordinary `//` comments remain ordinary comments.
- Supported scopes are `component`, `prop`, `event`, `method`, and `service`.
- All `// @meta` comments for a source file must be written in one contiguous source-meta header block near the top of the file. They must not be interleaved with public declarations, tree code, style declarations, event handlers, method bodies, service bodies, or any implementation body code.
- In `.cp`, `.sc`, and `.vs`, the source-meta header block must appear after the root declaration line and before the first `#` section. In `.wc`, it must appear in the existing top header comment area before implementation code.
- `component` meta applies to the current file root component. A component file must have only one `component` meta group.
- `prop`, `event`, `method`, and `service` meta bind to public declarations only through `name:"..."` locators. Each declaration-level meta line must use `name:"..."` to identify the public declaration it describes.
- For `.cp/.sc`, `@meta prop name:"..."`, `@meta event name:"..."`, and `@meta method name:"..."` must match declarations in `# Frontend Public Props`, `# Frontend Public Events`, and `# Frontend Public Methods`.
- For `.wc`, public interface structure still follows the existing `Props:`, `Events:`, and `Methods:` header lines. `@meta prop name:"..."`, `@meta event name:"..."`, and `@meta method name:"..."` must identify the target header declaration.
- For `.vs`, `@meta service name:"..."` must identify a `SERVICE` or `PUBLIC_SERVICE` declaration in `# Services`.
- A `// @meta ...` line must not contain a public declaration, `SERVICE`, `METHOD`, `EVENT`, tree component, or any other VL statement on the same physical line.
- Source meta is required only for public interfaces. Internal variables, private helper methods, and intermediate implementation state do not require source meta unless they are promoted into a public contract.

Reserved public prop names:

- Project-defined public prop names in `.cp`, `.sc`, and `.wc` must not collide with VL reserved component-instance property namespaces.
- The reserved set includes all closed-model CSS property names, VL structure attributes such as `containerType`, the `style` coordinate attribute, and all fixed `sk.*` skin override props.
- Collision matching includes exact spelling, kebab-case CSS spelling, camelCase spelling, snake_case spelling, and `sk.*` aliases. For example, `background-color` reserves `backgroundColor`, `background-color`, and `background_color`; `border-radius` reserves `borderRadius`, `border-radius`, and `border_radius`; `sk.bg` reserves `skBg`, `sk.bg`, and `sk_bg`.
- If a component needs a semantic business input related to visual behavior, it must use a domain-specific non-reserved name such as `chartPalette`, `surfaceTone`, `accentColor`, `panelVariant`, or `emptyStateTone`.

Component-level fields:

- `summary`
- `keywords`
- `useCases`
- `notFor`
- `dependsOn`

Property-level fields:

- `description`
- `enum`
- `enumLabels`
- `control`
- `nullable`
- `required`
- `example`

Event-level fields:

- `description`
- `params`

Method-level fields:

- `description`
- `params`
- `returns`
- `effects`

Service-level fields:

- `description`
- `params`
- `returns`
- `effects`
- `auth`

Completeness rules:

- Every public prop must have `@meta prop` with non-empty `description`.
- Every public event must have `@meta event` with non-empty `description`; when the event declares parameters, `params` must cover every parameter.
- Every public method must have `@meta method` with non-empty `description`; when the method declares parameters, `params` must cover every parameter; when the method returns a non-empty public result, `returns` must describe that result.
- Every `SERVICE` and `PUBLIC_SERVICE` must have `@meta service` with non-empty `description`; when the service declares parameters, `params` must cover every parameter; when the service returns a public result, `returns` must describe that result.
- Every parameter-level meta object must include non-empty `description`.
- Public interface meta should describe semantic contract, input shape, null semantics, requiredness, enum meaning, return meaning, and observable side effects. It must not rely on field names being self-explanatory.

Notes:

- `params` is a parameter-name keyed object. Parameter-level fields follow the same metadata semantics as properties where applicable, including `description`, `enum`, `enumLabels`, `control`, `nullable`, `required`, and `example`.
- `returns` is a return-value metadata object. For object returns, it is keyed by declared return field name. For scalar returns, it uses top-level `description`. Empty object returns may omit `returns`.
- `effects` is an optional non-empty string array that declares observable side effects such as data writes, external requests, cookie or session changes, file generation, message sending, or canvas reads.
- `auth` is an optional object that declares service-level authentication or permission semantics for tools and AI. It does not replace platform permission configuration.
- `dependsOn` is a string array of required component `importName` values.
- `nullable` is optional boolean metadata. Use `nullable:true` when `null` is a valid semantic input value for the property or parameter, rather than only an implementation accident.
- `required` is required boolean metadata for public props, public method params, public event params, and public service params. Use `required:true` when the public contract requires the caller to provide the value, and `required:false` otherwise. `required` only expresses caller obligation; null acceptance continues to be expressed by `nullable:true` / `nullable:false`. Other spellings such as `optional`, `mandatory`, `isRequired`, or string values like `"true"` are not accepted as standard requiredness metadata.
- `example` is optional sample-value metadata for public props, public method params, public event params, and public service params. It is consumed by component catalogs, IDE panels, AI component understanding, and preview shells assembling sample instances. `example` is not a runtime default and does not change requiredness. The value must be parseable as a metadata literal and must match the declared interface type when the type can be statically checked. When multiple interface examples form a composite preview sample (for example a dropdown's `options` example contains `{label:"男",value:"male"}`), authors should keep `value`, `SetValue.value`, and `Change.value` examples consistent (e.g. all using `"male"`).
- `control` is optional. Preview shells, online browsers, and IDE panels should first infer a default editor from declared type, runtime shape, and metadata structure, and only treat `control` as an explicit override.
- Recommended default editor mapping is:
  - `BOOL` -> switch
  - scalar `STRING` -> single-line text input
  - `enum` with 1 to 3 options -> radio
  - `enum` with more than 3 options -> select
  - scalar-number types such as `INT` / `FLOAT` / `NUMBER` -> number input
  - scalar arrays -> one-column table
  - object arrays -> multi-column table
  - two-dimensional scalar arrays -> two-dimensional table
  - generic `JSON` / `OBJECT` and mixed-shape arrays -> JSON editor
- Use `control` only when the desired editor differs from the inferred default and the system truly needs an explicit override. Current standardized override values are only `control:"textarea"`, `control:"json"`, and `control:"color"`.
- `control:"color"` means the editor should use a color picker for a string value. It does not change the runtime type; the accepted color syntax must be explained in the public interface `description`.
- Authors should omit redundant `control` declarations when the default inferred editor already matches the intended experience.
- When `nullable:true` is declared, authors should explain the semantic meaning of `null` in `description`, such as "null means no selection yet" or "null means inherit outer configuration".
- `nullable` and `required` are independent metadata. Absence of `nullable:true` does not imply required; preview shells and IDE panels should only show a required marker when `required:true` is explicitly declared.
- If a system still maintains `module.json` or another registry file, that file may be generated from extracted source metadata. It is not a higher-priority authoring truth source than VL source itself.
- Published `interfaceJson`, published `metadataJson`, and any project-level meta map are caches or projections of source meta. They must not become higher-priority authoring truth than the source files.

Single-line examples:

- `// @meta component summary:"柱状图组件" keywords:(["柱状图","bar chart"]) useCases:(["业务图表"]) notFor:(["地图"])`
- `// @meta prop name:"legendPosition" description:"图例位置" required:false enum:(["left","right","top","bottom"]) enumLabels:({left:"左侧",right:"右侧",top:"顶部",bottom:"底部"}) example:"top"`
- `// @meta prop name:"showLegend" description:"是否显示图例" required:false example:true`
- `// @meta prop name:"value" description:"当前选中值；null 表示尚未选择任何项。" required:false nullable:true example:"male"`
- `// @meta prop name:"options" description:"候选项数组。" required:true example:([{label:"男",value:"male"}])`
- `// @meta prop name:"direction" description:"柱状图方向" required:false enum:(["vertical","horizontal"]) enumLabels:({vertical:"纵向",horizontal:"横向"}) example:"vertical"`
- `// @meta prop name:"emptyText" description:"空态文案" required:false control:"textarea" example:"暂无数据"`
- `// @meta prop name:"accentColor" description:"主题色，使用 hex / rgb / 颜色名。" required:false control:"color" example:"#2563EB"`
- `// Invalid public prop: $backgroundColor(STRING) = ""`
- `// Valid public prop: $accentColor(STRING) = "#2563EB"`
- `// @meta event name:"pointClick" description:"点击数据点时触发" params:({name:{description:"点位名称",required:true,example:"Q1"},value:{description:"点位值",required:true,example:120}})`
- `// @meta method name:"ExportImage" description:"导出图表图片" params:({cb:{description:"结果回调",required:true,control:"json"}}) returns:({description:"通过回调返回 base64 字符串"}) effects:(["read:canvas"])`
- `// @meta service name:"QueryOrders" description:"查询订单列表" params:({keyword:{description:"搜索关键词",required:false,nullable:true,example:"库存"}}) returns:({success:{description:"是否成功"},rows:{description:"订单行数组"}}) effects:(["read:Order"]) auth:({required:true})`

#### Component External Size Consumption Contract

Component instances participate in an external size contract with their parent layout. This contract is what makes `width`, `height`, `flex`, `margin`, `min-*`, `max-*` carry stable cross-component meaning, and it is the only stable external layout contract between sibling and parent.

**External attributes that constitute the contract on `Section-*` / `Component-*` / `WebComponent-*` instance usage sites:** `width`, `height`, `flex`, `margin`, `margin-*`, `min-width`, `max-width`, `min-height`, `max-height`.

`width` and `height` describe the instance's footprint or size constraint inside the parent layout. A module component implementation MUST forward this external size contract to its observable outer body. Charts, canvases, dropdown triggers, selectors, etc. must not allow a state where external `width` / `height` is set on the instance but the visible component body fails to follow.

A component must not let only the outer footprint enlarge while the visible content keeps its natural size. When external `width` / `height` takes effect, the user-visible body, interactive area, canvas, or main visual surface MUST resize together with the outer frame. "Empty container grows but content stays curled in the corner" is non-compliant.

**Source-side contract via `@contract component sizeMode`:**

The `.sc/.cp` source MAY declare its size-consumption shape via `// @contract component sizeMode`. This is a source-internal engineering contract used by parser, lint, authoring checks, and pre-upload preflight; it MUST NOT be extracted into `metadataJson`, MUST NOT be displayed in component catalog search results, downloaded usage hints, or IDE public-interface panels. The core parser/lint does not emit an error merely because a normal `.sc/.cp` source omits `sizeMode`; product-specific upload or group preflight tools MAY require it as an authoring policy.

`sizeMode` fields:

- `mode` — required: `"full-frame"` or `"content-height"`. Full-frame components (charts, canvases, maps, video) consume both axes; content-height components (badges, empty states, dropdown triggers, plain text cards) do not require `height:"100%"` on the frame carrier.
- `frameCarrier` — required: a non-empty string id of a direct visual child under the real root that holds the default-state main body.
- `responsiveAxes` — optional: a non-empty array containing only `"width"` and/or `"height"`. For `mode:"full-frame"` the effective responsive axes are always `["width","height"]`. For `mode:"content-height"`, omitted `responsiveAxes` defaults to `["width"]`; declare `["width","height"]` explicitly to opt into responding to external height.
- `expandedSurface` — optional: object describing a two-state component's expanded auxiliary surface. Required sub-fields are non-empty `anchorCarrier` (a layout container that hosts layer order and overflow) and `surface` (the expanded surface node id). Optional `trigger` is the interactive control id (e.g. a Button). The `anchorCarrier` MUST point to a normal layout container (`Row`, `Col`, `Grid`, `Block`); it must not point to atomic components like `Button`, `Text`, or `Input`. If the trigger is a `Button`, place that Button inside an outer layout container and let `expandedSurface.anchorCarrier` point to the outer container.

When `sizeMode` is declared, `mode` and `frameCarrier` are mandatory. When `mode:"full-frame"`, the referenced `frameCarrier` MUST consume `width:"100%"` and `height:"100%"` (or an equivalent compiler-resolvable full-frame strategy), and MUST NOT carry fixed pixel `width` / `height` values on the responsive axes.

**Two-state component frame vs. expanded surface:**

Dropdowns, menus, popovers, tooltips, floating panels and similar two-state components must distinguish the default-state frame from the expanded auxiliary surface:

- External `width` / `height` constrain the default-state visible body — the dropdown trigger, the collapsed selector, the button body, the input body
- The expanded surface should follow the default-state outer width or anchor width, but its height should grow by content, max-height, or viewport-avoidance strategy. The expanded surface MUST NOT be clipped by the default-state `height`.
- The expanded surface is not part of the default-state frame fill contract; never apply `height:"100%"` to a menu, popup, or floater so it gets squished to default-state height.
- The ancestor chain of the expanded surface must allow overflow visibility. At minimum, declare `overflow:"visible"` on the source-level `expandedSurface.anchorCarrier` and on the `expandedSurface.surface` itself; the expanded surface should follow the default-state width and decide its own height independently.

Single-line examples:

- `// @contract component sizeMode:({mode:"full-frame",frameCarrier:"rootFrame"})`
- `// @contract component sizeMode:({mode:"content-height",frameCarrier:"rootFrame",responsiveAxes:["width"]})`
- `// @contract component sizeMode:({mode:"content-height",frameCarrier:"triggerFrame",responsiveAxes:["width"],expandedSurface:{anchorCarrier:"triggerFrame",trigger:"trigger",surface:"menu"}})`
- Full-frame implementation: `-<Col-RootFrame "rootFrame"> width:"100%" height:"100%"`
- Content-height implementation: `-<Row-RootFrame "rootFrame"> width:"100%" min-height:"36px"`
- Default-state trigger: `-<Button-Trigger "trigger"> width:"100%" height:"100%"`
- Expanded surface (legal): `-<Col-Menu "menu"> width:"100%" min-height:"116px" overflow:"visible"`
- Expanded surface (illegal — over-constrained by default-state height): `-<Col-Menu "menu"> width:"100%" height:"100%"`

#### App, Page, and Modal Special Boundaries

`App`, `Page`, and `Modal` are special boundary nodes. They are excluded from the per-node-class applicability matrix in `Closed CSS Property Model` and from the module component instance scope of the external size consumption contract above. Their property surfaces are governed by their own dedicated chapters.

**`App` boundary:** `.vx` files' `App` is the application-root boundary, responsible for global routing, theme, application-level state, and page mounting. `App` root is not a module component instance, and its skeleton CSS / `sk.*` surface is governed by the App chapter. The per-node-class applicability matrix MUST NOT be inferred onto `App`.

**`Page` boundary:** `.vx` files' page root is the page boundary, responsible for hosting the current route's page content. `Page` root is not a module component instance and is not a normal layout container in the matrix. ALL `Page-*` nodes — root or otherwise — go through the Page chapter; `padding`, `overflow`, `gap`, `sk.*`, etc. on `Page-*` are not automatically derived from the layout-container row of the matrix.

**`Modal` boundary:** `Modal` is a special built-in component. It is neither a normal layout container nor a module component instance. Modal's outer body is a platform overlay; the author-declared `width` / `height` map to Modal's inner panel, not to the full-screen overlay. Modal's mask, layering, scroll lock, close behavior, centering strategy, and panel sizing are governed by the Modal chapter. The matrix MUST NOT treat Modal as `Row` / `Col` / `Grid` / `Block` / `Page`.

Single-line examples:

- `<App-Main "app"> theme:"default"`
- `<Page-Home "home"> path:"/"`
- `<Modal-Detail "detailModal"> width:"640px" height:"480px"` — Modal `width` / `height` are explicitly allowed, mapping to the inner panel.

## Naming Conventions

#### File Naming:

All file names are uniformly PascalCase. `.vx/.sc/.cp/.vs` file names must match the root component. `.wc` file names must match the `WebComponent-*` import name used by VL source files.

`UserAuth.vs` file, root component is:

```
<ServiceDomain-UserAuth>
File body
</ServiceDomain-UserAuth>
```

For a `.wc` file, `RichChart.wc` is referenced as `<WebComponent-RichChart "chart"> option:$chartOption`.

#### **Component Instance Naming**:

- Component names use **PascalCase** (e.g., `UserCard`, `LoginButton`, `NoteItemCard`, `AddButton`).
- Component names should reflect component purpose.
- Component IDs must be quoted static locator strings, must use camelCase, must be unique within the current VL file, and must not contain whitespace, dots, colons, quotes, hyphens, or expression syntax. Examples include `<Button-Submit "submitButton">`, `<Col-ModuleRoot "root">`, `<VirtualTable-Users "userTable">`.
- JavaScript reserved words are forbidden only when they appear as VL identifiers that are translated into JavaScript identifiers. They are not forbidden inside component class/name segments, quoted component ids, string literals, or comments. For example, `<Button-ExportBtn "exportBtn"> value:"Export"` is valid.

#### **Variable Naming**

- **Frontend global variables** (`# Frontend Global Vars`): `$` prefix + **camelCase** (e.g., `$currentPage`, `$userData`).
- **Derived variables** (`# Frontend Derived Vars`): `$` prefix + **camelCase** (e.g., `$fullName`, `$canSubmit`).
- **Local variables created in methods** (`_varName(TYPE) = ...` or `_varName({}) = {}` or `_varName = []`): uniformly use `_` prefix + **camelCase** (e.g., `_result`, `_tempData`). **Must** declare type or infer through empty structure, **no** `let` keyword.
- **Loop variables**:
  - Loop variables are a "**special local variable**" that can be used directly without definition.
  - `FOR...IN` loops: Loop item variables and index variables **must** use `_` prefix + **camelCase**. Index variables should use `_indexX` (X starts from 0, e.g., `_index0`, `_index1`). Loop item variables can be named based on context (e.g., `_user`, `_product`, `_noteItem`) or use `_itemX` for generic iteration (e.g., `_item0`, `_item1`, `_item2`). Example: `FOR (_user, _index0) IN $userList` or `FOR (_item0, _index0) IN dataArray`.
  - `FOR (...)` counting loops: Loop control variables (like `i`, `j`, `k` or more meaningful names like `counter`) **must** use `_` prefix + **camelCase** (e.g., `_i`, `_j`, `_k`, `_loopCounter`). Declare in `FOR` parentheses using `_varName(TYPE) = initialValue` (e.g., `FOR (_i(INT) = 0; _i < 10; _i++)`).

#### **Method/Function Names**:

- **Frontend public methods** (`METHOD` under `# Frontend Public Methods`): **PascalCase** (e.g., `ProcessData`, `ValidateForm`, `ReloadList`)
- **Frontend/backend internal methods** (`METHOD`): **camelCase** (e.g., `loadData`, `loadNotesFromServer`, `validateBackendInput`)
- **Backend services** (`SERVICE`): **PascalCase** (e.g., `UserLogin`, `GetDocList`)
- **Frontend/backend pipeline (PIPE) functions**: (for pure data transformation): `_` prefix + **camelCase** (e.g., `_formatCurrency`, `_formatDate`)

#### Event Names:

**Events** (`<Component>.@click`): Event names all use camelCase (e.g., `@click`, `@init`, `@tick`, `@keyDown`).

#### **Method/Function Parameters**:

- All types of method/function **parameter names use camelCase, without `_` (underscore) prefix**.
- Example: `METHOD myMethod(userId(INT), configData({}))`, parameters are `userId`, `configData`.
- Example: `PIPE _formatDate(dateValue(TIMESTAMP), formatString(STRING))`.
- **Note**: When referencing these parameters inside methods, use their defined names directly (e.g., `userId`, `configData`).

#### Tables and Fields

- All table names use PascalCase, including Database Tables and ServiceDomain VirtualTables
- All field names use camelCase

#### Other

- **Public event definitions** (`EVENT @eventName`): **camelCase** (e.g., `itemSelected`, `formSubmitted`).

## Components

### Component Instance Creation

#### Creation Location

| Applicable Files | Definition Section | Core Purpose                                            |
| ---------------- | ------------------ | ------------------------------------------------------- |
| vx, sc, cp       | # Frontend Tree    | Frontend UI layout and functional component declaration |
| vs               | # Backend Tree     | Backend component declaration                           |

`# Frontend Tree` and `# Backend Tree` are declaration-only structure sections. They may contain component instance declarations and legal tree nesting, but they must not contain event-handler listener definitions, executable action calls, variable assignments, local variable declarations, `IF` / `FOR` / `WHILE`, `GUARD`, `RETURN`, or rollback statements.

Component event-handler listener lines such as `<Button-Submit "submitButton">.@click()` belong only in `# Frontend Event Handlers` / `# Backend Event Handlers`. Their handler body must be indented under the listener line in that event-handler section. Public interface event declarations such as `EVENT @selected(value(STRING))` belong only in `# Frontend Public Events`; they are declarations, not children of `# Frontend Tree`.

#### Component Definition Format

```vl
<ComponentClass-ComponentName "componentId"> functionalProp1:value1 functionalProp2:value2 layoutCSS1:value1...
```

**Special Notes:**

- **<> must appear in pairs; absolutely cannot have only opening "<" without closing ">".**
- **Inside <> only "ComponentClass-ComponentName" and "componentId"; everything else must be outside the angle brackets.**
- The closing `>` ends the component reference. Component functional props, `style`, `sk.*`, skeleton CSS props, event bindings, and other attributes must all be written after `>`, never before it.
- Valid: `<Button-Submit "submitBtn"> style:"primary|filled|pill|actionable" value:"提交"`. Invalid: `<Button-Submit style:"primary|filled|pill|actionable"> value:"提交"`.

##### Component Elements

| Element                      | Description                                               | Naming Rule | Example                           | Notes                                                                                                |
| ---------------------------- | --------------------------------------------------------- | ----------- | --------------------------------- | ---------------------------------------------------------------------------------------------------- |
| ComponentClass               | System-predefined component type                          | Fixed       | `Button`, `Text`, `Input`   | Cannot be modified, defined by system                                                                |
| ComponentName                | Developer-defined descriptive name reflecting purpose     | PascalCase  | `SubmitButton`, `UserStatus`  | Must be meaningful for understanding                                                                 |
| ComponentId                  | Unique identifier for component within file               | camelCase   | `"submitBtn"`, `"userStatus"` | Optional, only specify when reference needed; pure static quoted string only; hyphenated ids such as `"nav-item-overview"` are invalid |
| Functional Properties        | Properties defined in component documentation             | -           | `value`, `disabled`           | Strictly reference component docs, forbidden to invent properties                                    |
| Layout CSS (frontend only)   | Skeleton CSS properties from the closed VL whitelist (skin managed by Theme + `sk.*`) | -           | `width`, `padding`, `margin`, `font-size`  | Only explicit whitelist skeleton properties allowed; skin CSS names forbidden — use `sk.*` for component-local skin override or Theme style coordinate for reusable static skin |

**Key Distinctions:**

- **ComponentName** vs **ComponentId**:
  - ComponentName is the descriptive identifier for the component type, used for type matching in definition and reference
  - ComponentId is the unique identifier for the component instance, only used to distinguish different instances within the same file. It must be globally unique in that file's component tree / backend tree.
  - When referencing, must use `<ComponentClass-ComponentName "componentId">`, cannot replace ComponentName with ID

**Runtime ID Injection (Platform Behavior):**

When a component instance declares an ID (e.g., `"submitButton"`), the platform injects `data-vl-id="submitButton"` onto the **root HTML element** of the rendered component at runtime. If one VL component maps to multiple HTML elements (a DOM subtree), the attribute is placed on the subtree root only. This enables automated testing tools (e.g., Playwright) to locate VL components in the rendered page via `[data-vl-id="submitButton"]`. Components without an explicit ID do not receive this attribute.

ComponentId is a VL locator string and an authoring identifier. It must follow the camelCase convention above, and generated code must not depend on it being a JavaScript variable name.

##### ✅ Correct Examples

```vl
<Button-Submit "submitButton"> value:"Submit" disabled:(!$canSubmit)
<Text-Welcome> value:"Hello"
<Image-Logo "logo"> sourceUri:$logoUrl alt:"Company Logo" height:"40px" width:"auto"
<Text-UserStatus> value:($isLoggedIn ? "Online" : "Offline")
```

##### Error 1: Component Definition Line Break

```vl
❌ <Section-DynamicFormFields>
     value:$formData[_field.name]
     error:$formErrors[_field.name]

✅ <Section-DynamicFormFields> value:$formData[_field.name] error:$formErrors[_field.name]
```

### Component Tree Structure

#### Root Component

The root component is a special component that serves as the starting marker for the entire file, added at the beginning of the file. Otherwise, its usage rules are the same as regular components:

- Frontend root components (`<App>`, `<Section>`, `<Component>`) are real root containers. All components in `# Frontend Tree` are added directly under this root.
- For `.sc/.cp`, the root container type is controlled by `containerType`; default is `col`, and `row` / `grid` are also allowed in `VL_VERSION:4.3.1`.
- For `.sc/.cp`, root properties define the internal root container only; outer-instance layout remains controlled by the host file.
- For `.sc/.cp`, root layout-property validation follows the effective `containerType`, not the raw `Section-*` / `Component-*` tag. `containerType:row` is checked as `Row`, `containerType:col` as `Col`, and `containerType:grid` as `Grid`.
- Frontend root components can add `@init()` and other common layout container events, e.g., `<App-MyApp "root">@init()`, `<Section-MySection "root">@init()`
- `<ServiceDomain>` root component currently has no properties or methods, serves only as file start declaration

For `.sc/.cp`, `VL_VERSION:4.3.1` uses the real root-container model described in this document, and `containerType` defaults to `col`.

#### Component Creation Order

In frontend component tree, component creation order is:

1. Functional components (no UI), such as FrontendApi, Trigger, WindowEventListener, ClientUserCenter, etc.
2. UI and container components

In backend component tree, component creation order is:

1. Functional components, such as ServerApi, TokenIssuer, UserStore, etc.
2. Backend data components, such as VirtualTable

#### Component Parent-Child Relationships

When any component B is a child of another component A, add B under A with one additional indentation level from A. In VL, parent-child relationships exist in the following scenarios:

**Cases Where Child Components Are Allowed**

- All container components (UI containers, logic containers) can have other frontend components (containers, basic UI components, extended UI components, etc.) as children
- Non-UI functional components (FrontendApi, Trigger, etc.) can only be direct children of root component
- Any component with UI (UI containers, UI components) can add widgets to extend its functionality (StateStyle, Animation, UseDrag, etc.)

**Cases Where Child Components Are Strictly Limited or Forbidden**

- Non-UI functional components: strictly forbidden to add any children
- Non-container UI components (basic/extended UI components): forbidden to add any children except widgets
- Module components (`Section-*` / `Component-*` / `WebComponent-*`): similar to extended UI components, forbidden to add any children except widgets

### Subsequent Component Usage and Access

#### Component Usage Scenarios

After components are defined in the component tree, they can be used in subsequent events/methods/functions/expressions. Strict rules as follows:

| Usage                                                                   | Events | Methods | PIPE Functions/Expressions |
| ----------------------------------------------------------------------- | ------ | ------- | -------------------------- |
| Listen to component events, e.g.,`.@init()`                           | ✅     | ❌      | ❌                         |
| Call component methods, e.g.,`.scrollToBottom()`                      | ✅     | ✅      | ❌                         |
| Access component read-only properties, e.g.,`_position = .offsetLeft` | ✅     | ✅      | ✅                         |

#### Correct Component Access Method

Reference using `<ComponentClass-ComponentName "componentId">`. Note: ComponentClass, ComponentName, and componentId must strictly match the definition in the component tree. **Do not confuse ComponentName with componentId.**

```
# Frontend Tree
<Button-Next "nextButton"> disabled:$status

# Frontend Event Handlers
<Button-Next "nextButton">.@click() // Component reference inside angle brackets strictly matches component tree
```

#### Incorrect Access Methods (Strictly Avoid)

```
❌ <Button-BackToProjects "backToProjects">.@click()  // Wrong: used ID as ComponentName
✅ <Button-BackButton "backToProjects">.@click()      // Correct: ComponentName must match definition

❌ _currentObj = <Col-ScrollItem "scroller">         // Wrong: cannot assign component to variable
✅ _distance = <Col-ScrollItem "scroller">.scrollTop // Correct: access read-only property directly

❌ <Col-Container "box">.style.color = "red"         // Wrong: no .style access exists
✅ <Col-Container "box"> width:$boxWidth             // Correct: bind a supported declarative prop, then modify the variable
   -$boxWidth = "240px"

❌ _userSubMenu.style.display = "block"                // Wrong: no display manipulation
✅ <Col-UserSubMenu "userSubMenu"> show:$show          // Correct: use show property
   -$show = true
```

#### Define Before Use Principle

Except for system method class components, all other components need to be defined in the component tree first before they can be referenced in subsequent methods/functions.

Please pay special attention to these functional non-UI components that also need to be defined first: `Trigger`, `FrontendApi`, `WindowEventListener`, `ClientUserCenter`

#### System Method Class Components

System method class components are special components with only one global instance, providing convenient access to system methods. Therefore, they don't need to be defined in the component tree and can be used directly, e.g.:

`<ClientUtils>.consoleLog(123)`

### Component Properties

#### Property Order

**Please strictly follow this four-segment order when specifying component properties:**

```
<ComponentType-Name "id"> ① Component prop → ② style coordinate → ③ sk.* component skin → ④ Skeleton prop
```

| Order | Category | Examples |
|---|---|---|
| ① | Component functional props | `value:"Send"` `placeholder:"Search..."` `sourceArray:$data` |
| ② | style coordinate | `style:"primary|filled|default"` |
| ③ | sk.* component skin props | `sk.bg:_item0.color` `sk.fg:(cond ? "red" : null)` |
| ④ | Skeleton CSS props | `width:"100px"` `padding:"10px"` `font-size:"14px"` |

Example:

`<Text-NavIcon "navIcon"> value:_item0.icon style:"caption" sk.fg:(_index0 === 0 ? "#2563EB" : null) font-size:"20px" width:"24px" text-align:"center"`
`//                       ① component prop   ② style coord  ③ component-local skin                         ④ skeleton`

> Note: When no `style` coordinate or `sk.*` props are needed, the order naturally reduces to ① + ④.

#### Read-Only and Non-Read-Only Properties

- **Non-read-only properties** define component behavior, data, logic, core characteristics, and non-visual state. They determine what the component "is" and "how it works".
- **Read-only properties** are mainly read-only properties of DOM elements, such as offsetWidth. These properties don't need to be defined in `# Frontend Tree` and can be accessed directly in methods/functions later.
- Whether read-only or non-read-only properties, please **strictly refer to component documentation**; inventing any properties is not allowed.

#### Property Read/Write Rules (Unidirectional Data Flow)

VL uses **Unidirectional Data Flow** architecture. **Editable input components require explicit event writeback; VL does not provide any built-in two-way binding or v-model semantics.**

##### Non-Read-Only Property Read/Write Standards (Attribute Mutability)

1. **Forbidden** to **directly read or modify component non-read-only properties** in any method/event/function/expression (like `Button.color` or `Modal.show = false`).
2. If dynamic property control is needed, follow the **"variable intermediary"** pattern:
   - First **bind** a variable to the property (like `show:$isModalVisible`).
   - In logic, **only modify that variable** (`$isModalVisible = false`), letting reactive binding drive component refresh.

##### Read-Only Property Read/Write Standards

Read-only properties can be read in methods/functions (not written), won't break unidirectional data flow:

```
# Frontend Event Handlers

<Button-CheckSize "checkButton">.@click()
// ✅ Allowed: Reading size information
-_buttonWidth(FLOAT) = <Button-Submit "submitButton">.offsetWidth
-_buttonHeight(FLOAT) = <Button-Submit "submitButton">.offsetHeight
-_rect({}) = <Col-MainContainer "mainContainer">.getBoundingClientRect()

// ✅ Allowed: Reading position information
-_scrollTop(FLOAT) = <Col-Content "docContent">.scrollTop
-_offsetLeft(FLOAT) = <Image-Logo "logo">.offsetLeft
```

## Variables

### Basic Types

- **STRING**: Text string, e.g., "Hello". String values are usually wrapped with double quotes `"`. When string value needs to contain double quotes inside, single quotes `'` can be used. **Strictly forbidden to use any `\` escape character in VL.**
- **INT**: Integer, e.g., 42
- **FLOAT**: Floating point number, e.g., 3.14
- **BOOL**: Boolean value, true or false
- **TIMESTAMP**: Internally stores standard time string format, e.g., 2024-12-12 11:11:11.342. When defining, strings without milliseconds can be passed, e.g., 2024-12-12 11:11:11. As "time currency", it can be used in all time-related scenarios. (See Timestamp Type detailed section)

### Compound Types

- **Array**: `[elementType]`, e.g., `[INT]`, `[STRING]`, `[{id:INT,name:STRING}]`. When internal structure is uncertain, use `[]`.
- **Object**: `{field:Type}`, e.g., `{id:INT,name:STRING,items:[{id:INT}]}`. When internal structure is uncertain, use `{}`.
- **Compound type initialization** must include all required fields, or use legal empty structures (like empty object `{}` or empty array `[]`);

### Timestamp Type

#### Timestamp Variable Assignment

VL's TIMESTAMP type uses a unified string format as "time currency", like `2024-12-12 11:11:11.342` or `2024-12-12 11:11:11`, milliseconds can be omitted. When assigning "=" to a timestamp global or local variable, the following input types are accepted:

- **System variable**: e.g., `$serverDate = SYSENV.currentTime`.
- **Unix timestamp**: Millisecond precision; for second precision, use `setUnixS()` method
- **String** (five formats, all case-sensitive):

  - `"YYYY-MM-DD HH:mm:ss"`
  - `"YYYY-MM-DD HH:mm:ss.SSS"`
  - `"YYYY-MM-DD"`
  - `"YYYY-MM-DDTHH:mm:ssZ"`
  - `"YYYY-MM-DDTHH:mm:ss.SSSZ"`
- **Recommended initialization format**:

  - Preferably use complete format when defining: `$createTime(TIMESTAMP) = "2024-12-12 11:11:11.342"`
  - System variable `SYSENV.currentTime` returns standard time string format (with milliseconds)

#### Timestamp Variable Usage

TIMESTAMP variables can be used directly in most scenarios without explicit conversion

- All time-related variable assignments: `$eventTime(TIMESTAMP) = "2024-12-12 11:11:11"`
- Parameter passing: `processEvent(eventTime)` where eventTime is TIMESTAMP type
- Database time comparison: `[["createTime", "gt", $startTime]]`

**Example**:

```vl
$eventTime(TIMESTAMP) = "2024-12-12 15:30:00"
$displayTime(STRING) = $eventTime.format("YYYY-MM-DD HH:mm")
<Text-EventTime> value:($eventTime.format("MM/DD HH:mm"))
```

#### Timestamp Variable Methods and Functions

**Immutable methods:**

- **Format output**: Use `.format(outputFormat)` method for formatted display. Output format includes: YYYY (4-digit year, e.g., 2024), YY (2-digit year, e.g., 24), M (1-digit month, 1-12), MM (2-digit month, 01-12), MMM (short month name, e.g., Jan, Feb), MMMM (full month name, e.g., January), D (1-digit day, 1-31), DD (2-digit day, 01-31), d (day of week, 0-6), H (24-hour 1-digit hour, 0-23), HH (24-hour 2-digit hour, 00-23), h (12-hour 1-digit hour, 1-12), hh (12-hour 2-digit hour, 01-12), m (1-digit minute, 0-59), mm (2-digit minute, 00-59), s (1-digit second, 0-59), ss (2-digit second, 00-59), SSS (3-digit millisecond, 000-999), A (12-hour AM/PM uppercase), a (12-hour am/pm lowercase)
- **Timestamp retrieval**: Use `.unixS()` and `.unixMS()` methods

**Mutable functions:**

- `variable.setUnixS(secondTimestamp)`
- `variable.setUnixMS(millisecondTimestamp)`
- `variable.setCustomFormat("timeString", "correspondingFormatString")`

**Forbidden operations**:

- **Forbidden to directly call JavaScript Date API**: Direct calls to `new Date(...)`, `.getTime()`, `.getFullYear()`, `.getMonth()`, `.getDate()` and other native JS Date methods are not supported

### Variable Definition

#### Frontend Global Variable Definition (`# Frontend Global Vars`)

Frontend global variables are accessible at file level in the frontend environment, using `$` prefix:

```
$variableName(Type) = initialValue
```

Examples:

```vl
$userName(STRING) = ""
$userCount(INT) = 0
$isLoggedIn(BOOL) = false
$userData({id:INT,name:STRING,age:INT}) = {id:0,name:"",age:0}
$items([{id:INT,title:STRING}]) = []
$genericArray([{}]) = [{}]
```

**Important Rules:**

- All variables must have initial values; initial values can be empty (`""/[]/{}`) but cannot be `null`

#### Derived Variable Definition (`# Frontend Derived Vars`)

Derived variables are read-only variables automatically calculated from other variables:

```vl
# Frontend Global Vars
$firstName(STRING) = "John"
$lastName(STRING) = "Doe"
$isLoading(BOOL) = false
$cartItems([{name:STRING,price:FLOAT,quantity:INT}]) = []

# Frontend Derived Vars
$itemCount(INT) = $cartItems.reduce((acc, item) => acc + item.quantity, 0)
$canSubmit(BOOL) = ($firstName != "" && !$isLoading)
$userSummary(STRING) = ("User: " + ($firstName + " " + $lastName) + " (Status: " + ($isLoading ? "Loading" : "Ready") + ")")
$loadingStatus(STRING) = $isLoading._mapLoading()
```

**Important Rules:**

- Can only contain pure calculations; cannot call METHOD/SERVICE, but can use PIPE functions and variable immutable functions
- Derived variables are read-only, cannot be assigned in code
- Important: **Derived variables must define type**

#### Local Variable Definition

##### Method/Function Internal Local Variables

Local variables are used inside methods, using `_` prefix. They **must** explicitly declare type (e.g., `_varName(TYPE) = ...`) or infer type through empty structure initialization (e.g., `_varName({}) = {}`, `_varName([]) = []`). **No** `let` keyword:

```
_variableName(Type) = value
// or
_variableName({}) = {}
_variableName([]) = []
```

Example:

```vl
METHOD processData()
-_result({type:STRING}) = {}
-_count(INT) = 0
-_userList([{id:INT,name:STRING}]) = []
-_lookupTable({}) = {}
-_genericItems([{}]) = [{}]
```

- Local variables, like global variables, need type declaration or type inference through empty structure at initialization. (Loop control variables are a special case of this rule, see Loop section)
- When declaring local variables, **do not forget the equals sign and initial value**, otherwise local variable definition will be mistaken for public method call:

```
METHOD processData()
-_condition([]) = [] // Define a local variable named _condition
-_condition([]) // Call a public method named _condition
```

##### Local Variable Block Scope

Local variables use block scope. A local variable is visible from its definition statement to the end of the block where it is defined.

- The top-level body of each `METHOD`, `SERVICE`, `PUBLIC_SERVICE`, `PIPE`, and `TRANSACTION` is a scope block.
- Each `IF`, `ELSE IF`, and `ELSE` branch body is a child scope block.
- Each `FOR` and `WHILE` loop body is a child scope block.
- A local variable first defined inside a branch or loop body is not visible after that child block ends.
- If one result must be assigned inside multiple branches and then used after the branch, declare the local variable in the shared outer block first, then assign to it inside each branch.
- Indentation is the scope boundary. Reducing the leading `-` count closes all deeper blocks until the new indentation level is reached. A line with the same indentation as `FOR` or `IF` is a sibling of that block, not a child of it.
- Loop variables such as `_item0` and `_index0` are visible only inside the loop body: lines one level deeper than the `FOR` statement and nested child blocks under that loop body. If an `IF` is logically inside a `FOR`, the `IF` line itself must be one level deeper than the `FOR`, and the `IF` body must be one more level deeper.

Correct sequence: `SERVICE GetBoard(keyword(STRING));RETURN {success:BOOL,rows:[{}]}`; `-_listResult({}) = {}`; `-IF keyword == ""`; `--<VirtualTable-Requirements "requirementTable">.select(null,[["_create","desc"]],[0,500],null) -> _listResult`; `-ELSE`; `--<VirtualTable-Requirements "requirementTable">.select([["searchVec","l2str",keyword,0.85]],[["searchVec",keyword]],[0,500],null) -> _listResult`; `-_rows([{}]) = (_listResult.dataArray ? _listResult.dataArray : [])`.

Incorrect sequence: `SERVICE GetBoard(keyword(STRING));RETURN {success:BOOL,rows:[{}]}`; `-IF keyword == ""`; `--<VirtualTable-Requirements "requirementTable">.select(null,[["_create","desc"]],[0,500],null) -> _listResult`; `-ELSE`; `--<VirtualTable-Requirements "requirementTable">.select([["searchVec","l2str",keyword,0.85]],[["searchVec",keyword]],[0,500],null) -> _listResult`; `-_rows([{}]) = (_listResult.dataArray ? _listResult.dataArray : [])`.

Incorrect loop nesting: `-FOR (_item0,_index0) IN rows`; `-IF mode == "day"`; `--_label = _item0.createdAt.format("YYYY-MM-DD")`. Here the `IF` is a sibling of `FOR`, so `_item0` is out of scope.

Correct loop nesting: `-FOR (_item0,_index0) IN rows`; `--IF mode == "day"`; `---_label = _item0.createdAt.format("YYYY-MM-DD")`; `--_bucket[_label] = _item0.count`. Here both the `IF` and the later bucket update remain inside the `FOR` loop body.

##### Loop Container Local Variables

In For, Tree and other loop container component properties, local variables for the current loop container can be declared. For example:

```
<For-UserList> sourceArray:$users,loopVar:[_item0,_index0] // _item0, _index0 are the loop variable and loop index for current structural loop, local variables declared in component tree, please use _ prefix
```

#### System Variables (`SYSENV`, `WINDOW`)

System variables can be used directly to get current system environment information.

| Variable Name      | Applicable Scope                       | Internal Properties                                                                                                                                                                                                                                                                                                                                                   |
| :----------------- | :------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SYSENV.currentUser | Frontend and Backend                   | **isLogin**: BOOL (whether user is logged in) **userId**: STRING (user ID) **userInfo**: {id:INT, username:STRING, avatar:STRING, deleted:INT, deptId:INT, disableStatus:INT, email:STRING, lastTimeLogin:TIMESTAMP, fullName:STRING, nickname:STRING, phoneNumber:STRING} (current user info object) **tenantId**: INT (tenant ID, may be 0) |
| SYSENV.currentTime | Frontend and Backend                   | None. Can be directly assigned to a time variable, e.g.,`$currentTime(TIMESTAMP) = SYSENV.currentTime`                                                                                                                                                                                                                                                              |
| SYSENV.requestInfo | Backend Only                           | **ip**: STRING (request client IP) **userAgent**: STRING (request client UA string) **headerInfo**: {} (current request header info) **cookie**: {} (current request cookie)                                                                                                                                                                  |
| WINDOW             | Frontend Only (expressions and events) | Provides read-only access to browser `window` object properties. Used with components to respond to window events (like `resize`, `scroll`). **Forbidden to call** `WINDOW` object methods. Accessible properties include: `innerWidth`, `innerHeight`, `outerWidth`, `outerHeight`, `devicePixelRatio`, `scrollX`, `scrollY`, etc.       |

### Variable Assignment and Modification

#### Variable Assignment

Variable assignment uses equals sign `=`, no need to re-declare type:

```
$variableName = newValue
_variableName = newValue
```

- `=` operation follows JS rules: when new value is a simple value (STRING, INT, FLOAT, BOOL), assigns the value itself; when new value is a complex data structure (array, object, JSON), assigns a reference to that data structure.

#### Complex Data Structure "In-Place Modification" Support

VL **does not require** **data immutability** for complex data structures (objects, arrays, JSON) as in React/Vue frameworks; the framework automatically handles data updates and performance optimization after in-place modification. VL **supports and recommends directly modifying parts of complex structures**:

```
# Frontend Global Vars
$users([{name:STRING}]) = []

# Frontend Tree
-<For-UserList> sourceArray:$users loopVar:[_item0,index0]
--<Text-Name> value:_item0.name

# Frontend Event Handlers
<Button-Change "changeButton">.@click()
$users[0].name = "adam" // Directly modify a value in the array; bound UI will automatically change; no need to follow immutability, create a new array and assign back
$users.push("alice") // Modify array in-place via mutable method; bound UI will also automatically update
```

#### Variable Mutable Methods

In methods and events, variable **mutable** methods (that modify the original variable) can be called. These operation statements follow the indentation rules of their code block (usually at least one level `-`).

**Important: Mutable methods can only be called directly in methods; any expression (including property bindings, `conditions`, `IF/FOR` conditions/loop bodies, etc.) can only use immutable functions.**

| **Variable Type** | **Mutable Methods Allowed in Methods**                                             |
| :---------------------- | :--------------------------------------------------------------------------------------- |
| Array                   | `push`, `pop`, `shift`, `unshift`, `splice`, `sort`, `reverse`, `delete` |
| Object                  | `Object.assign`, `delete`                                                            |
| Time                    | `setUnixS`, `setUnixMS`, `setCustomFormat`                                         |

> **⚠️ Usage Restriction**: Mutable operations can only appear as independent statements in method bodies; they cannot be used in expressions, conditional judgments, or chained calls.
> Incorrect example: `_result = items.push(newItem)`
> Correct example:
>
> ```vl
> items.push(newItem)
> _result = items
> ```

**Example (in method):**

```vl
METHOD manageList(items([STRING]), newItem(STRING)); RETURN {updatedList:[STRING], success:BOOL}
-_listCopy([STRING]) = items.slice()
-_listCopy.push(newItem)
-_listCopy.splice(0, 1)
-_listCopy.sort()
-RETURN {updatedList:_listCopy, success:true}

METHOD updateTimestamp(timeVar(TIMESTAMP)); RETURN {newTime:TIMESTAMP, success:BOOL}
-_localTime(TIMESTAMP) = timeVar
-_localTime.setUnixS(1700000000)
-RETURN {newTime:_localTime, success:true}
```

#### Variable Built-in Immutable Functions/Properties

Variable built-in immutable functions can be called in expressions. Types include:

| Category         | Immutable Functions                                                                                                                                                                |
| :--------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Array            | `map()`, `filter()`, `reduce()`, `indexOf()`, `includes()`, `slice()`, `concat()`, `join()`, `length` (property)                                                 |
| String           | `split()`, `trim()`, `toLowerCase()`, `toUpperCase()`, `replace()`, `replaceAll()`, `substring()`, `slice()`, `includes()`, `indexOf()`, `length` (property) |
| Number           | `toFixed()`, `toPrecision()`, `toExponential()`                                                                                                                              |
| Object           | `Object.keys()`, `Object.values()`, `Object.entries()`                                                                                                                       |
| Time             | `unixS()`, `unixMS()`, `format()`                                                                                                                                            |
| Type Check       | `typeof`                                                                                                                                                                         |
| Math             | `Math.random()`, `Math.floor()`, `Math.ceil()`, `Math.round()`, `Math.max()`, `Math.min()`, `Math.abs()`, `Math.pow()`                                             |
| JSON Processing  | `JSON.parse()`, `JSON.stringify()`                                                                                                                                             |
| Regex Processing | `match()`, `test()`                                                                                                                                                            |
| Type Conversion  | `String()`, `Number()`, `parseInt()`, `parseFloat()`, `Boolean()`, `isNaN()`, `isFinite()`                                                                           |

- Variable built-in functions can be called directly without pre-definition.
- When calling, use function name directly, **do not add `_` prefix:**
  - Correct (direct use): `_num.toFixed()`
  - Incorrect (confused with PIPE function, adding `_`): `_num._toFixed()`

#### Forbidden JS Methods:

- `toString()`: Please use `String()` instead, which has broader applicability

#### "Callback Function Parameter" Usage Rules in Variable Methods

##### Hard Rules (Must / Must Not)

- **MUST**: In map / filter / reduce / some / every / sort callback function parameters, **must use arrow functions, and function body must be a single expression (concise body).**
- **MUST**: **Callbacks must be pure synchronous, no side effects** (must not modify external/parent variables, must not call async METHOD/component methods)
- **MUST NOT**: Use statements with side effects in callbacks (like acc[k]=..., push/splice and other write operations)
- **MUST NOT**: Write return statement blocks.

✅ **Correct (pure expression)**:

```vl
arr.reduce((acc, x) => acc + x, 0)
arr.map(x => ({ id: x.id, name: x.name?.trim() }))
params.reduce((acc, p) => ({ ...acc, [p.name]: p.def ?? "" }), {})
```

❌ **Incorrect (statement block + side effects)**:

```vl
arr.reduce((acc, p) => { acc[p.name] = p.def || ""; return acc; }, {})
JSON.parse(...)._reduce((acc, p) => { ... }, {})
```

## Database Table and Virtual Table Field Types

Backend Database tables and virtual tables support the following field types: STRING, INT, FLOAT, BOOL, TIMESTAMP, JSON, VEC

Note: Backend table field types differ from variable types in the following ways:

- Backend has no object and array type fields; for complex structures, use JSON type uniformly
- Backend has additional VEC vector type field for vector search; VL currently doesn't support VEC type variables

Except for these two differences, other field types have identical data definitions to variable data types.

### Vector Field Usage

Vector fields are used for vector search. In VL, you only need to define a vector field's vecSource (source fields for vectorization), and the system will automatically vectorize source fields when table data is inserted/updated.

#### Vector Field Definition

**1. Define vector field in virtual table**

```
<ServiceDomainRoot-Root>
-<VirtualTable-Documents "docTable">  mockData:[{...}]
--<Field-title> type:STRING
--<Field-content> type:STRING
--<Field-contentVec> type:VEC vecSource:["title","content"] // Vector field data source is title and content, data will automatically vectorize these two fields
```

- In most cases, a virtual table only needs one vector field. Add all fields that may participate in content search as vecSource of the vector field, rather than having one vector field per content field.

**2. Define vector field in Database entity table**

```
<Table-documents> data:[{"_id":1,"title":"VL Framework Guide","content":"VL is an innovative visual programming language supporting full-stack development...","_user":"1","_create":"2024-01-15 09:30:00","_update":"2024-01-15 09:30:00"}] // No contentVec field in test data, this field is auto-generated by system
-<Field-_id> type:INT
-<Field-title> type:STRING
-<Field-content> type:STRING
-<Field-contentVec> type:VEC vecSource:["title","content"]
-other fields
```

Vector field binding differs from regular fields:

- Vector field binding doesn't support expressions, can only be one-to-one binding
- Virtual table and entity table vector field vecSource must be strictly consistent. For example, if virtual table's virtualVec field has sourceVec as virtualA, virtualB two virtual fields, and binds to entity table's realVec field; realVec's sourceVec is realA, realB. Then realA, realB and virtualA, virtualB must strictly correspond. When a virtual table vector field binds to an entity table vector field, their vectorization sources must also be one-to-one bound.

#### Vector Search

Vector search is implemented through `select` method's `conditions` (filtering) and `orderBy` (sorting) parameters:

**Vector sorting (orderBy):**

- Format: `[["vectorFieldName","searchText"],...]`
- Automatically sorts by similarity from high to low (vector distance from small to large)
- Can mix with regular field sorting: `[["contentVec","AI Tech"],["_create","desc"]]`

**Vector filtering (conditions):**

- Format: `["vectorFieldName","l2str","searchText",maxDistance]`
- maxDistance is maximum distance from search text, value is less than or equal to 1
- Can combine with regular filter conditions: `[["status","eq","published"],["contentVec","l2str","keyword",0.4]]`
- Supports use in OR conditions: `[["OR",["titleVec","l2str","keyword",0.3],["contentVec","l2str","keyword",0.3]]]`

**Example:**

```vl
-<VirtualTable-Documents "docTable">.select([["status","eq","published"],["contentVec","l2str",keyword,0.4]],[["contentVec",keyword],["_create","desc"]],[_offset,pageSize],null) -> _result
```

**Notes:**

- Vector fields don't need manual assignment during `insert`/`update`; system auto-generates based on `vecSource` fields
- Vector fields don't support `eq`, `contains` and other regular operators, only support `l2str` filtering and similarity sorting
- Search text is automatically vectorized by system for matching

## Logic Definition

### Conceptual Distinction: Events, Methods, Functions, and Expressions

#### Core Comparison Table

| Type                 | Definition                                                                       | Side Effects | Call Location                                                        | Can Use Internally              | Typical Use Cases                                    |
| -------------------- | -------------------------------------------------------------------------------- | ------------ | -------------------------------------------------------------------- | ------------------------------- | ---------------------------------------------------- |
| **Event**      | Passively triggered logic responding to user interaction or system state changes | ✅ Yes       | `# Frontend Event Handlers` section, listening to component events | Methods, Functions, Expressions | Handle clicks, inputs, initialization, etc.          |
| **Method**     | Actively callable units that execute complete business logic                     | ✅ Yes       | Event handlers, other methods                                        | Methods, Functions, Expressions | Data processing, service calls, state modification   |
| **Function**   | Pure data transformation computation units                                       | ❌ No        | Events, methods, functions, expressions                              | Functions, Expressions          | Formatting, validation, array transformation         |
| **Expression** | Computation formulas that return a single value                                  | ❌ No        | Property bindings, conditional judgments, derived variables          | -                               | UI display, conditional control, dynamic computation |

#### Mutual Call Rules (Strictly Follow)

|                         | Call Event | Call Method | Use Function | Use Expression |
| ----------------------- | ---------- | ----------- | ------------ | -------------- |
| **In Event**      | ❌         | ✅          | ✅           | ✅             |
| **In Method**     | ❌         | ✅          | ✅           | ✅             |
| **In Function**   | ❌         | ❌          | ✅           | ✅             |
| **In Expression** | ❌         | ❌          | ✅           | ✅             |

#### Quick Reference Examples

```vl
// Event: passively triggered, parameters have no type declaration
<Button-Submit "submitBtn">.@click()
-validateForm() -> _validation

<Input-Email "emailInput">.@change(newValue, oldValue)  // ✅ No types on event params
<Input-Email "emailInput">.@change(newValue(STRING), oldValue(STRING))  // ❌ Wrong

// Method: actively callable, has side effects, returns object
METHOD loadUserData(userId(INT)); RETURN {success:BOOL, data:{}}
-<ServiceDomain-User "userService">.GetUserInfo(userId) -> _result
-RETURN {success:_result.success, data:_result.data}

// Function (PIPE): pure, no side effects, named with _ prefix, usable in expressions
PIPE _formatPrice(price(FLOAT)); RETURN STRING
-RETURN ("¥" + price.toFixed(2))

<Text-Price> value:$price._formatPrice()  // ✅ PIPE in expression
<Text-Result> value:calculateTotal()      // ❌ METHOD in expression — forbidden

// Expression: property bindings, conditions, derived vars — no METHOD calls, no mutable methods
<Text-FullName> value:($firstName + " " + $lastName)
<If-HasItems> conditions:($items.length > 0)
$displayName(STRING) = ($userName != "" ? $userName : "Guest")  // Derived var
$sorted([{}]) = $items.sort((a, b) => a.price - b.price)       // ❌ .sort() is mutable
```

### Methods

#### Method Definition Location and Format

| Method Type                   | Applicable Files | Definition Section          | Definition Prefix | Method Name Format |
| ----------------------------- | ---------------- | --------------------------- | ----------------- | ------------------ |
| Frontend internal method      | vx, sc, cp       | # Frontend Internal Methods | METHOD            | camelCase          |
| Frontend module public method | sc, cp           | # Frontend Public Methods   | METHOD            | PascalCase         |
| Service (internal)            | vs               | # Services                  | SERVICE           | PascalCase         |
| Service (public HTTP)         | vs               | # Services                  | PUBLIC_SERVICE    | PascalCase         |
| Backend internal method       | vs               | # Backend Internal Methods  | METHOD            | camelCase          |

**Definition Format**

Method definition line is at top level, no indentation. All code lines in method body need at least one level of indentation (`-`).

```
<DefinitionPrefix> <MethodName>(param1(Type1),param2(Type2),...);RETURN {returnVal1:Type1,returnVal2:Type2,...}
-method body
```

- Internal service declaration:

```
SERVICE ServiceName(...);RETURN {...}
```

- Public HTTP service declaration:

```
PUBLIC_SERVICE ServiceName(...);RETURN ReturnType;EXPOSE {method:STRING,receive:STRING,response:STRING}
```

`PUBLIC_SERVICE` is external-only entry. `Section` cannot call `PUBLIC_SERVICE` directly.

#### Method Parameter Definition

Method parameters use camelCase and need type specification, e.g.: `rawEmail(STRING), tags([FLOAT])`

#### Method Parameter Rules

##### Parameter Read-Only Principle

All method parameters are **read-only** — cannot be modified inside methods. Copy to a local variable first if mutation is needed. Only global variables (`$` prefix) can be directly modified inside methods.

```vl
METHOD processData(inputList([STRING])); RETURN [STRING]
-inputList.push("newItem")              // ❌ Cannot modify parameter
-_localList([STRING]) = [...inputList]  // ✅ Copy first
-_localList.push("newItem")            // ✅ Modify local copy
-$globalList.push("item")             // ✅ Global variables can be modified
-RETURN _localList
```

#### Method RETURN Structure

For all methods, **`RETURN` structure must be an object `{}`**, even if there's only one return value.

**To work with system default error handling mechanism, it's recommended that return objects always include** **success:BOOL** **field, and optionally** **message:STRING** **field. For example:**

> **Exception — `PUBLIC_SERVICE`**: Public HTTP services are **not** required to follow the `success:BOOL` wrapper convention. Their response structure is defined by external API contract requirements. See [`# Services`](#-services-backend-service-definition) for details.

```vl
METHOD calculateTotal(prices([FLOAT]),discount(FLOAT));RETURN {total:FLOAT,discounted:FLOAT,success:BOOL}
-_sum(FLOAT) = 0
-FOR (_price,_index0) IN prices
--_sum = _sum + _price
-_discountedValue(FLOAT) = _sum * (1 - discount)
-RETURN {total:_sum,discounted:_discountedValue,success:true}
```

#### Method Calling

**Call Methods**

- Calling file-internal custom methods: Use directly `<MethodName>(parameters)`. Frontend files can call frontend public methods and internal methods; ServiceDomain files can call services and backend internal methods;
- Calling system/component methods: Use `<ComponentClass-ComponentName "componentId">.methodName(parameters)`
- Calling variable methods: Use `<variableName>.methodName(parameters)`

**Handling Method Return Values**

Use `-> variableName` to store method return data to specified variable. `variableName` can be a pre-declared local variable (starting with `_`) or a pre-declared global variable (starting with `$`, depending on current environment). If an undefined local receiver is used, it is created in the current block scope. When there's no return value or return value isn't needed, `->` can be omitted.

```vl
-methodName(param1,param2,...) -> _apiResponse // Store value to a pre-defined local variable, or create a local variable in the current block if undefined
-<ServiceDomain-DomainName>.ServiceName(param1,param2,...) -> $globalStatus // Store value directly to global variable. Note: storing to global variable subfields is currently not supported, e.g., $globalStatus.type is not allowed
-<ClientUtils>.delay(1500) // Method has no return value
```

**Best Practice**: Prefer storing method/service call results to local variables (`_variableName`) to maintain clear scope and data flow. If a result needs to be reused across multiple branches, declare the local variable in the shared outer block first and assign to it inside each branch. Only consider using global variables (`$variableName`) as receivers when genuinely needing to directly update an existing global state.

**Example:**

```vl
$isUserCreated(BOOL) = false

METHOD process(); RETURN {success:BOOL}
-_validationResult({})
-_creationDetails({}) = {}
-calculateTotal($prices, 0.1) -> _totalResult
-_finalTotal(FLOAT) = _totalResult.discounted
-<Section-MySection "mySection">.ValidateInput($email, $password) -> _validationResult
-IF _validationResult.success
--createUser($userData) -> _creationDetails
--$isUserCreated = true
--@userCreated(_creationDetails.userId)
-RETURN {success:true}
```

#### Internal Method Usage Boundaries

In frontend and backend development, internal methods are only used for defining **reusable logic**; their usage scenarios have strict standards:

- When a piece of logic is only used in one event method/service/public method, write that logic directly in that event method/service/public method. Forbidden to first define an internal method then call it in this event method/service/public method;
- When a piece of logic is used in 2 or more event methods/services/public methods, must encapsulate this logic as an internal method to avoid duplicate code

### Functions

#### Pipe Function Definition File and Section

In VL, custom data transformation pipe functions can be defined in code files:

| Function Type          | Applicable Files | Definition Section        |
| ---------------------- | ---------------- | ------------------------- |
| Frontend pipe function | vx, sc, cp       | # Frontend Pipeline Funcs |
| Backend pipe function  | vs               | # Backend Pipeline Funcs  |

#### Pipe Function Definition Format

```
PIPE <functionName>(param1(Type1),param2(Type2),...);RETURN returnVal1
-function body
```

- Function names use `_` prefix + camelCase, e.g., `_calculateTotal`

PIPE function parameter format requirements are identical to method parameter format.

#### Pipe Function RETURN Structure

PIPE functions are only for data processing and transformation; their RETURN structure can be flexibly handled based on current needs, can return any type supported by the data type system, for example:

```vl
# Frontend Pipeline Funcs

PIPE _toUpperCase(inputString(STRING));RETURN STRING
-RETURN inputString.toUpperCase()

PIPE _checkPrice(price(STRING));RETURN BOOL
-RETURN (Number(price) > 30)

# Backend Pipeline Funcs

PIPE _trimAndValidateEmail(email(STRING));RETURN STRING
-_trimmed(STRING) = email.trim()
-IF _trimmed.includes("@")
--RETURN _trimmed
-ELSE
--RETURN ""
```

#### Function Calling

All functions **can only be chained in expressions**, using `.` operator. See Function Call Examples section.

### Events

#### Event Handler Definition

All event handlers are uniformly defined in `# Frontend Event Handlers` / `# Backend Event Handlers` sections, consisting of two parts:

- Event listening statement
- Event handling body

**Definition Format:**

```vl
<ComponentClass-ComponentName "componentId">.@eventName(param1, param2...) // Event listening statement
-Event handling body
```

**Format Description:**

- Event listening statement has no indentation
- Event handling body all code has at least one level indentation (`-`)
- Event name must have `@` symbol prefix
- Event listening statements must not be placed under `# Frontend Tree` / `# Backend Tree`; tree sections declare component structure only.

**Naming Conventions:**

- Event names uniformly use **camelCase**
- Event parameter names use **camelCase**
- Parameter names must strictly follow definitions in component documentation

**Event Body Writing Rules**

Event handling body is essentially a method; its writing rules (indentation, variable operations, method calls, control flow, error handling, etc.) are identical to methods, refer to "Methods" section.

#### RETURN Statement

- Event handlers can use `RETURN` to terminate execution early
- Event handlers **don't need return values**; `RETURN` is followed by nothing

```vl
<Button-Submit "submitButton">.@click()
-IF !$canSubmit
--<SysUI>.showToast("Please complete required fields first", "warning")
--RETURN
-submitForm()
```

#### GUARD Statement

`GUARD` is a dedicated fail-fast statement for high-frequency defensive checks.

**Syntax:**

```vl
GUARD condition "message"
```

Rules:

- `condition` is a failure condition
- When `condition == true`, the guard is triggered
- When `condition == false`, execution continues
- `GUARD` does not replace `IF`; it is only for "fail and exit now" semantics

**Default behavior by host:**

- In `METHOD / SERVICE`: equivalent to `RETURN {success:false,message:"..."}`
- In `TRANSACTION`: equivalent to `ROLLBACK {success:false,message:"..."}`
- In frontend event handlers: stop remaining actions and continue using the system default error-handling path

**Single-line example:**

```vl
METHOD loadCourses(); RETURN {success:BOOL, message:STRING, data:[{}]}
-<ServiceDomain-Course "courseService">.GetCourseList() -> _result
-GUARD (!_result.success) _result.message
-GUARD (_result.dataArray.length == 0) "Course list is empty"
-RETURN {success:true, message:"ok", data:_result.dataArray}
```

**Block form:**

```vl
-GUARD (_courseList.dataArray.length == 0) "Course list is empty"
--<SysUI>.showToast("Course list is empty", "error")
```

If a `GUARD` block contains no explicit `RETURN` / `ROLLBACK`, the default host exit behavior still applies after the block body finishes.

#### Event Parameters

Event parameters are defined in the event listening statement:

**When to Declare Parameters:**

- When event handling body needs to use event parameters, must declare parameters in parentheses of event listening statement
- Parameter names use **camelCase**, must exactly match definitions in component documentation
- Parameters **do not allow** type declarations; their types are entirely determined by the component

**When Parameters Can Be Omitted:**

- When event handling body doesn't need any event parameters, parentheses can be empty
- Example: `@click()` means listening to click event but not using event object

**Example:**

```vl
# Frontend Event Handlers

// Need to use event parameters
<Input-Username "usernameInput">.@change(newValue, oldValue)
-$username = newValue
-<ClientUtils>.consoleLog("Old value: " + oldValue)

// Don't need event parameters
<Button-Submit "submitButton">.@click()
-Submit logic, no need for @click parameters
```

##### Keyboard Event Standard Parameters

For `@keyDown`, `@keyUp`, and `@keyPress`, VL defines one standard parameter contract:

- `key`
- `code`
- `altKey`
- `ctrlKey`
- `shiftKey`
- `metaKey`
- `repeat`
- `isComposing`

Meanings:

- `key`: logical key value such as `Enter`, `ArrowDown`, or `a`
- `code`: physical key code such as `Enter`, `ArrowDown`, or `KeyA`
- `altKey` / `ctrlKey` / `shiftKey` / `metaKey`: modifier-key flags
- `repeat`: whether the event is an auto-repeat caused by long press
- `isComposing`: whether the IME composition session is still active

Boundary rules:

- Keyboard events use the standardized parameter list above rather than a raw `event` object as the author-facing contract.
- `type`, `target`, `currentTarget`, `preventDefault()`, and `stopPropagation()` are not part of the standardized VL keyboard parameter contract.
- Standard component-level examples are `<Col-Root "root">.@keyDown(key, code, altKey, ctrlKey, shiftKey, metaKey, repeat, isComposing)` and `<Input-Name "nameInput">.@keyUp(key, code, altKey, ctrlKey, shiftKey, metaKey, repeat, isComposing)`.
- A keyboard listener declared on a concrete component instance is a local focus-scope event of that component, not a global window hotkey listener.

#### Event Triggering Mechanism

**Triggering Rules:**

- Events are automatically triggered — **cannot be manually called** in code (no `.click()`, `.focus()`, `.blur()` DOM methods exist)
- Can only call methods explicitly declared in component documentation
- **No event propagation**: clicking a child component does not trigger parent's `@click`

#### Event Handling in Loops

- **Static IDs only**: ❌ `<Button-Action ("btn_" + _index0)>` → ✅ `<Button-Action "btn">`
- Loop variables (`_itemX`, `_indexX`) can be used directly in event handlers of components inside `<For>`/`<TreeFor>`. See For component docs for scope rules.

## Expressions

#### Allowed Operations in Expressions

1. **Arithmetic operators**: `+`, `-`, `*`, `/`, `%`, `**`
2. **Comparison operators**: `==`, `!=`, `>`, `<`, `>=`, `<=`
3. **Logical operators**: `&&`, `||`, `!` (operates on boolean values)
4. **Ternary operator**: `condition ? trueValue : falseValue`

```vl
$message = ($age >= 18) ? "Adult" : "Minor"
$complexResult = (($valueA > 10 && $valueB < 20) || $isOverride ? "Case1" : "Case2")
```

**Property values and derived expressions**: Parentheses required when there are operators; **Control statement conditions (IF/ELSE IF/ternary conditions)**: Outermost parentheses can be omitted, but parentheses are encouraged for complex logic to improve readability.

5. **Access variables**: `$globalVar`, `_localVar`, `_item0.field1`, `SYSENV.currentUser.userId`
6. **String concatenation**: Use `+` operator.
   **Important rule**: To ensure correct type and avoid potential issues, when concatenating strings, if expression doesn't start with a string literal (`"..."` or `'...'`), **must** add `"" +` at the beginning of expression.

```vl
$fullName = ($firstName + " " + $lastName)
$priceDisplay = ("" + "¥" + $price)
```

7. **Call PIPE functions**:

- Example: `_inputValue._trim()._toUpperCase()`

8. **Call variable built-in immutable methods**:
   These methods **cannot modify original data**, but return new processed results.

**Example:**

```vl
- _isValid(BOOL) = /^\d+$/.test($inputValue)
```

9. **Use arrow functions (`=>`) as callbacks**:
   When system default immutable methods (like `filter`, `map`) need callback functions, **must** use arrow function (`=>`) syntax. **Strictly forbidden** to use `function` keyword in expressions to define callbacks; **only limited to callbacks for JS built-in immutable prototype methods**;

   ```vl
   // Correct approach:
   -_filteredOptions([{value:INT,label:STRING}]) = $stockOptions.filter( (item) => item.value > 10 )
   -_firstMatchingOption({value:INT,label:STRING}) = $stockOptions.filter( (option) => option.value === 100 )[0]
   ```

#### Core Forbidden Rules

**1. Absolutely Forbidden to Call METHOD in Expressions**

Any method defined with `METHOD` cannot be called in expressions:

```vl
# ❌ Incorrect examples
<If-Check> conditions:(validateData())  # validateData is METHOD
<Text-Result> value:calculateTotal()    # calculateTotal is METHOD
$summary(STRING) = generateReport()     # generateReport is METHOD

# ✅ Correct approach: Use PIPE function instead
PIPE _validateData(); RETURN BOOL
-RETURN ($formData.name != "" && $formData.email != "")

<If-Check> conditions:$formData._validateData()
```

**Reason**: METHOD may contain side effects (like modifying variables, calling services); calling in expressions leads to unpredictable behavior and performance issues.

#### Parentheses Rules in Expressions

**General Principle**:

All expressions with operators must have parentheses. Otherwise don't use them.

```vl
$message = ($age >= 18) ? "Adult" : "Minor"
<StateStyle-EmptyState> conditions:(!$hasData)
<Text-Time> value:_item0.time
```

### Complex Expression Examples (Including Loop Variables)

```vl
# Access loop item properties
_item0.name
_user.profile.avatar

# Conditional judgment on loop item properties
_item0.status == "active" ? "Online" : "Offline"

# Combine loop index and loop item
("" + "Index:" + (_index0 + 1) + ", Name:" + _item0.name)
```

(Loop variable naming conventions: see "Variable Naming Rules" section.)

## Logic Writing Basic Rules

### Unidirectional Data Flow Rules (Strictly Follow)

**Downstream data is uniquely determined by upstream data.** Downstream cannot be independently modified or reverse-modify upstream.

| Upstream → Downstream | Rule | Error Example |
|----------------------|------|---------------|
| Variable → Component Property | Modify the variable, not the property. Editable input components require explicit event writeback; VL does not provide any built-in two-way binding or v-model semantics. | `<Input>.value = "x"` ❌ |
| Global Variable → Derived Variable | Derived vars are computed from globals; cannot be assigned in methods. | `$userCount = $users.length + 1` ❌ |
| Loop Data Source → Loop Variable | Loop vars cannot be independently modified; modify the source array. | `_item0.checked = true` ❌, use `$list[_index0].checked = true` ✅ |
| Parameter Declaration → Parameter Usage | Parameters are read-only (see Parameter Read-Only Principle above). | `inputList.push(x)` ❌ |

#### Writeback Event Semantics

VL provides three categories of explicit writeback events for editable input components:

- `@input`: Real-time input writeback — fires on every keystroke or input change
- `@change` / `@blur` / `@confirm`: Deferred commit writeback — fires on blur, content change confirmation, or explicit submit

All of these are explicit event writebacks. None of them constitute built-in two-way binding.

#### Explicit Input Writeback

Editable input components use controlled-value semantics. A `value:$var` binding supplies the displayed value, but VL does not provide built-in two-way binding or `v-model` semantics. User edits must be written back explicitly through an event handler.

Writing back to the same variable that is bound by `value` is legal and is the standard controlled-input pattern:

- Real-time writeback: `<Input-Name "nameInput"> value:$name` plus `<Input-Name "nameInput">.@input(value) -$name = value`
- Deferred commit writeback: `<Input-Name "nameInput"> value:$name` plus `<Input-Name "nameInput">.@change(value) -$name = value`
- Blur commit writeback: `<Textarea-Remark "remarkInput"> value:$remark` plus `<Textarea-Remark "remarkInput">.@blur(value) -$remark = value`
- Confirm commit writeback: `<Input-Code "codeInput"> value:$code` plus `<Input-Code "codeInput">.@confirm(value) -$code = value`

Use a draft/final split only when the UI needs transformation, validation, formatting, or a separate committed business value. Example: `<Input-Duration "durationInput"> value:$durationDraft`, `<Input-Duration "durationInput">.@input(value) -$durationDraft = value`, and `<Input-Duration "durationInput">.@change(value) -$duration = parseInt(value)`.

Assignments to component properties are still invalid. Authors must update variables, not component instance properties. Example: `<Input-Name "nameInput">.value = "x"` is invalid; `$name = "x"` or an explicit event writeback to `$name` is valid.

### Conditional Judgment Usage Rules (`IF / ELSE IF / ELSE`)

`IF` statement is used for conditional branching based on **boolean expressions**, can handle more complex conditional logic.

**Indentation Rules:**

- `IF booleanExpression` statement maintains **same indentation** as current line in its code block (e.g., `-`).
- `ELSE IF booleanExpression` and `ELSE` keywords also maintain **same indentation** as `IF` (e.g., `-`).
- Code blocks inside `IF`, `ELSE IF`, `ELSE` are **one level deeper** than their keywords (e.g., `--`). Nested `IF` continues increasing indentation.
- Each `IF`, `ELSE IF`, and `ELSE` branch body is a child local-variable scope. Local variables first defined inside a branch cannot be referenced after the branch unless they were already declared in the shared outer block.
- When an `IF` belongs inside a `FOR` / `WHILE` body, the `IF` line must be indented one level deeper than the loop statement. Writing the `IF` at the same indentation as the loop makes it a sibling after the loop, so loop variables are no longer in scope.

```
-IF condition1
--execution block 1
---IF nested condition 1.1
----...
-ELSE IF condition2
--execution block 2
---...
-ELSE
--default execution block
---...
```

**Example 1: Simple IF/ELSE**

```vl
METHOD checkAge(age(INT)); RETURN {canVote:BOOL}
-IF age >= 18
--RETURN {canVote:true}
-ELSE
--RETURN {canVote:false}
```

**Example 2: IF / ELSE IF / ELSE Nested**

```vl
# Frontend Internal Methods

PIPE _extractTypes(input([{type:STRING, name:STRING}]));RETURN [STRING]
-_types([STRING]) = []
-_seen({}) = {}
-FOR (_item0, _index0) IN input
--IF !_seen[_item0.type]
---_seen[_item0.type] = true
---_types.push(_item0.type)
---IF _item0.type == "INT"
----_types.push("INT")
---ELSE IF _item0.type == "BOOL"
----_types.push("BOOL")
--ELSE IF _item0.type == "STRING"
---_types.push("STRING")
---IF _item0.type == "INT"
----_types.push("INT")
---ELSE
----_types.push("STRING")
--ELSE
---_seen[_item0.type] = true
-RETURN _types
```

### Loop Usage Rules (`FOR`)

VL supports three `FOR` loop structures:

- **In Frontend Component Tree (# Frontend Tree):** Use `<For>`, `<TreeFor>` and other loop structure containers to create child components in loops
- **In Method/Function Body:** Use loop syntax `FOR...IN` and `FOR(...)` for array loops and counting loops

##### 1. Loop Structure Containers in Component Tree (`<For>`, `<TreeFor>`)

See For/TreeFor component documentation for syntax and properties.

##### 2. Array Loop in Methods/Functions (`FOR ... IN`)

Use `FOR ... IN ...` keyword to traverse arrays.

**Indentation Rules:**

- `FOR (_itemVarN, _indexVarN) IN arrayVariable` statement maintains **same indentation** as current line in its code block (e.g., `-`).
- Code blocks inside loop body are **one level deeper** than `FOR` statement (e.g., `--`).
- Loop variables can be used directly without pre-definition. Naming: see "Variable Naming Rules" section.
- Loop variables are scoped to the loop body only. Any line that returns to the same indentation level as the `FOR` statement is outside the loop and must not reference `_itemVarN` or `_indexVarN`. Nested `IF` / `ELSE` / `WHILE` / `FOR` blocks that should use loop variables must remain under the loop body's indentation.

**Syntax:**

```
-FOR (_itemVarN, _indexN) IN arrayVariable
--loop execution code
--...
```

**Example:**

```vl
METHOD calculateTotalAge(userList([{name:STRING, age:INT}])); RETURN {totalAge:INT, success:BOOL}
-_totalAge(INT) = 0
-FOR (_user0, _index0) IN userList
--_totalAge = _totalAge + _user0.age
-RETURN {totalAge:_totalAge, success:true}
```

When nesting `FOR...IN` loops, each `FOR` adds one indentation level, and loop variables `N` increment:

```vl
METHOD processCategories(categories([{name:STRING, products:[{name:STRING, price:FLOAT}]}])); RETURN {processed:BOOL}
-FOR (_category0, _index0) IN categories
--log(("Processing category: " + _category.name))
--FOR (_product1, _index1) IN _category.products
---log(("-- Processing product: " + _product.name))
-RETURN {processed:true}
```

##### 3. Counting Loop in Methods/Functions (`FOR (...)`)

Provides C/Java/JavaScript-like `for` loop structure, including initialization, condition check, and increment/decrement expressions.

**Indentation Rules:**

- `FOR (initialization; condition; increment/decrement)` statement maintains **same indentation** as current line in its code block (e.g., `-`).
- Code blocks inside loop body are **one level deeper** than `FOR` statement (e.g., `--`).
- Loop control variable is declared in initialization part, using `_varName(TYPE) = initialValue`. **Must** declare type, **no** `let` keyword. **Must** use `_` prefix + **camelCase** name (e.g., `_i`, `_j`, `_k`, `_loopCounter`).

**Syntax:**

```
-FOR (_loopVar(TYPE) = initialValue; loopCondition; incrementExpression)
--loop execution code
--...
```

- **`_loopVar(TYPE) = initialValue`**: Initialization statement, declares and initializes loop variable. **Must** include type declaration and `_` prefix.
- **`loopCondition`**: Condition for loop continuation, an expression returning `BOOL`.
- **`incrementExpression`**: Expression executed after each loop iteration, usually modifies loop variable (e.g., `_i++`, `_i--`, `_i = _i + 2`).

**Example:**

```vl
# Frontend Event Handlers

<Button-LoopDemo "loopDemoButton">.@click()
-<ClientUtils>.consoleLog("--- FOR loop 0 to 9 ---")
-FOR (_i(INT) = 0; _i < 10; _i++)
--<ClientUtils>.consoleLog(_i)

-<ClientUtils>.consoleLog("--- FOR loop with step 2 ---")
-FOR (_step(INT) = 0; _step < 10; _step = _step + 2)
--<ClientUtils>.consoleLog(_step)
```

#### Error Handling

**Use System Default Error Handling** (Recommended):

- Standard CRUD operations
- Simple data fetching
- No special error recovery logic needed

**Use `IF` Custom Judgment**

- Need different handling based on specific error types
- Need cleanup operations on errors
- Need detailed error logging

After **method calls** or **service calls**, there are two error handling approaches:

**1. System Default** — No explicit handling needed. If previous step's `success != true`, system auto-returns error and (if frontend-called) shows error toast. Prefer this unless custom logic is required.

**2. Manual `IF` Handling** — Check result and branch:

```vl
-someServiceCall() -> _result
-IF !_result.success
--$isLoading = false
--<SysUI>.showToast(("" + "Failed: " + _result.message), "error")
--RETURN
```

## Style Definition

#### VL Style Model vs HTML/CSS

VL is **not** a CSS authoring language. Understanding what VL does **not** have is essential to writing correct VL code.

| HTML/CSS Concept | VL Equivalent |
|---|---|
| **CSS selectors** (`.class`, `#id`, `div > p`, `*`) | **Do not exist.** Each component's visual properties are declared directly on itself. There is no way to target other components by selector. |
| **CSS cascade & inheritance** | **Does not exist.** A parent's skin properties do not cascade to children. Every component explicitly declares its own style coordinate or skeleton properties. |
| **CSS pseudo-classes** (`:hover`, `:focus`, `:disabled`) | **Not written by developers.** Handled automatically by the platform runtime-state model (`StateTriggers` + template consumption rules). |
| **CSS pseudo-elements** (`::before`, `::after`) | **Not available to VL authors.** Used internally by the platform (e.g., `stateOverlay` renders via `::after`). |
| **`style="color:red"` (inline CSS)** | VL `style:"primary|filled"` is a **dimension coordinate**, not inline CSS. Skin properties are resolved by Theme, not written inline. |
| **External stylesheets / `<style>` blocks** | **Do not exist.** Skeleton properties are written on components; skin properties live in the `.vth` Theme file. |
| **CSS `!important` / specificity conflicts** | **Do not exist.** Priority is deterministic: component-local `sk.*` override > Theme coordinate values > Platform fallback. |
| **CSS custom properties `var(--x)`** | VL style coordinates are resolved at compile time by Theme. The authoring model is different from CSS custom properties. |
| **`<svg>` implicit sizing from viewBox** | VL Icon component wraps SVG in a `<div>`. A `<div>` has no intrinsic size, so **explicit `width` and `height` are required**. If omitted, the platform applies a 24×24 default to prevent the browser's 300×150 replaced-element fallback. |
| **CSS `@media` queries** | **Not written by developers.** Responsive behavior is handled at the platform level via `DEVICE_TARGET` and `SCREEN_RESOLUTION` in SysConfig. |
| **CSS animations / `@keyframes`** | **Not available in VL.** Interaction transitions are managed by the platform runtime-state model and runtime defaults; component animation is managed by platform behavior logic. |

**Key mental model shift:** In HTML/CSS, style is a global, cascading, selector-driven system. In VL, style is **local, explicit, and coordinate-driven** — each component owns its appearance through a combination of skeleton properties (layout, written inline) and a style coordinate (skin, resolved by Theme).

Theme coordinates express reusable visual semantics; `sk.*` expresses component-local skin exceptions. `style` is never an expression container — AI/tools MUST NOT generate `style:$var`, ternary `style:(cond ? ... : ...)`, or function-driven style values.

#### Theme System

**Skin / Skeleton Responsibility Boundary:**

The style space manages **skin** (visual appearance) only. **Skeleton** (layout and typographic structure) is defined in component code. This is a hard boundary.

| | Skin | Skeleton |
|---|---|---|
| **Managed by** | Theme designer, via `.vth` file | Component developer, via VL code |
| **Scope** | Global theme | Component-level decisions |
| **Core criterion** | Changing Theme alters appearance, not layout | Changing value causes layout reflow or typographic change |
| **Style space** | ✅ Expressed via coordinates | ❌ Not in style space |
| **VL code** | ❌ CSS original skin property names inline forbidden; component-local skin override via fixed `sk.*` props only | ✅ Written directly |

**Skin properties** (managed by Theme style space; CSS original names must not be inlined in VL code. Component-local skin overrides MUST use `sk.*` prefix props — see §Component Skin Props):

| Property Category | Examples |
|---------|------|
| Colors | color, background-color, border-color |
| Decoration | box-shadow, opacity, border-radius |
| Background layer | background-image, background-repeat, background-position, background-size |
| Border style | border-style, border-width |
| Border shorthand | border, border-top, border-bottom, border-left, border-right |
| Text transform | text-transform |
| State effects | hover overlay, focus ring, disabled opacity |

> Border governance rule: in VL, the full border triad (width/style/color) and any border shorthand are managed as skin. Border CSS literals must not be authored in SC/CP code. Use Theme slots (`surfaceBorder*`, `surfaceDivider*`), `Divider` with style coordinate, or `sk.borderColor` / `sk.borderWidth` / `sk.borderTop` / `sk.borderRight` / `sk.borderBottom` / `sk.borderLeft` for component-local skin override.

**Skeleton properties** (defined by component developers directly in VL code):

| Property Category | Examples |
|---------|------|
| Layout structure | width, height, min-width, max-width, min-height, max-height, margin, flex, position, align-items, justify-content |
| Inner spacing | padding, padding-* |
| Gap | gap |
| **Typography** | **font-size, font-style, font-weight, line-height** |
| Interaction behavior | transform |

> Clarification: `border-*` is not skeleton. Although `border-width` participates in CSS box model, VL treats border as a visual concern for governance consistency.

> `font-size` / `font-style` / `font-weight` / `line-height` directly affect text layout and typographic structure, belonging to skeleton. A single page requires multiple font sizes (e.g., 10px for auxiliary labels, 13px for body, 28px for statistics, 32px for headings) — this is a component-level typographic decision, not a Theme-level skin preference.

Every CSS property belongs to exactly one of three categories: **Platform Baseline** (hard-locked), **Skin** (Theme manages), or **Skeleton** (code manages). There is no overlap zone. **Exception: the `size` dimension** bridges skin and skeleton — it provides Theme-based defaults for skeleton properties (`padding`, `font-size`, `font-weight`, `line-height`, `min-height`) on Button/Input/Textarea, plus `gap` for Button child layout when Button renders UI children. Explicit skeleton declarations always take priority over `size` theme values (see §size dimension).

**Platform Baseline (Hard-Locked Properties):**

Platform Baseline is a built-in constraint of the syntax rules. Baseline rules can only be added, never modified or deleted once published. The following properties are force-emitted by the platform in compiled output. Both Theme (`.vth`) and VL code **must not** declare these properties; otherwise the compiler reports error.

| Property | Fixed Value | Rationale |
|----------|-------------|-----------|
| `box-sizing` | `border-box` | Unify box model calculation; overriding breaks padding/width computation |
| `font-synthesis` | `none` | Prevent browser synthetic bold/italic; ensure predictable font rendering |

Platform Baseline serves **browser normalization** only — it does not carry brand visuals (skin) or layout structure (skeleton). Hard-locked properties take priority over both skin and skeleton — any override is a bug. Future versions may add new baseline properties, but published entries cannot be modified.

Skeleton properties use CSS literal values (e.g., `padding:6px`), but must not write skin properties.

**Style Coordinate System (`style`):**

VL uses the style space coordinate system. Components declare visual semantics via the `style` attribute; the Theme fills slot values for each dimension point.

```vl
<Button-DeleteUser "delBtn"> style:"danger|ghost|pill|actionable" value:"Delete"
```

`style` is a component attribute written outside `<>`. Its value **must be a static string literal**; VL expressions, variables, and ternary operators are forbidden. The value is a `|`-separated string; each word uniquely resolves to a static dimension point. Order does not matter. For component-local **skin** exceptions (static or dynamic), use `sk.*` props (see §Component Skin Props). For business-state conditional styling that does not require runtime-computed skin values, use `StateStyle` with `conditions`.

**Closed-world rule for `style`:**

1. Only the seven static dimensions listed below may appear in `style`: `intent`, `emphasis`, `shape`, `surface`, `textRole`, `size`, `affordance`.
2. `state` is a special runtime dimension and **must not** appear in `style`.
3. The point lists below are the **complete legal point sets** for VL 4.2.5. Any dimension name or point name not listed here is invalid. AI/tools **must not** invent aliases, synonyms, or new point names.
4. A component may only use points from dimensions it supports; see the authoritative Component × Dimension Support Table later in this chapter.
5. Each static dimension may contribute at most one point to a single `style` coordinate.

**Style Space Dimensions (Static, complete point sets):**

| Dimension | Purpose | Legal points (complete list) |
|-----------|---------|------------------------------|
| `intent` | Business semantic intent | `primary`, `neutral`, `danger`, `success`, `warning`, `info`, `inverse` |
| `emphasis` | Visual weight / expression mode | `filled`, `outlined`, `ghost`, `text`, `tonal` |
| `shape` | Geometric form (border-radius) | `default`, `pill`, `square`, `soft`, `sharp` |
| `surface` | Container layer / backdrop (containers, plus Button baseline in 4.2) | `solid`, `subtle`, `elevated`, `overlay`, `dark` |
| `textRole` | Text visual hierarchy (color/opacity/transform) | `body`, `caption`, `hint`, `muted`, `weak`, `contrast` |
| `size` | Interactive component sizing (padding, typography, Button content gap) | `sm`, `md`, `lg` |
| `affordance` | Interaction role semantics | `passive`, `listitem`, `navitem`, `actionable`, `selectable` |

**Runtime states**: Interaction states (`hover`, `active`, `focus`, `selected`, `disabled`, `invalid`, `rest`) are not static dimensions. They are produced by the platform runtime-state model and consumed by component template rules. Runtime states must not appear in `style`.

Examples of invalid `style` generation:

- `style:"primary|critical|pill"` -> invalid because `critical` is not in any legal point set.
- `style:"primary|filled|solid"` on `Text` -> invalid because `solid` belongs to `surface`, and Text does not support `surface`.
- `style:"primary|filled|hover"` -> invalid because `hover` is a `state` point, and `state` is not allowed in static `style`.

**Theme File Format:** The section carrying `dimension.point.slot:value` assignments must use the heading `# Point Slot Values`. Using any other heading for this section is a compile error. Recommended section order: `# Meta` → `# Point Slot Values`.

`style` is **optional**. When not declared, all static dimensions are unset; no CSS is generated for those dimensions. `affordance` may also be omitted from a declared `style`; when omitted, parser uses the current component default.

##### `affordance` (Static Dimension)

Purpose: interaction-role semantics. `affordance` declares whether an element is a passive container, a list item, a navigation item, an actionable target, or a selectable target.

Rules:

1. `affordance` is a static single-value dimension and may contribute at most one point to a single `style` coordinate.
2. `affordance` does not encode runtime states such as `hover`, `active`, `focus`, `selected`, `disabled`, or `invalid`.
3. Runtime states are not written inside `style`; they are produced by platform runtime triggers and consumed by component template rules.
4. `hover / active / focus / selected` interaction visuals only apply to components that participate in the affordance-enabled runtime state model.
5. `selected` is a runtime state and may be driven by a component runtime input such as `selected:boolean`; it is not a static style point.
6. Use `passive` for non-interactive wrappers, layout containers, and structural shells that should not render hover / active / focus / selected feedback.
7. Default: if a first-release template (`Block`, `Row`, `Col`, `Grid`, `Button`, `ButtonContainer`) declares `style` but omits `affordance`, parser uses the current component default. Current defaults are `passive` for `Block` / `Row` / `Col` / `Grid`, and `actionable` for `Button` / `ButtonContainer`.
8. For text-like atomic components that do not participate in the affordance-enabled runtime state model, such as `Text` and `Icon`, omitted `affordance` is treated as `passive` no-op. Explicit `passive` is also accepted as a no-op compatibility point; other affordance points do not apply to these components.

**Style Priority Chain:**

| Priority | Source | Description |
|----------|--------|-------------|
| 1 (highest) | `sk.*` component override | Component-local skin override via `sk.*` string literal or expression |
| 2 | Component `style` coordinate | `style:"danger|ghost|pill"` resolves to Theme slot values |
| 3 (lowest) | Platform built-in fallback | Fallback when Theme is missing or slot unset |

> Skeleton properties (layout, typography) are generally not in this priority chain — skeleton and skin control different CSS property sets with no overlap. **Exception: the `size` dimension** (see §size dimension).

**Preset Themes:** Platform provides multiple preset Themes (Default, Enterprise, Dark, etc.) with complete Point Slot Values. Select one at project creation for zero-configuration use.

---

### Style Space Specification

#### 一、Style Space Dimension Definitions

The style space is fixed by the platform; the Theme only fills values. Platform-reserved dimension/point/slot names follow a stability-first principle: they should not be deleted or renamed without necessity. When an existing point name breaks style-shorthand uniqueness or creates a normative conflict, a corrective rename may be introduced together with synchronized Theme and implementation updates.

##### `intent` (Static Dimension)

Purpose: Business semantic intent. Does not control geometry or layout.

| Point | Description |
|-------|-------------|
| `primary` | Primary action / primary brand semantic |
| `neutral` | Neutral / secondary semantic |
| `success` | Success / completion semantic |
| `warning` | Warning / risk semantic |
| `danger` | Danger / delete / failure semantic |
| `info` | Informational prompt semantic |
| `inverse` | Inverted color semantic (light/dark reversal) |

| Slot | Description |
|------|-------------|
| `intentFg` | Semantic foreground color |
| `intentBg` | Semantic background color |
| `intentBorder` | Semantic border color |
| `intentOnBg` | High-contrast foreground on background (e.g., white text) |
| `intentFocusRing` | Focus ring (box-shadow format) |
| `intentSubtleBg` | Muted background (tonal / light-tint scenes) |

##### `emphasis` (Static Dimension)

Purpose: Visual weight and expression mode. Does not change business semantic; only changes presentation strategy.

| Point | Description |
|-------|-------------|
| `filled` | Strong expression — semantic solid background |
| `outlined` | Medium expression — transparent background + semantic border |
| `ghost` | Weak expression — transparent background + weak/no border |
| `text` | Minimal container feel — text/icon only |
| `tonal` | Medium-weak expression — semantic light background |

| Slot | Description |
|------|-------------|
| `emphasisBg` | Background value (may reference `@intent.*`) |
| `emphasisFg` | Foreground value |
| `emphasisBorderColor` | Border color |
| `emphasisBorderWidth` | Border width |
| `emphasisTextDecoration` | Text decoration (reserved; current values all `none`) |
| `emphasisShadow` | Shadow value (reserved; current values all `none`) |

**Boundary constraints (platform-fixed):**

1. `emphasis` does not define brand/state/semantic colors (those belong to `intent`)
2. `emphasis` does not define container layer (belongs to `surface`)
3. `emphasis` does not define hover/focus/disabled triggers (belong to `state`); `state` does not define expression weight (belongs to `emphasis`)
4. Slot values only allow two types: CSS value, cross-dimension reference (`@intent.*`)
5. Prohibit "secondary semantic words" (e.g., `solid`, `subtle`, `strong`) as final slot values

**Orthogonal relationship with `intent`:** `intent` provides semantic color sources; `emphasis` only selects which expression path to use. Cross-dimension references via `@intent.*` are allowed; hardcoding new brand semantic colors in `emphasis` is forbidden.

**`emphasis` value constraints:**
- `emphasisBorderWidth` must be a CSS length value (e.g., `0`, `1px`)
- `@intent.xxx` cannot reverse-reference `@emphasis.xxx` (circular reference is a compile error)

##### `shape` (Static Dimension)

Purpose: Geometric form (border-radius style).

| Point | Description |
|-------|-------------|
| `default` | Default border-radius |
| `pill` | Capsule shape (full radius) |
| `square` | No radius |
| `soft` | Large soft radius |
| `sharp` | Small sharp radius |

| Slot | Description |
|------|-------------|
| `shapeRadius` | Border-radius value (CSS `border-radius`) |

##### `surface` (Static Dimension — Containers + Button Baseline)

Purpose: Container layer, backdrop material, and decoration. In VL 4.2.5 it primarily applies to containers and the upgraded Button baseline. Input / Text / Icon do not enable `surface`; Divider is a structural visual element and uses `surface`.

| Point | Description |
|-------|-------------|
| `solid` | Solid backdrop |
| `subtle` | Muted backdrop |
| `elevated` | Elevated backdrop (with shadow) |
| `overlay` | Overlay backdrop (modals etc., with mask) |
| `dark` | Dark surface (e.g., sidebar / dark container) |

| Slot | Description |
|------|-------------|
| `surfaceBg` | Container background |
| `surfaceBorder` | Container border color |
| `surfaceShadow` | Container shadow |
| `surfaceBackdrop` | Mask value (only `overlay` uses this) |
| `surfaceBgImage` | Background image (CSS `background-image`; supports gradients and `url(...)`) |
| `surfaceBgRepeat` | Background repeat (companion of `surfaceBgImage`) |
| `surfaceBgPosition` | Background position (companion of `surfaceBgImage`) |
| `surfaceBgSize` | Background size (companion of `surfaceBgImage`) |
| `surfaceAccentBorderStart` | Directional accent border — start edge (platform maps to physical side, default left) |
| `surfaceAccentBorderEnd` | Directional accent border — end edge |
| `surfaceDividerTop` | Top divider line (maps to `border-top`) |
| `surfaceDividerBottom` | Bottom divider line (maps to `border-bottom`) |
| `surfaceSideRailBg` | Side rail decoration background (platform implements via pseudo-element) |
| `surfaceSideRailWidth` | Side rail width |
| `surfaceBorderStyle` | Container border style (CSS `border-style`) |
| `surfaceBorderWidth` | Container border width (CSS `border-width`) |

Decoration extension rules:

1. The 12 decoration slots above are container-level extensions, primarily for layout containers.
2. Slots not declared in Theme produce no CSS output.
3. Slot values are limited to CSS literals or cross-dimension references (`@dimension.slot`); expressions are not allowed.
4. `surfaceBgImage` maps to CSS `background-image`, supporting both gradient functions (`linear-gradient(...)`) and image references (`url(...)`).
5. `surfaceBgRepeat` / `surfaceBgPosition` / `surfaceBgSize` are companion properties of `surfaceBgImage`; they are only meaningful when `surfaceBgImage` is declared.
6. `surfaceBorderStyle` / `surfaceBorderWidth` combine with `surfaceBorder` (border-color) to fully express the three border elements.
7. Decoration slots are supported according to platform component capability — `Row`/`Col`/`Grid` support the full decoration slot set; `Page` supports background-image related slots; `Modal`/`Table*` support base `surface` slots (`surfaceBg`/`surfaceBorder`/`surfaceShadow`). Developers only rely on these observable effects; lower-level rendering details are platform-defined.
8. When a component consumes `surfaceBorderWidth` and the final CSS contains a non-zero border width but no border style, platform fallback injects `border-style:solid`. The current version does **not** provide an automatic `surfaceBorderWidth -> 1px` fallback.
9. `Divider` consumes `surfaceBorder + surfaceBorderStyle + surfaceBorderWidth`; rendering direction is `horizontal -> border-top`, `vertical -> border-left`.

##### `textRole` (Static Dimension)

Purpose: Text visual hierarchy — color, opacity, and text transform. Does not carry `font-size` / `font-weight` / `line-height` (those are skeleton properties).

| Point | Description |
|-------|-------------|
| `body` | Body text |
| `caption` | Caption / auxiliary text |
| `hint` | Hint / placeholder text |
| `muted` | Muted text on dark surfaces (e.g., `rgba(..., .55)`) |
| `weak` | Weak text on dark surfaces (e.g., `rgba(..., .35)`) |
| `contrast` | High-contrast text |

| Slot | Description |
|------|-------------|
| `textRoleColor` | Text role color |
| `textRoleColorOnDark` | Text role color on dark surfaces |
| `textRoleAlpha` | Opacity tier (for layered transparent text on dark backgrounds) |
| `textRoleTransform` | Text transform by role (e.g., caption `uppercase`); skin-layer role expression |

`textRole` rules:

1. `textRole` expresses text role visual hierarchy (skin) — color, opacity, and text transform only.
2. `textRoleColorOnDark` is used for text hierarchy color on dark container surfaces.
3. `textRoleAlpha` enables opacity tiers for multi-level transparent text on dark backgrounds.
4. `textRole` only takes effect on components that have this dimension enabled.
5. `muted` and `weak` are intended for mid-to-low hierarchy text in dark areas.
6. `textRoleTransform` sets text transform by text role (e.g., caption defaults to `uppercase`), belonging to skin-layer role expression.

##### `state` (Special Dimension)

Purpose: Interaction state overlay. Developers do not declare `state` points in `style`; the platform triggers them automatically based on component interaction state and applies them implicitly to all state-enabled components.

| Point | Description |
|-------|-------------|
| `rest` | Default state (no overlay) |
| `hover` | Hover state |
| `active` | Pressed state |
| `focus` | Focus state |
| `disabled` | Disabled state |
| `invalid` | Error state |

| Slot | Description |
|------|-------------|
| `stateOverlay` | Semi-transparent overlay color; does not replace baseline background (overlay layer) |
| `stateShadow` | Shadow override (e.g., focus ring) |
| `stateBorder` | Border color override |
| `stateOpacity` | Opacity override |
| `stateTransitionDuration` | Transition duration (e.g., `150ms`, `0.2s`) |
| `stateTransitionTiming` | Transition timing function (e.g., `ease`, `cubic-bezier(...)`) |
| `stateTransitionProperty` | Transition property list (e.g., `all`, `opacity, box-shadow`) |

Runtime-state transition semantics:

1. `stateTransitionDuration`, `stateTransitionTiming`, and `stateTransitionProperty` are runtime defaults, not Theme point-slot values.
2. The platform emits these transition values into baseline CSS for templates that participate in the runtime-state model.
3. Runtime-state transitions cover affordance-driven hover / active / focus / selected feedback and runtime disabled / invalid feedback.

> Note: `cursor` is supplied by `affordance.cursor` for affordance-enabled templates. Authors should not override `cursor` via raw CSS property names in VL component code. `transform` remains a skeleton / behavior property managed in component code.

**Relationship with VL StateStyle widget:** The runtime-state model is separate from `StateStyle`. VL code `StateStyle` widgets with `conditions:` are for business-logic conditional style switches (e.g., `conditions:$isHighlighted`), but they are not a dynamic skin value system. Business-condition skin changes should prefer `StateStyle + style` coordinate patches; `sk.*` remains a local escape hatch for cases the style space cannot yet express.

---

#### 二、Style Synthesis and Priority Rules

##### `textRole` Synthesis Chains

For components with `textRole` enabled, the `color` synthesis chain is:

`color ← textRole.textRoleColorOnDark + textRole.textRoleColor + intent.intentFg + emphasis.emphasisFg`

`+` uses last-wins composition:

1. When multiple sources are present: `emphasis` > `intent` > `textRole`.
2. When only `textRole` is declared: `textRole` takes effect.

Opacity chain:

`opacity ← textRole.textRoleAlpha + runtime disabled opacity when disabled`

Text transform chain:

`text-transform ← textRole.textRoleTransform`

`textRoleTransform` is a single source — no last-wins composition.

##### `surface` Decoration Slot Generation Semantics

1. `surfaceBgImage` maps to `background-image` (supports gradient functions and `url(...)` image references).
2. `surfaceBgRepeat` maps to `background-repeat`; `surfaceBgPosition` maps to `background-position`; `surfaceBgSize` maps to `background-size`.
3. `surfaceAccentBorderStart` / `surfaceAccentBorderEnd` map to directional borders (platform maps to physical sides, default left).
4. `surfaceDividerTop` / `surfaceDividerBottom` map to top/bottom divider lines (`border-top` / `border-bottom`).
5. `surfaceBorderStyle` maps to `border-style`; `surfaceBorderWidth` maps to `border-width`. Combined with `surfaceBorder` (border-color) they express the three border elements.
6. `surfaceSideRailBg` / `surfaceSideRailWidth` map to container side rail decoration (platform implements via pseudo-element).
7. The above slots only take effect on component types that support those visual effects.

##### Container `intent` Synthesis Chains

For container components with `intent` enabled (Row / Col / Grid), the synthesis chains are:

`background-color ← intent.intentSubtleBg + surface.surfaceBg`

`border-color ← intent.intentBorder + emphasis.emphasisBorderColor + surface.surfaceBorder`

`border-width ← emphasis.emphasisBorderWidth + surface.surfaceBorderWidth`

`border-style ← surface.surfaceBorderStyle`

Rules:

1. When only `intent` is declared: uses `intentSubtleBg` as semantic-color light background.
2. When `intent`, `emphasis`, and `surface` all contribute to the same container-border property, they resolve in the synthesis order shown above (last-wins within the chain).
3. `Row` / `Col` / `Grid` using `outlined`-style emphasis points must produce visible borders whenever the Theme provides non-empty border width/color values for those points.
4. Container `intent` is only for semantic-color background/border expression (e.g., success/warning/danger alert containers); it does not affect inner child element colors.
5. No `intentSubtleBorder` slot is added. Weak border semantics are expressed through existing `intent.intentBorder` combined with `emphasis`/`surface`, avoiding slot semantic overlap.

---

#### 三、`style` Coordinate Syntax and Resolution Rules

##### Input Form

Developers use a shorthand string literal: `style:"danger|ghost|pill"`

The value **must be a string literal** — VL expressions (`$var`, ternary `? :`, function calls) are **not allowed**. The compiler splits by `|` at compile time and looks up the unique static dimension point for each word; order does not matter. Read it as a fixed `|`-separated coordinate string. Object-form coordinates are not part of VL authoring syntax.

`style` is optional. When not declared, all static dimensions are unset and generate no CSS. The platform does not auto-fill default points for unset dimensions.

Each component type has a platform-defined set of supported static style dimensions. Dimensions declared in `style` must be within that set; otherwise a compile warning is reported.

Writing position: `style` is a component attribute, written outside `<>`; only the component type name and optional id are allowed inside `<>`. Example: `<Button-DeleteUser "delBtn"> style:"danger|ghost|pill|actionable" value:"Delete"`

**Global uniqueness rule for dimension points:** Static dimension point names are globally unique across the entire platform (no duplicate names across dimensions). If the platform itself violates this constraint, that is a platform-specification error rather than an ambiguity VL authors are expected to resolve in project code.

##### Conflict Rules (Compile Warnings)

1. Same dimension, multiple points (e.g., `"danger|primary"`) → warning
2. Duplicate point (e.g., `"danger|danger"`) → warning
3. Unknown point (not in any dimension point set) → warning
4. `state` point appears in `style` → warning
5. `style` value contains VL expression, variable, function call, or any non-literal content (e.g., `style:($cond ? "success" : "danger")`, `style:$buttonStyle`, `style:computeStyle()`) → warning (STY-001)

##### Unset Handling (Platform-Fixed)

1. Static dimensions not declared in `style` have status: unset
2. All slots of an unset dimension return empty; no contribution to final CSS properties
3. If all sources for a CSS property are empty → that CSS declaration is not generated
4. Platform must not auto-fill default points for unset dimensions

---

#### 四、Cross-Dimension Reference Syntax and Theme Responsibility Boundary

##### `@dimension.slot` Reference Syntax

Theme slot values may reference slots of other dimensions:

- Syntax: `@dimension.slot` (e.g., `@intent.intentBg`)
- This syntax is only legal inside Theme `# Point Slot Values`, where it appears as the `value` of a `dimension.point.slot:value` assignment.
- Component-instance properties must not directly write Theme tokens, including `style`, `sk.*`, skeleton props, or functional props.
- When the referenced dimension is not declared, the reference resolves to empty without affecting other sources
- Circular reference → compile error
- Missing reference → compile error

##### Theme Responsibility Boundary

The Theme only fills `DimensionPoint → Slot → Value`; it must not define or modify:

1. Which style dimensions a component type supports
2. How component states are triggered
3. Platform style synthesis order and composition rules
4. The platform-reserved dimension/point/slot set
5. Component-instance authoring syntax beyond legal static `style` coordinates

Direct authoring boundary:

- Theme tokens are only consumed inside the style space.
- Component instances do not directly reference Theme tokens; they consume Theme through static `style` coordinates.
- `sk.*` is a component-local override channel for CSS literals or runtime expression results; it is not a Theme token channel.
- Allowed examples: `<Button-Submit "submitBtn"> style:"primary|filled|pill|actionable" value:"提交"` and `emphasis.filled.emphasisBg:@intent.intentBg`
- Forbidden examples: `style:"@intent.intentBg"` and `sk.bg:"@intent.intentBg"`

---

#### 五、Theme File Specification

##### File Structure (Fixed Order)

1. **Root node** (required): `<Theme-Name>`
2. **`# Meta` section** (optional): metadata, e.g., `mode:"light"`, `base_theme:"Platform/Theme-Default-Light@1"`
3. **`# Point Slot Values` section** (required): `dimension.point.slot:value` assignments

The section heading for coordinate value assignments must be `# Point Slot Values`. Using any other heading is a compile error.

##### Theme Rule Contract

Theme rule compatibility is determined by the `VL_VERSION` declaration in the first line of the Theme file.

Rules:

1. Theme file validity and rule matching are governed by the `VL_VERSION` declared in the file's first-line comment (e.g., `// VL_VERSION:4.3.1`)
2. Authors do not need to understand or maintain a separate Theme template version number (e.g., `version:"7.0.0"`) or `styleSpaceVersion` for rule-matching purposes
3. The recommended author-facing guidance is: "use the current latest VL syntax version" and "use the Theme template that matches this VL version"

##### Theme Scope Model

VL supports a two-level theme scope model:

1. **Project Theme**
2. **App Theme**

The effective theme selection order is:

1. use the current App's own App Theme if present
2. otherwise fall back to the Project Theme
3. if neither exists, parser/runtime may apply the existing system fallback theme behavior

VL does **not** introduce page-level theme scope.

##### Theme Directory Convention

Theme files SHOULD stay under the project `Theme/` directory.

Recommended structure:

1. project-level theme:
   - `Theme/<ProjectThemeName>.vth`
2. app-level themes:
   - `Theme/Apps/<AppThemeName>.vth`

Theme file names do not define scope.
Theme scope is determined by directory position:

1. files directly under `Theme/` are project-level theme files
2. files under `Theme/Apps/` are app-level theme files

App-level theme files MUST declare their target App explicitly in `# Meta`:

```vl
# Meta
app:"SystemDocs"
mode:"light"
```

App-level theme binding is determined by this `app` meta field, not by file name.

##### Theme Uniqueness Rules

1. A project may declare **at most one** project-level theme file.
2. Each App may declare **at most one** app-level theme file.
3. App theme is optional.
4. If an App has no app-level theme file, it MUST inherit the project-level theme.
5. An app-level theme file without `# Meta app:"<AppName>"` is invalid.

Recommended `# Meta` example:

```vl
# Meta
mode:"light"
base_theme:"Platform/Theme-Default-Light@1"
```

##### Allow and Forbid

Allowed:
1. Define dimension point slot values (e.g., `intent.danger.intentBg:#F53F3F`)
2. Use cross-dimension references (`@dimension.slot`)
3. Define `affordance` point slot values (e.g., `affordance.listitem.hoverOverlay`, `affordance.navitem.focusRing`, `affordance.selectable.selectedBg`) when interaction-role visuals are needed.

Forbidden:
1. Define which style dimensions a component type supports
2. Define or modify state trigger behavior
3. Modify the platform-reserved dimension / point / slot set
4. Add a `# Overrides` section for instance-level styling

##### Value Constraints (Compile Validation)

1. `dimension` / `point` / `slot` must exist in the platform-defined legal set
2. `value` only allows: valid CSS value or cross-dimension reference (`@dimension.slot`)
3. Reference cycle → compile error
4. Missing reference → compile error

##### Overrides

Instance-specific visual changes MUST be authored on the target component itself:

- change reusable/default semantics via component `style`
- change component-local static or dynamic skin via component `sk.*`

---

#### 六、Component Skin Props (`sk.*`)

##### Positioning

The VL style system has three layers for skin:

| Layer | Name | Source | Binding Method | Priority |
|---|---|---|---|---|
| 1 (highest) | Component-local skin override | `sk.*` props | Static string literal or runtime expression | Highest |
| 2 | Static reusable skin | Theme + `style` coordinate | Compile-time static | Medium |
| 3 (lowest) | Platform baseline | Platform Baseline | Fixed | Lowest |

Skeleton properties (Skeleton) are generally not in the skin priority chain — skeleton and skin control different CSS property sets with no overlap. **Exception: the `size` dimension** provides theme-based defaults for a limited set of skeleton properties (`padding`, `font-size`, `font-weight`, `line-height`, `min-height`) on Button/Input/Textarea, and `gap` for Button child layout when Button renders UI children. When `size` and explicit skeleton properties target the same CSS property, the explicit skeleton declaration takes priority (see §size dimension).

##### Naming Convention

- Prefix: `sk.`
- Property name: platform-fixed semantic short name (not CSS original name)
- Examples: `sk.bg`, `sk.fg`, `sk.borderColor`

##### Complete Property Mapping (Platform-Fixed, Not Extensible)

| sk prop | Maps to CSS | Description |
|---|---|---|
| `sk.bg` | `background-color` | Background color |
| `sk.fg` | `color` | Foreground / text color |
| `sk.bgImage` | `background-image` | Background image / gradient |
| `sk.borderColor` | `border-color` | Border color |
| `sk.borderWidth` | `border-width` | Border width |
| `sk.borderTop` | `border-top` | Top border |
| `sk.borderRight` | `border-right` | Right border |
| `sk.borderLeft` | `border-left` | Left border (e.g., selected indicator) |
| `sk.borderBottom` | `border-bottom` | Bottom border (e.g., Tab active indicator) |
| `sk.shadow` | `box-shadow` | Shadow |
| `sk.opacity` | `opacity` | Opacity |
| `sk.radius` | `border-radius` | Border radius |

- Total 12 properties, platform-level fixed definitions.
- Developers cannot create custom `sk.*` property names; using undefined `sk.*` properties MUST trigger a compile error.
- The whitelist is fixed at 12 properties; allowing different literal forms does **not** expand the property set.

##### `sk.*` Override Semantics

`sk.*` is a standardized component-instance skin override channel.

It is used for:

- runtime dynamic skin changes
- precise visual restoration on a single instance
- local visual deviation without changing Theme defaults

`style` coordinates are constrained by the component's supported dimensions and the platform point set.
`sk.*` is **not** constrained by reusable Theme coordinate support tables. If the `sk.*` property name is valid and the value form is valid, it is accepted as a component-instance override.

In practical terms:

- `style` = reusable Theme semantics
- `sk.*` = local override

Tooling must not emit a warning solely because a valid `sk.*` property targets a CSS property outside that component's reusable Theme-coordinate behavior.

##### Value Constraint

The table below describes whether the `sk.*` value form itself is legal.

| Syntax | Compiler Judgment | Handling |
|---|---|---|
| `sk.bg:"#2563EB"` | Static string literal | ✅ Allowed |
| `sk.bgImage:"linear-gradient(135deg, #3b82f6, #8b5cf6)"` | Static string literal | ✅ Allowed |
| `sk.radius:"12px"` | Static string literal | ✅ Allowed |
| `sk.opacity:0.5` | Bare number literal | ✅ Allowed |
| `sk.bg:_item0.color` | Expression (contains variable reference) | ✅ Allowed |
| `sk.bg:(_item0.unread ? "#2563EB" : "transparent")` | Expression (contains operator) | ✅ Allowed |
| `sk.bg:($isActive ? "#2563EB" : null)` | Expression (variable + null) | ✅ Allowed |
| `sk.bg:true` | Bare boolean literal | ❌ Compile error |
| `sk.bg:null` | Bare null literal | ❌ Compile error (never overrides = not written) |

Compiler judgment rule:

- `sk.*` allows **static string literals** for component-local static skin exceptions
- `sk.*` allows **static number literals** where the target CSS property accepts numbers
- `sk.*` allows **expressions** whose runtime result is `string`, `number`, or `null`
- bare `boolean` / `null` literals remain forbidden

> Note: `null` as standalone literal is also forbidden (`sk.bg:null` is meaningless — never overriding equals not written). `null` is only meaningful inside ternary branches (e.g., `sk.bg:(cond ? "#EFF6FF" : null)`), where the whole expression is an expression, not a pure literal.

Example:

- `<Row-MailItem "mailItem"> style:"neutral|text|square|listitem" sk.bg:(_index0 === 0 ? "#EFF6FF" : null) sk.borderLeft:(_index0 === 0 ? "3px solid #2563EB" : "3px solid transparent") padding:"12px 16px" gap:"10px"`
- `<Row-Card "card"> style:"neutral|solid|default|passive" sk.borderTop:($isPinned ? "2px solid #F59E0B" : null) sk.borderRight:($hasAlert ? "1px solid #EF4444" : null)`

##### `null` Semantics

When a `sk.*` expression runtime value is `null`:

- No corresponding inline style is generated
- Falls back to Theme static style value (i.e., the CSS class value from `style` coordinate takes effect)

This allows developers to conditionally override Theme:

```
sk.bg:(_index0 === 0 ? "#EFF6FF" : null)
// _index0 === 0: inline background-color:#EFF6FF overrides theme
// _index0 !== 0: null → no inline style → theme value takes effect
```

##### CSS Output

`sk.*` values go through slot override → consumption routing, landing as inline style on the target DOM element:

```
// Theme CSS class (generated at compile time)
.MailItem--neutral-text-square {
  background-color: transparent;
}

// sk.* landing (runtime calculation → slot override → consumption routing → target element inline style)
// When condition is true: target element style="background-color: #EFF6FF"
// When condition is false (null): no output → theme class value takes effect
```

##### CSS Skin Property Names Forbidden in Skeleton

When developers write CSS skin property names directly on components (without `sk.` prefix):

| Syntax | Handling |
|---|---|
| `background-color:_item0.color` | ❌ Compile error, suggest using `sk.bg` |
| `color:(_index0 === 0 ? "#2563EB" : "#64748B")` | ❌ Compile error, suggest using `sk.fg` |
| `border-left:"3px solid #2563EB"` | ❌ Compile error, suggest using `sk.borderLeft` |

Skin property name blocklist (compiler intercepts): `background-color`, `background-image`, `background`, `color` (when used for text color), `border`, `border-top`, `border-right`, `border-bottom`, `border-left`, `border-color`, `border-width`, `border-style`, `box-shadow`, `opacity`, `border-radius`, `text-transform`, `cursor`.

##### Relationship with Theme Coordinates

Theme coordinates remain the reusable/default path. `sk.*` is the component-local exception path. New code should not rely on Theme `# Overrides`; instance-specific visual changes belong on the target component itself.

##### Typical Usage Patterns

**Tag color dot:**

`<Row-LabelDot "labelDot"> style:"filled|pill|passive" sk.bg:_item0.color width:"8px" height:"8px"`

**Unread dot:**

`<Row-UnreadDot "unreadDot"> style:"primary|filled|pill|passive" sk.bg:(_item0.unread ? null : "transparent") width:"8px" height:"8px"`

**Mail list selected item (hover + custom background coexistence):**

`<Row-MailItem "mailItem"> style:"neutral|text|square|listitem" sk.bg:(_index0 === 0 ? "#EFF6FF" : null) sk.borderLeft:(_index0 === 0 ? "3px solid #2563EB" : "3px solid transparent") padding:"12px 16px" gap:"10px"`

All items use `style:"neutral|text|square|listitem"` uniformly for theme hover effect; selected item dynamically overrides background via `sk.bg`; unselected items `sk.bg` is `null`, falling back to theme's transparent.

**Nav icon selected color:**

`<Text-NavIcon "navIcon"> value:_item0.icon style:"caption" sk.fg:(_index0 === 0 ? "#2563EB" : null) font-size:"20px"`

---

#### 七、Style Coordinate Quick Reference

This section is an authoritative developer-facing reference for style coordinates. Developers and AI tools can rely on it directly.

##### Component × Dimension Support Table

Each token in a `style` coordinate belongs to a "dimension". Different component types support different dimension combinations:

| Component Type | intent | emphasis | shape | surface | textRole | size | affordance | Note |
|---|---|---|---|---|---|---|---|---|
| **Button** | ✅ | ✅ required | ✅ | ✅ | | ✅ | ✅ optional, defaults to `actionable` | Interactive button; first-release affordance template |
| **Input / Textarea** | ✅ | ✅ required | ✅ | | | ✅ | | Form input |
| **Text** | ✅ | ✅ | ✅ | | ✅ | | passive no-op | Text; shape provides border-radius (rounded text label) |
| **Icon** | ✅ | ✅ | ✅ | | ✅ | | passive no-op | Icon; supports shape for rounded icon badge/chip scenarios |
| **Row / Col / Grid** | ✅ | ✅ | ✅ | ✅ | | | ✅ optional, defaults to `passive` | Flex/grid layout container; first-release affordance templates |
| **Image / Video** | | | ✅ | | | | | Shape only (border-radius) |
| **Divider** | | | | ✅ required | | | | Divider, must declare surface |
| **Page** | | | | ✅ | | | | Page background |
| **Modal** | | | ✅ | ✅ required | | | | Dialog, must declare surface |

Using an unsupported dimension point is invalid and triggers compile/lint diagnostics. Example: `<Icon> style:"primary|filled|solid"` — `solid` belongs to surface dimension, Icon doesn't support surface.

For first-release affordance templates (`Block`, `Row`, `Col`, `Grid`, `Button`, `ButtonContainer`):

1. If `style` includes an `affordance` point, it must include exactly one legal `affordance` point.
2. If `style` omits `affordance`, parser uses the current component default and no lint diagnostic is produced.
3. Legal `affordance` points: `passive`, `listitem`, `navitem`, `actionable`, `selectable`.

Non-applicable components: `Input`, `Textarea`, `Text`, `Icon` and other components not in the first-release affordance template set do NOT require `affordance`. For `Text` and `Icon`, omitted `affordance` is equivalent to passive no-op; explicit `passive` is accepted and has no visual or interaction effect.

##### Coordinate → Visual Effect Quick Reference

**intent (color semantics):**

| Point | Meaning | Typical Color Family |
|---|---|---|
| `primary` | Primary action / brand | Blue family |
| `neutral` | Neutral / default | Gray family |
| `success` | Success / confirm | Green family |
| `warning` | Warning / caution | Amber family |
| `danger` | Danger / error / delete | Red family |
| `info` | Information / hint | Light blue family |
| `inverse` | Inverted / dark background | Dark family |

**emphasis (visual emphasis mode):**

| Point | Background | Border | Content Color | Use Case |
|---|---|---|---|---|
| `filled` | Intent solid color | None | White | Primary action button, emphasis card |
| `outlined` | Transparent | Intent color border | Intent color | Secondary action button, form input |
| `tonal` | Intent light color | None | Intent color | Soft emphasis, label, chip |
| `ghost` | Transparent | None | Intent color | Toolbar button, minimal interaction |
| `text` | Transparent | None | Intent color | Pure text link, text button |

> Note: The same coordinate may produce **different background intensity** on different component types. For example, `primary|filled` produces **solid blue background** (#2563EB) on Button, and also solid blue background on Row/Col/Grid containers. But if a container only declares intent without emphasis (e.g., `style:"primary"`), the container gets a **light background** (intentSubtleBg), not solid color.

**shape (border-radius):**

| Point | border-radius | Use Case |
|---|---|---|
| `default` | 8px | General button, card |
| `pill` | 9999px | Capsule button, badge, circular avatar |
| `square` | 0px | Tab button, table, no-radius element |
| `soft` | 12px | Large card, dialog |
| `sharp` | 4px | Compact element, label |

**surface (container backdrop):**

| Point | Background | Border | Shadow | Use Case |
|---|---|---|---|---|
| `solid` | Pure white | Yes | None | Card, panel |
| `subtle` | Light gray | None | None | Area separator, background layer |
| `elevated` | White | None | Has shadow | Floating card, dropdown menu |
| `overlay` | Semi-transparent | None | Has shadow | Dialog mask layer |
| `dark` | Dark | Yes | None | Dark area, footer |

**textRole (text hierarchy):**

| Point | Meaning | Typical Effect |
|---|---|---|
| `body` | Body text | Default text color, 100% opacity |
| `caption` | Auxiliary text | Gray text |
| `hint` | Placeholder hint | Lighter gray |
| `muted` | Weakened text | Reduced opacity |
| `weak` | Weakest text | Lowest opacity |
| `contrast` | White text on dark background | White / light color |

##### Multi-Source Composition Strategy

When a component declares multiple dimensions and those dimensions contribute to the same CSS property, composition is needed. Composition strategy is **per CSS property**:

| CSS Property | Strategy | Meaning |
|---|---|---|
| `background-color` | lastWins | Later-declared dimension overrides the former |
| `border-color` | lastWins | Later-declared dimension overrides the former |
| `color` | lastWins | Later-declared dimension overrides the former |
| `background-image` | lastWins | Later-declared dimension overrides the former |
| `border-width` | lastWins | Later-declared dimension overrides the former |
| `border-radius` | lastWins | Later-declared dimension overrides the former |
| `opacity` | lastWins | Later-declared dimension overrides the former |
| `box-shadow` | comma-stack | Multiple shadow layers comma-joined, coexist and stack |

Most properties use lastWins (later overrides earlier); only `box-shadow` uses comma-stack (multi-layer stacking). Developers only need to know: **when a container has both emphasis and surface, background-color is determined by surface (because surface is parsed after emphasis)**.

> Note: This table gives the author-facing composition conclusion only. Lower-level platform implementation details are not part of the VL authoring contract.

##### Size Dimension

The `size` dimension provides standardized sizing for interactive components (Button, Input, Textarea) via Theme.

**Points:** `sm`, `md`, `lg`

**Slots:**

| Slot | CSS Property | Description |
|---|---|---|
| `sizePaddingY` | `padding-top` / `padding-bottom` | Vertical padding |
| `sizePaddingX` | `padding-left` / `padding-right` | Horizontal padding |
| `sizeFontSize` | `font-size` | Font size |
| `sizeFontWeight` | `font-weight` | Font weight |
| `sizeLineHeight` | `line-height` | Line height |
| `sizeMinHeight` | `min-height` | Minimum height |
| `sizeGap` | `gap` | Default gap between Button UI children when Button renders child components |

**Padding synthesis rules:**
1. Both `sizePaddingY` and `sizePaddingX` non-empty → `padding: {Y} {X}`
2. Only `sizePaddingY` non-empty → `padding-top: {Y}; padding-bottom: {Y}`
3. Only `sizePaddingX` non-empty → `padding-left: {X}; padding-right: {X}`
4. Both empty → no padding output

**Priority: explicit skeleton > size theme > browser default.** When a developer declares `padding`, `font-size`, etc. directly on the component, those explicit skeleton properties always override `size` dimension theme values. `padding` overrides all size padding; `padding-top` overrides only the top direction.

**Button child layout rule:** When Button renders UI child components and no `value`, the children are laid out in a centered horizontal row. The child-to-child gap uses `gap:size.sizeGap`; if Button omits the `size` dimension, the platform fallback is `gap:8px`.

**`size` is optional.** Button/Input/Textarea can omit `size` and specify skeleton properties manually. However, always declaring `size` is recommended for global consistency.

**Generator default rule:** When AI/tools generate a new Button / Input / Textarea instance, they SHOULD declare an explicit `size` point (`sm` / `md` / `lg`) by default. Omitting `size` is a deliberate manual-sizing mode, not the preferred generated default.

**Style coordinate examples:**
```
// Full: intent + emphasis + shape + size + affordance
<Button-Submit "submitBtn"> value:"Submit" style:"primary|filled|default|md|actionable"

// Size with other dimensions
<Button-Cancel "cancelBtn"> value:"Cancel" style:"neutral|outlined|default|sm|actionable"
<Input-Search "searchInput"> style:"neutral|outlined|default|md" placeholder:"Search..."

// No size — manual sizing (has padding + font-size, no BT-003 warning)
<Button-Custom "customBtn"> value:"Go" style:"primary|filled|pill|actionable" padding:"6px 20px" font-size:"15px"

// No size, no padding, no font-size — triggers BT-003 warning
<Button-Bare "bareBtn"> value:"Go" style:"primary|filled|pill|actionable"

// Size with explicit override (skeleton takes priority)
<Button-Special "specialBtn"> value:"Go" style:"primary|filled|default|md|actionable" font-size:"18px"
// → font-size uses 18px (explicit override), padding/font-weight/line-height/min-height use md theme values
```

##### Component Type Differences Reminder

**Container vs Atomic component key differences:**

1. **Containers (Row/Col/Grid) using emphasis only affect their own background, border, and shadow**. Container emphasis does not affect child element text colors or decoration. Child elements must independently declare their own `style` coordinates.

2. **Containers support both emphasis and surface (two background-related dimensions)**. When both are declared, surface takes priority (overrides emphasis background). Usually choose one:
   - Need "emphasis color background" (e.g., solid blue card) → use emphasis (e.g., `primary|filled`)
   - Need "container backdrop" (e.g., white card, light gray area) → use surface (e.g., `solid`, `subtle`)

3. **Input / Text / Icon do not support surface dimension.** Button supports `surface` in VL 4.2.5; input and text-like atomic components still do not.

4. **Text and Icon both support shape dimension** (provides border-radius for rounded label/badge/icon scenarios). If rounded icon container also needs background/border/shadow, prefer declaring those visuals on an outer Row/Col container.

---

#### 八、Compile Checks Summary

| # | Check | Level |
|---|-------|-------|
| 1 | `style` point name not in any dimension point set | warning |
| 2 | `style` same dimension multiple points conflict | warning |
| 3 | `style` duplicate point | warning |
| 5 | `state` point appears in `style` | warning |
| 6 | Component `style` declares a dimension outside that component's supported static style-dimension set (note: `size` is supported on Button/Input/Textarea) | warning |
| 7 | Theme `dimension` / `point` / `slot` invalid | error |
| 8 | Cross-dimension reference cycle | error |
| 9 | Cross-dimension reference missing | error |
| 10 | `emphasisBorderWidth` is not a CSS length value | error |
| 11 | CSS property with all-empty sources outputs empty declaration | error |
| 12 | Platform static dimension point names are not globally unique | error |
| 13 | `textRole` unknown point name | error |
| 14 | `textRole` declared on a component that does not enable this dimension | error |
| 15 | `stateTransitionDuration` is not a valid CSS time value (e.g., `100ms`/`0.2s`) | error |
| 16 | `stateTransitionTiming` is not a valid timing function (`ease`/`linear`/`cubic-bezier(...)` etc.) | error |
| 17 | `stateTransitionProperty` is not a valid CSS property list or `all`/`none` | error |
| 18 | `textRoleAlpha` is not a valid CSS opacity value (0–1 number) | error |
| 19 | `surfaceBgImage` is not a valid CSS `background-image` value (gradient, `url(...)`, or cross-ref) | error |
| 20 | `surfaceBgRepeat`/`surfaceBgPosition`/`surfaceBgSize` is not a valid value for the corresponding CSS property | error |
| 21 | `surfaceAccentBorderStart`/`End` is not a valid border value (width+style+color or equivalent) | error |
| 22 | `surfaceDividerTop`/`Bottom` is not a valid border value | error |
| 23 | `surfaceSideRailWidth` is not a valid CSS length; `surfaceSideRailBg` is not a valid color/gradient | error |
| 24 | `surfaceBorderStyle` is not a valid CSS `border-style` value (`solid`/`dashed`/`dotted`/`double`/`none` etc.) | error |
| 25 | `surfaceBorderWidth` is not a valid CSS `border-width` value (length or `thin`/`medium`/`thick`) | error |
| 26 | `textRoleTransform` is not a valid CSS `text-transform` value (`none`/`uppercase`/`lowercase`/`capitalize` etc.) | error |
| 27 | `font-size`/`font-weight`/`line-height`/`transform` in VL code are skeleton properties — compiler must **not** report error. When `size` dimension provides these via theme, explicit skeleton declarations take priority; `size` only serves as theme default | allowed |
| 28 | Theme (`.vth`) or VL code declares platform baseline hard-locked property (`box-sizing`/`font-synthesis`) | error |
| 29 | Theme uses `# Coordinate Values` as point slot values section heading (must use `# Point Slot Values`) | error |
| 30 | SC/CP code contains border literal properties (`border`, `border-top`, `border-bottom`, `border-left`, `border-right`, `border-color`, `border-style`, `border-width`) outside style-space resolution. In VL 4.2.5 this is also covered by the closed CSS-property whitelist / skin blocklist rules. | error |
| 31 | SC/CP code uses `height:Npx` (N <= 3) + `background-color` on empty Row/Col to simulate divider | warning |
| 32 | `StateStyle` uses `trigger:"hover|active|focus|disabled|invalid"` (interaction states should be expressed via the runtime-state model, not static `style` or component-local `StateStyle trigger`) | warning |
| 33 | `StateStyle` mixes `trigger` and `conditions` in one node | warning |
| 34 | `StateStyle conditions` writes skin literal properties (`color/background*/border*/box-shadow/opacity/text-transform`) | warning |
| 35 (SK-001) | `sk.*` property name not in the sk.* mapping table (§Component Skin Props) | error |
| 36 (SK-002) | `sk.*` value is a bare boolean / null literal | error |
| 37 (SK-003) | CSS skin property name directly used on component (in the skin blocklist) — suggest using corresponding `sk.*` prop or Theme style coordinate | error |
| 38 (SK-004) | `sk.*` value runtime type is not string / number / null (runtime check, not compile-time static) | runtime warning |
| 41 (SS-001) | Container style coordinate contains both emphasis point and surface point simultaneously | warning |
| 42 (BLK-001) | `Block` component type used (use Row or Col when flex layout is required) | error |
| 43 (BT-001) | Button declares both `value` and UI child components (`value` takes priority, children ignored) | warning |
| 44 (BT-002) | `ButtonContainer` component type used (use Button with children) | error |
| 45 (BT-003) | Button/Input/Textarea without `size` dimension and without explicit `padding` + `font-size` | warning |
| 46 (SZ-001) | `size` dimension point not in [sm, md, lg] | error |
| 47 (SZ-002) | `size` dimension declared on unsupported component (only Button/Input/Textarea) | error |
| 48 (THM-OVR-001) | Theme file contains `# Overrides` section | warning |
| 49 (STY-001) | `style` value is not a static string literal | warning |
| 50 (OVF-001) | Scroll container (`overflow:"auto"` / `overflow:"scroll"`) has no stable size carrier (no explicit size, no `flex:"1"` with stable parent, no resolvable percentage size) | error |
| 51 (SCR-001) | Repeated list item inside a scroll container uses same-axis `flex:"1"` (attempting to absorb overflow via compression) | error |
| 52 (US-001) | `UserStore` declared outside `# Backend Tree` | error |
| 53 (US-002) | `UserStore` missing `sourceTable` or `identitiesTable` declaration | error |
| 54 (US-003) | `UserStore.sourceTable` value is not `AuthUsers` | error |
| 55 (US-004) | More than one `UserStore` instance in a project | error |
| 56 (US-005) | Project declares `UserStore` but `.vdb` does not declare `Table-AuthUsers` | error |
| 57 (US-006) | Project declares `UserStore` but `.vdb` does not declare `Table-AuthUserIdentities` | error |
| 58 (AU-001) | `Table-AuthUsers` missing required user principal field declarations | error |
| 59 (AU-002) | `Table-AuthUserIdentities` missing required identity credential field declarations | error |
| 60 (PP-001) | Parent prop passing uses `$propName:` syntax (attribute name must not have `$` prefix) | error |
| 61 (VT-001) | `VirtualTable` referenced using shorthand `<id>` instead of full `<VirtualTable-Name "id">` form | error |
| 62 (AF-001) | First-release affordance template (`Block`/`Row`/`Col`/`Grid`/`Button`/`ButtonContainer`) declares `style` without an `affordance` point | error |

---

**Style Authoring Rules (Strict):**

- VL expressions can be used in **skeleton** layout properties, e.g.: `<Row-ProgressBar> width:("" + $progress*300 +"px")`. Note: VL expressions are only supported in regular component instances; **`StateStyle` widgets are for static style configuration and do not allow VL expressions**
- `style` MUST be a static string literal; runtime expressions, variables, or function calls are forbidden.
- VL expressions can be used in **`sk.*` component skin props** for runtime dynamic skin override (see §Component Skin Props). `sk.*` values may be static string literals, static number literals, or expressions; bare `boolean` / `null` literals are forbidden.
- Skin properties MUST NOT be written using CSS original names in `.sc`/`.vx` code. Component-local skin needs MUST use `sk.*` prefix props. Reusable static skin needs MUST use Theme `style` coordinate.
- `StateStyle` is not the preferred path for dynamic skin values, but a `StateStyle` node may still use legal `sk.*` props as a local escape hatch.
- **Strictly forbidden to directly access/modify component styles in events/methods**

```
# ❌ Wrong
-<Text-Title "title">.color = "blue"
-_textColor = <Text-Title "title">.color

# ✅ Correct — bind to variable in tree, modify a supported declarative prop via variable
<Text-Title "title"> value:"Hello" width:$titleWidth
-$titleWidth = "220px"
```

#### Compound Property Usage Rules

- **Allowed compound value properties (keep complete)**: `box-shadow`, `transform`, `clip-path`, `filter`, `text-shadow`, and `background-image` (for gradients) and `grid-template-columns/rows/areas` property values are inherently compound structures and **must** be kept in complete string form.
  - `box-shadow:0 8px 25px rgba(0,0,0,0.15)`
  - `transform:translateY(-2px) scale(1.02)`
  - `filter:blur(5px) brightness(0.8)`
  - `grid-template-columns:repeat(3, 1fr)`
- **Forbidden shorthand properties (must be split)**:
  - `background` (should be split into `background-color`, `background-image`, `background-repeat`, etc.)
  - `border` (should be split into `border-width`, `border-style`, `border-color`)
  - `font` (should be split into `font-size`, `font-family`, `font-weight`, etc.)
  - `transition` (should be split into `transition-property`, `transition-duration`, etc.)
  - `flex` (should be split into `flex-grow`, `flex-shrink`, `flex-basis`)
  - And other similar bundled shorthands.
- **Allowed multi-direction shorthands (follow "either-or" rule)**:
  - `padding`, `margin`, `overflow`, `gap`
  - **"Either-or" principle**: If using these shorthand forms (like `padding:"10px"` or `padding:"10px 20px"`), **forbidden** to also use corresponding single-side properties (like `padding-left`) in same style rule set.
- **Compound Property Usage Example:**

```
// Wrong (forbidden shorthand)
<Row-Card> border:1px solid #eee
// Wrong (split border literals are also forbidden in VL code)
<Row-Card> border-width:1px border-style:solid border-color:#eee
// Correct
<Row-Card> style:"subtle|passive"
-<Divider-Rule> style:"subtle"

// Text hierarchy uses textRole.muted instead of textRole.subtle
<Text-Meta> style:"muted"
```

#### **CSS Properties Forbidden in VL**:

- **Media queries (strictly forbidden)**: **Strictly forbidden** to use `@media` queries in any style definitions. VL's design philosophy is to create independent applications for each target device (PC, Phone, Pad), rather than responsive adaptation within a single application.
- **Animation keyframes**: Only **@keyframes** `animations` and **animation** `property` are not supported in VL syntax; other CSS styles are all supported.
- **display**: `display` property is entirely managed internally by components; not allowed to directly write `display:xxx`

### `<StateStyle>` Conditional Style Widget

Conditional style widget, through its Conditions and Trigger properties, can clearly specify a UI component's styles in different scenarios. Note: Conditional style widget's style definitions only support literal values and calc expressions; VL expressions are strictly restricted.

#### Components That Cannot Have Styles

The following components are pure logic containers with no DOM elements. Therefore, no visual style, `sk.*`, skeleton CSS, or event definitions can be added to these components. Their own control props, when supported, are still written after the closing `>` according to the universal component reference rule. Consider these components "transparent" in layout, not participating in UI element layout at all. These components include: `If`, `For`, `TreeFor`, `AnimationGroup` (and other similar pure logic loop or conditional rendering components).

## Database File Rules

### Table Definition

**Example**

```vl
<Table-Users> data:[{"_id":1,"username":"user1","email":"user1@example.com","role":"Admin","status":1,"_user":"1","_create":"2024-01-15 09:30:00","_update":"2024-01-15 09:30:00"}...]
-<Field-username> type:STRING
-<Field-email> type:STRING
-<Field-role> type:STRING
-<Field-status> type:INT
```

**Rules**

- Table names use **PascalCase**, field names use **camelCase**
- `default` supports literals: strings need quotes (like `default:""`), boolean (`true|false`), numbers, time strings, etc.
- Following system fields are **automatically added during system table creation**, **forbidden to explicitly declare equivalent fields**:
  - `_id (INT)`: Auto-increment primary key, starting from 1
  - `_user (STRING)`: Records submitting user's ID
  - `_create (TIMESTAMP)`: Creation time
  - `_update (TIMESTAMP)`: Update time
- `vecSource` is vector type field specific property, specifying vector field's source data fields.
- `Index` is optional child component; index field order takes effect.
- `data` is optional property; used to initialize some test data. Initialize 3-7 rows of data based on actual requirements. Unlike normal insert/update in services, test data needs to declare system fields for testing convenience:
  - `_id` field: Required, please start from 1;
  - `_user` field: Required, if no special requirements, please fix as "1";
  - `_create`/`_update` fields: Required, if no special requirements, please fix as current time

### Special System Authentication Tables: `Table-AuthUsers` and `Table-AuthUserIdentities`

When a project uses `UserStore`, the `.vdb` file MUST declare both `Table-AuthUsers` and `Table-AuthUserIdentities`. These are authentication-specific tables, not regular business tables.

Their purpose is:

- To provide a project-level user store anchor for `UserStore`
- To ensure the project contains a complete set of official user identity source resources that can be uniquely bound by `UserStore`
- `Table-AuthUsers` stores user principal data
- `Table-AuthUserIdentities` stores identity credentials (password hash, OAuth bindings, etc.)

**DB definition layer declaration:**

```vl
<Database-Db>

<Table-AuthUsers>
-<Field-nickname> type:STRING
-<Field-display_name> type:STRING
-<Field-avatar> type:STRING
-<Field-status> type:STRING
-<Field-primary_email> type:STRING
-<Field-primary_phone> type:STRING
-<Field-last_login_at> type:TIMESTAMP
-<Field-extra_json> type:JSON

<Table-AuthUserIdentities>
-<Field-user_id> type:STRING notNull:true
-<Field-identity_type> type:STRING notNull:true
-<Field-identifier> type:STRING notNull:true
-<Field-identifier_norm> type:STRING
-<Field-credential_hash> type:STRING
-<Field-provider> type:STRING
-<Field-provider_user_id> type:STRING
-<Field-provider_union_id> type:STRING
-<Field-verified_at> type:TIMESTAMP
-<Field-is_primary> type:BOOL
-<Field-extra_json> type:JSON

</Database-Db>
```

Rules:

1. `Table-AuthUsers` MUST declare user principal fields (at minimum: `nickname`, `display_name`, `avatar`, `status`, `primary_email`, `primary_phone`, `last_login_at`, `extra_json`)
2. `Table-AuthUserIdentities` MUST declare identity credential fields (at minimum: `user_id`, `identity_type`, `identifier`, `identifier_norm`, `credential_hash`, `provider`, `provider_user_id`, `provider_union_id`, `verified_at`, `is_primary`, `extra_json`)
3. Both tables are authentication-specific resources and must not be repurposed as arbitrary business tables
4. A project using `UserStore` MUST declare both tables; declaring only one is invalid
5. `AuthUsers` and `AuthUserIdentities` are fixed logical resource names bound by `UserStore`

### Relation Definition

**Format**

```
<Relation-RelationName relation1 relation2 ..>
```

**Examples**

```
<Relation-UserProfile> users._id--userProfiles.userId // One-to-one single relation
<Relation-UserPosts> users._id<<posts.authorId  // One-to-many, single relation
<Relation-User&Messages> users._id<<messages.senderId users._id<<messages.receiverId // One-to-many, multiple relations
<Relation-Departments&Employees> departments._id<<employees.departmentId departments.managerId>>employees._id // One-to-many + many-to-one dual relations
<Relation-Warehouse&Inventory> warehouses.(code,region)<<inventory.(warehouseCode,warehouseRegion) // One-to-many, single relation composite key
```

- A Relation describes one or more relationships between two tables. The name after `Relation-` is a relation identifier; it is recommended to use a clear alias such as `<Relation-MaterialStock>`, and `<Relation-TableName1&TableName2>` remains compatible but is not required.
- Two tables may have multiple relations
- Each relation contains Table1's field, relation symbol, and Table2's field
- Relation symbols use `--`, `<<`, and `>>` representing one-to-one, one-to-many, and many-to-one respectively
- A relation may contain composite keys. Note: Composite key's multiple fields actually represent one pointer, so use `Table1.(compositeKeyField1,compositeKeyField2...)` format; do not write as two relations.

## App (.vx) Application File Writing Supplement

### `#SysConfig` (Application System Configuration)

`.vx` application files must declare system configuration parameters in `#SysConfig` section:

```vl
# SysConfig

DEVICE_TARGET:"PC"
SCREEN_RESOLUTION:"1920x1080"
```

**Required Configuration Items:**

- **DEVICE_TARGET**: Target device type, options: "PC", "Phone", "Pad"
- **SCREEN_RESOLUTION**: Target screen resolution

## Section/Component (.sc/.cp) File Writing Supplement

### Root Container

In `.sc/.cp`, `<Section-Name "root">` / `<Component-Name "root">` is the real internal root container.

Rules:

1. `containerType` is only valid on `.sc/.cp` root
2. Allowed values are `col`, `row`, and `grid`
3. Default is `containerType:col`
4. Root may contain multiple direct children through `# Frontend Tree`
5. Root properties describe the internal root container, not the host instance's external layout
6. Compiler/runtime must preserve this root as a real container rather than removing or replacing it with a proxy wrapper
7. Root follows the same skin-entry rules as normal `Row / Col / Grid`: use Theme `style` coordinate or legal `sk.*` props; raw CSS skin names remain forbidden

### Internal Scroll Area Recommended Pattern

When a Section/Component contains an internal scroll list area, the recommended structure is:

1. The internal root takes responsibility for receiving the host-provided size
2. The scroll area node handles `overflow:"auto"`
3. List items maintain their own basic readable height

Recommended structure:

```vl
<Col-Root> flex:"1" min-height:"0" overflow:"hidden"
```

```vl
-<Col-List> flex:"1" min-height:"0" overflow:"auto"
```

```vl
--<For-Items> ...
```

```vl
---<Col-Item> min-height:"84px" ...
```

If at runtime the `flex:"1"` height carrier chain remains unstable, explicit size carrier may be used as a transitional approach, for example:

```vl
<Col-List> height:"calc(100vh - 148px)" overflow:"auto"
```

Note: Explicit `calc(...)` expressions are acceptable as an engineering transitional approach. The long-term mainline should prefer returning to a more semantic size carrier chain.

### `# Frontend Public Props` (Public Property Definition)

Use frontend global variables (`$`) to define public property variables. These variables are used within the file the same way as regular global variables, but during application compilation, public property variable values can be passed in from upper-level files (App or Section).

**Definition Method:**

```vl
# Frontend Public Props
$propertyName(Type) = initialValue
```

**Example:**

```vl
# Frontend Public Props
$initialConfig({primaryColor:STRING,fontSize:INT}) = {primaryColor:"#1890ff",fontSize:14}
$initialUserId(INT) = 0
```

**Parent prop passing rule:** When a parent file passes values to a `Section` / `Component` instance, the attribute name MUST be a bare property name without the `$` prefix. The `$` prefix is only used in the declaration (`$propName(TYPE) = default`) and in expressions that reference the variable. Example: `<Component-Card "card"> product:_item0` (correct), NOT `<Component-Card "card"> $product:_item0` (syntax error).

Public prop names must stay out of the reserved style and skin namespace. Do not define custom public props named after CSS properties, VL structure attributes, `style`, or `sk.*` props, including camelCase and snake_case aliases. For example, `$backgroundColor`, `$width`, `$style`, and `$skBg` are invalid public prop names; use semantic API names such as `$accentColor`, `$chartPalette`, `$contentMaxWidth`, or `$panelVariant` instead.

### `# Frontend Tree`

`# Frontend Tree` is the direct textual representation of `root.children`.

Rules:

1. `.sc/.cp` root is a real internal container
2. `# Frontend Tree` may contain multiple direct children
3. The system must not auto-wrap multiple first-level nodes into a proxy root
4. The system must not collapse the real root just because there is only one first-level child
5. Absolutely forbidden to add another Section inside Section, meaning Section's Frontend Tree will NOT have statements like `<Section-ModuleName>...`

### `# Frontend Public Events` (Frontend Public Event Definition)

In .sc/.cp files, public events can be defined, i.e., events sent from module internals to outside. These events can be listened to in application layer (.vx files) to complete inter-module/component interaction. Definition has two parts:

1. First in `# Frontend Public Events` section, top level, no indentation, use `@` symbol to define event method name:

**Definition Format:**

```
EVENT @eventName(param1(Type1),param2(Type2),...)
```

- **Parameter naming**: All parameters use **camelCase**, **without `_` prefix**.
- **Compatibility**: Inside `# Frontend Public Events` only, parser/lint also accepts shorthand declarations without the `EVENT` keyword, and accepts either `param(Type)` or `param:TYPE` parameter syntax. Example: `@sectionSelected(sectionKey:STRING)` is equivalent to `EVENT @sectionSelected(sectionKey(STRING))`. The normalized/main emitted form remains `EVENT @eventName(param(Type))`.

**Example:**

```vl
EVENT @userSelected(userId(INT),userName(STRING))
EVENT @formSubmitted(formData({id:INT,timestamp:TIMESTAMP}))
EVENT @requestLogin()
```

2. Then inside .sc/.cp file's program logic (usually in event handlers or methods), define trigger logic:

```vl
<Button-Submit "submitButton">.@click()
-validateForm() -> _validation
-IF _validation.success
--@formSubmitted($formData)
-ELSE
--<SysUI>.showToast(_validation.message, "error")
```

### `# Frontend Public Methods` (Frontend Public Method Definition)

In .sc/.cp files, a method defined under `# Frontend Public Methods` is a public method.

These methods can be called from upper-level application files (.vx/.sc).

Public methods can also be called internally within the file; calling method is identical to internal methods:

```
# Frontend Public Methods

METHOD Hide();RETURN {success:BOOL}
-$isVisible = false
-@modalClosed()
-RETURN {success:true}

# Frontend Event Handlers
<Button-Close "closeButton">.@click()
-Hide() // Call method directly, don't mistake public method as root component's method
```

Note: When calling public methods internally within file, do not mistake public methods as file root component's methods. For example inside Section, **there is no call method like `<Section-MyModule>.Hide()`**.

Generated VL code for this specification must use `METHOD` only in `# Frontend Public Methods`.

### Calling Services in Section

Section files can call internal services (`SERVICE`) defined in ServiceDomain files under the same project.

- `SERVICE`: callable from Section.
- `PUBLIC_SERVICE`: external HTTP entry only; Section cannot call it directly.
- If internal/external logic needs reuse, extract shared logic into backend `METHOD`, then call it from both `SERVICE` and `PUBLIC_SERVICE`.

Calling method:

1. Add needed ServiceDomain in component tree as a functional component:

```
# Frontend Tree

<ServiceDomain-DomainName "serviceId"> // First introduce service domain component
-<Service-ServiceName1> params:(param1(Type),param2(Type)...) returns:(output1(Type),output2(Type)...) // Introduce needed internal service as component property
-<Service-ServiceName2> ... // Another internal service under service domain
```

2. In Section's event handling or methods, call internal services defined in this ServiceDomain:

```
<ServiceDomain-DomainName "serviceId">.ServiceName()
```

3. Complete Example:

```
# Frontend Tree
<ServiceDomain-Doc "docService"> // First introduce service domain component
-<Service-DeleteDocument> params:(docId(INT),deleteType(BOOL)) returns:(success(BOOL),message(STRING)) // Introduce needed service with params and returns as component properties
...

<Button-Logout "logoutButton">.@click()
-<ServiceDomain-Doc "docService">.DeleteDocument() -> _out
-<ClientUtils>.switchRoute("home")
```

## ServiceDomain (.vs) Definition Supplement

### Virtual Table Definition

Virtual tables are "intermediate clients" for backend service logic to operate real database tables. VL follows the principle of separating code logic from data source definitions; service code cannot directly operate database tables, must use VirtualTable as bridge. Use sourceField property in virtual table to specify virtual table-entity table correspondence.

**Format:**

```
<VirtualTable-TableName "componentId"> extraSpecs?:{specName1(Type):"propertyDescription"..} sourceTable:correspondingEntityMainTable
-<Field-fieldName> type:FieldType vecSource?:["virtualTableField1","virtualTableField2"] sourceField?:correspondingEntityTableField additionalProp1:value.. // Field as VirtualTable child, must have extra indentation
```

**Example:**

```vl
<VirtualTable-Users "userTable"> sourceTable:GroupUsers extraSpecs:{label(STRING):"Field Label",readOnly(BOOL):"Read Only",inputType(STRING):"Input Type"} mockData:[{..}]
-<Field-_id> type:INT label:"User ID" readOnly:true
-<Field-username> type:STRING label:"Username" inputType:"text" // No sourceField means direct association with same-named field in main table
-<Field-email> type:STRING sourceField:userEmail label:"Email" inputType:"email" // Specify sourceField to adapt when data source field differs from virtual table field name
-<Field-departmentName> type:STRING sourceField:departmentId~Departments[_id].name label:"Department Name" readOnly:true // Found via relation
-<Field-managerName> type:STRING sourceField:departmentId~Departments[_id].managerId~Employees[_id].name label:"Manager Name" readOnly:true
-<Field-_create> type:TIMESTAMP label:"Creation Time" readOnly:true // System field, needs label set in current virtual table
```

**VirtualTable Declaration**

- A VirtualTable defines a virtual table in a service domain; table names should follow VL style using **PascalCase**, e.g., `UserMessage`
- `id`: Required, please name as "xxTable", where xx is the table's independent unique name
- `sourceTable`: Bound entity table; a virtual table can only bind one entity table (main table)
- `extraSpecs`: Optional property. Defines extra properties for virtual table fields; these properties will be output as part of `structure` field during select, for extra rendering information on frontend. E.g., `{comment(STRING):"Field description",label(STRING):"Field label display",readOnly(BOOL):"Is read only"}`
- `mockData`: Virtual table test data; only set when ServiceDomain file needs standalone preview debugging

**Fields Declaration**

- Each `Field`: Defines one field of its parent virtual table. Field names should use **camelCase**, e.g., `userName`
- **Local table fields** and **foreign table fields**
  - Local table field associates with a field in current main table; if virtual table field name matches main table field name, no need to specify `sourceField` property; otherwise specify corresponding main table field via `sourceField` property
  - Foreign table field: Set a field path via `sourceField` property following entity table relations (see Field Path Writing Rules)
  - `sourceField` property supports pointing to local or foreign table field, **does not support expressions or aggregate functions**
- Field **extra properties** must first be defined in `extraSpecs`
- **Vector field**: When field type is `VEC`, must additionally specify `vecSource` property
- **System fields**:
  - `_id`, `_user`, `_create`, `_update` are system-auto-managed fields (types INT, STRING, TIMESTAMP, TIMESTAMP); can be used directly without declaring in virtual table (e.g., as select filter/sort conditions and output fields)
  - If need to set extra properties or aliases for local table system fields, can declare in virtual table fields, e.g., `Field-docId type:INT sourceField:_id label:"Creation Time" readOnly:true`
  - Whether declared or not, system fields are **read-only fields** and cannot be assigned values in insert/update

#### **sourceField Writing Rules**

- If current virtual table field binds to a field in current main table with different names, specify current main table field name in `sourceField` property, e.g., `<Field-title type:STRING sourceField:docTitle> // Virtual table field title corresponds to current main table entity field docTitle`
- If current virtual table binds to a field in another related table, use `~` jump operator, meaning "follow" Relation from current table to another table. Format: `localStartField~RelatedTable[anchorField].targetField`

  - Single jump: `<Field-departmentName> type:STRING sourceField:departmentId~Departments[_id].departmentName`
  - Multi-jump: `<Field-managerName> type:STRING sourceField:departmentId~Departments[_id].managerId~Users[_id].userName`
- Any jump must have corresponding Relation, and this Relation must be many-to-one or one-to-one, otherwise causes data expansion

**Examples**

```vl
// ✅ Multi-jump (each jump must be many-to-one >>)
sourceField:departmentId~Departments[_id].managerId~Users[_id].userName

// ❌ One-to-many jump forbidden (causes data duplication)
sourceField:_id~Orders[userId].amount  // users._id << orders.userId is <<, error
```

#### Foreign Table Field Read-Only Access Rule

When a field is a foreign table field, that field can only be read, not written.

**❌Error Example**

```vl
<VirtualTable-Orders> "orderTable" sourceTable:Orders
-<Field-productName> type:STRING sourceField:productId~Products[_id].name // Foreign table field
-<Field-quantity> type:INT // Local table field

SERVICE CreateOrder(productId(INT),quantity(INT),productName(STRING));RETURN {success:BOOL,orderId:INT}
-_orderData({productId:INT,quantity:INT,productName:STRING}) = {productId:productId,quantity:quantity,productName:productName} // ❌ Error! productName is foreign table field, cannot be written
-<VirtualTable-Orders "orderTable">.insert(_orderData) -> _result
-RETURN {success:true,orderId:_result.dataId}
```

### `# Services` (Backend Service Definition)

In `.vs` files, backend services are defined under `# Services`. Two service types exist with distinct call boundaries:

| Type | Entry | Callable from Section | External HTTP |
|------|-------|-----------------------|---------------|
| `SERVICE` | Internal service | ✅ Yes | ❌ No |
| `PUBLIC_SERVICE` | Public HTTP service | ❌ No | ✅ Yes |

- `SERVICE ServiceName(...)`: internal service, callable from Section via `<ServiceDomain-DomainName "serviceId">.ServiceName()`.
- `PUBLIC_SERVICE ServiceName(...);RETURN ReturnType;EXPOSE {method:STRING,receive:STRING,response:STRING}`: public HTTP service, external-only entry. Section cannot call `PUBLIC_SERVICE` directly.

Endpoint rule for `PUBLIC_SERVICE`:

```
{METHOD} /xxx/{servicedomainname}/{servicename}
```

`/xxx` is project/user-defined prefix; domain and service segments are lower-case.

`EXPOSE` defaults:

- `method`: `"POST"` (options: `GET|POST|PUT|PATCH|DELETE|UNLIMITED`)
- `receive`: `"JSON"` (options: `JSON|STRING`)
- `response`: `"JSON"` (options: `JSON|XML|TEXT`)

Request parameter mapping:

- `receive:"JSON"`
  - `GET` / `DELETE`: params come from query
  - `POST` / `PUT` / `PATCH`: params come from JSON body
  - `UNLIMITED`: prefer JSON body, fallback to query
- `receive:"STRING"`
  - raw request body is passed as a single string input
  - recommended to define only one `STRING` parameter; otherwise runtime should raise parameter-mapping error

- Both `SERVICE` and `PUBLIC_SERVICE` use `RETURN`.
- `PUBLIC_SERVICE` with `response:"JSON"` does **not** require internal wrapper fields such as `success/message`; response contract is defined by external API requirements.

Custom HTTP response termination:

```
CUSTOM_RETURN {statusCode:INT,headers:{},contentType:STRING,body:ANY}
```

- Available in both `SERVICE` and `PUBLIC_SERVICE`.
- `CUSTOM_RETURN` terminates current execution path immediately.
- `RETURN` and `CUSTOM_RETURN` may exist in different branches, but one path can hit only one terminal statement.
- all fields are optional; when omitted, defaults follow normal response mapping (`statusCode:200` by default)

Cookie API in service:

```
SET_COOKIE {name:STRING,value:STRING,path:STRING,domain:STRING,maxAge:INT,expires:TIMESTAMP,httpOnly:BOOL,secure:BOOL,sameSite:STRING}
```

- Available in both `SERVICE` and `PUBLIC_SERVICE`.
- `name` and `value` are required.
- Default values: `path:"/"`, `httpOnly:true`, `secure:true`, `sameSite:"Lax"`.
- Delete cookie: `SET_COOKIE {name:"sid",value:"",maxAge:0,path:"/"}`.
- `SET_COOKIE` is effective only in HTTP request context; in non-HTTP context runtime should report an error.

Recommended reuse pattern:

- extract shared business logic into backend `METHOD`
- keep `SERVICE` as internal entry
- keep `PUBLIC_SERVICE` as external entry

Example (`PUBLIC_SERVICE` — GET with TEXT response):

```vl
PUBLIC_SERVICE VerifyWebhook(token(STRING));RETURN STRING;EXPOSE {method:"GET",receive:"JSON",response:"TEXT"}
-IF token == "abc"
--RETURN "challenge-123"
-RETURN "forbidden"
```

Example (`PUBLIC_SERVICE` — 302 redirect using `CUSTOM_RETURN`):

```vl
PUBLIC_SERVICE RedirectToLogin();RETURN STRING;EXPOSE {method:"GET",receive:"JSON",response:"TEXT"}
-CUSTOM_RETURN {statusCode:302,headers:{"Location":"https://example.com/login"},body:""}
```

Example (`SERVICE` — login with `SET_COOKIE`):

```vl
SERVICE Login(user(STRING),pwd(STRING));RETURN {success:BOOL}
-auth(user,pwd) -> _r
-IF !_r.success
--RETURN {success:false}
-SET_COOKIE {name:"sid",value:_r.sessionId,httpOnly:true,secure:true,sameSite:"Strict",maxAge:86400}
-RETURN {success:true}
```

Example (`PUBLIC_SERVICE` — webhook with `SET_COOKIE`):

```vl
PUBLIC_SERVICE Webhook(raw(STRING));RETURN STRING;EXPOSE {method:"POST",receive:"STRING",response:"TEXT"}
-SET_COOKIE {name:"traceId",value:"abc123",httpOnly:false,maxAge:300}
-RETURN "ok"
```

Example (reuse pattern — shared logic via `METHOD`, separate internal and external entries):

```vl
METHOD _handleOrder(orderId(INT));RETURN {success:BOOL,data:{}}
-...
-RETURN {success:true,data:_r}

SERVICE GetOrder(orderId(INT));RETURN {success:BOOL,data:{}}
-_handleOrder(orderId) -> _ret
-RETURN _ret

PUBLIC_SERVICE GetOrderWebhook(orderId(INT));RETURN STRING;EXPOSE {method:"POST",receive:"JSON",response:"TEXT"}
-_handleOrder(orderId) -> _ret
-RETURN "ok"
```

### `# Backend Event Handlers` (Backend Event Listeners)

Backend message-type component message handling logic definitions. For example `<ServerWSClient>`, `<MQ>` message events. Example:

```
# Backend Event Handlers
<MQ-DataPipe "dataTransform">.@message(rawData,dataType)
-convertData(rawData,dataType) -> _result
-log(_result)
```

### `# Transactions` (Database Transaction Definition)

Define a database operation transaction; rollback operations are supported within transactions. Example:

```
# Transactions
TRANSACTION txTransferMoney(fromId(INT), toId(INT), amount(FLOAT)); RETURN {success:BOOL, message:STRING}
-<VirtualTable-Accounts "accountTable">.select([["_id","eq",fromId]]) -> _fromAccount
-IF _fromAccount.dataArray[0].balance < amount
--ROLLBACK {success:false, message:"Insufficient balance"}
-<VirtualTable-Accounts "accountTable">.update([["_id","eq",fromId]], [["balance","inc",-amount]], 1) -> _updateFrom
-<VirtualTable-Accounts "accountTable">.update([["_id","eq",toId]], [["balance","inc",amount]], 1) -> _updateTo
-RETURN {success:true, message:"Transfer successful"}
```

- Naming uses camelCase prefixed with `tx`
- Use ROLLBACK keyword to rollback transaction
- Transaction usage is identical to internal methods; cannot be called directly from frontend like services

**Call Example**

```
# Calling transaction in Service
SERVICE TransferMoney(fromUserId(INT), toUserId(INT), amount(FLOAT)); RETURN {success:BOOL, message:STRING}
-txTransferMoney(fromUserId, toUserId, amount) -> _result
-RETURN _result
```

### `# Backend Internal Methods` (Backend Internal Method Definition)

Define backend internal methods, prefixed with METHOD; format is identical to frontend internal methods.

## User Login and Permission Control

### Identity Trust Boundary

`SYSENV.currentUser` is available in both frontend and backend, but carries fundamentally different trust levels:

| Scope | Data Source | Trust Level | Permitted Uses |
|-------|-------------|-------------|----------------|
| Frontend | Parsed from client-side token | **Untrusted** (can be forged) | UI display (username, avatar, etc.); login state check (`isLogin`); frontend-only UI condition control |
| Backend SERVICE | Read from Redis at runtime, verified by TokenIssuer | **Trusted** | All logic that determines "who is performing this action" — data writes, permission checks |

**The backend `SYSENV.currentUser` is the only trusted source of user identity in the VL platform.**

#### User Center

VL system uniformly provides User Center application; any project will automatically bind a User Center without specifying in VL. User Center provides:

- User registration, login
- User information editing
- Admin user management (user list viewing, user disable/deactivation)
- Admin user permission configuration (ABAC combined with RBAC permission management system, configurable resource groups, manage resource group access rules, etc.)

#### Application Interaction with User Center

In frontend applications, `<ClientUserCenter>` component can be used to call User Center interface methods for frontend custom login interface related functionality.

Frontend and backend applications can both use `SYSENV.currentUser` to get current logged-in user information.

#### Login Mode Decision Matrix

| Decision Dimension | Mode 1 (ClientUserCenter / User Center managed) | Mode 2 (Custom Login + TokenIssuer) |
|---|---|---|
| User and credential ownership | User Center | Project-owned store or third-party identity system |
| Where credential verification runs | Built-in User Center flow | Project backend SERVICE (can integrate external SSO/IdP) |
| Login UI shape | Can redirect to default login page; can also use a project-custom UI that calls ClientUserCenter | Project-custom UI |
| Token issuance | Completed internally by User Center | Backend must call `TokenIssuer.generateLoginToken` |
| Default recommendation | Yes (when there is no explicit external authentication requirement) | No (only when user data/authentication is not managed by User Center) |

**Normative Rule**

- A custom login UI is not the deciding factor between Mode 1 and Mode 2.
- Mode 2 is selected only when authentication and user data are not managed by User Center.

#### Standard Login Patterns

**Mode 1: ClientUserCenter (User Center managed)**

Use when user data and authentication logic are managed by the User Center. The project only builds the UI trigger; all authentication is handled internally by the User Center.

```vl
# Frontend Tree
<ClientUserCenter-UserAuth "userCenter">

<Page-Home "home"> path:"home"
-<If-NotLogin> conditions:(!SYSENV.currentUser.isLogin)
--<Button-Login "loginBtn"> value:"Login" style:"primary|filled|default|md|actionable"

# Frontend Event Handlers
<Page-Home "home">.@init()
-IF !SYSENV.currentUser.isLogin
--<ClientUserCenter-UserAuth "userCenter">.redirectToLogin()

<ClientUserCenter-UserAuth "userCenter">.@loginDone()
-<ClientUtils>.refreshCurrentUser()
```

**Mode 2: Custom Login (project-managed, TokenIssuer)**

Use when user data is managed by the project itself, or authenticated via a third-party system. The project builds its own authentication SERVICE; after identity is verified (by any means — querying a local table, calling an external SSO, invoking a third-party API, etc.), it must call `TokenIssuer.generateLoginToken` to issue the token. The frontend does not participate in token generation.

Minimum runnable example (backend ServiceDomain + frontend Section):

```vl
// VL_VERSION:4.3.1
<ServiceDomain-Auth>

# Backend Tree
<UserStore "userStore"> sourceTable:AuthUsers identitiesTable:AuthUserIdentities
<TokenIssuer "tokenIssuer">

# Services
SERVICE UserLogin(username(STRING), password(STRING));RETURN {success:BOOL, message:STRING}
-<UserStore "userStore">.verifyUserByPassword(username, password) -> _loginResult
-GUARD (!_loginResult.success) _loginResult.message
-<TokenIssuer "tokenIssuer">.generateLoginToken(_loginResult.userId, "default", _loginResult.subjectInfo, _loginResult.user, 5, NULL) -> _tokenResult
-GUARD (!_tokenResult.success) _tokenResult.message
-RETURN {success:true, message:""}

# Backend Event Handlers
# Transactions
# Backend Internal Methods
# Backend Pipeline Funcs
</ServiceDomain-Auth>
```

```vl
// Frontend Section: login form
# Frontend Tree
<ServiceDomain-Auth "authService">
-<Service-UserLogin> params:(username(STRING),password(STRING)) returns:(success(BOOL),message(STRING))

<Col-LoginForm> gap:"16px" padding:"24px"
-<Input-Username "usernameInput"> style:"neutral|outlined|default|md" placeholder:"Username"
-<Input-Password "passwordInput"> style:"neutral|outlined|default|md" placeholder:"Password" inputType:"password"
-<Button-Submit "submitBtn"> value:"Login" style:"primary|filled|default|md|actionable"

# Frontend Event Handlers
<Input-Username "usernameInput">.@input(value)
-$username = value

<Input-Password "passwordInput">.@input(value)
-$password = value

<Button-Submit "submitBtn">.@click()
-<ServiceDomain-Auth "authService">.UserLogin($username, $password) -> _r
-IF !_r.success
--<SysUI>.showToast(_r.message, "error")
--RETURN
-<ClientUtils>.refreshCurrentUser()
```

#### Logic NOT Allowed in Applications

Entity tables, virtual tables, services, frontend element access permissions in applications will be set by administrators or an independent AI agent in User Center management interface; therefore **please do not hardcode any user permission-related logic in code**, including:

- Whether a service can be accessed
- Data access scope restrictions, e.g., whether can only access self-created data, or can access current department, or global data; these restrictions will be configured directly in User Center, project code should not have user-based data filtering logic
- Whether a page can be accessed
- Whether a table or field can be accessed, can be updated, etc.

Example:

```vl
// Error: Hardcoding user-related filter logic in code
<messageTable>.select([["_user","eq",SYSENV.currentUser.userId]],[["_update","desc"]],[offset,limit],null) -> _result

// Correct: Code only handles business logic; user-related data scope permissions left to permission configuration module
<messageTable>.select(null,[["_update","desc"]],[offset,limit],null) -> _result
```

**Forbidden: passing frontend user identity to backend SERVICE as the current operator.**

The current operator's identity must always be read from `SYSENV.currentUser` inside the backend SERVICE. The frontend must not read `userId` (or any other identity field) from `SYSENV.currentUser` and pass it as a parameter to a SERVICE for the purpose of identifying who is performing the operation.

```vl
// ❌ Error: frontend reads userId and passes it into SERVICE
$userId(STRING) = SYSENV.currentUser.userId
<ServiceDomain-Order>.CreateOrder($userId, $orderData) -> _r

// ❌ Error: SERVICE accepts userId as parameter for the current operator
SERVICE CreateOrder(userId(STRING), orderData({}));RETURN {success:BOOL}
-<orderTable>.insert({creatorId:userId, ...orderData}) -> _r

// ✅ Correct: frontend passes no identity; SERVICE reads from SYSENV internally
<ServiceDomain-Order>.CreateOrder($orderData) -> _r

SERVICE CreateOrder(orderData({}));RETURN {success:BOOL}
-<orderTable>.insert({creatorId:SYSENV.currentUser.userId, ...orderData}) -> _r
```

**Exception**: Admin operations that act on another user's data (e.g., an admin modifying a target user's record) may accept a `targetUserId` parameter. This must be clearly named `targetUserId` (not `userId`) to distinguish it semantically from the current operator.

### Default Platform Behavior: Write Operations Require Login

When a SERVICE contains any db write operation (`insert` / `update` / `delete`), the runtime automatically requires the request to be authenticated. Unauthenticated requests are rejected at the SERVICE entry point with a 401 error, before any logic executes.

**Judgment scope**:

- Each SERVICE is evaluated **only at its own entry point** — no static analysis of the call chain is performed.
- If SERVICE A (read-only) calls SERVICE B (contains writes), B enforces the login check at its own entry. A's already-executed read operations have no side effects; the failure propagates up the call chain and A returns an error.
- Pure read operations (`select` / `count`) are **not** subject to this rule. Their access control is managed by the project-level resource permission configuration layer.

**Override**: Projects may explicitly override this default behavior via the resource permission configuration layer (e.g., to permit anonymous writes to a specific SERVICE). This rule acts as the platform's secure-by-default fallback when no permission configuration exists.

## VL Generation Hard Rules (MUST / MUST NOT)

### 1. Parent Prop Passing: No `$` Prefix on Attribute Names

- **MUST**: When a parent file passes values to `Section` / `Component` instances, the attribute name MUST be a bare property name (e.g., `label:_item0`, `isActive:(_index0 == $selectedIndex)`, `product:_item0`).
- **MUST NOT**: The `$` prefix MUST NOT be used on attribute names in parent prop passing (e.g., `$label:_item0`, `$isActive:expr`, `$product:_item0` are all syntax errors).
- The `$` prefix is only for variable declaration (`$label(STRING) = ""` in `# Frontend Public Props`) and variable reference in expressions. It is not used in parent-side attribute key positions.

### 2. VirtualTable Full Reference Form Required

- **MUST**: When referencing a `VirtualTable` instance in `Service` / `METHOD` / event handler / subsequent statements, the full component reference form MUST be used: `<VirtualTable-Name "id">`.
- **MUST NOT**: Shorthand forms using only the instance id (e.g., `<taskTable>.select(...)`) are forbidden. The correct form is `<VirtualTable-Task "taskTable">.select(...)`.
- This is consistent with the universal VL component reference rule: all subsequent references must use `<ComponentClass-ComponentName "componentId">`.

### 3. Widget Mounting Rules

- **MUST**: `StateStyle/Animation/UseDraggable/UseDropTarget/...` widgets can only be mounted under UI-bearing components (Basic UI / Layout Containers / Frontend Root Components).
- **MUST NOT**: Widgets cannot be placed directly under logic containers like `If/For/TreeFor`; nor under non-UI functional components like `Trigger/FrontendApi/WindowEventListener/ClientUserCenter`.

### 4. Logic Container Restrictions

- **MUST NOT**: Logic containers have no UI, cannot have visual style / `sk.*` / skeleton CSS props, have no id, and cannot be accessed in subsequent code. Their supported control props still appear after the closing `>`.

### 5. Define Before Use

- **MUST**: All `$variables` must be declared in `Public Props / Global Vars / Derived Vars` before being referenced.
- **MUST**: Except for system method classes, any component instance must first appear in `# Frontend Tree` before being called. Specifically, `Trigger/FrontendApi/WindowEventListener/ClientUserCenter` must also be declared first.

### 6. Public Events

- **MUST**: Any public event exposed externally must be declared in `# Frontend Public Events` before being triggered; event names use camelCase. Main syntax is `EVENT @eventName(param(Type))`; section-scoped compatibility also accepts `@eventName(param:TYPE)` and normalizes it to the main syntax.

### 7. Expression / Derived Variable Computation

- **MUST**: Expressions and Derived Vars can only use pure expressions and `PIPE _xxx()` functions.
- **MUST NOT**: Call `METHOD` in expressions or Derived Vars.

### 8. Component Reference Name Strict Matching

- **MUST**: In `.vx` files, `<Section-xxx>` / `<Component-xxx>` where `xxx` must exactly match the corresponding file's root component name; do not substitute component name with id.
- **MUST**: App does not directly access internal child component instances of Section/Component; communicate only through Public Props/Events/Methods.

### 9. Trigger Properties and Units

- **MUST**: Trigger only uses `repeatTimes / interval(seconds) / autoPlay / isAnimate`; invented properties like `active` are forbidden.
- **MUST**: Units are strict: `Trigger.interval` = seconds; `ClientUtils.delay` = milliseconds; `SysUI.showToast.duration` = seconds.

### 10. App (.vx) Structure (Per Current Editor Version)

- **MUST**: `.vx` files do not generate Stage; Page must be a direct child node of App root component.
- **MUST NOT**: Forbidden to generate `Stage.@routeChange` and other Stage events.
- **MUST**: Route entry logic goes in `Page.@init()`; if browser events need to be monitored, use `WindowEventListener` (event names in camelCase).

---

# VL Basic Components Reference

## 1. Frontend Components

Frontend components can be used in Section, Component, and App files. Note that some components can only be used in Section/Component or only in App. Components without special notes can be used in both Section/Component and App.

### Frontend Functional Components (No UI)

All functional components, although having no UI, can have multiple instances and **must be defined in `# Frontend Tree` (component tree) before use**.

#### Trigger (Frontend Timer Trigger)

Frontend timer trigger component.

**Properties:**

- `repeatTimes(INT)`: Sets the maximum number of times the trigger can fire. Set to **-1** for infinite playback. Default value is **-1**.
- `interval(FLOAT)`: Sets the time interval between trigger fires in seconds. When empty string `""`, fires every frame (depends on device refresh rate, typically 1/40s or 1/60s), approximating continuous triggering.
- `autoPlay(BOOL)`: Controls whether the trigger auto-plays. Set to true to enable, false to disable. Default is false (no auto-play).
- `isAnimate(BOOL)`: Animation optimization: enabled by default when using trigger for animations. When enabled, trigger uses requestAnimationFrame to auto-adapt to device frame rate for optimal animation. When disabled, trigger fires strictly at setTimeout-defined intervals, suitable for features that need to run in inactive browser tabs.

**Methods:**

- `play()`: Activates trigger playback. If not playing (or reset), starts from beginning; if paused, continues from pause point.
- `pause()`: Pauses current playback. When resumed, continues from pause position.
- `stop()`: Stops trigger and resets to initial state, clearing all playback progress and pause state.

**Events:**

- `@tick(counts, interval, duration)`: Fires when trigger activates, can add any actions to execute when triggered.

#### WindowEventListener (Browser Window Event Listener)

Listens to all events supported by the window object. Note: All events should use React-style camelCase naming, e.g., `@keyDown()`, `@hashChange()`, `@beforeUnload()`

Scope boundary:

- `WindowEventListener` keyboard listeners are global browser-window listeners, suitable for page-wide hotkeys such as global `Esc`, global command palette shortcuts, or browser-window lifecycle hooks.
- Component-instance keyboard listeners remain the default path for local focus-scope interactions such as tabs, option groups, lists, and form controls.
- Global window listeners do not replace component-local keyboard handling.
- The standardized keyboard event signature is the same as component keyboard events: `@keyDown(key, code, altKey, ctrlKey, shiftKey, metaKey, repeat, isComposing)` and `@keyUp(key, code, altKey, ctrlKey, shiftKey, metaKey, repeat, isComposing)`.

#### FrontendApi (Frontend API Request Client, Section/Component Only)

Frontend API request client component. This component has no UI and is only used to encapsulate API request configuration and trigger logic.

**Properties:**

- `url(STRING)`: Required. API URL, supports path parameters like `/users/{id}`
- `method(STRING)`: Optional. HTTP method, defaults to "GET"
- `headers(OBJECT)`: Optional. Default request headers
- `timeout(INT)`: Optional. Request timeout duration (seconds)
- `params:(...)`: Optional. Parameter type declarations for `send()` method. Simple mode example: `params:(page(INT),size(INT))`. Explicit-location mode example:
  ```vl
  params:(id(INT) @path,page(INT) @query,keyword(STRING) @query,name(STRING) @body)
  ```
- `returns:(...)`: Optional. Return value type declarations, e.g., `returns:(success(BOOL),data([{}]))`

**Methods:**

- `send(param1, param2, ...)` - Smart parameter allocation (recommended)

  - **Description**: Standard API calling entrance. Use `send(...)` for normal business requests.
  - **Allocation Rules**:

    - In simple mode (no explicit `@path/@query/@body`):
      - GET/DELETE: non-path parameters → URL query parameters
      - POST/PUT/PATCH: non-path parameters → Request body
      - URL path parameters like `{id}` are automatically identified and replaced
    - In explicit-location mode:
      - only `@path`, `@query`, and `@body` are allowed
      - parameter declaration order must be `@path -> @query -> @body`
      - argument order in `send(...)` must match the declaration order
      - `@path` params replace URL placeholders and do not also enter query/body
      - `@query` params build the URL query string
      - `@body` params build the request body object
      - `GET/DELETE` may use `@path` and `@query` but must not use `@body`
  - **Return Value**: `{success(BOOL), message(STRING), data(JSON)}`
  - **Examples**:

    ```vl
    <FrontendApi-GetUsers "getUsersApi"> url:"/api/users" method:"GET" params:(page(INT),size(INT)) returns:(success(BOOL),users([{}]))

    <Button-Load "loadButton">.@click()
    -<FrontendApi-GetUsers "getUsersApi">.send(1, 20) -> _result
    # Actual request: GET /api/users?page=1&size=20
    ```

    ```vl
    <FrontendApi-CreateUser "createUserApi"> url:"/api/users" method:"POST" params:(name(STRING),email(STRING)) returns:(success(BOOL),userId(INT))

    <Button-Create "createButton">.@click()
    -<FrontendApi-CreateUser "createUserApi">.send("John Doe", "JohnDoe@gmail.com") -> _result
    # Actual request: POST /api/users, Body: {"name":"John Doe","email":"JohnDoe@gmail.com"}
    ```

    ```vl
    <FrontendApi-UpdateUser "updateUserApi"> url:"/api/users/{id}" method:"PUT" params:(id(INT),name(STRING)) returns:(success(BOOL))

    <Button-Update "updateButton">.@click()
    -<FrontendApi-UpdateUser "updateUserApi">.send(123, "Jane Doe") -> _result
    # Actual request: PUT /api/users/123, Body: {"name":"Jane Doe"}
    ```

    ```vl
    <FrontendApi-SearchUsers "searchUsersApi"> url:"/api/users/{id}" method:"POST" params:(id(INT) @path,page(INT) @query,keyword(STRING) @query,name(STRING) @body) returns:(success(BOOL),users([{}]))

    <Button-Search "searchButton">.@click()
    -<FrontendApi-SearchUsers "searchUsersApi">.send(123, 1, "john", "John Doe") -> _result
    # Actual request: POST /api/users/123?page=1&keyword=john, Body: {"name":"John Doe"}
    ```
- `customSend(headers?, body?, params?, url?, timeout?, method?)` - Full customization

  - **Description**: Advanced transport-layer override entrance. It is not the default business-parameter entrance.
  - **Parameters**:

    - `headers(OBJECT)`: Optional. Custom request headers
    - `body(JSON)`: Optional. Request body
    - `params(OBJECT)`: Optional. URL query parameters
    - `url(STRING)`: Optional. Override default URL
    - `timeout(INT)`: Optional. Override default timeout
    - `method(STRING)`: Optional. Override default method
  - **Return Value**: `{success(BOOL), message(STRING), data(JSON)}`
  - **Example**:

    ```vl
    <FrontendApi-SearchUsers "searchApi"> url:"/api/users/search" method:"POST" returns:(success(BOOL),users([{}]))

    <Button-Search "searchButton">.@click()
    -<FrontendApi-SearchUsers "searchApi">.customSend({Authorization:"Bearer " + $token}, {keyword:"John Doe"}, null, "/api/users/search/advanced", null, null) -> _result
    # Actual request: POST /api/users/search/advanced, Header: Authorization=Bearer ..., Body: {"keyword":"John Doe"}
    ```

**Core Usage Rules**

- Only use `customSend` when `send` method cannot meet requirements, **limited to** these scenarios:
  - runtime override of `headers`
  - runtime override of `url`
  - runtime override of `timeout`
  - runtime override of `method`

- The following scenarios should use `send(...)`, not `customSend(...)`:
  - normal `path/query/body` parameter splitting
  - fixed URL + fixed method business requests
  - requests that contain both query and body but do not need runtime transport overrides

- When `params:(...)` uses explicit location declarations:
  - every param in that declaration block must use an explicit location
  - URL `{param}` placeholders must match `@path` params one-to-one

#### ClientUserCenter (User Center Client)

Frontend user center client. Must be declared in component tree, e.g., `<ClientUserCenter-UserAuth>`. Typically only one instance is needed per file.

**Methods:**

- `redirectToLogin()`: Redirects to the User Center default login page. If the platform supports return-to-origin, the user is redirected back to the current page after login completes.
- `userLoginPassword(userName, password)`: Username/password login
  - Parameters:
    - `userName(STRING)`: Username
    - `password(STRING)`: Password
  - Return Value:
    - `isSuccess`
    - `failReason`
- `userLogout()`: Logout
  - Return Value:
    - `isSuccess`
    - `failReason`
- `jumpToUserCenter()`: Navigate to user center
- `sendRegistrationSMS(phoneNumber)`: Send SMS verification code
  - Parameters:
    - `phoneNumber(STRING)`: Phone number
  - Return Value:
    - `isSuccess`
    - `failReason`
- `registerUser(userName, password, verificationCode, phoneNumber)`: Register user
  - Parameters:
    - `userName(STRING)`: Username
    - `password(STRING)`: Password
    - `verificationCode(STRING)`: SMS verification code
    - `phoneNumber(STRING)`: Phone number
  - Return Value:
    - `isSuccess`
    - `failReason`

**Events:**

- `@loginDone()`: Triggered when login completes

### Basic UI Components (Section/Component Only)

All basic UI components can only be added in Section/Component, not in App.

**!!Note:** Except for `<Button>`, all other basic UI components **cannot have UI child components** (excluding widgets such as StateStyle/Animation)!

**!!Note:** The "Fixed CSS styles" section is **descriptive only**; **`display` cannot be overwritten in VL code**;

#### Text (Basic Text)

**Properties:**

- `value(STRING)`: Text content

**Fixed CSS styles:**

display: block

#### Button (Basic Button)

**Properties:**

- `value(STRING)`: Button text. When Button has no UI child components, `value` renders as the button's text.
  When both `value` and UI child components exist, `value` takes priority,
  UI child components are ignored, and the compiler emits warning BT-001.
  Note: widgets (StateStyle, Animation) are not considered UI child components
  and do not trigger this rule.
- `disabled(BOOL)`: Whether disabled

**Fixed CSS styles:**

- display: inline-flex

**Child component rules:**

- Button may have UI child components (Icon, Text, Row, etc.) to create composite
  buttons (icon + text, icon-only, etc.).
- When Button has UI child components and no `value`, all visible content is rendered
  through children in a centered flex row content wrapper.
- When Button has both `value` and UI child components, `value` takes priority,
  children are ignored, and the compiler emits warning BT-001.
- When Button has no UI child components, `value` renders as the button's text.
- Widgets (StateStyle, Animation) can always be added regardless of `value`.

#### ButtonContainer (Button Container) [DEPRECATED]

**DEPRECATED**: Use Button with child components instead. See §Button.

Create composite buttons by adding child components, such as an Icon and Text forming a button. Note: ButtonContainer itself cannot set the value property; all content is rendered through its child components.

**Properties:**

- `disabled(BOOL)`: Whether disabled

**Fixed CSS styles:**

- display: inline-flex

##### Button Component Usage Notes

Button with children (recommended):

```vl
-<Button-PrevMonthBtn "preMonthButton"> style:"neutral|ghost|default|sm|actionable" ariaLabel:"Previous Month"
--<Icon-Arrow "arrow"> fontSet:"fa-solid-900" value:"f053"
--<Text-Label "label"> value:"上月"
```

Button without children (simple text button):

```vl
-<Button-Submit "submitBtn"> value:"Submit" style:"primary|filled|default|md|actionable"
```

Button can have UI child components (Icon, Text, Row, etc.) to create composite buttons.
When Button has child components, direct children are rendered horizontally by default.
If `value` and UI child components are both declared, `value` wins and UI children are ignored with warning BT-001.
Use Button with children directly.

#### Image (Basic Image)

**Properties:**

- `sourceUri(STRING)`: Image resource URL. For placeholder images, please use **unsplash.com** image URLs uniformly in `<Image sourceUri:"...">`.

**Fixed CSS styles:**

display: inline-block

#### Video (Basic Video)

**Properties:**

- `sourceUri(STRING)`: Video resource URL (please use legitimate external resource URLs per VL standards)

**Fixed CSS styles:**

display: inline-block

#### Divider (Divider Line)

Visual element for separating content. Divider reads appearance from the `surface` dimension.

**Properties:**

- `orientation(STRING)`: Divider direction, options: "horizontal", "vertical", defaults to "horizontal".

**Skin consumption (from surface dimension):**

| Slot | Maps To |
|------|---------|
| `surfaceBorder` | Divider line color |
| `surfaceBorderStyle` | Divider line style (solid, dashed, etc.) |
| `surfaceBorderWidth` | Divider line width |

Platform renders horizontal Divider as `border-top`, vertical Divider as `border-left`.

**Fixed CSS styles:**

display: block

#### Input (Basic Input)

Basic text input.

**Default CSS styles:**

display: inline-block

**Properties:**

- `value(STRING)`: Input content. Binding `value` to a variable does not create built-in two-way binding; if the author needs to write input back to a variable, it must be done through explicit events such as `@input`, `@change`, `@blur`, or `@confirm`. Same-variable explicit writeback is allowed and is the standard controlled-input pattern (see §Explicit Input Writeback).
- `disabled(BOOL)`: Whether input is disabled
- `type(STRING)`: Basic input, only supports these types: text, email, password, number, url, tel. For other input types, use corresponding extended components.

**Events:**

- `@change(newValue, oldValue)`: Triggered when input loses focus and content differs from when it gained focus. Emits old and new values.
- `@focus()`: Triggered when input gains focus
- `@blur(current)`: Triggered when input loses focus
- `@input(newValue, delta)`: Triggered when input content changes
- `@clickOutside()`: Triggered when clicking outside the input area

#### Textarea (Multi-line Input)

Basic multi-line input component.

**Properties:**

Same as basic single-line input.

**Events:**

Same as basic single-line input.

**Default CSS styles:**

display: inline-block

#### Icon (Icon)

For rendering SVG, external resource URLs, or Font Awesome library icons. All icons should use this component uniformly. Icon resources can come from `svgCode`, `url`, or Font Awesome (`fontSet+value`), with priority: **svgCode > url > fontSet+value**.

**Properties:**

- `svgCode(STRING)`: Directly specify SVG code
- `url(STRING)`: External resource URL, VL internally uses HTML mask functionality to convert it to an icon

* `fontSet(STRING)`: Font Awesome icon library, options:
  - fa-solid-900
  - fa-regular-400
  - fa-brands-400
* `value(STRING)`: Font Awesome icon's **Unicode character code**, e.g., `"f170"`. **Note**: This value **must** be the icon's Unicode code, **not** a CSS class name (like `"fas fa-book"`) or icon name (like `"book"`). If set incorrectly, the icon won't display and you may see text like "OOK".
* `content(STRING)`: Compatibility alias for old source files. New VL code SHOULD use `value`. Parser accepts `content`, but lint MUST report an error if one Icon declares both `value` and `content`; keep `value` and remove `content`.

**Fixed CSS styles:**

display: inline-block

**Notes:**

- When icon content is specified via svgCode or url, size **SHOULD** be set via CSS width/height properties. If neither `width` nor `height` is specified, the platform applies a default size of **24×24** (px) to prevent the browser's 300×150 replaced-element fallback. Omitting explicit dimensions will produce a compiler **warning**.
- When icon content is specified via `fontSet+value`, it's essentially text, so size should be set via CSS font-size property (CSS styles go outside angle brackets), e.g., `<Icon-User> fontSet:"fa-solid-900" value:"f007" font-size:"16px"`
- `text-align` may be written on `Icon`; it aligns icon-font content inside the Icon's own box, such as `<Icon-NavIcon> fontSet:"fa-solid-900" value:"f007" width:"20px" text-align:"center"`.

#### Chart

`Chart` is not part of the VL core built-in component set.

If a project needs chart visualization, it should use:

1. an imported chart module component
2. a project-local `WebComponent-*` or `Component-*` chart module

Direct authoring of `<Chart-...>` is not part of the standard VL syntax contract.

#### MarkdownEdit (Markdown Editor)

Component for editing Markdown format text. Note: The markdown editor has a built-in toolbar (such as font, heading, embed tool icons), so there's no need to implement the toolbar from UI designs separately.

**Properties:**

- `value(STRING)`: Editor's Markdown text content
- `disabled(BOOL)`: When true, editing is disabled. Default is false
- `placeholder(STRING)`: Text hint displayed when content is empty

**Events:**

- `@change(newValue, oldValue)`: Triggered after blur when content differs from before.
- `@focus()`: Triggered when editor gains focus
- `@blur()`: Triggered when editor loses focus
- `@select(value)`: Triggered when text is selected in editor

#### Markdown (Markdown Renderer)

Component for rendering Markdown text as HTML and displaying it.

**Properties:**

- `value(STRING)`: Markdown text content to render

### Layout Container Components

**display Rules**: All containers' `display` is controlled internally by components; **forbidden** to set directly via style properties.

! All layout containers are for layout only, have no value property, and cannot contain inline text.

#### Modal (Modal Layer)

Modal consists of an **outer full-screen flex container** and an **inner content panel**: the outer handles overlay and positioning, its `display` cannot be changed. Width and height written on Modal control the inner panel's dimensions. Skin values such as background color must use Theme style coordinates or legal `sk.*` props rather than raw CSS skin property names. The outer flex container is a full-screen container (serving as overlay) and cannot have width, height, color, or other CSS styles set.

**Properties:**

- `mask(BOOL)`: Whether to enable background overlay. If enabled, automatically generates a full-screen overlay, default overlay color is black (#000000)
- `maskColor(STRING)`: When background overlay is enabled, overlay color can be set. Default is black #000000
- `placement(STRING)`: Banner position relative to current window, options: "topLeft", "topCenter", "topRight", "middleLeft", "middleCenter", "middleRight", "bottomLeft", "bottomCenter", "bottomRight". Default is "middleCenter"

**Special Events:**

- `@clickMask()`: Triggered when clicking the component's overlay layer (requires "Background Overlay" property enabled).

For `modal` components, background overlay effect should be controlled via the component's `mask:true` and `maskColor:"rgba(0,0,0,0.5)"` functional properties. For skin values such as panel background color, use Theme style coordinates or legal `sk.*` props instead of raw CSS skin property names.

#### Col, Row, Grid (Flex Column, Flex Row, Grid Container)

Containers with fixed flex and grid layouts.

- Col: fixed single-column flex container
- Row: fixed single-row flex container
- Grid: fixed two-dimensional grid container

Rules:

1. `Row / Col / Grid` layout semantics are determined by the container type itself
2. Authors must not write `flex-direction` or `flex-flow` to modify these semantics
3. `flex-wrap` is only valid on `Row`, and only accepts `wrap` or `nowrap`; it does not change Row's main-axis direction
4. `row-reverse`, `column-reverse`, and `wrap-reverse` are not part of VL standard authoring
5. Card walls, thumbnail grids, and other grid-like repeat layouts should use `Grid`; use `Row flex-wrap:"wrap"` only for simple inline wrapping flows such as chips or tags

#### Block (Block Container) [DEPRECATED]

Block uses `display: block`. `alignItems`, `justifyContent`, and `gap` do not take effect on it. Use Row or Col when flex layout behavior is required.

**Fixed CSS styles:**

- display: block

#### Table, TableHeader, TableRow, TableCell, TableCellContainer (Table Series Components, Section/Component Only)

For creating HTML tables:

- Table (Basic Table Container): Corresponds to table tag, automatically creates tbody tag for TableRow components added directly under it, no need to add tbody component separately
- TableHeader: Corresponds to thead tag
- TableRow: Corresponds to tr tag, can be added directly under Table or under TableHeader
- TableCell, TableCellContainer: Can only be added under TableRow component. When added under TableRow in TableHeader, corresponds to th tag; when added under TableRow directly under Table, corresponds to td tag.
  - Core Properties
    - rowspan and colspan: Specify cells to span
  - Content Modes:
    - TableCell component can only specify text content via Value property, **cannot add child components**
    - TableCellContainer component **has no Value property** but can add child components to render content

**Mandatory Hierarchy:**

```
<Table>
-<TableHeader-Xx> (optional)
--<TableRow-Xx>
---<TableCell-Xx> value:"Header 1"> or <TableCellContainer-Xx>...
--<TableRow-Xx>
---<TableCell-Xx> value:"Data 1" or <TableCellContainer-Xx>...
```

**Example:**

```
# Frontend Tree

<Table-UserList> border-collapse:"collapse" width:100%
-<TableHeader-Header>
--<TableRow-HeaderRow>
---<TableCell-HId> value:"ID"
---<TableCell-HName> value:"Name"
---<TableCell-HAction> value:"Action"
-<For-UserRows> sourceArray:$userList loopVar:[_user, _index0]
--<TableRow-UserRow>
---<TableCell-CellId> value:_user.id
---<TableCell-CellName> value:_user.name
---<TableCellContainer-CellAction> // Use TableCellContainer to hold buttons
----<Button-Delete> value:"Delete"
```

### Structural Container Components (App Only)

#### Page Component

Application page container, responsible for route mapping and page lifecycle management.

**Properties:**

- `path(STRING)`: Page's route path, logical path identifier without leading slash

**Events:**

- `@init()`: Triggered when page initialization completes, common entry point for application logic

**Usage Example:**

```vl
# Frontend Tree
<App-Main "app">
-<Page-MainPage "mainPage"> path:"main"

# Frontend Event Handlers
<Page-MainPage "mainPage">.@init()
```

**Important Rules:**

- `<Page>` is a direct child of the `.vx` `<App>` real root in `# Frontend Tree`
- `<Page>` cannot be nested inside any other container
- `<Page>` cannot contain other `<Page>` components
- `App` and `Page` boundaries follow the `App, Page, and Modal Special Boundaries` chapter; the per-node-class applicability matrix in `Closed CSS Property Model` does not apply to them

### Logic Container Components

Logic container components have no UI themselves, cannot be used for layout, and since logic containers cannot be accessed in subsequent methods, they **have no id property**. Their control props, such as `sourceArray`, `loopVar`, and `conditions`, still follow the universal component reference rule and are written after the closing `>`. They cannot declare visual style, `sk.*`, skeleton CSS, or event props.

#### For (Loop Container, Section/Component Only)

Container for dynamically creating repeated components based on data source.

**Properties:**

- `sourceArray(ARRAY)`: Sets data source for loop creation, can be one-dimensional array, two-dimensional array, object array, etc.
- `loopVar([STRING, STRING])`: Defines variable names representing current array element and index in each loop iteration. Format is `[_itemVar, _indexVar]`, e.g., `[_item0, _index0]`. These two variables can be used directly in loop container's child components.

**Syntax:**

```
<For-AnyName> sourceArray:arrayVariable loopVar:[_itemX, _indexX]
-<Child component 1 rendered for each array element>
-<Child component 2 rendered for each array element>
```

**Notes:**

- `For-` prefix + PascalCase name. `sourceArray` and `loopVar` must be written after the closing `>`, not inside angle brackets.
- `loopVar` naming: see "Variable Naming Rules" section. Format: `[_itemX, _indexX]`.
- **Loop Variable Scope**: Loop variables are only valid in property bindings, conditional expressions, and event handlers of `<For-name>`'s direct children and their descendant components. Referencing outside is an error.

**Example:**

```
<For-UserItems> sourceArray:$userList loopVar:[_item0, _index0]
-<Row-UserItem>
--<Text-UserName> value:("Name: " + _item0.name)
--<If-IsAdult> conditions:(_item0.age >= 18)
---<Icon-AdultIcon> value:"f007"
```

- Use `<StateStyle>` for conditional styling; use `<If-name>` only for conditionally rendering different component tree structures.

#### If (Conditional Container)

Container component that decides whether to render content based on conditional expression.

**Properties:**

- `conditions(BOOL)`: Conditional container's judgment condition expression, elements under this container are only rendered when condition is met (expression returns true)

**Syntax:**

```
<If-AnyName> conditions:(booleanExpression)
-<Child component 1 needing conditional rendering>
-<Child component 2 needing conditional rendering>
```

**Notes:**

- `If-` is a fixed prefix, followed by an arbitrary descriptive name (following component ID naming conventions, PascalCase).
- `conditions` property receives an expression that returns a boolean value. This property must be written after the closing `>`, not inside angle brackets.
- Child components inside `<If-name>` are only rendered when `conditions` expression evaluates to `true`.
- `conditions` expression can reference global variables (`$var`), local variables (`_var`), especially **loop variables** (`_itemX`, `_indexX`) from loops, and system variables (`SYSENV.xxx`).
- **Core Principle Warning**: `<If-name>` **must never** be used to switch different UI layouts based on device type (like PC or mobile). This seriously violates the "one application, one target device" design philosophy. This component's `conditions` property **must** reflect application's **business logic state** (e.g., whether user is logged in, whether data is valid, whether in edit mode, etc.), not device environment.
  - **Wrong usage (strictly forbidden)**: `<If-DeviceBranch> conditions:(SYSENV.deviceType == "mobile")`
  - **Correct usage (recommended)**: `<If-FormValid> conditions:($formData.isValid && SYSENV.currentUser.isLogin)`
  - **Correct usage (recommended, with comparison operator)**: `<If-HighScore> conditions:($user.points > 100)`

**Example:**

```
<Col-UserStatus>
-<If-IsAdmin> conditions:($currentUser.role == "admin")
--<Text-AdminLabel> value:"Admin"
-<If-IsGuest> conditions:($currentUser.role == "guest")
--<Text-GuestLabel> value:"Guest"
-<If-IsLoggedIn> conditions:(SYSENV.currentUser.isLogin && ($userPermissions.canEdit || SYSENV.currentUser.isAdmin))
--<Button-EditProfile "editButton"> value:"Edit Profile"
```

#### TreeFor (Tree Container, Section/Component Only)

Container component for displaying and handling hierarchically structured data.

**Properties:**

- `sourceArray(ARRAY)`: Sets data source for tree expansion creation, must be an object array with one column for current node ID and another for parent node ID
- `idField(STRING)`: Field name for current node ID in source object array, usually "data ID" if from database
- `pidField(STRING)`: Field name for parent node ID in source object array, this field value cannot be 0; if top-level, this field is empty
- `loopVar([STRING,STRING,STRING,STRING,STRING])`: Defines variable names for current array element, current index, current level, expanded state, and sibling index in each loop iteration. Format is `[_itemVar,_indexVar,_levelVar,_expandedVar,_levelIndexVar]`, e.g., `[_item0,_index0,_level0,_expanded0,_levelIndex0]`. These variables can be used directly in tree container's child components

  - `_itemX`: Current node's data object.
  - `_indexX`: Current node's index in the entire flattened array.
  - `_levelX`: Current node's level depth (top level is 0).
  - `_expandedX`: Tri-state INT describing the current node's branch state. **Not a boolean.** Values:
    - `0`: leaf node — has no children.
    - `1`: branch node that is currently collapsed (children exist but are not in the rendered list).
    - `2`: branch node that is currently expanded (children are in the rendered list).

    Authors gating UI on this value must use explicit equality (`_expandedX == 2`, `_expandedX == 1`, `_expandedX == 0`). Do NOT write `_expandedX == true` — VL evaluates `==` with JS loose semantics, so `1 == true` matches but `0` and `2` do not, silently producing the wrong branch. Do NOT write `_expandedX` as a bare truthy check either, since `0` (leaf) is falsy while both `1` and `2` are truthy.
  - `_levelIndexX`: Current node's index among its sibling nodes.

**Methods:**

- `expandAllNodes()`: Expands all expandable nodes in tree container
- `collapseAllNodes()`: Collapses all expanded nodes, keeping only top-level node list
- `expandOneNode(nodeIndex)`: Expands specified node.
- `collapseOneNode(nodeIndex)`: Collapses specified node.

**Syntax:**

```
<TreeFor-AnyName> sourceArray:objectArray idField:"idFieldName" pidField:"parentIdFieldName" loopVar:[_itemX, _indexX, _levelX, _expandedX, _levelIndexX]
-<Child component rendered for each node>
```

### Frontend System Method Components (No Declaration Needed in Component Tree)

Frontend system method classes have only one global instance and can be used without defining in component tree.

#### ClientUtils (Frontend Utility)

Frontend system utility component for common client-side operations (timing, logging, routing) and system-level user refresh.

**Methods**

**Basic Utilities**

* `delay(milliSecond)`: Delays execution for the given milliseconds. Return: `success(BOOL), message(STRING)`
* `consoleLog(title, message)`: Prints debug logs to console. Return: `success(BOOL), message(STRING)`

**User Info Refresh**

* `refreshCurrentUser()`: Refreshes `SYSENV.currentUser`; calls a system API and updates local token if `newToken` is returned. Return: `success(BOOL), user({isLogin(BOOL), userId(STRING), userInfo({})}), newToken(STRING), message(STRING)`

#### SysUI (System UI Component)

Frontend system UI method class. **Do not** use ClientUtils instead of SysUI for modals/toasts.

**Methods:**

- `showModal(title, content)`: Standard styled modal, can get user's confirm or cancel result
  - Return Value:
    - `confirm(BOOL)`: Whether user clicked confirm, returns true/false
    - `cancel(BOOL)`: Whether user clicked cancel, returns true/false
- `showLoading(title)`: Shows loading icon at current page center, no other operations possible while displayed, typically used during async operation waiting
- `hideLoading()`: Hides current page's loading icon, restores page interaction
- `showToast(message, iconType, customizeIcon, duration)`: Shows a timed notification at page center, typically used for feedback after user interactions. Note: `duration` unit is seconds, not milliseconds
- `hideToast()`: Immediately hides the notification at page center, can be used to close toast early

#### SysFile (File System Component)

System component for frontend file upload and download operations.

**Methods:**

- `uploadImage(url)`: Uploads one image to server, can get image info in callback like url, name, type, size, resolution, etc.
  - Return Value: `url(STRING)`, `name(STRING)`, `type(STRING)`, `size(INT)`, `sizeWithUnit(STRING)`, `progress(INT)`, `failureReason(STRING)`, `resolution(STRING)`, `width(INT)`, `height(INT)`
- `uploadImages(quantityLimit, urls)`: Batch upload images to server (max 20), can get image info in callback.
  - Return Value: `data([{}])`, `urlList([STRING])`, `nameList([STRING])`, `typeList([STRING])`, `sizeList([INT])`, `sizeWithUnitList([STRING])`, `progress(INT)`, `failureReason(STRING)`, `resolutionList([STRING])`, `widthList([INT])`, `heightList([INT])`
- `uploadFile(url, accept)`: Uploads one file to server, can get file info in callback.
  - Return Value: `url(STRING)`, `name(STRING)`, `type(STRING)`, `size(INT)`, `sizeWithUnit(STRING)`, `progress(INT)`, `failureReason(STRING)`
- `uploadFiles(quantityLimit, urls)`: Batch upload files to server (max 20), can get file info in callback.
  - Return Value: `data([{}])`, `urlList([STRING])`, `nameList([STRING])`, `typeList([STRING])`, `sizeList([INT])`, `sizeWithUnitList([STRING])`, `progress(INT)`, `failureReason(STRING)`
- `downloadFile(fileName, url)`: Downloads file from specified URL to local
  - Return Value: `success(BOOL)`, `bytesLoaded(INT)`, `progress(INT)`, `failureReason(STRING)`
- `saveTextAsFile(content, fileName)`: Saves a text string as a local file download
  - Parameters:
    - `content(STRING)`: Required. The text content to save
    - `fileName(STRING)`: Required. The download file name, e.g., `"report.md"`
  - Return Value: (none)
  - Note: This method does not involve network requests. It creates a browser-side Blob from the given string and triggers a file download. For downloading files from a remote URL, use `downloadFile(fileName, url)` instead.

* `browseImage(outputBase64, compressedWidth, compressedHeight, type, encoderOptions)`: Read local image
  - Description: Reads one local image's info, returns Base64, name, type, size, width/height, temporary local path, file object, etc.
  - Parameters:
    - `outputBase64(Boolean)`: **Output base64** (whether to convert to base64 format after reading, default is yes. Converting to base64 has some performance cost. If only displaying image and uploading, use temporary path + file object method without outputting base64. If choosing to output base64, src field contains base64 result, otherwise no base64 result returned.)
    - `type(STRING)`: **Output format** (Choose jpg or png format for read image. If jpg, can further specify image quality.)
    - `encoderOptions(INT)`: **Output quality** (Output image quality, a value between 0-1. Note: This option only works when output format is jpg; png is a non-compressible format.)
  - Return Value:
    - `base64Code`: **base64 image** (Image converted to base64 format resource URL. Note: Only has result when output base64 is enabled during reading, otherwise empty.)
    - `name`: **Name** (Image name.)
    - `type`: **Type** (Image type.)
    - `size`: **Size** (Image file size.)
    - `width`: **Width** (Image width.)
    - `height`: **Height** (Image height.)
    - `blobUrl`: **Temporary local path** (Image temporary local path.)
    - `failureReason`: **Read failure reason** (Reason for failed image reading.)
    - `file`: **File object** (Read image file object, can be used as file path for uploading.)
* `browseImages(maxNumberPic, outputBase64, compressedWidth, compressedHeight, type, encoderOptions)`: Read multiple local images
  - Description: Reads multiple local images' info, returns an object array where each object contains src, name, type, size, width, height, path, file, etc.
  - Parameters:
    - `maxNumberPic(INT)`: **Max image count** (Maximum number of images allowed per upload.)
    - `type(STRING)`: **Output format** (Choose jpg or png format for read images.)
    - `encoderOptions(INT)`: **Output quality** (Output image quality, value between 0-1.)
  - Return Value:
    - `imageList`: **Image list** (Array of all read images' info. Returns object array, each containing src, name, type, size, width, height, path, file - see single image read return value description for details.)
    - `failureReason`: **Read failure reason** (Reason for failed image reading.)
* `browseFile(outputBase64, accept)`: Read local file
  - Description: Reads one local file's info, returns name, type, size, temporary local path, base64 data, etc. Not supported in WeChat browser, webApp WeChat mini-program, Alipay/DingTalk mini-programs.
  - Parameters:
    - `outputBase64(Boolean)`: **Convert to base64** (Default no. When yes, automatically converts file to base64, allowing base64 string to be read from result, otherwise cannot read.)
    - `accept(STRING)`: **Allowed file types** (Parameter can limit selectable file types, a string constructed from file mimeTypes. For multiple types, separate with comma, e.g., "image/\*,application/x-zip-compressed")
  - Return Value:
    - `name`: **Name** (File name.)
    - `type`: **Type** (File type.)
    - `size`: **Size** (File size.)
    - `blobUrl`: **Temporary local path** (File temporary local path.)
    - `base64Code`: **base64 data** (File's base64 format data. Note: Must choose base64 format during reading to access.)
* `browseFiles(accept, maxNumberFile)`: Read multiple local files
  - Description: Reads multiple local files' info, returns object array where each object contains name, type, size, temporary local path, etc. Not supported in WeChat browser, webApp WeChat mini-program, Alipay/DingTalk mini-programs.
  - Parameters:
    - `accept(STRING)`: **Allowed file types** (Parameter can limit selectable file types, a string constructed from file mimeTypes.)
    - `maxNumberFile(INT)`: **Max file count** (Maximum number of files allowed per upload.)
  - Return Value:
    - `fileList`: **File list** (Array of all read files' info.)
    - `failureReason`: **Read failure reason** (Reason for failed file reading.)

#### SysLocalStorage (Local Storage)

System component for storing data on client side, supports Cookie, SessionStorage, and LocalStorage.

**Methods:**

- `setCookie(path, domain, maxAge, key, value)`: Sets cookie for current domain in browser, requires Key and corresponding value.
- `setCookies(path, domain, maxAge, keyValue)`: Sets multiple cookies for current domain in browser, can pass multiple Key-value pairs.
- `getCookie(key)`: Gets cookie value for specified key under current domain.
- `removeCookie(key)`: Removes cookie for specified key under current domain.

#### SysDevice (Device Access)

System component for frontend access and control of device hardware features.

**Methods:**

- `scanCode()`: Invokes phone camera for scanning, returns scan result
  - Return Value:
    - `data(STRING)`: Information obtained from successful scan.
    - `errMsg(STRING)`: Error message for failed scan.

#### SysRoute （App only）

Frontend system routing component for .vx files to manage navigation, URL parameters, and browser history operations.

**Methods**

**Navigation**

* `navigate(pathPattern, params)`: Navigate to a route with optional dynamic parameters. Parameters: `pathPattern(STRING)` - route path template or full path (e.g., "products/:category/:id"), `params({})` - optional dynamic parameters (e.g., {category:"electronics", id:123})

**Route Information Retrieval**

* `getPathParams()`: Get path parameters from the current route. Returns: `params({})`
* `getQueryParams()`: Get URL query parameters. Returns: `params({})`
* `getHash()`: Get URL hash value. Returns: `hash(STRING)`
* `getCurrentPath()`: Get the current complete path. Returns: `path(STRING)`
* `getCurrentOrigin()`: Get the current domain. Returns: `origin(STRING)`
* `getCurrentFullUrl()`: Get the current complete URL. Returns: `url(STRING)`

**Browser History**

* `goBack()`: Navigate back in browser history
* `goForward()`: Navigate forward in browser history
* `reload()`: Refresh the current page

### Widgets

Widgets are additional functional components added under entity UI components (basic UI, layout containers, etc.) that can provide form field handling, dynamic styling, animation, drag, scroll, and other behaviors for target entity UI components.

#### StateStyle (State and Conditional Style Component)

`<StateStyle>` is used for conditional styling attached to the parent component. It is primarily for business-condition states (`conditions:`). Interaction skin states should be expressed by the runtime-state model, not by author-written `StateStyle trigger` branches.

**Syntax:**

```
<Parent Component ...>
-<StateStyle-StateName> trigger:"stateString" style1:value ..
-<StateStyle-StateName> conditions:(booleanExpression) style:"static|coordinate" style1:value ..
```

**Notes:**

- `<StateStyle>` must be a direct child component of the UI component it affects (one level deeper indentation).
- **StateStyle component names should reflect their purpose**, e.g., `<StateStyle-HoverEffect>`.
- A parent component can have multiple `<StateStyle>` child components for handling different states and conditions.
- `<StateStyle>` style properties **do not allow property expressions**, only literal values or CSS native calc expressions.
- `trigger` and `conditions` properties are functional properties.
  - **`trigger` property**: Reserved for UI built-in interaction states; it is not the preferred authoring path for new interaction skin rules.
  - **`conditions` property**: Receives an expression returning a boolean value. When expression is `true`, the `StateStyle` node becomes active.
- `StateStyle` under `conditions:` may declare `style:"..."` as a static style-coordinate patch. This `style` value must be a static string literal and may only contain legal static dimension points.
- `StateStyle.style` must not use variables, expressions, ternaries, or Theme token syntax such as `@intent.intentBg`.
- When a `StateStyle conditions` node is active, its `style` coordinate patches the parent component's base `style` by dimension rather than replacing the whole coordinate string. Unspecified dimensions continue to inherit the parent component's base `style`.
- If multiple sibling `StateStyle` nodes with `conditions` are active at the same time, their `style` patches are merged in sibling order; the later node wins on the same dimension.
- Boundary rule: `conditions` controls activation only. `StateStyle` is a static style switch, not a dynamic value binding system. Business-condition skin changes should prefer `StateStyle + style` coordinate patches; `sk.*` remains a local escape hatch for cases the style space cannot yet express.

**Example:**

- Recommended coordinate patch: `<Input-Email "emailInput"> style:"neutral|outlined|default|md" value:$email` then `-<StateStyle-Error> conditions:$hasError style:"danger"`
- Recommended partial patch: `<Button-Save "saveBtn"> style:"primary|filled|pill|md|actionable" value:"Save"` then `-<StateStyle-Readonly> conditions:$readonly style:"neutral|outlined"`
- Recommended non-skin business condition: `<Button-Submit "submitBtn"> style:"primary|filled|pill|actionable" value:"Submit" disabled:$isSubmitting` then `-<StateStyle-BusyLock> conditions:$isSubmitting pointer-events:"none"`
- Forbidden non-literal coordinate: `-<StateStyle-Error> conditions:$hasError style:$errStyle`
- Forbidden Theme token in `StateStyle.style`: `-<StateStyle-Error> conditions:$hasError style:"@intent.intentBg"`
- Forbidden raw skin literals in `StateStyle`: `-<StateStyle-ErrSkin> conditions:$hasError border-color:"#F53F3F" background-color:"#FFF1F0"`
- Escape-hatch `StateStyle` example: `-<StateStyle-ErrSkin2> conditions:$hasError sk.borderColor:"#F53F3F"`

#### Animation (Animation Effect)

Added as a "widget" to target UI component to add animation effects. After adding, VL system automatically wraps the target UI element with a transparent Div to implement animation effects.

**Properties**

- **animationName (Animation Type)**

  - **Description:** Selects animation type from system default animation library, divided into three categories: emphasis, entrance, and exit animations. Each animation type has multiple effects, each possibly with multiple directions.
  - **Options:**

    - Emphasis: bounce, flip, flash, jello, pulse, rubberBand, shake, swing, tada, wobble, hinge, rotateC, rotateAC;
    - Entrance: fadeIn, fadeInDown, fadeInUp, fadeInLeft, fadeInRight, bounceIn, bounceInDown, bounceInUp, bounceInLeft, bounceInRight, rotateIn, rotateInDownLeft, rotateInUpLeft, rotateInDownRight, rotateInUpRight, slideInDown, slideInUp, slideInLeft, slideInRight, zoomIn, zoomInDown, zoomInUp, zoomInLeft, zoomInRight, flipInX, flipInY, rollIn;
    - Exit: fadeOut, fadeOutDown, fadeOutUp, fadeOutLeft, fadeOutRight, bounceOut, bounceOutDown, bounceOutUp, bounceOutLeft, bounceOutRight, rotateOut, rotateOutDownLeft, rotateOutUpLeft, rotateOutDownRight, rotateOutUpRight, slideOutDown, slideOutUp, slideOutLeft, slideOutRight, zoomOut, zoomOutDown, zoomOutUp, zoomOutLeft, zoomOutRight, flipOutX, flipOutY, rollOut;
- **duration (Animation Duration in Seconds)**

  - **Description:** Animation effect duration in seconds.
- **delay (Start Delay in Seconds)**

  - **Description:** Animation start delay in seconds.
- **iterationCount (Play Count)**

  - **Description:** Number of times to play each trigger, default is 1 (play once then stop).
- **loop (Loop Playback)**

  - **Description:** Toggle to control animation looping, disabled by default.
- **playAnimate (Preview Animation)**

  - **Description:** Click button to preview current animation effect.
- **trigger (Trigger Timing)**

  - **Description:** Method to trigger current animation.
  - **Options:**
    - **onRender (Auto Play):** On enter.
    - **onHide (On Object Hide):** On leave.
    - **onClick (Click):** On click.
    - **onHover (Mouse Enter):** On mouse enter.
    - **custom (Manual Call):** Manual call.
- **initialVisibility (Hide Before Start)**

  - **Description:** Whether to hide component with this animation before trigger.
- **editAnimate (Edit Animation)**

  - **Description:** Click button to open animation editor for current animation.
- **hideAfterCompletion (Hide After Completion)**

  - **Description:** Whether to hide component after animation ends. Useful for exit-style animations.

**Methods**

- **play (Replay)**
  - **Description:** Plays current animation group from beginning. Note: Animation group is a whole; each play starts from beginning, unlike timeline animations that continue from pause point.
- **stop (Stop Playback)**
  - **Description:** Stops current animation group playback.

#### AnimationGroup (Animation Group)

`data-animate-group` component manages triggering and playback of a group of animations. It's a container that can only contain animation components.

**Properties**

- **trigger (Trigger Timing)**
  - **Description:** Selects when animation group triggers, defaults to auto-play. Can also select click, mouse enter, etc., equivalent to adding an auto event. Can also select "manual call" which requires control via event panel.
  - **Options:**
    - **onRender (Auto Play):** Triggers on object enter/load.
    - **onHide (On Object Hide):** Triggers when object is set invisible via action.
    - **onClick (Click):** Triggers on object click.
    - **onHover (Mouse Enter):** Triggers on mouse enter object.
    - **custom (Manual Call):** Manually triggered via other event actions.
- **initialVisibility (Hide Before Start)**
  - **Description:** Whether to hide object before animation group starts.
- **hideAfterCompletion (Hide After Completion)**
  - **Description:** Whether to hide object after animation group ends.

**Methods**

- **play (Replay)**
  - **Description:** Plays current animation group from beginning. Note: Animation group is a whole; each play starts from beginning, unlike timeline animations that continue from pause point.
- **stop (Stop Playback)**
  - **Description:** Stops current animation group playback.

#### UseDraggable (Drag Source Widget, Section/Component Only)

useDraggable gives any parent UI component it's attached to the ability to be dragged as a drag source. It manages data setting at drag start and state cleanup at drag end. Drag operations are complex user interactions and can only be added in Section/Component.

**Core Events**

- **@dragStart**: Triggered when user starts dragging the element with this widget attached. This is the only time `setData()` can be called on `dataTransfer`. Widget also sets `event.dataTransfer.effectAllowed` to declare allowed drag operation types, affecting mouse cursor style during drag.
- **@dragEnd**: Triggered when drag operation ends, whether drop succeeded, was cancelled, or dropped on invalid area. Event emits `event.dataTransfer.dropEffect` parameter to determine final result of drag-drop operation.

#### UseDropTarget (Drop Target Widget, Section/Component Only)

useDropTarget gives any parent UI component it's attached to the ability to serve as a drop target (receive drag-drop operations). Used to manage drop zone highlighting and data extraction from drop events.

**Core Events**

- **@dragEnter**: Triggered when dragged element first enters the area of component with this widget attached. Widget **automatically calls `event.preventDefault()`**. Also emits `event.dataTransfer.types` to check if drag data type is acceptable.
- **@dragOver**: Continuously triggered when dragged element moves within the area of component with this widget attached. **Must call `event.preventDefault()`** to allow `drop` event to trigger. Also writes `event.dataTransfer.dropEffect` to set area's preferred operation type, affecting mouse cursor style.
- **@dragLeave**: Triggered when dragged element leaves the area of component with this widget attached.
- **@drop**: Triggered when user releases mouse button over the area of component with this widget attached. Widget internally calls `event.preventDefault()` and reads `event.dataTransfer.getData()` to get data passed from drag source.

**Key Rules:**

1. Drag events (`@dragStart`, `@dragEnd`, `@dragOver`, `@drop`) must be registered on drag widgets (`<UseDraggable>`, `<UseDropTarget>`), not regular components
2. `@dragOver` must call `event.preventDefault()` for `@drop` to fire

### Module Components

#### Section (App Only)

Section defined in current project used in App. When added to component tree, it acts as a component (e.g., `<Section-MySection "mySection"> property1:value... `).

Note: Section components behave similarly to extended UI components, **strictly forbidden to add child components other than widgets**. For example, the following usage is strictly forbidden:

**Wrong Usage**:

```
<Section-ChatRoomLayout "layoutView">
-<Text-RoomTitle> value:$title //Cannot add Text and other basic UI components under Section; Section is not a container
-<Section-OnlineUsersPanel "onlinePanel"> // Forbidden to add other Sections under Section
```

Please note:

- Section's properties/methods/events must strictly follow Section's file definition, **strictly forbidden to invent Section properties/methods/events**

#### Component (App or Section)

Custom additional components defined in current project can be used in App or Section, introduced using `<Component-ComponentName>`. Note: Component is an independent component and cannot have non-widget child components added under it.

#### WebComponent (App, Section, or Component)

Project-local WebComponent modules defined in `ExtComponents/*.wc` can be used in App, Section, or Component files through `<WebComponent-ComponentName>`. A WebComponent is an opaque module component instance: it accepts only its declared public props/events/methods plus the common external frame properties allowed for module instances, and cannot have non-widget child components added under it.

Single-line example: `<WebComponent-RichChart "chart"> option:$chartOption width:"100%" height:"360px"`


## 2. Frontend Component Common Properties/Methods/Events

### Common Properties

#### Non-Read-Only Properties

**The following common properties apply to all non-structural container frontend UI components (basic UI components, layout containers):**

- **show(BOOL)**:

  - Default: true
  - Description: When set to false, component is not rendered on page (equivalent to display:none). Usually doesn't need declaration unless initial state is false.
  - Note: `show` is a VL component property, not a CSS property. Authors must use `show` for declarative visibility control and must not treat it as a raw CSS `display` authoring entrance.

#### Read-Only Properties

##### Size Properties (excluding `transform`)

- **`offsetWidth` / `offsetHeight`**: Element's full size (content + padding + border + scrollbar). When element has no scale or rotate, prefer this over `getBoundingClientRect()` method
- **`clientWidth` / `clientHeight`**: Element's internal visible size (content + padding). When element has no scale or rotate, prefer this over `getBoundingClientRect()` method
- **`scrollWidth` / `scrollHeight`**: Element's full content size (content + padding).

##### Relative Position Properties

- **`offsetLeft` / `offsetTop`**: Element position relative to **nearest positioned ancestor**.

### Common Events

!!Note: Not all native JS events can be listened to on all components. Common events only include those listed in this document.

**The following common events apply to all UI components (frontend root components, basic UI components, layout containers, structural containers):**

- `@init()`: Triggered when component initialization completes
- `@click(event)`: Click event
- `@mouseOn(event)`: Mouse enter event
- `@mouseOut(event)`: Mouse leave event
- `@mouseDown(event)`: Mouse down event
- `@mouseUp(event)`: Mouse up event

**The following common events apply to layout containers, structural containers:**

- `@scrollToBottom()`: Triggered when container scrolls to bottom (or rightmost for horizontal layout)
- `@scrollToTop()`: Triggered when container scrolls to top (or leftmost for horizontal layout)
- `@clickOutside()`: Triggered when clicking outside container area

### Common Methods

**The following methods apply to all UI components (basic UI components, layout containers, structural containers)**:

- `focus()`, `blur()`: Gain/lose focus. When adding these methods to a component, system automatically sets its tabindex property to -1, no manual specification needed;
- `getBoundingClientRect()`: Returns an object containing element's real-time position and full size relative to **viewport**. Note: When clientWidth/Height or offsetWidth/Height can be used, prefer those to reduce overhead and simplify code.

**The following common methods apply to all layout containers, structural containers:**

- `scrollToBottom()`: Scroll to bottom. When direction is column, scrolls to bottom; when row, scrolls to rightmost, and so on;
- `scrollToTop()`: Scroll to top
- `scrollTo(left,top,speed)`: Scroll to position, left and top are distances from left and top, speed is value from 1-100 where 1 is fastest, 100 slowest

## 3. Components Used in ServiceDomain

All backend components must first be created under ServiceDomainRoot component before use. (Define before use principle)

### Backend Data Components

#### VirtualTable (Virtual Table)

Component for accessing and operating database tables in backend services.

**Methods**

| Method              | Parameters                                    | Return Value                           | Description                             |
| ------------------- | --------------------------------------------- | -------------------------------------- | --------------------------------------- |
| `select`          | conditions, orderBy, outputRows, outputFields | success, message, dataArray, structure | Query data                              |
| `insert`          | valueObj({})                                  | success, message, dataId, dataObj      | Insert data (system fields auto-filled) |
| `update`          | conditions, set, limit                        | success, message, affect               | Update data                             |
| `batchUpdateById` | source([{}])                                  | success, message, affect               | Batch update by ID                      |
| `count`           | conditions                                    | success, message, count                | Count records                           |
| `delete`          | conditions, limit                             | success, message, affect               | Delete data (soft delete recommended)   |

##### **Conditions Array (conditions)**

- **Format**: `[[field, operator, value], ...]`
- **Operators**: `eq`(=), `neq`(!=), `gt`(>), `gte`(>=), `lt`(<), `lte`(<=), `in`, `nin`, `contains`(%val%), `startsWith`(val%), `endsWith`(%val), `isNull`, `notNull`
- **OR Conditions**: `["OR", [condition1], [condition2], ...]`

**`isNull` / `notNull` Operators:**

One-way operators (no `value` field):
- `["field","isNull"]` — matches records where field IS NULL
- `["field","notNull"]` — matches records where field IS NOT NULL
- `*isNull` / `*notNull` are **not supported**; whether to push the condition is controlled by caller logic

**Dynamic Filtering with `*` Prefix Operators:**

When you need to conditionally ignore a filter based on whether a value is provided, use `*` prefix operators (like `*eq`, `*contains`). This is especially useful for optional search parameters.

`*op` operators automatically **skip the condition** when any of the following is true:
1. `value == null`
2. `value` is a STRING and `value.trim() == ""` (empty or whitespace-only)
3. Operator is `*in` or `*nin` and `value` is an empty array `[]`

When none of the above apply, the filter executes normally.

**Explicit null / empty value queries (non-`*` operators):**
- Query empty string: `["field","eq",""]`
- Query NULL: `["field","isNull"]`

**Examples**:

```vl
# Optional keyword — skipped when null, empty string, or whitespace-only
[["name","*contains",null]]        # Ignored
[["name","*contains",""]]          # Ignored
[["name","*contains","   "]]       # Ignored
[["name","*contains","prod"]]      # Applied

# Optional set filter — skipped when empty array
[["status","*in",[]]]              # Ignored
[["status","*in",["running"]]]     # Applied

# Explicit null / empty queries (non-* operators)
[["name","eq",""]]                 # Matches records with empty string
[["name","isNull"]]                # Matches records where name IS NULL
```

Use `*` prefix operators for optional search parameters. `*` operators auto-ignore null, empty/whitespace strings, and empty arrays (`*in`/`*nin`). Use non-`*` operators (`eq`, `isNull`) for explicit empty/null queries.

##### **Order Array (orderBy)**

- **Format**: `[[field, "asc"/"desc"], ...]`
- **No Sorting**: `null`

##### **Pagination Array (outputRows)**

- **Format**: `[offset, limit]`
- **No Pagination**: `null`

##### **Update Operation Array (set)**

- **Format**: `[[field, operationType, value], ...]`
- **Operation Types**: `"set"`(set value), `"inc"`(increment/decrement), `"mul"`(multiply), `"append"`(append to array)

##### System Fields

**System Fields**: `_id`, `_user`, `_create`, `_update` are auto-managed by system and cannot be specified during insert/update

**`_user` and unauthenticated requests**: When a write operation is performed by an unauthenticated user, `_user` is automatically set to `null`. No placeholder value (such as `"anonymous"`) is filled in. Tracking anonymous visitors is a business-layer concern. If needed, define a dedicated field on the business table (e.g., `guestToken(STRING)`) and write to it explicitly in the SERVICE logic. The platform identity system only covers users authenticated through TokenIssuer.

##### **Quick Examples**

```vl
# Query - with conditions, sorting, pagination
<VirtualTable-Users "userTable">.select([["status","eq","active"], ["age","gte",18]], [["_create","desc"]], [0, 20], ["id","name","email"]) -> _result

# Insert - system fields auto-filled
<VirtualTable-Users "userTable">.insert({name:"John Doe", email:"JohnDoe@gmail.com"}) -> _result
# _id, _user, _create, _update are auto-filled by system

# Update - single record
<VirtualTable-Users "userTable">.update([["id","eq",123]], [["name","set","New Name"]], 1) -> _result

# Batch update
<VirtualTable-Products "productTable">.batchUpdateById([{"_id":101, "price":299}, {"_id":102, "price":199}]) -> _result

# Count
<VirtualTable-Orders "orderTable">.count([["status","eq","pending"]]) -> _result
```

##### **Advanced Examples**

```vl
# OR condition + fuzzy search
<VirtualTable-Posts "postTable">.select([["OR", ["title","contains","VL"], ["content","contains","VL"]], ["status","eq","published"]], null, [0,10]) -> _result

# ✅ Dynamic filtering (recommended) - use * prefix to auto-ignore null / empty / whitespace
<VirtualTable-Users "userTable">.select([["name","*contains",$keyword], ["role","*eq",$role]], null, [0,20]) -> _result
# Automatically ignores when $keyword/$role is null, empty string, or whitespace-only

# ❌ Dangerous approach - manual condition concatenation (similar to SQL injection risk)
-_conditions([]) = [["role","eq","admin"]]
-IF $keyword != null
--_conditions.push(["name","contains",$keyword])
-<VirtualTable-Users "userTable">.select(_conditions, null, [0,20]) -> _result
```

#### ServerCache (Cache Table)

Cache table for cached data read/write. Unlike VirtualTable, cache table doesn't need to bind to an entity table; system automatically creates and maintains corresponding cache data storage.

**Methods**

**Single Value Cache Operations**

- `set(key, value, expire)`: Sets single value cache. Updates if record exists, creates if not. Return: `success(BOOL)`, `message(STRING)`
- `get(key)`: Gets single value type cache record. Return: `value(STRING | NUMBER)`, `success(BOOL)`, `message(STRING)`
- `getMultiple(keys)`: Gets multiple cache records. Return: `values([STRING | NUMBER])`, `success(BOOL)`, `message(STRING)`
- `insertIfNotExists(key, value, expire)`: Inserts single value cache. Fails if key already exists. Return: `success(BOOL)`, `message(STRING)`
- `updateIfExists(key, value, expire)`: Updates single value cache. Fails if key doesn't exist. Return: `success(BOOL)`, `message(STRING)`
- `incrBy(key, value)`: Increments numeric cache record value (can be negative, can include decimals). Return: `value(NUMBER)`, `success(BOOL)`, `message(STRING)`

**Expiration Time Management**

- `setExpire(key, expire)`: Sets cache record expiration duration (applies to single value or set types). Return: `success(BOOL)`, `message(STRING)`
- `getTimeToLive(key)`: Gets cache record's remaining time to live. Returns -1 if no expiration set. Return: `ttl(INT)`, `success(BOOL)`, `message(STRING)`

**Delete Operations**

- `delete(key)`: Deletes cache record (applies to single value or set types). Return: `success(BOOL)`, `message(STRING)`

**Set Cache Operations**

- `addToSet(key, values)`: Creates set type record or adds elements to existing set. All elements are auto-converted to string type after adding. Return: `addedNumber(INT)`, `success(BOOL)`, `message(STRING)`
- `removeFromSet(key, values)`: Removes one or more elements from set. Return: `removedNumber(INT)`, `success(BOOL)`, `message(STRING)`
- `getSetMembers(key)`: Gets all elements in set type record. Return: `members(ARRAY)`, `success(BOOL)`, `message(STRING)`
- `isSetMember(key, value)`: Checks if an element is in the set. Return: `isMember(BOOL)`, `success(BOOL)`, `message(STRING)`

**Cache Management**

- `scanKeys(cursor, limit)`: Gets list of all record key names in cache database, supports pagination. Return: `keys(ARRAY)`, `cursor(STRING)`, `success(BOOL)`, `message(STRING)`

### Backend Functional Components

#### TokenIssuer (Server-side Authentication & Session Management)

System authentication component for token issuance, session tracking, and user identity refresh. Supports multi-device sessions, concurrent login control, and instant invalidation via versioning.

**Methods**

**Token Issuance**

* `generateLoginToken(userId, authRealmCode, subjectInfo, userInfo, maxConcurrent, duration)`:Generates and issues a new login token. If the maximum concurrent session limit is exceeded, the oldest session is revoked. **Return**: `success(BOOL), token(STRING), tokenId(STRING), expiresAt(TIMESTAMP), message(STRING)`
* **userId**: Unique user identifier.
* **authRealmCode**: Auth realm for the login session. Authors should explicitly pass `"default"` or the project-specific realm code. Pass `NULL` to use the platform default realm. Tokens are only valid within this realm.
* **subjectInfo**: Auth-relevant user attributes at the auth-realm level (e.g. department, attributes). Written to runtime cache; does not include project roles.
* **userInfo**: Non-sensitive user display data (e.g. name, avatar). Embedded in the JWT only, not used for authorization.
* **maxConcurrent**: Maximum allowed concurrent sessions for the user; oldest sessions are removed when exceeded.
* **duration**: Login session duration in seconds (e.g., `7200` for 2 hours). Pass `NULL` to use the platform default duration.

**Token Revocation**

* `revokeToken(tokenId)`: Revokes a single token immediately. Return: `success(BOOL), message(STRING)`
* `revokeAllUserTokens(userId, authRealmCode)`: Revokes all tokens of a user within the given scope (user-level versioning). Return: `success(BOOL), affectedCount(INT)`
* `revokeAllTokens(authRealmCode)`: auRealm-level global revoke (emergency). Return: `success(BOOL)`

Compatibility: `revokeUserTokens(userId, authRealmCode)` remains accepted as the old method name and has the same runtime behavior. New VL code SHOULD use `revokeAllUserTokens`.

**User & Token Refresh**

* `refreshUserInfo(userId, authRealmCode, newUserInfo)`: Refreshes userInfo for all active tokens of the user. Return: `success(BOOL), message(STRING)`

**Session Query**

* `getUserSessions(userId, authRealmCode)`: Gets all active sessions (device list). Return: `success(BOOL), sessions([{}])`
* `getCurrentSession(authRealmCode)`: Gets current session detail (includes deviceInfo). Return: `success(BOOL), session({userInfo({}), deviceInfo({}), tokenId(STRING), expiresAt(TIMESTAMP)})`

**Notes**

* `subjectInfo` is for middleware authorization only (Redis), not exposed to frontend
* `userInfo` is embedded in JWT for frontend display
* Token validity is enforced via user-level and global version comparison

#### UserStore (Official User Identity Source)

Backend functional component for hosting the official user identity source. It is responsible for:

- User principal creation and retrieval
- Account/password credential verification
- Third-party identity lookup and binding
- Returning standardized identity objects for `TokenIssuer` to issue login tokens

It is NOT responsible for:

- Login state, cookie, or session issuance and verification
- Current login state restoration
- User role and permission assignment

**Declaration:**

`UserStore` is a backend functional component and MUST be declared in `# Backend Tree`.

```vl
<UserStore "userStore"> sourceTable:AuthUsers identitiesTable:AuthUserIdentities
```

Rules:

1. `UserStore` MUST declare both `sourceTable` and `identitiesTable`
2. `sourceTable` value MUST be `AuthUsers`; `identitiesTable` value MUST be `AuthUserIdentities`
3. Both `Table-AuthUsers` and `Table-AuthUserIdentities` MUST be declared with field definitions in the project `.vdb` file
4. A project supports at most one `UserStore` instance
5. `UserStore.userId` is uniformly `STRING` across all methods

**Methods:**

All methods use VL author-side positional parameter syntax.

- `createUserByPassword(account, password, nickname, avatar, email, phone, extra)`: Creates a user with account/password credentials. Return: `success(BOOL), message(STRING), userId(STRING), user({})`
- `verifyUserByPassword(account, password)`: Verifies user credentials by account/password. Return: `success(BOOL), message(STRING), userId(STRING), user({}), subjectInfo({})`
- `getUserById(userId)`: Retrieves user by ID. Return: `success(BOOL), message(STRING), user({})`
- `updateBasicProfile(userId, nickname, displayName, avatar, extra)`: Updates user basic profile. Return: `success(BOOL), message(STRING), user({})`
- `createOrGetUserByOAuth(provider, providerUserId, providerUnionId, nickname, avatar, email, phone)`: Creates or retrieves a user via third-party OAuth identity. Return: `success(BOOL), message(STRING), userId(STRING), user({}), created(BOOL)`
- `bindOAuthIdentity(userId, provider, providerUserId, providerUnionId)`: Binds a third-party OAuth identity to an existing user. Return: `success(BOOL), message(STRING), bound(BOOL)`
- `clearAllUsers()`: Clears all user data. **Test/admin use only; must not be exposed to end users in production.** Clearing users does not automatically revoke issued tokens; call `TokenIssuer.revokeAllTokens(authRealmCode)` separately if needed. Return: `success(BOOL), message(STRING)`

All methods uniformly return `success` and `message`. On success, methods may additionally return `userId`, `user`, `subjectInfo`, `created`, or `bound` as applicable.

**Profile update refresh chain:**

After calling `updateBasicProfile(...)`, the database record is updated but the JWT `userInfo` is not automatically refreshed. To complete the identity refresh chain, three steps are required:

1. Call `<UserStore "userStore">.updateBasicProfile(userId, nickname, NULL, NULL, NULL) -> _updateResult`
2. Call `<TokenIssuer "tokenIssuer">.refreshUserInfo(userId, "default", _updateResult.user) -> _refreshResult`
3. On the frontend, after the SERVICE succeeds, call `<ClientUtils>.refreshCurrentUser() -> _refreshResult`

`updateBasicProfile(...)` only writes to the database. `refreshUserInfo(...)` and `refreshCurrentUser()` are the required refresh chain, not optional enhancements.

**Method call examples:**

```vl
-<UserStore "userStore">.createUserByPassword(username, password, nickname, NULL, NULL, NULL, NULL) -> _signupResult
```

```vl
-<UserStore "userStore">.verifyUserByPassword(username, password) -> _loginResult
```

```vl
-<UserStore "userStore">.createOrGetUserByOAuth("google", _oauthInfo.sub, NULL, _oauthInfo.name, _oauthInfo.picture, _oauthInfo.email, NULL) -> _oauthResult
```

**Relationship with `TokenIssuer`:**

`UserStore` and `TokenIssuer` MUST be used in separate layers:

- `UserStore` does not issue login tokens
- `TokenIssuer` does not verify account/password credentials
- `UserStore` output feeds directly into `TokenIssuer` input

Recommended chain:

```vl
-<UserStore "userStore">.verifyUserByPassword(username, password) -> _loginResult
```

```vl
-<TokenIssuer "tokenIssuer">.generateLoginToken(_loginResult.userId, "default", _loginResult.subjectInfo, _loginResult.user, 5, NULL) -> _tokenResult
```

#### ServerApi (Backend API Request Client)

Properties and methods are identical to Frontend API Request Client component.

#### MQ (Message Queue)

Receives messages sent by services/methods for asynchronous processing. Used for basic async processing and request peak shaving.

**Properties**

- struct({}): Message structure, a single-layer object, e.g., `<MQ-DataStream "dataStream"> struct:{structField1:STRING,structField2:INT}`

**Events**

- @Message(structField1, structField2...): Message received event, message processing logic can be defined in event body

**Methods**

- send(structField1, structField2...): Sends message to message queue. Note: Message structure must strictly follow message queue component's struct property. E.g., `-<MQ-DataStream "dataStream">.send(_data,_num)`

#### Encryption (Cryptographic Operations)

Cryptographic component for hash generation, RSA signing, verification, encryption, and decryption operations, as well as symmetric encryption/decryption.

**Methods**

**Hash Operations**

- `genHashCode(text)`: Generates hash code from text. **Return**: `hashCode(STRING)`
- `verifyHashCode(text, hash)`: Verifies text against hash code. **Return**: `success(BOOL)`

**RSA Signature Operations**

- `signWithRSA(data, type, privateKey)`: Signs data with RSA private key. **Return**: `sign(STRING)`
  - **type**: Signature algorithm type. Options: `RSA`, `RSA2`
- `verifySignWithRSA(sign, data, type, publicKey)`: Verifies RSA signature with public key. **Return**: `success(BOOL), message(STRING)`
  - **type**: Signature algorithm type. Options: `RSA`, `RSA2`

**RSA Encryption/Decryption Operations**

- `asymmetricEncryption(plaintext, ciphertextEncoding, plaintextEncoding, publicKey)`: Encrypts plaintext with RSA public key. **Return**: `data(STRING)`
  - **ciphertextEncoding**: Encoding format for the encrypted output. Options: `base64`, `hex`. Default: `base64`
  - **plaintextEncoding**: Encoding format of the input plaintext. Options: `base64`, `hex`, `raw`. Default: `raw`
- `asymmetricDecryption(ciphertext, plaintextEncoding, ciphertextEncoding, privateKey)`: Decrypts ciphertext with RSA private key. **Return**: `data(STRING)`
  - **plaintextEncoding**: Encoding format for the decrypted output. Options: `base64`, `hex`, `raw`. Default: `raw`
  - **ciphertextEncoding**: Encoding format of the input ciphertext. Options: `base64`, `hex`. Default: `base64`

**Symmetric Encryption/Decryption Operations**

- `symmetricEncryption(plaintext, ciphertextEncoding, plaintextEncoding, key, keyEncoding, iv, ivEncoding)`: Encrypts plaintext with symmetric encryption. **Return**: `data(STRING), iv(STRING), key(STRING)`
  - **plaintextEncoding**: Encoding format of the input plaintext. Options: `base64`, `hex`, `raw`. Default: `raw`
  - **ciphertextEncoding**: Encoding format for the encrypted output. Options: `base64`, `hex`. Default: `base64`
  - **keyEncoding**: Encoding format of the encryption key. Options: `base64`, `hex`, `raw`. Default: `raw`
  - **ivEncoding**: Encoding format of the initialization vector. Options: `base64`, `hex`, `raw`. Default: `raw`
- `symmetricDecryption(ciphertext, plaintextEncoding, ciphertextEncoding, key, keyEncoding, iv, ivEncoding)`: Decrypts ciphertext with symmetric decryption. **Return**: `data(STRING), iv(STRING), key(STRING)`
  - **plaintextEncoding**: Encoding format for the decrypted output. Options: `base64`, `hex`, `raw`. Default: `raw`
  - **ciphertextEncoding**: Encoding format of the input ciphertext. Options: `base64`, `hex`. Default: `base64`
  - **keyEncoding**: Encoding format of the decryption key. Options: `base64`, `hex`, `raw`. Default: `raw`
  - **ivEncoding**: Encoding format of the initialization vector. Options: `base64`, `hex`, `raw`. Default: `raw`

#### Email (Email Service)

Email service component for sending single or multiple emails with optional attachments.

**Methods**

- `sendEmail(receiver, title, body, sendFormat, attachments)`: Sends a single email. **Return**: `success(BOOL), message(STRING)`
  - **receiver**: Email recipient address.
  - **title**: Email subject line.
  - **body**: Email content.
  - **sendFormat**: Email format. Options: `text`, `html`. Default: `text`
  - **attachments**: List of attachments. Format: `[{key, value}]`
- `sendEmails(receivers, title, body, sendFormat, attachments)`: Sends multiple emails. **Return**: `success(BOOL), message(STRING)`
  - **receivers**: Array of email recipient addresses.
  - **title**: Email subject line.
  - **body**: Email content.
  - **sendFormat**: Email format. Options: `text`, `html`. Default: `text`
  - **attachments**: List of attachments. Format: `[{key, value}]`

### Backend System Method Components (No Declaration Needed in Component Tree)

Backend system method classes have only one global instance and can be used without defining in component tree.

#### ServerUtils (Backend Utility Class)

System method class callable in backend services. Not allowed to call from frontend.

**Methods:**

- `getCurrentTime()`: Gets server's current time. Returns server system time, useful for time synchronization or getting standard time.
- `delay(second)`: Executes wait operation on server side, pausing for specified seconds.
