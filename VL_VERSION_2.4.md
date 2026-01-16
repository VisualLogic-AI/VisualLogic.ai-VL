# VL Syntax Reference

## Version Declaration

```vl
// VL_VERSION: version_number
```

This version declaration must be placed in the first line comment of all files to ensure the parser uses the correct version rules.

Current version: `// VL_VERSION:2.4`

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
├─ ExtComponents/         # Component files (internal encapsulated components, usable in Section/App)
│   ├─ InputField.cp
│   └─ DataAuth.cp
├─ Theme/                 # Theme files (project visual standards: colors, fonts, spacing tokens and preset component style definitions, only one Theme file per project)
│   └─ MyProject.vth   
└─ Apps/                  # App files (one file per application)
    ├─ ShopApp.vx   
    └─ AdminApp.vx
```

All files participate in the final project code compilation.

### File Cross-References

VL code files support a certain degree of cross-referencing. Please strictly follow these rules:

|                             | App | Section | ServiceDomain | Component |
| --------------------------- | --- | ------- | ------------- | --------- |
| App can reference           | ❌  | ✅      | ❌            | ✅        |
| Section can reference       | ❌  | ❌      | ✅            | ✅        |
| Component can reference     | ❌  | ❌      | ❌            | ❌        |
| ServiceDomain can reference | ❌  | ❌      | ❌            | ❌        |

### File Sections and Structure Examples

VL files follow strict section division. **All sections must appear in order and only once**; otherwise compilation will fail.

#### 1. `.vx` — App (Application Entry)

**Core Responsibilities**: Route management, Section orchestration, cross-module coordination

**Required Sections**: `# SysConfig`, `# Frontend Tree`, `# Frontend Event Handlers`

```vl
// VL_VERSION:2.2
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
--<Section-Sidebar "sidebar"> flex:"0 0 240px"
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

**Layout Control Examples**: Header `flex:"0 0 64px"`, Sidebar `flex:"0 0 240px"`, Main `flex:"1"`

#### 2. `.sc` — Section (Business Module)

**Core Responsibilities**: Business logic, data interaction, ServiceDomain calls

**Required Sections**: `# Frontend Tree`, `# Frontend Event Handlers`

```vl
// VL_VERSION:2.2
// Preview: width:100% min-height:400px
<Section-ProductList "root"> padding:"24px" background-color:"#ffffff" border-radius:"8px"

# Frontend Public Props
$categoryId(INT) = 0

# Frontend Public Events
EVENT @productSelected(productId(INT))

# Frontend Public Methods
METHOD_PUB RefreshWithKeyword(keyword(STRING)); RETURN {success:BOOL}
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
-IF !_result.success
--RETURN {success:false}
-$products = _result.data
-RETURN {success:true}

# Frontend Pipeline Funcs
</Section-ProductList>
```

#### 3. `.cp` — Component (Pure UI Component)

**Core Responsibilities**: Pure UI display, reusable components, no business logic

**Required Sections**: `# Frontend Tree`, `# Frontend Event Handlers`

```vl
// VL_VERSION:2.2
// Preview: width:280px height:360px
<Component-ProductCard "root"> padding:"16px" background-color:"#ffffff" border-radius:"8px" border-width:"1px" border-style:"solid" border-color:"#e0e0e0" cursor:"pointer"

# Frontend Public Props
$product({id:INT,name:STRING,price:FLOAT,image:STRING}) = {id:0,name:"",price:0,image:""}

# Frontend Public Events
EVENT @cardClick(productId(INT))

# Frontend Derived Vars
$formattedPrice(STRING) = ("¥" + $product.price.toFixed(2))

# Frontend Tree
<Col-CardContent> gap:"12px"
-<Image-ProductImage "thumb"> sourceUri:$product.image width:100% height:"200px" border-radius:"4px"
-<Text-ProductName "title"> value:$product.name StyleClass:TextLarge
-<Text-ProductPrice "price"> value:$formattedPrice StyleClass:TextPrimary color:"#ff4d4f"

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

#### 4. `.vs` — ServiceDomain (Backend Service Domain)

**Required Sections**: `# Backend Tree`, `# Services`

```vl
// VL_VERSION:2.2
<ServiceDomain-Doc>

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

#### 5. `.vdb` — Database (Database Structure)

```vl
// VL_VERSION:2.2
<Database-ProjectName>
<Table-Documents> data:[{"_id":1,"title":"Doc1","_create":"2024-01-15 09:30:00"}]
-<Field-title> type:STRING notNull:true
-<Field-content> type:STRING
-<Index-TitleIdx> type:UNIQUE fields:["title"]
<Relation-User&Docs> users._id<<documents._user
</Database-ProjectName>
```

#### 6. `.vth` — Theme (Theme Configuration)

**Rules**: Singleton file, can only add Tokens but not delete/modify; In Design Tokens, each line represents a Token Group, and a Token Group consists of multiple Tokens; Component Styles defines preset style classes for all basic components, style values preferentially use Design Tokens.

```vl
// VL_VERSION:2.2
<Theme-ProjectName>
# Design Tokens
<Color-Brand> colorBrandPrimary:#0052D9 colorBrandPrimaryHover:#366EF4 ...
<Size-Height> heightXs:20px heightSm:24px ...
...

# Component Styles
<Button-Primary> background-color:--colorBrandPrimary padding:--spacing4
-<StateStyle-Hover> trigger:hover background-color:--colorBrandHover
</Theme-ProjectName>
```

### App, Section, Component Responsibility Division

#### Three-Tier Architecture Overview

| Tier                | File Type | Core Responsibilities                                            | Can Contain          |
| ------------------- | --------- | ---------------------------------------------------------------- | -------------------- |
| **App**       | `.vx`   | App entry, route management, global state, Section orchestration | Section, Component   |
| **Section**   | `.sc`   | Business module, data interaction, complete business logic       | Component (internal) |
| **Component** | `.cp`   | Pure UI reuse, no business logic, communicates via Props/Events  | None                 |

**Hierarchical Relationship:**

```
App (.vx)
├── Page-Home
│   ├── Section-Header (.sc)      // Section uses Component internally
│   ├── Section-ProductList (.sc) // Section uses Component internally
│   └── Section-Footer (.sc)
└── Page-Detail
    └── Section-ProductDetail (.sc)
```

**Important Rules:**

- Section and Component are both **independent components** when used, similar to extended UI components
- **Strictly forbidden** to nest other components under Section/Component (except widgets)
- Component can only be used when **internally defined** in Section/Component, cannot be nested externally

#### App (.vx) Responsibility Boundaries

**Core Responsibilities:** Route management, Section orchestration, cross-module coordination

**Should Do:**

- ✅ Define Pages and routes
- ✅ Add and arrange Section/Component
- ✅ Listen to Section/Component public events
- ✅ Call Section/Component public methods
- ✅ Manage cross-Section global state
- ✅ Control Section/Component position, size, visibility

**Forbidden:**

- ❌ Directly call ServiceDomain
- ❌ Add `<For>`, `<TreeFor>` and other loop containers
- ❌ Contain business logic
- ❌ Directly add basic UI components
- ❌ Nest child components under Section/Component

**Typical Scenarios:** Page routing, inter-Section data passing, global state management, layout control

#### Section (.sc) Responsibility Boundaries

**Core Responsibilities:** Business module, data interaction, complete business logic

**Should Do:**

- ✅ Call ServiceDomain to fetch/submit data
- ✅ Contain complete business logic
- ✅ Use Components internally to build UI
- ✅ Define public properties/events/methods to communicate with App
- ✅ Use `<For>`, `<If>` and other logic containers
- ✅ Define internal styles on root component (padding, background, etc.)

**Forbidden:**

- ❌ Nest other Sections
- ❌ Directly call other Section's methods
- ❌ Handle route navigation
- ❌ Define external layout properties on root component (width, height, margin, flex, etc.)

**Typical Scenarios:** User list, product details, comment section, search module

#### Component (.cp) Responsibility Boundaries

**Core Responsibilities:** Pure UI display, reusable components, no business logic

**Should Do:**

- ✅ Receive Props and render UI
- ✅ Communicate with parent via public events
- ✅ Handle pure UI interactions (hover, animations)
- ✅ Use `<If>` for UI conditional rendering
- ✅ Define internal styles on root component

**Forbidden:**

- ❌ Call ServiceDomain
- ❌ Contain business logic
- ❌ Nest other Components
- ❌ Define external layout properties on root component

**Typical Scenarios (Important):**

- Common UI: buttons, inputs, cards, labels, avatars
- **Confirmation dialogs**, **error messages**, **loading states**, **empty states**
- Pagination components, image galleries, rating components

**Encapsulation Principle:**

> Any UI functional module not directly related to specific business logic should be encapsulated as a Component

#### Comparison Table

| Dimension                             | App                  | Section          | Component          |
| ------------------------------------- | -------------------- | ---------------- | ------------------ |
| **Call ServiceDomain**          | ❌                   | ✅               | ❌                 |
| **Contain Section**             | ✅                   | ❌               | ❌                 |
| **Contain Component**           | ✅                   | ✅               | ❌                 |
| **Use For/If**                  | ❌                   | ✅               | ✅                 |
| **Can nest children when used** | N/A                  | ❌               | ❌                 |
| **Typical Scenarios**           | Routes, global state | Business modules | Common UI, dialogs |

#### Style Property Responsibility Division

**Core Principle:** Outer file controls "position and space", Section/Component definition file controls "appearance and content". (Outer file means App file, or if Section embeds Component, then relative to Component, this Section file is the outer file)

| Property Type              | Specific Properties                        | Outer | Section/Component Internal |
| -------------------------- | ------------------------------------------ | ----- | -------------------------- |
| **External Size**    | width, height                              | ✅    | ❌                         |
| **Flex/Grid**        | flex, grid-column, etc.                    | ✅    | ❌                         |
| **External Spacing** | margin, margin-*                           | ✅    | ❌                         |
| **Positioning**      | position, top, left, etc.                  | ✅    | ❌                         |
| **Visibility**       | show                                       | ✅    | ❌                         |
| **Internal Spacing** | padding, padding-*                         | ❌    | ✅                         |
| **Visual Styles**    | background-*, border-*, box-shadow, etc. | ❌    | ✅                         |
| **Content Overflow** | overflow, overflow-*                       | ❌    | ✅                         |
| **Size Constraints** | min-width, max-width, min-height, etc.     | ⚠️  | ✅                         |

**Preview Size Annotation:** Section/Component root component does not define external layout properties, only annotates preview values via comments

```vl
// Preview: width:240px height:100vh
<Section-Sidebar "root"> padding:"16px" background-color:"#f5f5f5" min-width:"200px"
```

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
<Section-ProductCard "root"> width:"320px" margin:"16px" padding:"24px"
```

## Core Syntax Constraints (Globally Applicable)

VL follows strict formatting rules to ensure code consistency and parsability. The following constraints apply globally unless specific components or syntax have explicit exceptions:

#### Single-Line Rule (Mandatory)

Many definitions and statements **must** be completed on a single line; line breaks are not allowed. This includes but is not limited to:

* Variable definitions (global `$var(...) = ...`, local `_var(...) = ...`, `_var({}) = {}`, `_var([]) = []`).
* Method, service, event, pipe definition lines (i.e., the first line, including all parameters and `RETURN` declarations).
* All `RETURN {...}` (for METHOD/SERVICE) or `RETURN value` (for PIPE) statements.
* All assignment statements (`$var = ...`, `_var = ...`).
* Object and array initialization assignments (even if structurally complex).
* Component property definitions on a single line (even if multiple properties separated by spaces, still on the same line).

Violating this rule will cause compilation errors. The following sections will not repeat "single-line rule" for each applicable case; developers should remember this global constraint.

#### No Leading Spaces and Flush Output Rule (Mandatory)

* All `-` (minus/hyphen) symbols representing hierarchical structure **must be flush left**; no spaces are allowed before them.
* In fact, in VL syntax, as long as a line has content, there should be **no spaces before any content** (except JSON code blocks or specific embedded languages).

#### Semicolon (;) Usage (Strict)

* Semicolons are also used as separators in `FOR` counting loop control parts (e.g., `FOR (_i(INT)=0; _i<10; _i++)`).
* **Forbidden** to use semicolons at the end of ordinary code lines (such as variable assignments, method calls, RETURN statements).
* **Forbidden** to use semicolons between style properties or after the last style property.

#### String Escaping

**Strictly forbidden to use any `\` escape character! Strictly forbidden to use `\` escape character anywhere! Check and remove all `\` escape characters before output!**

Important things said three times! Otherwise it will cause serious compilation errors!!

#### Indentation

VL uses `-` without spaces to represent indentation:

* **Top-level definitions (no indentation):** Root components (`<Section-..>`), section headings (`# Frontend Global Vars`, `# Services`, `# Frontend Tree`, etc.), and method/service/event/pipe definition lines (`METHOD...`, `SERVICE...`, `EVENT...`, `PIPE...`) are usually at the leftmost position of the code file, without leading `-`.
* **Frontend Tree:**
  * Components added directly under root are written without leading `-`.
  * Child components add one `-` at the beginning for each deeper level. For example:
    ```vl
    <Block-Header>
    -<Text-Title> value:"Title"
    <Block-Content>
    -<Button-Submit> value:"Submit"

    <VirtualTable-Users> ..
    -<Field-_id> ... // Field is VirtualTable child component
    ```
* **Inside methods/event handlers:**
  * Method/event handler body code blocks must all be indented at least one level (starting with `-`).
  * Control flow structures (`IF`, `FOR` blocks) introduce deeper indentation levels.

#### Section Markers and Comments

`#` represents standard section structure markers (such as `# Frontend Tree`, `# Frontend Default Styles`). Note: please strictly follow the preset section structure and order according to the current file development specification.

`//` represents inline comments and line comments.

`/*` and `*/` represent block comments.

Example:

```vl
// VL_VERSION:2.2

<Section-antUploadDragger>
/*
This module is mainly used to simulate the UploadDragger component in Ant Design.
*/

# Frontend Global Vars
$uploadedFiles([{uid:STRING,name:STRING,status:STRING,url:STRING,size:INT}]) = []

</Section-antUploadDragger>
```

## Naming Conventions

#### File Naming:

All file names are uniformly PascalCase and must match the root component:

`UserAuth.vs` file, root component is:

```
<ServiceDomain-UserAuth>
File body
</ServiceDomain-UserAuth>
```

#### **Component Instance Naming**:

- Component names use **PascalCase** (e.g., `UserCard`, `LoginButton`, `NoteItemCard`, `AddButton`).
- Component names should reflect component purpose.
- Component IDs use camelCase. Component IDs should reflect component purpose and type, e.g., `<Button-Submit "submitButton">`, `<Block-ModuleRoot "root">`, `<VirtualTable-Users "userTable">`

#### **Variable Naming**

- **Frontend global variables** (`# Frontend Global Vars`): `$` prefix + **camelCase** (e.g., `$currentPage`, `$userData`).
- **Derived variables** (`# Frontend Derived Vars`): `$` prefix + **camelCase** (e.g., `$fullName`, `$canSubmit`).
- **Local variables created in methods** (`_varName(TYPE) = ...` or `_varName({}) = {}` or `_varName = []`): uniformly use `_` prefix + **camelCase** (e.g., `_result`, `_tempData`). **Must** declare type or infer through empty structure, **no** `let` keyword.
- **Loop variables**:
  - Loop variables are a "**special local variable**" that can be used directly without definition.
  - `FOR...IN` loops: Loop item variables and index variables **must** use `_` prefix + **camelCase**. Index variables should use `_indexX` (X starts from 0, e.g., `_index0`, `_index1`). Loop item variables can be named based on context (e.g., `_user`, `_product`, `_noteItem`) or use `_itemX` for generic iteration (e.g., `_item0`, `_item1`, `_item2`). Example: `FOR (_user, _index0) IN $userList` or `FOR (_item0, _index0) IN dataArray`.
  - `FOR (...)` counting loops: Loop control variables (like `i`, `j`, `k` or more meaningful names like `counter`) **must** use `_` prefix + **camelCase** (e.g., `_i`, `_j`, `_k`, `_loopCounter`). Declare in `FOR` parentheses using `_varName(TYPE) = initialValue` (e.g., `FOR (_i(INT) = 0; _i < 10; _i++)`).

#### **Method/Function Names**:

- **Frontend public methods** (`METHOD_PUB`): **PascalCase** (e.g., `ProcessData`, `ValidateForm`, `ReloadList`)
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

* All table names use PascalCase, including Database Tables and ServiceDomain VirtualTables
* All field names use camelCase

#### Other

- **Public event definitions** (`EVENT @eventName`): **camelCase** (e.g., `itemSelected`, `formSubmitted`).

## Components

### Component Instance Creation

#### Creation Location

| Applicable Files | Definition Section | Core Purpose                                            |
| ---------------- | ------------------ | ------------------------------------------------------- |
| vx, sc, cp       | # Frontend Tree    | Frontend UI layout and functional component declaration |
| vs               | # Backend Tree     | Backend component declaration                           |

#### Component Definition Format

```vl
<ComponentClass-ComponentName "componentId"> functionalProp1:value1 functionalProp2:value2 StyleClass:styleClass overrideCSS1:value1...
```

**Special Notes:**

* **<> must appear in pairs; absolutely cannot have only opening "<" without closing ">".**
* **Inside <> only "ComponentClass-ComponentName" and "componentId"; everything else must be outside the angle brackets.**

##### Component Elements

| Element                      | Description                                               | Naming Rule | Example                           | Notes                                                                                                |
| ---------------------------- | --------------------------------------------------------- | ----------- | --------------------------------- | ---------------------------------------------------------------------------------------------------- |
| ComponentClass               | System-predefined component type                          | Fixed       | `Button`, `Text`, `Input`   | Cannot be modified, defined by system                                                                |
| ComponentName                | Developer-defined descriptive name reflecting purpose     | PascalCase  | `SubmitButton`, `UserStatus`  | Must be meaningful for understanding                                                                 |
| ComponentId                  | Unique identifier for component within file               | camelCase   | `"submitBtn"`, `"userStatus"` | Optional, only specify when reference needed,**pure static string, expressions not supported** |
| Functional Properties        | Properties defined in component documentation             | -           | `value`, `disabled`           | Strictly reference component docs, forbidden to invent properties                                    |
| StyleClass (frontend only)   | Component preset style class, supports expressions        | PascalCase  | `ButtonPrimary`, `TextTitle1` | Must be predefined in current project's Theme file                                                   |
| Override CSS (frontend only) | Standard CSS properties, can override StyleClass defaults | -           | `color`, `font-size`          | Not needed if StyleClass presets meet requirements                                                   |

**Key Distinctions:**

* **ComponentName** vs **ComponentId**:
  * ComponentName is the descriptive identifier for the component type, used for type matching in definition and reference
  * ComponentId is the unique identifier for the component instance, only used to distinguish different instances within the same file
  * When referencing, must use `<ComponentClass-ComponentName "componentId">`, cannot replace ComponentName with ID

##### ✅ Correct Examples

```vl
<Button-Submit "submitButton"> value:"Submit" disabled:(!$canSubmit) StyleClass:ButtonSubmit
<Text-Welcome> value:"Hello" StyleClass:Caption
<Image-Logo "logo"> sourceUri:$logoUrl alt:"Company Logo" StyleClass:Logo height:"40px" width:"auto"
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

* Frontend root components (`<App>`, `<Section>`, `<Component>`) also have layout container functionality; all components in `# Frontend Tree` are added under this container. Its layout function is the same as a `Col` container; style properties can be added, e.g., `<Section-ProductCard "root"> width:"320px" min-height:"400px" background-color:"#ffffff"`
* Frontend root components can add `@init()` and other common layout container events, e.g., `<App-MyApp "root">@init()`, `<Section-MySection "root">@init()`
* `<ServiceDomain>` root component currently has no properties or methods, serves only as file start declaration

#### Component Creation Order

In frontend component tree, component creation order is:

1. Functional components (no UI), such as FrontendApi, Trigger, WindowEventListener, ClientUserCenter, etc.
2. UI and container components

In backend component tree, component creation order is:

1. Functional components, such as ServerApi, etc.
2. Backend data components, such as VirtualTable

#### Component Parent-Child Relationships

When any component B is a child of another component A, add B under A with one additional indentation level from A. In VL, parent-child relationships exist in the following scenarios:

**Cases Where Child Components Are Allowed**

* All container components (UI containers, logic containers) can have other frontend components (containers, basic UI components, extended UI components, etc.) as children
* Non-UI functional components (FrontendApi, Trigger, etc.) can only be direct children of root component
* Any component with UI (UI containers, UI components) can add widgets to extend its functionality (StateStyle, Animation, UseDrag, etc.)

**Cases Where Child Components Are Strictly Limited or Forbidden**

* Non-UI functional components: strictly forbidden to add any children
* Non-container UI components (basic/extended UI components): forbidden to add any children except widgets
* Module components (Section/Component): similar to extended UI components, forbidden to add any children except widgets

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
<Button-Next "nextButton">.@click() // Content inside angle brackets strictly matches component tree
```

#### Incorrect Access Methods (Strictly Avoid)

**Error 1: Confusing ComponentName and componentId**

```vl
❌ Incorrect usage:
# Frontend Tree
<Button-BackButton "backToProjects"> value:"← Project List"

# Frontend Event Handlers
<Button-BackToProjects "backToProjects">.@click()
// Error: Used ID "backToProjects" to replace ComponentName "BackButton"

✅ Correct usage:
# Frontend Tree
<Button-BackButton "backToProjects"> value:"← Project List"

# Frontend Event Handlers
<Button-BackButton "backToProjects">.@click()
// Correct: Keep ComponentClass and ComponentName consistent
```

**Error 2: Directly assigning component itself to a variable**

```
❌ Incorrect usage:
_currentObj = <Block-ScrollItem "scroller"> // In VL, components can directly access their read-only properties; no need to first store the object in a local variable like document.getElementById()
_distance = _currentObj.scrollTop
✅ Correct usage:
_distance = <Block-ScrollItem "scroller">.scrollTop
```

**Error 3: Accessing component styles (no .style access method exists)**

```
❌ Incorrect usage:
-_color = <Block-Container "box">.style.color // Component styles cannot be accessed in events or methods
-<Block-Container "box">.style.color = "red" // Component styles cannot be modified in events or methods

✅ Correct usage:
// Define variable binding in component tree
<Block-Container "box"> color:"$color"
// Modify variable in event or method
-$color = "red"
```

```
❌ Incorrect usage: Controlling component visibility via display property
-_userSubMenu = <Col-UserSubMenu "userSubMenu"> // Error 1: Cannot directly assign component instance to a variable
-_currentDisplay = _userSubMenu.style.display // Error 2: Cannot access .style property
-IF _currentDisplay == "none"
--_userSubMenu.style.display = "block" // Error 3: Trying to modify component's display style; all VL components don't have display style, this style is fixed in component definition and cannot be modified

✅ Correct usage: Control visibility via component's generic show property
// Define variable binding in component tree
<Col-UserSubMenu "userSubMenu"> show:"$show"
// Modify variable in event or method
-IF $show == false
-- $show = true
```

#### Define Before Use Principle

Except for system method class components, all other components need to be defined in the component tree first before they can be referenced in subsequent methods/functions.

Please pay special attention to these functional non-UI components that also need to be defined first: `Trigger`, `FrontendApi`, `WindowEventListener`, `ClientUserCenter`

#### System Method Class Components

System method class components are special components with only one global instance, providing convenient access to system methods. Therefore, they don't need to be defined in the component tree and can be used directly, e.g.:

`<ClientUtils>.consoleLog(123)`

### Component Properties

#### Property Order

**Please strictly follow this order when specifying component properties:**

* Functional properties: such as value, type, disabled, etc., strictly refer to component documentation
* StyleClass: Current component's preset style class, strictly refer to current project's `.vth` file definition
* Override CSS style properties: Additional CSS styles, can override styles defined in StyleClass preset classes

#### Read-Only and Non-Read-Only Properties

* **Non-read-only properties** define component behavior, data, logic, core characteristics, and non-visual state. They determine what the component "is" and "how it works".
* **Read-only properties** are mainly read-only properties of DOM elements, such as offsetWidth. These properties don't need to be defined in `# Frontend Tree` and can be accessed directly in methods/functions later.
* Whether read-only or non-read-only properties, please **strictly refer to component documentation**; inventing any properties is not allowed.

#### Property Read/Write Rules (Unidirectional Data Flow)

VL uses **Unidirectional Data Flow** architecture, **there is no v-model syntactic sugar**.

##### Non-Read-Only Property Read/Write Standards (Attribute Mutability)

1. **Forbidden** to **directly read or modify component non-read-only properties** in any method/event/function/expression (like `Button.color` or `Modal.show = false`).
2. If dynamic property control is needed, follow the **"variable intermediary"** pattern:
   - First **bind** a variable to the property (like `show:$isModalVisible`).
   - In logic, **only modify that variable** (`$isModalVisible = false`), letting reactive binding drive component refresh.
3. Design Intent
   - Maintain **strict unidirectional data flow**, avoiding hard-to-track side effects.
   - Improve code readability: All mutable state is concentrated in variable naming, making it clear at a glance.

##### Read-Only Property Read/Write Standards

Read-only properties can be read in methods/functions (not written), won't break unidirectional data flow:

```
# Frontend Event Handlers

<Button-CheckSize "checkButton">.@click()
// ✅ Allowed: Reading size information
-_buttonWidth(FLOAT) = <Button-Submit "submitButton">.offsetWidth
-_buttonHeight(FLOAT) = <Button-Submit "submitButton">.offsetHeight
-_rect({}) = <Block-MainContainer "mainContainer">.getBoundingClientRect()

// ✅ Allowed: Reading position information
-_scrollTop(FLOAT) = <Block-Content "docContent">.scrollTop
-_offsetLeft(FLOAT) = <Image-Logo "logo">.offsetLeft
```

#### StyleClass Property Special Rules

StyleClass property is used to reference preset component style classes defined in Theme file.

**Property Value Rules:**

* **Literal values**: Write style class name directly, **no** double quotes needed

  ```vl
  <Button-Submit> StyleClass:ButtonPrimary
  <Text-Title> StyleClass:TextLarge
  ```
* **Expression values**: When dynamic style class switching is needed, use expressions; strings inside expressions need double quotes

  ```vl
  <Button-Submit> StyleClass:($isActive ? "ButtonActive" : "ButtonInactive")
  <Text-Status> StyleClass:($hasError ? "TextError" : "TextNormal")
  ```
* **Variable values**: When style class name is stored in a variable, reference the variable directly

  ```vl
  <Block-Container> StyleClass:$currentStyle
  ```

**Notes:**

* StyleClass value is the **name part** of the style class, not including component type prefix
* For example, if Theme defines `<Button-ButtonPrimary>`, use `StyleClass:ButtonPrimary` when referencing
* A component can only use StyleClass of its corresponding type

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

* **System variable**: e.g., `$serverDate = SYSENV.currentTime`.
* **Unix timestamp**: Millisecond precision; for second precision, use `setUnixS()` method
* **String** (five formats, all case-sensitive):

  * `"YYYY-MM-DD HH:mm:ss"`
  * `"YYYY-MM-DD HH:mm:ss.SSS"`
  * `"YYYY-MM-DD"`
  * `"YYYY-MM-DDTHH:mm:ssZ"`
  * `"YYYY-MM-DDTHH:mm:ss.SSSZ"`
* **Recommended initialization format**:

  - Preferably use complete format when defining: `$createTime(TIMESTAMP) = "2024-12-12 11:11:11.342"`
  - System variable `SYSENV.currentTime` returns standard time string format (with milliseconds)

#### Timestamp Variable Usage

TIMESTAMP variables can be used directly in most scenarios without explicit conversion

* All time-related variable assignments: `$eventTime(TIMESTAMP) = "2024-12-12 11:11:11"`
* Parameter passing: `processEvent(eventTime)` where eventTime is TIMESTAMP type
* Database time comparison: `[["createTime", "gt", $startTime]]`

**Example**:

```vl
$eventTime(TIMESTAMP) = "2024-12-12 15:30:00"
$displayTime(STRING) = $eventTime.format("YYYY-MM-DD HH:mm")
<Text-EventTime> value:($eventTime.format("MM/DD HH:mm"))
```

#### Timestamp Variable Methods and Functions

**Immutable methods:**

* **Format output**: Use `.format(outputFormat)` method for formatted display. Output format includes: YYYY (4-digit year, e.g., 2024), YY (2-digit year, e.g., 24), M (1-digit month, 1-12), MM (2-digit month, 01-12), MMM (short month name, e.g., Jan, Feb), MMMM (full month name, e.g., January), D (1-digit day, 1-31), DD (2-digit day, 01-31), d (day of week, 0-6), H (24-hour 1-digit hour, 0-23), HH (24-hour 2-digit hour, 00-23), h (12-hour 1-digit hour, 1-12), hh (12-hour 2-digit hour, 01-12), m (1-digit minute, 0-59), mm (2-digit minute, 00-59), s (1-digit second, 0-59), ss (2-digit second, 00-59), SSS (3-digit millisecond, 000-999), A (12-hour AM/PM uppercase), a (12-hour am/pm lowercase)
* **Timestamp retrieval**: Use `.unixS()` and `.unixMS()` methods

**Mutable functions:**

* `variable.setUnixS(secondTimestamp)`
* `variable.setUnixMS(millisecondTimestamp)`
* `variable.setCustomFormat("timeString", "correspondingFormatString")`

**Forbidden operations**:

* **Forbidden to directly call JavaScript Date API**: Direct calls to `new Date(...)`, `.getTime()`, `.getFullYear()`, `.getMonth()`, `.getDate()` and other native JS Date methods are not supported

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
| --themeTokenName   | Frontend                               | All Token variables defined in Theme.vth can be directly referenced in frontend code, e.g.,`font-size:--fontSizeLg color:--colorTextDefault`                                                                                                                                                                                                                        |

### Variable Assignment and Modification

#### Variable Assignment

Variable assignment uses equals sign `=`, no need to re-declare type:

```
$variableName = newValue
_variableName = newValue
```

* `=` operation follows JS rules: when new value is a simple value (STRING, INT, FLOAT, BOOL), assigns the value itself; when new value is a complex data structure (array, object, JSON), assigns a reference to that data structure.

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

* Variable built-in functions can be called directly without pre-definition.
* When calling, use function name directly, **do not add `_` prefix:**
  * Correct (direct use): `_num.toFixed()`
  * Incorrect (confused with PIPE function, adding `_`): `_num._toFixed()`

#### Forbidden JS Methods:

* `toString()`: Please use `String()` instead, which has broader applicability

#### "Callback Function Parameter" Usage Rules in Variable Methods

##### Hard Rules (Must / Must Not)

* **MUST**: In map / filter / reduce / some / every / sort callback function parameters, **must use arrow functions, and function body must be a single expression (concise body).**
* **MUST**: **Callbacks must be pure synchronous, no side effects** (must not modify external/parent variables, must not call async METHOD/component methods)
* **MUST NOT**: Use statements with side effects in callbacks (like acc[k]=..., push/splice and other write operations)
* **MUST NOT**: Write return statement blocks.

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

* Backend has no object and array type fields; for complex structures, use JSON type uniformly
* Backend has additional VEC vector type field for vector search; VL currently doesn't support VEC type variables

Except for these two differences, other field types have identical data definitions to variable data types.

### Vector Field Usage

Vector fields are used for vector search. In VL, you only need to define a vector field's vecSource (source fields for vectorization), and the system will automatically vectorize source fields when table data is inserted/updated.

#### Vector Field Definition

**1. Define vector field in virtual table**

```
<ServiceDomainRoot-Root>
-<VirtualTable-Documents> "docTable" mockData:[{...}]
--<Field-title> type:STRING
--<Field-content> type:STRING
--<Field-contentVec> type:VEC vecSource:["title","content"] // Vector field data source is title and content, data will automatically vectorize these two fields
```

* In most cases, a virtual table only needs one vector field. Add all fields that may participate in content search as vecSource of the vector field, rather than having one vector field per content field.

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

* Vector field binding doesn't support expressions, can only be one-to-one binding
* Virtual table and entity table vector field vecSource must be strictly consistent. For example, if virtual table's virtualVec field has sourceVec as virtualA, virtualB two virtual fields, and binds to entity table's realVec field; realVec's sourceVec is realA, realB. Then realA, realB and virtualA, virtualB must strictly correspond. When a virtual table vector field binds to an entity table vector field, their vectorization sources must also be one-to-one bound.

#### Vector Search

Vector search is implemented through `select` method's `conditions` (filtering) and `orderBy` (sorting) parameters:

**Vector sorting (orderBy):**

* Format: `[["vectorFieldName","searchText"],...]`
* Automatically sorts by similarity from high to low (vector distance from small to large)
* Can mix with regular field sorting: `[["contentVec","AI Tech"],["_create","desc"]]`

**Vector filtering (conditions):**

* Format: `["vectorFieldName","l2str","searchText",maxDistance]`
* maxDistance is maximum distance from search text, value is less than or equal to 1
* Can combine with regular filter conditions: `[["status","eq","published"],["contentVec","l2str","keyword",0.4]]`
* Supports use in OR conditions: `[["OR",["titleVec","l2str","keyword",0.3],["contentVec","l2str","keyword",0.3]]]`

**Example:**

```vl
-<VirtualTable-Documents "docTable">.select([["status","eq","published"],["contentVec","l2str",keyword,0.4]],[["contentVec",keyword],["_create","desc"]],[_offset,pageSize],null) -> _result
```

**Notes:**

* Vector fields don't need manual assignment during `insert`/`update`; system auto-generates based on `vecSource` fields
* Vector fields don't support `eq`, `contains` and other regular operators, only support `l2str` filtering and similarity sorting
* Search text is automatically vectorized by system for matching

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

#### 1. Event

**Essence**: Passively triggered interaction response entry point

**Characteristics**:

- Cannot be actively called, can only be listened to
- Automatically triggered by user operations or component state changes
- Independently defined in `# Frontend Event Handlers` section
- Event parameters are entirely determined by the component, cannot be customized, no need for types

**Usage Scenarios**:

```vl
# Frontend Event Handlers

// ✅ Typical usage: Handle button click
<Button-Submit "submitBtn">.@click()
-validateForm() -> _validation
-IF _validation.success
--submitData()

// ✅ Typical usage: Handle input change
<Input-Email "emailInput">.@change(newValue, oldValue)
-$email = newValue
-$isValid = newValue._isValidEmail()

// ❌ Error: Event parameters should not have type declarations
<Input-Email "emailInput">.@change(newValue(STRING), oldValue(STRING))  // Wrong! Event parameters don't need types
-$email = newValue

// ✅ Correct: Event parameters just write names
<Input-Email "emailInput">.@change(newValue, oldValue)
-$email = newValue

// ❌ Error: Events cannot be called
METHOD processData(); RETURN {success:BOOL}
-<Button-Submit "submitBtn">.@click()  // Wrong!
```

#### 2. Method

**Essence**: Reusable business logic encapsulation units

**Characteristics**:

- Can be actively called
- Can modify variables, call services, perform I/O
- Must return object format `{key:Type, ...}`

**Usage Scenarios**:

```vl
// ✅ Frontend public method: For external calls
METHOD_PUB ValidateInput(email(STRING)); RETURN {valid:BOOL, message:STRING}
-IF email == ""
--RETURN {valid:false, message:"Email cannot be empty"}
-IF !email.includes("@")
--RETURN {valid:false, message:"Invalid email format"}
-RETURN {valid:true, message:""}

// ✅ Internal method: Encapsulate reusable logic
METHOD loadUserData(userId(INT)); RETURN {success:BOOL, data:{}}
-<ServiceDomain-User "userService">.GetUserInfo(userId) -> _result
-IF !_result.success
--RETURN {success:false, data:{}}
-$userData = _result.data
-RETURN {success:true, data:_result.data}

// ✅ Service method: Backend business logic
SERVICE GetDocList(keyword(STRING)); RETURN {success:BOOL, data:[{}]}
-<VirtualTable-Documents "docTable">.select([["title","*contains",keyword]]) -> _result
-RETURN {success:true, data:_result.dataArray}

// ❌ Error: Methods cannot be called in expressions
<Text-Result> value:calculateTotal()  // Wrong! Need to use PIPE instead
```

#### 3. Function (PIPE)

**Essence**: Pure data transformation pipeline

**Characteristics**:

- No side effects (doesn't modify external state)
- Only for expressions
- Must be named with `_` prefix

**Usage Scenarios**:

```vl
// ✅ Pipe function definition
PIPE _formatPrice(price(FLOAT)); RETURN STRING
-RETURN ("¥" + price.toFixed(2))

PIPE _isValidEmail(email(STRING)); RETURN BOOL
-RETURN (email.includes("@") && email.includes("."))

// ✅ Call in expressions
<Text-Price> value:$price._formatPrice()
<If-ValidEmail> conditions:$email._isValidEmail()

# Frontend Derived Vars
$formattedTotal(STRING) = $totalPrice._formatPrice()

// ✅ Variable built-in functions (no _ prefix)
-_names([STRING]) = $users.map((u) => u.name)
-_total(FLOAT) = $prices.reduce((sum, p) => sum + p, 0)
-_formatted(STRING) = $timestamp.format("YYYY-MM-DD")

// ❌ Error: Functions cannot have side effects
PIPE _saveAndFormat(value(STRING)); RETURN STRING
-$savedValue = value  // Wrong! Modified global variable
-RETURN value.trim()
```

#### 4. Expression

**Essence**: Computation formulas that return a single value

**Characteristics**:

- No side effects
- Cannot call METHOD
- Supports operators, functions, ternary operators

**Usage Scenarios**:

```vl
// ✅ Property binding
<Text-FullName> value:($firstName + " " + $lastName)
<Button-Submit> disabled:(!($formValid && $dataReady))
<Text-Status> value:($age >= 18 ? "Adult" : "Minor")

// ✅ Conditional containers
<If-HasItems> conditions:($items.length > 0)
<If-CanSubmit> conditions:($email._isValidEmail() && $password.length >= 6)

// ✅ Derived variables
# Frontend Derived Vars
$totalPrice(FLOAT) = $items.reduce((sum, item) => sum + item.price, 0)
$displayName(STRING) = ($userName != "" ? $userName : "Guest")
$canCheckout(BOOL) = ($cartItems.length > 0 && $totalPrice > 0)

// ❌ Error: Expressions cannot call METHOD
<Text-Result> value:calculateTotal()  // Wrong!
<If-Check> conditions:validateForm()  // Wrong!

// ❌ Error: Expressions cannot use mutable methods
<Text-Count> value:$list.push(5)  // Wrong!
# Frontend Derived Vars
$sorted([{}]) = $items.sort((a, b) => a.price - b.price)  // Wrong!
```

### Methods

#### Method Definition Location and Format

| Method Type                   | Applicable Files | Definition Section          | Definition Prefix | Method Name Format |
| ----------------------------- | ---------------- | --------------------------- | ----------------- | ------------------ |
| Frontend internal method      | vx, sc, cp       | # Frontend Internal Methods | METHOD            | camelCase          |
| Frontend module public method | sc, cp           | # Frontend Public Methods   | METHOD_PUB        | PascalCase         |
| Service                       | vs               | # Services                  | SERVICE           | PascalCase         |
| Backend internal method       | vs               | # Backend Internal Methods  | METHOD            | camelCase          |

**Definition Format**

Method definition line is at top level, no indentation. All code lines in method body need at least one level of indentation (`-`).

```
<DefinitionPrefix> <MethodName>(param1(Type1),param2(Type2),...);RETURN {returnVal1:Type1,returnVal2:Type2,...}
-method body
```

#### Method Parameter Definition

Method parameters use camelCase and need type specification, e.g.: `rawEmail(STRING), tags([FLOAT])`

#### Method Parameter Rules

##### Parameter Read-Only Principle

In VL 2.0, all method parameters are **read-only** and cannot be modified inside methods:

❌ Incorrect example:

```vl
METHOD processData(inputList([STRING])); RETURN {success:BOOL}
-inputList.push("newItem")      // Wrong: Cannot modify parameter
-inputList.splice(0,1)          // Wrong: Cannot modify parameter
-inputList[0] = "changed"       // Wrong: Cannot modify parameter
```

✅ Correct approach - Copy then modify:

```vl
METHOD processData(inputList([STRING])); RETURN [STRING]
-_localList([STRING]) = [...inputList]   // Copy parameter
-_localList.push("newItem")              // Modify local copy
-RETURN _localList
```

✅ Correct approach - Return new data:

```vl
METHOD collectIds(startId(STRING)); RETURN [STRING]
-_result([STRING]) = []
-_result.push(startId)
-// ... processing logic
-RETURN _result
```

##### Recursive Collection Pattern

When recursively collecting data, use return value merging instead of parameter modification:

❌ Incorrect:

```vl
METHOD collectAncestors(id(STRING),resultList([STRING])); RETURN {success:BOOL}
-resultList.push(id)                    // Wrong
-collectAncestors(parentId,resultList)  // Wrong accumulation method
```

✅ Correct:

```vl
METHOD collectAncestors(id(STRING)); RETURN [STRING]
-_ids([STRING]) = [id]
-_parentIds([STRING]) = collectAncestors(parentId)
-_ids = _ids.concat(_parentIds)
-RETURN _ids
```

##### Global Variable Exception

Only global variables ($ prefix) can be directly modified inside methods:

```vl
METHOD addToList(item(STRING)); RETURN {success:BOOL}
-$globalList.push(item)    // ✅ Correct: Global variables can be modified
-RETURN {success:true}
```

#### Method RETURN Structure

For all methods, **`RETURN` structure must be an object `{}`**, even if there's only one return value.

**To work with system default error handling mechanism, it's recommended that return objects always include** **success:BOOL** **field, and optionally** **message:STRING** **field. For example:**

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

* Calling file-internal custom methods: Use directly `<MethodName>(parameters)`. Frontend files can call frontend public methods and internal methods; ServiceDomain files can call services and backend internal methods;
* Calling system/component methods: Use `<ComponentClass-ComponentName "componentId">.methodName(parameters)`
* Calling variable methods: Use `<variableName>.methodName(parameters)`

**Handling Method Return Values**

Use `-> variableName` to store method return data to specified variable. `variableName` can be a pre-declared local variable (starting with `_`) or a pre-declared global variable (starting with `$`, depending on current environment). When there's no return value or return value isn't needed, `->` can be omitted.

```vl
-methodName(param1,param2,...) -> _apiResponse // Store value to pre-defined or undefined local variable. If undefined, system auto-creates this local variable
-<ServiceDomain-DomainName>.ServiceName(param1,param2,...) -> $globalStatus // Store value directly to global variable. Note: storing to global variable subfields is currently not supported, e.g., $globalStatus.type is not allowed
-<ClientUtils>.delay(1500) // Method has no return value
```

**Best Practice**: Prefer storing method/service call results to local variables (`_variableName`) to maintain clear scope and data flow. Only consider using global variables (`$variableName`) as receivers when genuinely needing to directly update an existing global state.

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

* When a piece of logic is only used in one event method/service/public method, write that logic directly in that event method/service/public method. Forbidden to first define an internal method then call it in this event method/service/public method;
* When a piece of logic is used in 2 or more event methods/services/public methods, must encapsulate this logic as an internal method to avoid duplicate code

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

* Function names use `_` prefix + camelCase, e.g., `_calculateTotal`

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

#### Event Concept

Events are the core mechanism in VL for responding to user interactions or system state changes. Unlike methods, events are passively triggered, automatically executing their internal logic when specific conditions are met. Events define response behavior through event handlers.

#### Event Handler Definition

All event handlers are uniformly defined in `# Frontend Event Handlers` / `# Backend Event Handlers` sections, consisting of two parts:

* Event listening statement
* Event handling body

**Definition Format:**

```vl
<ComponentClass-ComponentName "componentId">.@eventName(param1, param2...) // Event listening statement
-Event handling body
```

**Format Description:**

* Event listening statement has no indentation
* Event handling body all code has at least one level indentation (`-`)
* Event name must have `@` symbol prefix

**Naming Conventions:**

* Event names uniformly use **camelCase**
* Event parameter names use **camelCase**
* Parameter names must strictly follow definitions in component documentation

**Event Body Writing Rules**

Event handling body is essentially a method; its writing rules (indentation, variable operations, method calls, control flow, error handling, etc.) are identical to methods, refer to "Methods" section.

#### RETURN Statement

* Event handlers can use `RETURN` to terminate execution early
* Event handlers **don't need return values**; `RETURN` is followed by nothing

```vl
<Button-Submit "submitButton">.@click()
-IF !$canSubmit
--<SysUI>.showToast("Please complete required fields first", "warning")
--RETURN
-submitForm()
```

#### Event Parameters

Event parameters are defined in the event listening statement:

**When to Declare Parameters:**

* When event handling body needs to use event parameters, must declare parameters in parentheses of event listening statement
* Parameter names use **camelCase**, must exactly match definitions in component documentation
* Parameters **do not allow** type declarations; their types are entirely determined by the component

**When Parameters Can Be Omitted:**

* When event handling body doesn't need any event parameters, parentheses can be empty
* Example: `@click()` means listening to click event but not using event object

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

#### Event Triggering Mechanism

**Forbidden to Manually Trigger Events or Call DOM-Style Methods via Component Instances**

* Event handlers are **automatically triggered** by user interactions or component state changes; **cannot be manually called in code**.
* The following are all **incorrect usages, strictly forbidden**:

```vl
# Frontend Event Handlers

// ❌ Error: Trying to trigger button click via DOM-style method
<Button-Submit "submitButton">.click()

// ❌ Error: Manually calling event as method
<Button-Submit "submitButton">.@click()
```

* In VL, Button, Input and other UI components **do not expose any `.click() / .focus() / .blur()` browser DOM methods**;
* Can only call component methods explicitly declared in component documentation (e.g., `.scrollToBottom()` for certain list components).

**Events Don't Support Propagation**

Events in VL don't support event bubbling or capturing mechanisms. Each event is triggered independently:

```vl
# Frontend Tree
<Block-Container "container">
-<Button-Inner "innerButton"> value:"Click"

# Frontend Event Handlers

<Block-Container "container">.@click()
-<ClientUtils>.consoleLog("Container clicked")

<Button-Inner "innerButton">.@click()
-<ClientUtils>.consoleLog("Button clicked")

# When clicking button, only button's @click event is triggered
# Container's @click event is NOT triggered
```

#### Event Handling in Loops

**Component ID Property in Loops**

When listening to events of components inside loop containers, no need to care about correspondence between multiple loop-created components; just use static IDs uniformly.

❌ Strictly forbidden: `<Button-Action ("btn_" + _index0)> value:_item0.name`

✅ Use static ID directly: `<Button-Action "btn"> value:_item0.name`

**Loop Variable Usage**

In event handlers of components defined inside `<For>` or `<TreeFor>` loop containers, loop variables can be used directly:

```vl
# Frontend Tree
<For-UserList> sourceArray:$users loopVar:[_user0, _index0]
-<Block-UserItem>
--<Text-UserName> value:_user0.name
--<Button-Select "selectButton"> value:"Select"

# Frontend Event Handlers

<Button-Select "selectButton">.@click() // ID is static value
-$selectedUserId = _user0.id
-$selectedUserName = _user0.name
-<SysUI>.showToast(("Selected: " + _user0.name), "success")
```

**Loop Variable Scope**

Loop variables (`_itemX`, `_indexX`) are only valid in event handlers of `<For>` container's direct children and their descendant components:

```vl
# Frontend Tree
<For-TaskList> sourceArray:$tasks loopVar:[_task0, _index0]
-<Block-TaskItem>
--<Button-Complete "completeButton"> value:"Complete" // ID is static value

<Button-ShowAll "showAllButton"> value:"Show All"

# Frontend Event Handlers

# Correct: Button defined inside loop can use loop variables
<Button-Complete "completeButton">.@click()
-$tasks[_index0].completed = true

# Error: Button defined outside loop cannot use loop variables
<Button-ShowAll "showAllButton">.@click()
-_task0.completed = true  # Error! _task0 is not available here
```

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

* Example: `_inputValue._trim()._toUpperCase()`

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

Any method defined with `METHOD` or `METHOD_PUB` cannot be called in expressions:

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

* `FOR...IN` loop variables use `_` prefix + camelCase (e.g., `_itemX`, `_user`, `_product`) and `_indexX` (X starts from 0).
* When nesting loops, index variables increment to `_item1`, `_index1`; `_item2`, `_index2`, etc.
* Counting loop `FOR` loop variables **must** use `_` prefix (e.g., `_i`, `_j`, `_counter`).

## Logic Writing Basic Rules

### Unidirectional Data Flow Rules (Strictly Follow)

In the following scenarios, **downstream data is uniquely determined by upstream data**; downstream data cannot be independently modified, **and especially cannot reverse modify upstream data through downstream data**.

#### Variable Bound to Property (Upstream) -> Component Property (Downstream)

When a component property is bound to a variable, this variable becomes the only factor controlling that component property. To modify component property, must modify its upstream variable.

For input-type components, this rule also applies: If an input component's value property is bound to a variable, when user inputs, must assign current user input content to bound variable through @change(), @blur(), @selected(), @input() and other input/completion events. Note: **There is no v-model syntactic sugar in VL**; all data modifications must be done explicitly. Example:

```
# Frontend Tree
-<Input-Name "nameInput"> value:$name // $name only initializes input box display content, won't automatically change with user input
-<Button-Submit "submitButton">

# Frontend Event Handlers

<Input-Name "nameInput">.@change(newValue,oldValue) // Key!! Must use input box event to reassign user input value to variable
-$name = newValue

<Button-Submit "submitButton">.@click()
-submitName($name)
```

#### Global Variable (Upstream) -> Derived Variable (Downstream)

Derived variable's only computation basis is the global variables it depends on at definition; cannot be independently assigned in subsequent operations, example:

```
# Frontend Global Vars
$users([{}]) = [{}]

# Frontend Derived Vars
$userCount = $users.length

# Frontend Event Handlers
<Button-Count "countButton">.@click()
-$userCount = $users.length + 1 // Serious error, $userCount is uniquely computed from $users, cannot be operated in subsequent methods
```

#### Loop Data Source (Upstream) -> Loop Variable (Downstream)

Loop variables are references to loop data source; their values are entirely determined by loop data source and cannot be independently modified. Example:

```
// Loop structure container example
# Frontend Tree
-<For-ItemList> sourceArray: $list loopVar:[_item0,index0]
--<Text-Name "itemName"> value:_item0.name
--<BinaryToggle_Checkbox-Checked> value:_item0.checked

# Frontend Event Handlers
// Wrong approach: Directly modify loop variable value in event
<Text-Name "itemName">.@click()
-_item0.checked = true // Error! _item0 loop variable cannot be independently modified

// Correct approach: Modify loop data source
<Text-Name "itemName">.@click()
-$list[_index0].checked = true // Modify loop data source
```

#### Method/Function/Event Parameter Declaration (Upstream) -> Parameter Usage in Method/Function Body (Downstream)

Method and event parameters, after being declared in method signature/event definition, subsequent usage is unidirectional reference to declaration; cannot modify parameters in event/method body. Example:

**Correct approach:**

```
METHOD manageList(items([STRING]), newItem(STRING)); RETURN {updatedList:[STRING], success:BOOL}
-_listCopy([STRING]) = items.slice() // items as parameter cannot be modified, can only be read, or use its immutable methods to get derived data based on it
-_listCopy.push(newItem) // Read parameter newItem
-_listCopy.splice(0, 1)
-_listCopy.sort()
-RETURN {updatedList:_listCopy, success:true}

METHOD updateTimestamp(timeVar(TIMESTAMP)); RETURN {newTime:TIMESTAMP, success:BOOL}
-_localTime(TIMESTAMP) = timeVar // timeVar as parameter cannot be modified, can only be read
-_localTime.setUnixS(1700000000)
-RETURN {newTime:_localTime, success:true}
```

**Wrong or invalid approach:**

```vl
METHOD attemptModifyList(originalList([INT]), valueToAdd(INT));RETURN {success:BOOL}
-originalList.push(valueToAdd) // Error, cannot perform mutable operations on parameters
-RETURN {success:false}

SERVICE ListPatientVisits(patientId(INT),page(INT),pageSize(INT));RETURN {...}
-IF page <= 0
--page = 1 // Cannot directly assign to parameter, must use another local variable as intermediary
...
```

### Conditional Judgment Usage Rules (`IF / ELSE IF / ELSE`)

`IF` statement is used for conditional branching based on **boolean expressions**, can handle more complex conditional logic.

**Indentation Rules:**

* `IF booleanExpression` statement maintains **same indentation** as current line in its code block (e.g., `-`).
* `ELSE IF booleanExpression` and `ELSE` keywords also maintain **same indentation** as `IF` (e.g., `-`).
* Code blocks inside `IF`, `ELSE IF`, `ELSE` are **one level deeper** than their keywords (e.g., `--`). Nested `IF` continues increasing indentation.

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

* **In Frontend Component Tree (# Frontend Tree):** Use `<For>`, `<TreeFor>` and other loop structure containers to create child components in loops
* **In Method/Function Body:** Use loop syntax `FOR...IN` and `FOR(...)` for array loops and counting loops

##### 1. Loop Structure Containers in Component Tree (`<For>`, `<TreeFor>`)

Create using `<For/TreeFor-ContainerName sourceArray:arrayToTraverse loopVar:[_itemN,_indexN]>`, see component usage documentation for details.

* Note: In loop structure container properties, `loopVar` property's declared local loop variables don't need pre-definition.
* Loop variables should uniformly use _itemN and _indexN format, where N represents nesting level, starting from 0. For example, first level loop has loop variables _item0, _index0

##### 2. Array Loop in Methods/Functions (`FOR ... IN`)

Use `FOR ... IN ...` keyword to traverse arrays.

**Indentation Rules:**

* `FOR (_itemVarN, _indexVarN) IN arrayVariable` statement maintains **same indentation** as current line in its code block (e.g., `-`).
* Code blocks inside loop body are **one level deeper** than `FOR` statement (e.g., `--`).
* Loop variables can be used directly without pre-definition
* Loop item variables and index variables **must** use `_` prefix + **camelCase**. Index variables should use `_indexN` (N starts from 0, e.g., `_index0`, `_index1`). Loop item variables can be named based on context (e.g., `_user0`, `_product1`, `_noteItem0`) or use `_itemN` for generic iteration (e.g., `_item0`, `_item1`, `_item2`). Note: Whether using _item or context-based naming, to avoid duplication, please add nesting level N (starting from 0) after variable name. Example: `FOR (_user0, _index0) IN $userList` or `FOR (_dataItem1, _index1) IN dataArray`.
* **Loop Variable Scope**: Loop variables (`_itemN`, `_indexN`) are only valid in property bindings and conditional expressions of `FOR...IN` container's direct children and their descendant components. Outside `FOR...IN`, or in event handlers of components not internally defined in `FOR...IN`, referencing these loop variables is an error.

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

* `FOR (initialization; condition; increment/decrement)` statement maintains **same indentation** as current line in its code block (e.g., `-`).
* Code blocks inside loop body are **one level deeper** than `FOR` statement (e.g., `--`).
* Loop control variable is declared in initialization part, using `_varName(TYPE) = initialValue`. **Must** declare type, **no** `let` keyword. **Must** use `_` prefix + **camelCase** name (e.g., `_i`, `_j`, `_k`, `_loopCounter`).

**Syntax:**

```
-FOR (_loopVar(TYPE) = initialValue; loopCondition; incrementExpression)
--loop execution code
--...
```

* **`_loopVar(TYPE) = initialValue`**: Initialization statement, declares and initializes loop variable. **Must** include type declaration and `_` prefix.
* **`loopCondition`**: Condition for loop continuation, an expression returning `BOOL`.
* **`incrementExpression`**: Expression executed after each loop iteration, usually modifies loop variable (e.g., `_i++`, `_i--`, `_i = _i + 2`).

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

### Form Creation Standards

Forms in VL follow **unidirectional data flow** principle, implementing real-time data collection by having input component events write back to corresponding global variables. Form creation divides into two scenarios; choose different implementation strategies based on form field count.

#### Core Principles

**Data Binding Rules:**

* Input component's `value` property binds global variable as initial value
* Write back global variable through input component events (like `@change()`, `@input()`, etc.)
* There is no v-model syntactic sugar in VL; all data modifications must be done explicitly

**Form Validation Rules:**

* Validation logic goes in derived variables (or use PIPE functions if expressions are too complex) or frontend internal methods
* Validation results control submit button's `disabled` property

#### Scenario 1: Field Count ≤ 3 (Direct Construction)

Declare input components directly in component tree, each corresponding to a global variable.

**Example: Simple Login Form**

```vl
# Frontend Global Vars
$username(STRING) = ""
$password(STRING) = ""

# Frontend Derived Vars
$isFormValid(BOOL) = ($username != "" && $password.length >= 6)

# Frontend Tree
<Block-FormContainer>
-<Input-Username "usernameInput"> value:$username type:"text"
-<Input-Password "passwordInput"> value:$password type:"password"
-<ButtonContainer-Submit "submitButton"> disabled:(!$isFormValid)
--<Text-SubmitLabel> value:"Login"

# Frontend Event Handlers

<Input-Username "usernameInput">.@change(newValue, oldValue)
-$username = newValue

<Input-Password "passwordInput">.@change(newValue, oldValue)
-$password = newValue

<ButtonContainer-Submit "submitButton">.@click()
-IF !$isFormValid
--<SysUI>.showToast("Please complete the form", "warning")
--RETURN
-userlogin($username, $password) -> _result
```

#### Scenario 2: Field Count > 3 (Loop Construction + Custom Component)

* Use `<For>` and `$formFields` loop to dynamically create input components, create a custom dynamic field component (switch displayed input component type via type property), uniformly manage field input and events
* Dynamic field input completion event writes current input value back to `$formData`, then calls a `validate` method to write all current validation errors to `$errors`
* Use `$errors` to display error messages after fields in real-time and control whether submit button can submit

**Example: Multi-field User Registration Form**

```vl
<Section-UserRegistration "root">

# Frontend Global Vars
$formFields([{name:STRING,label:STRING,type:STRING,options:[{label:STRING,value:STRING}]}]) = [{name:"username",label:"Username",type:"text"},{name:"email",label:"Email",type:"text"},{name:"password",label:"Password",type:"password"},{name:"confirmPassword",label:"Confirm Password",type:"password"},{name:"phone",label:"Phone Number",type:"tel"},{name:"role",label:"Role",type:"select",options:[{label:"Regular User",value:"user"},{label:"Premium User",value:"vip"}]}]
$formData({username:STRING,email:STRING,password:STRING,confirmPassword:STRING,phone:STRING,role:STRING}) = {username:"",email:"",password:"",confirmPassword:"",phone:"",role:""}
$errors({username:STRING,email:STRING,password:STRING,confirmPassword:STRING,phone:STRING,role:STRING}) = {username:"",email:"",password:"",confirmPassword:"",phone:"",role:""}
$canSubmit(BOOL) = false

# Frontend Tree
<Block-RegistrationForm>
-<For-FormFields> sourceArray:$formFields loopVar:[_field0, _index0]
--<Block-FieldWrapper>
---<Text-Label> value:_field0.label StyleClass:..
---<Component-FormField "formField"> fieldName:_field0.name type:_field0.type fieldValue:$formData[_field0.name] fieldOptions:_field0.options
---<If-HasError> conditions:($errors[_field0.name] != "")
----<Text-Error> value:$errors[_field0.name] StyleClass:..
-<Button-Submit "submitButton"> value:"Register" disabled:(!$canSubmit) StyleClass:..

# Frontend Event Handlers

<Custom_DynamicField-FormField "formField">.@fieldChange(fieldName, newValue)
-$formData[fieldName] = newValue
-validateForm() -> _validation
-$canSubmit = _validation.canSubmit

<Button-Submit "submitButton">.@click()
-registerUser($formData) -> _result
-IF _result.success
--<SysUI>.showToast("Registration Successful", "success")

# Frontend Internal Methods

METHOD validateForm(); RETURN {canSubmit:BOOL}
-$errors = {}
-IF $formData.username == ""
--$errors.username = "Username cannot be empty"
-ELSE IF $formData.username.length < 3
--$errors.username = "Username must be at least 3 characters"
// Other field validation logic, omitted here
-_hasErrors(BOOL) = ($errors.username != "" || $errors.email != "" || $errors.password != "" || $errors.confirmPassword != "" || $errors.phone != "")
-_allFilled(BOOL) = ($formData.username != "" && $formData.email != "" && $formData.password != "" && $formData.confirmPassword != "" && $formData.phone != "")
-RETURN {canSubmit:(!_hasErrors && _allFilled)}

</Section-UserRegistration>
```

#### Comparison Summary

| Aspect                | Field ≤ 3                            | Field > 3                                                           |
| --------------------- | ------------------------------------- | ------------------------------------------------------------------- |
| Component Declaration | Directly in component tree            | Use `<For>` + custom component                                    |
| Data Storage          | Multiple independent global variables | Single `$formData` object                                         |
| Event Handling        | Each field independent                | Unified through loop internal form field (usually custom component) |
| Validation Logic      | Derived variable                      | Frontend internal method                                            |

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

**1. System Default Error Handling Mechanism**

No need to explicitly declare error handling logic; any method call, as long as previous step returns success field not equal to true, will trigger default system error handling. Handling logic includes:

* Current method/event RETURNs
* If current method/event's return parameters include success and message fields, returns success:false, message:previous step result.message
* If current method/event is frontend-called, automatically calls SysUI method to display error message from previous step
* Above three points are all system-auto-handled; user doesn't need to declare IF statements and subsequent handling statements
* Use system default error handling when no special requirements, to simplify code
* Recommended to use system default error handling; only use IF special judgment when user feels need to separately handle errors

**2. Manual Error Handling Mechanism**

Use `IF` to manually determine error handling logic

**Custom Error Handling Example:**

```vl
SERVICE GetData(pageNum(INT), pageSize(INT), keyword(STRING));RETURN {success:BOOL, message:STRING, data:[{_id:INT,pic:STRING,price:STRING,title:STRING,address:STRING,desc:STRING}], total:INT}
-<VirtualTalbe-List "listDataTable">.count([["title","*contains",keyword]]) -> _countResult
-IF !(_countResult.count>0)
--RETURN {success:false,message:_countResult.message,data:null,total:0}

-_count(INT) = _countResult.count
-_offset(INT) = (pageNum - 1) * pageSize
-<VirtualTalbe-List "listDataTable">.select([["title","*contains",keyword]],null,[_offset,pageSize]) -> _selectResult
-IF _selectResult.dataArray.length == 0
--RETURN {success:false,message:_selectResult.message,data:null,total:0}

-RETURN {success:true,message:"",data:_selectResult.dataArray,total:_count}
```

**Frontend Error Handling Example (Combining State Updates and User Prompts):**

```vl
# Frontend Global Vars

$isLoading(BOOL) = false
$hasError(BOOL) = false
$errorMessage(STRING) = ""
$userData({}) = {}

# Frontend Event Handlers

<Button-LoadUser "loadUserButton">.@click()
-$isLoading = true
-$hasError = false
-$errorMessage = ""
-getUserData($userId) -> _result
-IF !_result.success
--$isLoading = false
--$hasError = true
--$errorMessage = _result.message
--<SysUI>.showToast(("" + "Load failed: " + _result.message), "error")
--RETURN

-$userData = _result.data
-$isLoading = false
-<SysUI>.showToast("User data loaded successfully", "success")
```

- In IF error handling blocks, typically these logics exist:
  * Update related state variables (like `$isLoading = false`, `$hasError = true`).
  * Update variables for displaying error messages (like `$errorMessage`).
  * Give user feedback using `<SysUI>.showToast` or `<SysUI>.showModal` (ensure message includes string literal `"" + ...`, and wrapped in parentheses as expression).
  * Decide whether to `RETURN` early based on logic needs.

## Style Definition

#### Overall Style Configuration Rules

**1. StyleClass Takes Priority Over Individual Property Overrides**

* Recommended to first define `# Component Styles` (component preset styles) in project Theme file, then in `# Frontend Tree`, use StyleClass property to reference, e.g., `<Block-ListContainer> StyleClass:CardDefault`, `<Text-Title> StyleClass:TextLarge`
* Note: StyleClass property only needs to include corresponding preset style name, not the class. For example, if a preset style is defined as `<Button-ButtonPrimary>` in .vth's #component style, when referencing, use `ButtonPrimary` as StyleClass property value, not `Button-ButtonPrimary`
* Note: A component can only use StyleClass of its corresponding class; e.g., Text component cannot use Block component's StyleClass

**2. ThemeToken Takes Priority Over Literal Values**

* Whether in Theme file's `# Component Styles` or in frontend file's `# Frontend Tree`, preferably use ThemeTokens like `--colorBrandPrimary`, `--spacing4`
* Only use literal values when a style property has no corresponding Token in `Theme.vth`; literal value usage rules are same as CSS rules, e.g., `width:100px`, `color:red`

**3. Dynamic Style Rules (Strict)**

* Prefer using dynamic StyleClass property, e.g., `StyleClass:($selected ? "TextSelected" : "TextNormal")`
* If dynamic StyleClass doesn't meet requirements, VL expressions can be used in component override CSS style properties, e.g.: `<Block-ProgressBar> width:("" + $progress*300 +"px")` Note: VL expressions are only supported in regular component instances; **`StateStyle` widgets are for static style configuration and do not allow VL expressions**
* **Strictly forbidden to directly access/modify component styles in events/methods**

```
# ❌ Wrong approach
-<Text-Title "title">.color = "blue" // Forbidden to directly modify component styles
-_textColor = <Text-Title "title">.color // Forbidden to read component styles
-<Text-Title "title">.style.color = "blue" // No .style.styleName access method exists

# ✅ Correct
// First define variable binding in component tree:
<Text-Title "title"> value:"Hello" color:$color
// Then modify bound variable:
-$color = "blue"
```

#### Compound Property Usage Rules

* **Allowed compound value properties (keep complete)**: `box-shadow`, `transform`, `clip-path`, `filter`, `text-shadow`, and `background-image` (for gradients) and `grid-template-columns/rows/areas` property values are inherently compound structures and **must** be kept in complete string form.
  * `box-shadow:0 8px 25px rgba(0,0,0,0.15)`
  * `transform:translateY(-2px) scale(1.02)`
  * `filter:blur(5px) brightness(0.8)`
  * `grid-template-columns:repeat(3, 1fr)`
* **Forbidden shorthand properties (must be split)**:
  * `background` (should be split into `background-color`, `background-image`, `background-repeat`, etc.)
  * `border` (should be split into `border-width`, `border-style`, `border-color`)
  * `font` (should be split into `font-size`, `font-family`, `font-weight`, etc.)
  * `transition` (should be split into `transition-property`, `transition-duration`, etc.)
  * `flex` (should be split into `flex-grow`, `flex-shrink`, `flex-basis`)
  * And other similar bundled shorthands.
* **Allowed multi-direction shorthands (follow "either-or" rule)**:
  * `padding`, `margin`, `overflow`, `border-radius`, `gap`
  * **"Either-or" principle**: If using these shorthand forms (like `padding:"10px"` or `padding:"10px 20px"`), **forbidden** to also use corresponding single-side properties (like `padding-left`) in same style rule set.
* **Compound Property Usage Example:**

```
// Wrong (forbidden shorthand)
<Block-Card> border:1px solid #eee
// Correct (split)
<Block-Card> border-width:1px border-style:solid border-color:#eee
```

#### **CSS Properties Forbidden in VL**:

* **Media queries (strictly forbidden)**: **Strictly forbidden** to use `@media` queries in any style definitions. VL's design philosophy is to create independent applications for each target device (PC, Phone, Pad), rather than responsive adaptation within a single application.
* **Animation keyframes**: Only **@keyframes** `animations` and **animation** `property` are not supported in VL syntax; other CSS styles are all supported.
* **display**: `display` property is entirely managed internally by components; not allowed to directly write `display:xxx`

### `<StateStyle>` Conditional Style Widget

Conditional style widget, through its Conditions and Trigger properties, can clearly specify a UI component's styles in different scenarios. Note: Conditional style widget's style definitions only support literal values and calc expressions; VL expressions are strictly restricted.

#### Components That Cannot Have Styles

The following components are pure logic containers with no DOM elements. Therefore, no style definitions can be added to these components (i.e., nothing outside angle brackets `<...>`). Consider these components "transparent" in layout, not participating in UI element layout at all. These components include: `If`, `For`, `TreeFor`, `AnimationGroup` (and other similar pure logic loop or conditional rendering components).

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

* Table names use **PascalCase**, field names use **camelCase**
* `default` supports literals: strings need quotes (like `default:""`), boolean (`true|false`), numbers, time strings, etc.
* Following system fields are **automatically added during system table creation**, **forbidden to explicitly declare equivalent fields**:
  * `_id (INT)`: Auto-increment primary key, starting from 1
  * `_user (STRING)`: Records submitting user's ID
  * `_create (TIMESTAMP)`: Creation time
  * `_update (TIMESTAMP)`: Update time
* `vecSource` is vector type field specific property, specifying vector field's source data fields.
* `Index` is optional child component; index field order takes effect.
* `data` is optional property; used to initialize some test data. Initialize 3-7 rows of data based on actual requirements. Unlike normal insert/update in services, test data needs to declare system fields for testing convenience:
  * `_id` field: Required, please start from 1;
  * `_user` field: Required, if no special requirements, please fix as "1";
  * `_create`/`_update` fields: Required, if no special requirements, please fix as current time

### Relation Definition

**Format**

```
<Relation-RelationName relation1 relation2 ..>
```

**Examples**

```
<Relation-User&Profile> users._id--userProfiles.userId // One-to-one single relation
<Relation-User&Posts> users._id<<posts.authorId  // One-to-many, single relation
<Relation-User&Messages> users._id<<messages.senderId users._id<<messages.receiverId // One-to-many, multiple relations
<Relation-Departments&Employees> departments._id<<employees.departmentId departments.managerId>>employees._id // One-to-many + many-to-one dual relations
<Relation-Warehouse&Inventory> warehouses.(code,region)<<inventory.(warehouseCode,warehouseRegion) // One-to-many, single relation composite key
```

* A Relation describes all relationships between two tables; relation names should strictly follow table names. For example, `<Relation-TableName1&TableName2>`, two table names connected by `&` symbol.
* Two tables may have multiple relations
* Each relation contains Table1's field, relation symbol, and Table2's field
* Relation symbols use `--`, `<<`, and `>>` representing one-to-one, one-to-many, and many-to-one respectively
* A relation may contain composite keys. Note: Composite key's multiple fields actually represent one pointer, so use `Table1.(compositeKeyField1,compositeKeyField2...)` format; do not write as two relations.

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

### `# Frontend Tree`

**Forbidden**: Absolutely forbidden to add another Section inside Section, meaning Section's Frontend Tree will NOT have statements like `<Section-ModuleName>...`.

### `# Frontend Public Events` (Frontend Public Event Definition)

In .sc/.cp files, public events can be defined, i.e., events sent from module internals to outside. These events can be listened to in application layer (.vx files) to complete inter-module/component interaction. Definition has two parts:

1. First in `# Frontend Public Events` section, top level, no indentation, use `@` symbol to define event method name:

**Definition Format:**

```
EVENT @eventName(param1(Type1),param2(Type2),...)
```

- **Parameter naming**: All parameters use **camelCase**, **without `_` prefix**.

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

In .sc/.cp files, a method can be defined as public method using `METHOD_PUB MethodName` format.

These methods can be called from upper-level application files (.vx/.sc).

Public methods can also be called internally within the file; calling method is identical to internal methods:

```
# Frontend Public Methods

METHOD_PUB Hide();RETURN {success:BOOL}
-$isVisible = false
-@modalClosed()
-RETURN {success:true}

# Frontend Event Handlers
<Button-Close "closeButton">.@click()
-Hide() // Call method directly, don't mistake public method as root component's method
```

Note: When calling public methods internally within file, do not mistake public methods as file root component's methods. For example inside Section, **there is no call method like `<Section-MyModule>.Hide()`**.

### Calling Services in Section

Section files can call any service defined in ServiceDomain files under the same project. Calling method:

1. Add needed ServiceDomain in component tree as a functional component:

```
# Frontend Tree

<ServiceDomain-DomainName "serviceId"> // First introduce service domain component
-<Service-ServiceName1> params:(param1(Type),param2(Type)...) returns:(output1(Type),output2(Type)...) // Introduce needed service with params and returns as component properties
-<Service-ServiceName2> ... // Another service under service domain
```

2. In Section's event handling or methods, call services defined in this ServiceDomain:

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
<VirtualTable-Users> "userTable" sourceTable:GroupUsers extraSpecs:{label(STRING):"Field Label",readOnly(BOOL):"Read Only",inputType(STRING):"Input Type"} mockData:[{..}]
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

* If current virtual table field binds to a field in current main table with different names, specify current main table field name in `sourceField` property, e.g., `<Field-title type:STRING sourceField:docTitle> // Virtual table field title corresponds to current main table entity field docTitle`
* If current virtual table binds to a field in another related table, use `~` jump operator, meaning "follow" Relation from current table to another table. Format: `localStartField~RelatedTable[anchorField].targetField`

  * Single jump: `<Field-departmentName> type:STRING sourceField:departmentId~Departments[_id].departmentName`
  * Multi-jump: `<Field-managerName> type:STRING sourceField:departmentId~Departments[_id].managerId~Users[_id].userName`
* Any jump must have corresponding Relation, and this Relation must be many-to-one or one-to-one, otherwise causes data expansion

**Correct Example**

```vl
sourceField:departmentId~Departments[_id].managerId~Users[_id].userName
            └─────┬─────┘ └─────┬─────┘└────┬────┘ └──┬──┘└───┬───┘
               Jump1 start    Jump1 anchor   Jump2 start  Jump2 anchor  Target field
```

**Jump 1**: `departmentId~Departments[_id]`

```
Find Relation: <Relation-Employees&Departments> employees.departmentId>>departments._id
Direction check: >> (many-to-one) ✅
```

**Jump 2**: `managerId~Users[_id]`

```
Find Relation: <Relation-Departments&Users> departments.managerId>>users._id
Direction check: >> (many-to-one) ✅
```

**Final Result**: Get `users.userName` field

**Error Example**

```vl
// ❌ Error: Contains one-to-many jump
sourceField:_id~Orders[userId].amount
            └┬┘ └──┬──┘└──┬──┘└──┬──┘
          Start  Target table  Anchor  Target field
```

**Error Reason**

```
Find Relation: <Relation-Users&Orders> users._id<<orders.userId
Direction check: << (one-to-many) ❌

Compilation error:
"sourceField path contains one-to-many relation (users._id << orders.userId)"
"One-to-many causes single record to map to multiple related records, producing data duplication"
"Suggestion: Set Orders as main table, then jump to Users"
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

In vs files, all services callable externally should be defined under `# Services` section, prefixed with `SERVICE ServiceName`

Each service acts as a public method of current ServiceDomain. These methods can be called in Section files using: `<ServiceDomain-DomainName "serviceId">.ServiceName()`

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

* Naming uses camelCase prefixed with `tx`
* Use ROLLBACK keyword to rollback transaction
* Transaction usage is identical to internal methods; cannot be called directly from frontend like services

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

#### User Center

VL system uniformly provides User Center application; any project will automatically bind a User Center without specifying in VL. User Center provides:

* User registration, login
* User information editing
* Admin user management (user list viewing, user disable/deactivation)
* Admin user permission configuration (ABAC combined with RBAC permission management system, configurable resource groups, manage resource group access rules, etc.)

#### Application Interaction with User Center

In frontend applications, `<ClientUserCenter>` component can be used to call User Center interface methods for frontend custom login interface related functionality.

Frontend and backend applications can both use `SYSENV.currentUser` to get current logged-in user information.

#### Logic NOT Allowed in Applications

Entity tables, virtual tables, services, frontend element access permissions in applications will be set by administrators or an independent AI agent in User Center management interface; therefore **please do not hardcode any user permission-related logic in code**, including:

* Whether a service can be accessed
* Data access scope restrictions, e.g., whether can only access self-created data, or can access current department, or global data; these restrictions will be configured directly in User Center, project code should not have user-based data filtering logic
* Whether a page can be accessed
* Whether a table or field can be accessed, can be updated, etc.

Example:

```
// Error: Hardcoding user-related filter logic in code
<messageTable>.select([["_user","eq",SYSENV.currentUser.userId]],[["_update","desc"]],[offset,limit],null) -> _result

// Correct: Code only handles business logic; user-related data scope permissions left to permission configuration module
<messageTable>.select(null,[["_update","desc"]],[offset,limit],null) -> _result
```

## VL Generation Hard Rules (MUST / MUST NOT)

### 1. Widget Mounting Rules

- **MUST**: `StateStyle/Animation/UseDraggable/UseDropTarget/...` widgets can only be mounted under UI-bearing components (Basic UI / Layout Containers / Extended UI / Frontend Root Components).
- **MUST NOT**: Widgets cannot be placed directly under logic containers like `If/For/TreeFor`; nor under non-UI functional components like `Trigger/FrontendApi/WindowEventListener/ClientUserCenter`.

### 2. Logic Container Restrictions

- **MUST NOT**: Logic containers have no UI, cannot have styles (nothing outside angle brackets), have no id, and cannot be accessed in subsequent code.

### 3. Define Before Use

- **MUST**: All `$variables` must be declared in `Public Props / Global Vars / Derived Vars` before being referenced.
- **MUST**: Except for system method classes, any component instance must first appear in `# Frontend Tree` before being called. Specifically, `Trigger/FrontendApi/WindowEventListener/ClientUserCenter` must also be declared first.

### 4. Public Events

- **MUST**: Any public event exposed externally must be declared with `EVENT` in `# Frontend Public Events` before being triggered; event names use camelCase.

### 5. Expression / Derived Variable Computation

- **MUST**: Expressions and Derived Vars can only use pure expressions and `PIPE _xxx()` functions.
- **MUST NOT**: Call `METHOD` in expressions or Derived Vars.

### 6. Component Reference Name Strict Matching

- **MUST**: In `.vx` files, `<Section-xxx>` / `<Component-xxx>` where `xxx` must exactly match the corresponding file's root component name; do not substitute component name with id.
- **MUST**: App does not directly access internal child component instances of Section/Component; communicate only through Public Props/Events/Methods.

### 7. Trigger Properties and Units

- **MUST**: Trigger only uses `repeatTimes / interval(seconds) / autoPlay / isAnimate`; invented properties like `active` are forbidden.
- **MUST**: Units are strict: `Trigger.interval` = seconds; `ClientUtils.delay` = milliseconds; `SysUI.showToast.duration` = seconds.

### 8. App (.vx) Structure (Per Current Editor Version)

- **MUST**: `.vx` files do not generate Stage; Page must be a direct child node of App root component.
- **MUST NOT**: Forbidden to generate `Stage.@routeChange` and other Stage events.
- **MUST**: Route entry logic goes in `Page.@init()`; if browser events need to be monitored, use `WindowEventListener` (event names in camelCase).

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

#### FrontendApi (Frontend API Request Client, Section/Component Only)

Frontend API request client component. This component has no UI and is only used to encapsulate API request configuration and trigger logic.

**Properties:**

* `url(STRING)`: Required. API URL, supports path parameters like `/users/{id}`
* `method(STRING)`: Optional. HTTP method, defaults to "GET"
* `headers(OBJECT)`: Optional. Default request headers
* `timeout(INT)`: Optional. Request timeout duration (seconds)
* `params:(...)`: Optional. Parameter type declarations for send() method, e.g., `params:(page(INT),size(INT))`
* `returns:(...)`: Optional. Return value type declarations, e.g., `returns:(success(BOOL),data([{}]))`

**Methods:**

* `send(param1, param2, ...)` - Smart parameter allocation (recommended)

  * **Description**: Automatically allocates parameters based on HTTP method
  * **Allocation Rules**:

    * GET/DELETE: All parameters → URL query parameters
    * POST/PUT/PATCH: All parameters → Request body
    * Path parameters (like `{id}` in URL): Automatically identified and replaced
  * **Return Value**: `{success(BOOL), message(STRING), data(JSON)}`
  * **Examples**:

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
* `customSend(headers?, body?, params?, url?, timeout?, method?)` - Full customization

  * **Description**: For complex scenarios requiring fine-grained control over request parameters
  * **Parameters**:
    * `headers(OBJECT)`: Optional. Custom request headers
    * `body(JSON)`: Optional. Request body
    * `params(OBJECT)`: Optional. URL query parameters
    * `url(STRING)`: Optional. Override default URL
    * `timeout(INT)`: Optional. Override default timeout
    * `method(STRING)`: Optional. Override default method
  * **Return Value**: `{success(BOOL), message(STRING), data(JSON)}`
  * **Example**:
    ```vl
    <FrontendApi-SearchUsers "searchApi"> url:"/api/users/search" method:"POST" returns:(success(BOOL),users([{}]))

    <Button-Search "searchButton">.@click()
    -<FrontendApi-SearchUsers "searchApi">.customSend(null, {keyword:"John Doe"}, {page:1,size:20}, null, null, null) -> _result
    # Actual request: POST /api/users/search?page=1&size=20, Body: {"keyword":"John Doe"}
    ```

**Core Usage Rules**

* Only use `customSend` when `send` method cannot meet requirements, **limited to** these scenarios:
  * POST/PUT/PATCH needs URL parameters
  * Additional header parameters needed
  * Dynamic URL or request method needed

#### ClientUserCenter (User Center Client)

Frontend user center client. Must be declared in component tree, e.g., `<ClientUserCenter-UserAuth>`. Typically only one instance is needed per file.

**Methods:**

* `userLoginPassword(userName, password)`: Username/password login
  * Parameters:
    * `userName(STRING)`: Username
    * `password(STRING)`: Password
  * Return Value:
    * `isSuccess`
    * `failReason`
* `userLogout()`: Logout
  * Return Value:
    * `isSuccess`
    * `failReason`
* `jumpToUserCenter()`: Navigate to user center
* `sendRegistrationSMS(phoneNumber)`: Send SMS verification code
  * Parameters:
    * `phoneNumber(STRING)`: Phone number
  * Return Value:
    * `isSuccess`
    * `failReason`
* `registerUser(userName, password, verificationCode, phoneNumber)`: Register user
  * Parameters:
    * `userName(STRING)`: Username
    * `password(STRING)`: Password
    * `verificationCode(STRING)`: SMS verification code
    * `phoneNumber(STRING)`: Phone number
  * Return Value:
    * `isSuccess`
    * `failReason`

**Events:**

* `@loginDone()`: Triggered when login completes

### Basic UI Components (Section/Component Only)

All basic UI components can only be added in Section/Component, not in App.

**!!Note:** Except for `<ButtonContainer>`, all other basic UI components **cannot have child components**!

**!!Note:** The "Fixed CSS styles" section is **descriptive only**; **`display` cannot be overwritten in VL code**;

#### Text (Basic Text)

**Properties:**

- `value(STRING)`: Text content

**Fixed CSS styles:**

display: block

#### Button (Basic Button)

**Properties:**

- `value(STRING)`: Button text
- `disabled(BOOL)`: Whether disabled

**Fixed CSS styles:**

* display: inline-block

#### ButtonContainer (Button Container)

Create composite buttons by adding child components, such as an Icon and Text forming a button. Note: ButtonContainer itself cannot set the value property; all content is rendered through its child components.

**Properties:**

- `disabled(BOOL)`: Whether disabled

**Fixed CSS styles:**

* display: inline-flex

##### Button Component Usage Notes

Correct VL form:

```vl
-<ButtonContainer-PrevMonthBtn "preMonthButton"> ariaLabel:"Previous Month" StyleClass:..
--<StateStyle-PrevBtnHover> trigger:"hover" background-color:"#f5f5f5"
--<Row-Center> StyleClass:..
---<Icon-PrevIcon> svgCode:'<svg xmlns="http://www.w3.org/2000/svg" width="18px" height="18px" fill="currentColor" viewBox="0 0 256 256"><path d="M165.66,202.34a8,8,0,0,1-11.32,11.32l-80-80a8,8,0,0,1,0-11.32l80-80a8,8,0,0,1,11.32,11.32L91.31,128Z"></path></svg>' width:"18px" height:"18px"
```

Button component cannot have child components; use ButtonContainer instead.

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

Visual element for separating content.

**Properties:**

- `orientation(STRING)`: Divider direction, options: "horizontal", "vertical", defaults to "horizontal".

**Fixed CSS styles:**

display: block

#### Input (Basic Input)

Basic text input.

**Default CSS styles:**

display: inline-block

**Properties:**

- `value(STRING)`: Input content. **Note: This property is mainly for setting initial values and does not reflect real-time user input.**
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

For rendering SVG, external resource URLs, or Font Awesome library icons. All icons should use this component uniformly. Icon resources can come from svgCode, url, or Font Awesome (`fontSet+content`), with priority: **svgCode > url > fontSet+content**.

**Properties:**

* `svgCode(STRING)`: Directly specify SVG code
* `url(STRING)`: External resource URL, VL internally uses HTML mask functionality to convert it to an icon

- `fontSet(STRING)`: Font Awesome icon library, options:
  - fa-solid-900
  - fa-regular-400
  - fa-brands-400
- `content(STRING)`: Font Awesome icon's **Unicode character code**, e.g., `"f170"`. **Note**: This value **must** be the icon's Unicode code, **not** a CSS class name (like `"fas fa-book"`) or icon name (like `"book"`). If set incorrectly, the icon won't display and you may see text like "OOK".

**Fixed CSS styles:**

display: inline-block

**Notes:**

* When icon content is specified via svgCode or url, size should be set via CSS width/height properties
* When icon content is specified via fontSet+content, it's essentially text, so size should be set via CSS font-size property (CSS styles go outside angle brackets), e.g., `<Icon-User fontSet:"fa-solid-900" content:"f007"> font-size:"16px"`

#### Chart (Chart)

Chart visualization component.

**Properties:**

- `option({})`: Standard ECharts option configuration for chart rendering. Example data:

  ```json
  {
    "title": { "text": "Data Analysis Chart", "left": "center" },
    "tooltip": { "trigger": "axis" },
    "xAxis": { "type": "category", "data": ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"] },
    "yAxis": { "type": "value" },
    "series": [{
      "name": "Visits",
      "type": "line",
      "data": [150, 230, 224, 218, 135, 147, 260],
      "itemStyle": { "color": "#5470c6" }
    }]
  }
  ```

  When option is assigned this data, it creates a weekly visits line chart.

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

#### Block (Basic Layout Container)

Layout container with display: block.

#### Modal (Modal Layer)

Modal consists of an **outer full-screen flex container** and an **inner block content container**: the outer handles overlay and positioning, its `display` cannot be changed; the inner hosts content (block). Note: Width, height, background-color and other style properties added to Modal component only control the inner container's dimensions. The outer flex container is a full-screen container (serving as overlay) and cannot have width, height, color, or other CSS styles set.

**Properties:**

- `mask(BOOL)`: Whether to enable background overlay. If enabled, automatically generates a full-screen overlay, default overlay color is black (#000000)
- `maskColor(STRING)`: When background overlay is enabled, overlay color can be set. Default is black #000000
- `placement(STRING)`: Banner position relative to current window, options: "topLeft", "topCenter", "topRight", "middleLeft", "middleCenter", "middleRight", "bottomLeft", "bottomCenter", "bottomRight". Default is "middleCenter"

**Special Events:**

- `@clickMask()`: Triggered when clicking the component's overlay layer (requires "Background Overlay" property enabled).

For `modal` components, background overlay effect should preferably be controlled via the component's `mask:true` and `maskColor:"rgba(0,0,0,0.5)"` functional properties. Avoid setting `position:fixed`, `top:0`, `left:0`, `background-color` and other styles for overlay effect on Modal component itself, unless highly customized overlay behavior is needed.

#### Col, Row, Grid (Flex Column, Flex Row, Grid Container)

Containers with fixed flex and grid layouts.

- Col: Flex column, display fixed as flex, flex-direction defaults to column but can be set to column-reverse;
- Row: Flex row, display fixed as flex, flex-direction defaults to row but can be set to row-reverse;
- Grid: Grid container, display fixed as grid

#### Table, TableHeader, TableRow, TableCell, TableCellContainer (Table Series Components, Section/Component Only)

For creating HTML tables:

* Table (Basic Table Container): Corresponds to table tag, automatically creates tbody tag for TableRow components added directly under it, no need to add tbody component separately
* TableHeader: Corresponds to thead tag
* TableRow: Corresponds to tr tag, can be added directly under Table or under TableHeader
* TableCell, TableCellContainer: Can only be added under TableRow component. When added under TableRow in TableHeader, corresponds to th tag; when added under TableRow directly under Table, corresponds to td tag.
  * Core Properties
    * rowspan and colspan: Specify cells to span
  * Content Modes:
    * TableCell component can only specify text content via Value property, **cannot add child components**
    * TableCellContainer component **has no Value property** but can add child components to render content

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
---<TableCell-HId> value:"ID" StyleClass:..
---<TableCell-HName> value:"Name" StyleClass:..
---<TableCell-HAction> value:"Action" StyleClass:..
-<For-UserRows> sourceArray:$userList loopVar:[_user, _index0]
--<TableRow-UserRow>
---<TableCell-CellId> value:_user.id StyleClass:..
---<TableCell-CellName> value:_user.name StyleClass:..
---<TableCellContainer-CellAction> StyleClass:.. // Use TableCellContainer to hold buttons
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
<Stage-Root>
-<Page-MainPage "mainPage"> path:"main"

# Frontend Event Handlers
<Page-MainPage "mainPage">.@init()
```

**Important Rules:**

- `<Page>` component must be placed directly under `<Stage>` tag
- `<Page>` cannot be nested inside any other container
- `<Page>` cannot contain other `<Page>` components

### Logic Container Components

Logic container components have no UI themselves, cannot be used for layout, **cannot add Style styles (nothing outside angle brackets)**, and since logic containers cannot be accessed in subsequent methods, they **have no id property**.

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

* `For-` is a fixed prefix, followed by an arbitrary descriptive name (following component ID naming conventions, PascalCase).
* `sourceArray` and `loopVar` properties **must** be inside angle brackets `<...>`. `sourceArray` value doesn't need parentheses if it's a variable, `loopVar` value is a literal array and also doesn't need parentheses.
* `sourceArray` property specifies the array variable to traverse (usually global variable `$var` or local variable `_var`).
* `loopVar` property defines variable names representing current array element and index in each loop iteration.

  * `loopVar` value is an array containing two string elements `[loop item variable name, index variable name]`.
  * Loop item and index variable names must use underscore `_` prefix and camelCase naming.
  * Recommended naming conventions:
    * For index variables, use `_indexX` format, where X is a number starting from 0 to distinguish nested loops (e.g., first level is `_index0`, nested second level is `_index1`).
    * For loop item variables, use descriptive names (like `_user`, `_product`, `_noteItem`) for better readability, or use `_itemX` for generic iteration (like `_item0`, `_item1`, `_item2`). Example: `FOR (_user, _index0) IN $userList` or `FOR (_item0, _index0) IN dataArray`.
* Inside `<For-name>`, child components and their properties can access current loop's element and index via variable names defined in `loopVar`.
* **Best Practice**: When `# Frontend Tree` needs to render list items, **must** use `<For-name>`. In event handlers registered on list items, when referencing current item's data or index, **must** use variables defined in `<For-name>`'s `loopVar` (e.g., `_item0`, `_index0`). Avoid manually flattening list items then incorrectly referencing non-existent loop variables in event handlers.
* **Loop Variable Scope**: Loop variables (`_itemX`, `_indexX`) are only valid in property bindings, conditional expressions, and event handlers of `<For-name>` container's direct children and their descendant components. Outside `<For-name>`, or in event handlers of components not statically defined inside `<For-name>`, referencing these loop variables is an error.

**Example:** (Rendering a user list)

```
# Frontend Global Vars
$userList([{id:INT,name:STRING,age:INT}]) = [{id:1,name:"Alice",age:30},{id:2,name:"Bob",age:25}]

# Frontend Tree
<Block-UserListContainer>
-<For-UserItems> sourceArray:$userList loopVar:[_item0, _index0]
--<Block-UserItem> // UserItem is For loop's direct child component
---<Text-UserName> value:("Name: " + _item0.name)
---<Text-UserAge> value:("Age: " + _item0.age)
---<Button-SelectUser "selectButton"> value:"Select" // Assuming this button has an event handler
---<If-IsAdult> conditions:(_item0.age >= 18)
----<Icon-AdultIcon> content:"f007"

# Frontend Event Handlers

// Event handler, if <Button-SelectUser> is part of <For-UserItems>
// and event is registered on loop item
<Button-SelectUser "selectButton">.@click() // Assuming this button is part of dynamically generated UserItem
-logMessage(("Selected user ID: " + _item0.id + " at index: " + _index0)) // Correct: _item0, _index0 are valid in this event handler because event source is inside loop
```

**Combining `<If-name>` and `<For-name>`:**

```
# Frontend Global Vars

$products([{id:INT, name:STRING, price:FLOAT, stock:INT}]) = [{id:101, name:"Laptop", price:1200.00, stock:5}, {id:102, name:"Mouse", price:25.50, stock:0}]

# Frontend Tree

<Block-ModuleRoot "root">
-<For-ProductCards> sourceArray:$products loopVar:[_item0, _index0]
--<Block-ProductCard>
---<Text-ProductName> value:_item0.name
---<Text-ProductPrice> value:("$" + _item0.price._formatPrice())// Assume PIPE exists
---<If-InStock> conditions:(_item0.stock > 0)
----<Text-StockStatus> value:("In Stock (" + _item0.stock + ")")
----<Button-AddToCart "addButton"> value:"Add to Cart"
---<If-OutOfStock> conditions:(_item0.stock <= 0)
----<Text-StockStatus> value:"Out of Stock"
----<Button-NotifyMe "notifyButton"> value:"Notify Me"
```

**Notes:**

* `<If-name>` and `<For-name>` are mainly used in component trees to control component rendering logic.
* In component trees, if you only need to render different styles for the same component based on conditions, use **conditional styling** (`<StateStyle>`) instead of `<If-name>`, which is only for conditionally rendering different component tree structures.

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

* `If-` is a fixed prefix, followed by an arbitrary descriptive name (following component ID naming conventions, PascalCase).
* `conditions` property receives an expression that returns a boolean value. This property **must** be inside angle brackets `<...>`.
* Child components inside `<If-name>` are only rendered when `conditions` expression evaluates to `true`.
* `conditions` expression can reference global variables (`$var`), local variables (`_var`), especially **loop variables** (`_itemX`, `_indexX`) from loops, and system variables (`SYSENV.xxx`).
* **Core Principle Warning**: `<If-name>` **must never** be used to switch different UI layouts based on device type (like PC or mobile). This seriously violates the "one application, one target device" design philosophy. This component's `conditions` property **must** reflect application's **business logic state** (e.g., whether user is logged in, whether data is valid, whether in edit mode, etc.), not device environment.
  * **Wrong usage (strictly forbidden)**: `<If-name> conditions:(SYSENV.deviceType == 'mobile')`
  * **Correct usage (recommended)**: `<If-name> conditions:($formData.isValid && SYSENV.currentUser.isLogin)`
  * **Correct usage (recommended, with comparison operator)**: `<If-name> conditions:($user.points > 100)`

**Example:**

```
<Block-UserStatus>
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

  * `_itemX`: Current node's data object.
  * `_indexX`: Current node's index in the entire flattened array.
  * `_levelX`: Current node's level depth (top level is 0).
  * `_expandedX`: Boolean indicating whether current node is expanded.
  * `_levelIndexX`: Current node's index among its sibling nodes.

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

#### ClientUtils (Frontend Utility Class)

Frontend general utility class.

**Methods:**

- `delay(milliSecond)`: Sets delayed operation, executes callback after specified milliseconds.
- `consoleLog(title,message)`: Prints debug info to console, can specify info name for easier debugging
- `switchRoute(path)`: Switches system path, path is a string like "home", "user/info"

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

* `browseImage(outputBase64, compressedWidth, compressedHeight, type, encoderOptions)`: Read local image
  * Description: Reads one local image's info, returns Base64, name, type, size, width/height, temporary local path, file object, etc.
  * Parameters:
    * `outputBase64(Boolean)`: **Output base64** (whether to convert to base64 format after reading, default is yes. Converting to base64 has some performance cost. If only displaying image and uploading, use temporary path + file object method without outputting base64. If choosing to output base64, src field contains base64 result, otherwise no base64 result returned.)
    * `type(STRING)`: **Output format** (Choose jpg or png format for read image. If jpg, can further specify image quality.)
    * `encoderOptions(INT)`: **Output quality** (Output image quality, a value between 0-1. Note: This option only works when output format is jpg; png is a non-compressible format.)
  * Return Value:
    * `base64Code`: **base64 image** (Image converted to base64 format resource URL. Note: Only has result when output base64 is enabled during reading, otherwise empty.)
    * `name`: **Name** (Image name.)
    * `type`: **Type** (Image type.)
    * `size`: **Size** (Image file size.)
    * `width`: **Width** (Image width.)
    * `height`: **Height** (Image height.)
    * `blobUrl`: **Temporary local path** (Image temporary local path.)
    * `failureReason`: **Read failure reason** (Reason for failed image reading.)
    * `file`: **File object** (Read image file object, can be used as file path for uploading.)
* `browseImages(maxNumberPic, outputBase64, compressedWidth, compressedHeight, type, encoderOptions)`: Read multiple local images
  * Description: Reads multiple local images' info, returns an object array where each object contains src, name, type, size, width, height, path, file, etc.
  * Parameters:
    * `maxNumberPic(INT)`: **Max image count** (Maximum number of images allowed per upload.)
    * `type(STRING)`: **Output format** (Choose jpg or png format for read images.)
    * `encoderOptions(INT)`: **Output quality** (Output image quality, value between 0-1.)
  * Return Value:
    * `imageList`: **Image list** (Array of all read images' info. Returns object array, each containing src, name, type, size, width, height, path, file - see single image read return value description for details.)
    * `failureReason`: **Read failure reason** (Reason for failed image reading.)
* `browseFile(outputBase64, accept)`: Read local file
  * Description: Reads one local file's info, returns name, type, size, temporary local path, base64 data, etc. Not supported in WeChat browser, webApp WeChat mini-program, Alipay/DingTalk mini-programs.
  * Parameters:
    * `outputBase64(Boolean)`: **Convert to base64** (Default no. When yes, automatically converts file to base64, allowing base64 string to be read from result, otherwise cannot read.)
    * `accept(STRING)`: **Allowed file types** (Parameter can limit selectable file types, a string constructed from file mimeTypes. For multiple types, separate with comma, e.g., "image/*,application/x-zip-compressed")
  * Return Value:
    * `name`: **Name** (File name.)
    * `type`: **Type** (File type.)
    * `size`: **Size** (File size.)
    * `blobUrl`: **Temporary local path** (File temporary local path.)
    * `base64Code`: **base64 data** (File's base64 format data. Note: Must choose base64 format during reading to access.)
* `browseFiles(accept, maxNumberFile)`: Read multiple local files
  * Description: Reads multiple local files' info, returns object array where each object contains name, type, size, temporary local path, etc. Not supported in WeChat browser, webApp WeChat mini-program, Alipay/DingTalk mini-programs.
  * Parameters:
    * `accept(STRING)`: **Allowed file types** (Parameter can limit selectable file types, a string constructed from file mimeTypes.)
    * `maxNumberFile(INT)`: **Max file count** (Maximum number of files allowed per upload.)
  * Return Value:
    * `fileList`: **File list** (Array of all read files' info.)
    * `failureReason`: **Read failure reason** (Reason for failed file reading.)

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

### Widgets

Widgets are additional functional components added under entity UI components (basic UI, layout containers, etc.) that can provide form field handling, dynamic styling, animation, drag, scroll, and other behaviors for target entity UI components.

#### StateStyle (State and Conditional Style Component)

`<StateStyle>` is used to define styles applied to its parent component during specific interaction states (like mouse hover) or when specific logic conditions are met. It makes dynamic style management more centralized and clear.

**Syntax:**

```
<Parent Component ...>
-<StateStyle-StateName> trigger:"stateString" conditions:booleanExpression style1:value ..
-<StateStyle-StateName> conditions:(booleanExpression) style1:value ..
```

**Notes:**

* `<StateStyle>` must be a direct child component of the UI component it affects (one level deeper indentation).
* **StateStyle component names should reflect their purpose**, e.g., `<StateStyle-HoverEffect>`.
* A parent component can have multiple `<StateStyle>` child components for handling different states and conditions.
* `<StateStyle>` style properties **do not allow property expressions**, only literal values or CSS native calc expressions.
* `trigger` and `conditions` properties are functional properties.
  * **`trigger` property**: Receives a specific string for responding to UI's built-in interaction states, corresponding to CSS pseudo-classes and some pseudo-elements. Common values include "hover", "active", "focus", "disabled", etc.;
  * **`conditions` property**: Receives an expression returning a boolean value. When expression is `true`, styles are applied. This allows style changes based on arbitrary application state (like `$isInvalid`, `$nightTheme == 'dark'`).

**Example:**

```
<ButtonContainer-Submit> disabled:(!$canSave) StyleClass:..
-<Text-SubmitLabel> value:"Submit"
-<StateStyle-HoverEffect> trigger:"hover" background-color:"#2980b9" // Darker on hover
-<StateStyle-DisabledState> trigger:"disabled" background-color:"#bdc3c7" cursor:"not-allowed" // Disabled state style
-<StateStyle-ErrorBorder> conditions:$formHasError border-width:"2px" border-style:"solid" border-color:"#e74c3c" // Error state style
```

#### Animation (Animation Effect)

Added as a "widget" to target UI component to add animation effects. After adding, VL system automatically wraps the target UI element with a transparent Div to implement animation effects.

**Properties**

* **animationName (Animation Type)**

  * **Description:** Selects animation type from system default animation library, divided into three categories: emphasis, entrance, and exit animations. Each animation type has multiple effects, each possibly with multiple directions.
  * **Options:**

    * Emphasis: bounce, flip, flash, jello, pulse, rubberBand, shake, swing, tada, wobble, hinge, rotateC, rotateAC;
    * Entrance: fadeIn, fadeInDown, fadeInUp, fadeInLeft, fadeInRight, bounceIn, bounceInDown, bounceInUp, bounceInLeft, bounceInRight, rotateIn, rotateInDownLeft, rotateInUpLeft, rotateInDownRight, rotateInUpRight, slideInDown, slideInUp, slideInLeft, slideInRight, zoomIn, zoomInDown, zoomInUp, zoomInLeft, zoomInRight, flipInX, flipInY, rollIn;
    * Exit: fadeOut, fadeOutDown, fadeOutUp, fadeOutLeft, fadeOutRight, bounceOut, bounceOutDown, bounceOutUp, bounceOutLeft, bounceOutRight, rotateOut, rotateOutDownLeft, rotateOutUpLeft, rotateOutDownRight, rotateOutUpRight, slideOutDown, slideOutUp, slideOutLeft, slideOutRight, zoomOut, zoomOutDown, zoomOutUp, zoomOutLeft, zoomOutRight, flipOutX, flipOutY, rollOut;
* **duration (Animation Duration in Seconds)**

  * **Description:** Animation effect duration in seconds.
* **delay (Start Delay in Seconds)**

  * **Description:** Animation start delay in seconds.
* **interationCount (Play Count)**

  * **Description:** Number of times to play each trigger, default is 1 (play once then stop).
* **loop (Loop Playback)**

  * **Description:** Toggle to control animation looping, disabled by default.
* **playAnimate (Preview Animation)**

  * **Description:** Click button to preview current animation effect.
* **trigger (Trigger Timing)**

  * **Description:** Method to trigger current animation.
  * **Options:**
    * **onRender (Auto Play):** On enter.
    * **onHide (On Object Hide):** On leave.
    * **onClick (Click):** On click.
    * **onHover (Mouse Enter):** On mouse enter.
    * **custom (Manual Call):** Manual call.
* **initialVisibility (Hide Before Start)**

  * **Description:** Whether to hide component with this animation before trigger.
* **editAnimate (Edit Animation)**

  * **Description:** Click button to open animation editor for current animation.
* **hideAfterCompletion (Hide After Completion)**

  * **Description:** Whether to hide component after animation ends. Useful for exit-style animations.

**Methods**

* **play (Replay)**
  * **Description:** Plays current animation group from beginning. Note: Animation group is a whole; each play starts from beginning, unlike timeline animations that continue from pause point.
* **stop (Stop Playback)**
  * **Description:** Stops current animation group playback.

#### AnimationGroup (Animation Group)

`data-animate-group` component manages triggering and playback of a group of animations. It's a container that can only contain animation components.

**Properties**

* **trigger (Trigger Timing)**
  * **Description:** Selects when animation group triggers, defaults to auto-play. Can also select click, mouse enter, etc., equivalent to adding an auto event. Can also select "manual call" which requires control via event panel.
  * **Options:**
    * **onRender (Auto Play):** Triggers on object enter/load.
    * **onHide (On Object Hide):** Triggers when object is set invisible via action.
    * **onClick (Click):** Triggers on object click.
    * **onHover (Mouse Enter):** Triggers on mouse enter object.
    * **custom (Manual Call):** Manually triggered via other event actions.
* **initialVisibility (Hide Before Start)**
  * **Description:** Whether to hide object before animation group starts.
* **hideAfterCompletion (Hide After Completion)**
  * **Description:** Whether to hide object after animation group ends.

**Methods**

* **play (Replay)**
  * **Description:** Plays current animation group from beginning. Note: Animation group is a whole; each play starts from beginning, unlike timeline animations that continue from pause point.
* **stop (Stop Playback)**
  * **Description:** Stops current animation group playback.

#### UseDraggable (Drag Source Widget, Section/Component Only)

useDraggable gives any parent UI component it's attached to the ability to be dragged as a drag source. It manages data setting at drag start and state cleanup at drag end. Drag operations are complex user interactions and can only be added in Section/Component.

**Core Events**

* **@dragStart**: Triggered when user starts dragging the element with this widget attached. This is the only time `setData()` can be called on `dataTransfer`. Widget also sets `event.dataTransfer.effectAllowed` to declare allowed drag operation types, affecting mouse cursor style during drag.
* **@dragEnd**: Triggered when drag operation ends, whether drop succeeded, was cancelled, or dropped on invalid area. Event emits `event.dataTransfer.dropEffect` parameter to determine final result of drag-drop operation.

#### UseDropTarget (Drop Target Widget, Section/Component Only)

useDropTarget gives any parent UI component it's attached to the ability to serve as a drop target (receive drag-drop operations). Used to manage drop zone highlighting and data extraction from drop events.

**Core Events**

* **@dragEnter**: Triggered when dragged element first enters the area of component with this widget attached. Widget **automatically calls `event.preventDefault()`**. Also emits `event.dataTransfer.types` to check if drag data type is acceptable.
* **@dragOver**: Continuously triggered when dragged element moves within the area of component with this widget attached. **Must call `event.preventDefault()`** to allow `drop` event to trigger. Also writes `event.dataTransfer.dropEffect` to set area's preferred operation type, affecting mouse cursor style.
* **@dragLeave**: Triggered when dragged element leaves the area of component with this widget attached.
* **@drop**: Triggered when user releases mouse button over the area of component with this widget attached. Widget internally calls `event.preventDefault()` and reads `event.dataTransfer.getData()` to get data passed from drag source.

#### Component Drag Event Registration Notes

Drag-related events must be registered on corresponding drag widgets, not on regular components:

**Example:**

```vl
<Section-SortableList "root">

# Frontend Global Vars
$draggedIndex(INT) = -1
$items([{id:INT,text:STRING}]) = [{id:1,text:"Item A"},{id:2,text:"Item B"},{id:3,text:"Item C"}]

# Frontend Tree
<Block-ModuleRoot>
-<Col-ListContainer>
--<For-Items> sourceArray:$items loopVar:[_item0,_index0]
---<Block-Item draggable:true> StyleClass:..
----<UseDraggable-ItemDrag "dragItem">
----<UseDropTarget-ItemDrop "dropTarget">
----<StateStyle-Dragging> conditions:(_index0 == $draggedIndex) opacity:"0.4"
----<StateStyle-Active> trigger:"active" cursor:"grabbing"
----<Text-ItemText> value:_item0.text

# Frontend Event Handlers

<UseDraggable-ItemDrag "dragItem">.@dragStart()
-$draggedIndex = _index0

<UseDropTarget-ItemDrop "dropTarget">.@dragOver(event)
-event.preventDefault()

<UseDropTarget-ItemDrop "dropTarget">.@drop()
-IF $draggedIndex == -1 || $draggedIndex == _index0
--$draggedIndex = -1
--RETURN
-_draggedItem({id:INT,text:STRING}) = $items[$draggedIndex]
-$items.splice($draggedIndex, 1)
-$items.splice(_index0, 0, _draggedItem)
-$draggedIndex = -1

<UseDraggable-ItemDrag "dragItem">.@dragEnd()
-$draggedIndex = -1

</Section-SortableList>
```

**Key Points:**

1. Drag-related events (`@dragStart`, `@dragEnd`, `@dragOver`, `@drop`) must be registered on corresponding drag widgets (`<UseDraggable>`, `<UseDropTarget>`)
2. For `@drop` event to work, must first add `@dragOver` event and prevent default behavior in that event
3. Regular components (like `<Row-SortableItem>`) cannot directly listen to drag events

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

* Section's properties/methods/events must strictly follow Section's file definition, **strictly forbidden to invent Section properties/methods/events**

#### Component (App or Section)

Custom additional components defined in current project can be used in App or Section, introduced using `<Component-ComponentName>`. Note: Component is an independent component and cannot have non-widget child components added under it.

### Extended UI Components (Section/Component Only)

#### 1. `SingleSelect Group` (Single Select Group)

`SingleSelect Group` handles all single-selection scenarios. Member components share core logic and data model, differing only in UI and interaction.

##### Group Common Interface

* **Functional Description**: All components in this group should implement a "clear" function internally (e.g., via a configurable clear button). After user interaction triggers it, component sets `value` to `null` and fires `onChange` event.
* **Properties**
  * `options`: `Array<Object>` - Required. Array of option objects. Each object should contain `label` (String) and `value` (String | Number).
  * `value`: `String | Number` - Current selected option's `value`. Defaults to `null` if nothing selected.
  * `disabled`: `Boolean` - Optional. Whether to disable options.
* **Methods**
  * `focus()`: Makes selector gain focus.
  * `blur()`: Makes selector lose focus.
* **Events**
  * `@onChange(newValue)`: Triggered when selected value changes. `newValue` is selected option's `value`.
  * `@onFocus()`: Triggered when selector gains focus.
  * `@onBlur()`: Triggered when selector loses focus.

##### Sub-component Introduction and Specific Interfaces

1. **`SingleSelect_Dropdown` (Dropdown Single Select)**
   * **Introduction**: Common PC-style dropdown menu. User clicks to show a list for selection. Features space-saving, mouse-friendly, commonly used for many options or contextual menus.
   * **Specific Properties**
     * `placeholder`: `String` - Optional. Placeholder text when nothing selected.
   * **Specific Events**
     * `@onOpen()`: Triggered when dropdown menu opens.
     * `@onClose()`: Triggered when dropdown menu closes.
2. **`SingleSelect_RadioButtons` (Radio Buttons)**
   * **Introduction**: Shows all options as a group of independent radio buttons. User clicks directly to select. Features visibility of all options, suitable for fewer options (like 2-5).
3. **`SingleSelect_OverlayPicker` (Overlay Single Select Picker)**
   * **Introduction**: Clicking shows option list in modal overlay/drawer at screen center or bottom. Features touch-friendly, large options, small screen adaptability, may include built-in search filtering.
   * **Specific Properties**
     * `placeholder`: `String` - Optional. Placeholder text when nothing selected.
   * **Specific Events**
     * `@onOpen()`: Triggered when overlay opens.
     * `@onClose()`: Triggered when overlay closes.

#### 2. `MultiSelect Group` (Multi Select Group)

`MultiSelect Group` handles all multi-selection scenarios. Member components are designed to return selected values as an array.

##### Group Common Interface

* **Functional Description**: All components in this group should implement a "clear" function internally (e.g., via a configurable clear button). After user interaction triggers it, component sets `value` to `[]` and fires `onChange` event.
* **Properties**
  * `options`: `Array<Object>` - Required. Array of option objects. Each object needs `label` (String) and `value` (String | Number).
  * `value`: `Array<String | Number>` - Array of selected items' `value`, defaults to empty array `[]`.
  * `readOnly`: `Boolean` - Optional. Whether read-only, user cannot modify value.
* **Methods**
  * `focus()`: Makes selector gain focus.
  * `blur()`: Makes selector lose focus.
* **Events**
  * `@onChange(newValues)`: Triggered when selected values array changes.
  * `@onFocus()`: Triggered when selector gains focus.
  * `@onBlur()`: Triggered when selector loses focus.

##### Sub-component Introduction and Specific Interfaces

1. **`MultiSelect_Dropdown` (Dropdown Multi Select)**
   * **Introduction**: Common PC multi-select dropdown menu. Clicking expands option list with checkboxes. Features space-saving, suitable for many options.
   * **Specific Properties**
     * `placeholder`: `String` - Optional. Placeholder text when nothing selected.
   * **Specific Events**
     * `@onOpen()`: Triggered when dropdown menu opens.
     * `@onClose()`: Triggered when dropdown menu closes.
2. **`MultiSelect_Checkboxes` (Checkbox Group)**
   * **Introduction**: Shows all options as a group of independent checkboxes. Features visibility of all options, suitable for fewer options.
3. **`MultiSelect_OverlayPicker` (Overlay Multi Select Picker)**
   * **Introduction**: Clicking shows option list with checkboxes in modal overlay at screen center or bottom. Features touch-friendly, small screen adaptability.
   * **Specific Properties**
     * `placeholder`: `String` - Optional. Placeholder text when nothing selected.
   * **Specific Events**
     * `@onOpen()`: Triggered when overlay opens.
     * `@onClose()`: Triggered when overlay closes.

#### 3. `BinaryToggle Group` (Binary Toggle Group)

`BinaryToggle Group` handles simple `true`/`false` state toggles.

##### Group Common Interface

* **Functional Description**: All components in this group have state toggle (like on/off) functionality triggered entirely by internal UI interaction.
* **Properties**
  * `checked`: `Boolean` - Whether switch is currently "on", defaults to `false`.
  * `readOnly`: `Boolean` - Optional. Whether read-only, user cannot modify value.
* **Methods**
  * None
* **Events**
  * `@onChange(newCheckedState)`: Triggered when switch state changes, `newCheckedState` is new boolean state.
  * `@onFocus()`: Triggered when switch gains focus.
  * `@onBlur()`: Triggered when switch loses focus.

##### Sub-component Introduction and Specific Interfaces

1. **`BinaryToggle_Switch` (Slide Switch)**
   * **Introduction**: Classic "toggle" style switch with a slidable thumb.
2. **`BinaryToggle_Checkbox` (Single Checkbox)**
   * **Introduction**: A simple checkbox, checked means "on", unchecked means "off".

#### 4. `FileUploader Group` (File Uploader Group)

`FileUploader Group` handles all file upload scenarios. Core functionality is fully encapsulated within components, reporting state changes to outside via events.

##### Group Common Interface

* **Functional Description**: File selection, upload (automatically starts after file selection), and deletion are all triggered and controlled by internal UI interaction.
* **Properties**
  * `value`: `Array<Object>` - Array of uploaded file info, defaults to empty array `[]`.
  * `multiple`: `Boolean` - Optional. Whether to allow multiple file uploads, defaults to `false`.
  * `accept`: `String` - Optional. Accepted file types (like `.jpg,.png` or `image/*`).
  * `maxSize`: `Number` - Optional. Maximum allowed file size in bytes.
  * `readOnly`: `Boolean` - Optional. Whether read-only, user cannot modify value.
* **Events**
  * `@onChange(newFiles)`: Triggered when file list changes.
  * `@onUploadSuccess(fileInfo, response)`: Triggered when file upload succeeds.
  * `@onUploadError(fileInfo, error)`: Triggered when file upload fails.
  * `@onProgress(fileInfo, progressEvent)`: Progress event triggered during upload.

##### Sub-component Introduction and Specific Interfaces

1. **`FileUploader_Button` (Button Upload)**
   * **Introduction**: A button that triggers file selection dialog and automatic upload on click.
2. **`FileUploader_Dropzone` (Drag-Drop Upload Zone)**
   * **Introduction**: A visual area where users can drag and drop files for upload.
3. **`FileUploader_ImageList` (Image List Upload)**
   * **Introduction**: A list-style upload component integrating file selection, image preview, and deletion. All operations (including file deletion) are done internally via UI interaction.

## 2. Frontend Component Common Properties/Methods/Events

### Common Properties

#### Non-Read-Only Properties

**The following common properties apply to all non-structural container frontend UI components (basic UI components, layout containers, extended UI components):**

* **show(BOOL)**:

  * Default: true
  * Description: When set to false, component is not rendered on page (equivalent to display:none). Usually doesn't need declaration unless initial state is false.

#### Read-Only Properties

##### Size Properties (excluding `transform`)

* **`offsetWidth` / `offsetHeight`**: Element's full size (content + padding + border + scrollbar). When element has no scale or rotate, prefer this over `getBoundingClientRect()` method
* **`clientWidth` / `clientHeight`**: Element's internal visible size (content + padding). When element has no scale or rotate, prefer this over `getBoundingClientRect()` method
* **`scrollWidth` / `scrollHeight`**: Element's full content size (content + padding).

##### Relative Position Properties

* **`offsetLeft` / `offsetTop`**: Element position relative to **nearest positioned ancestor**.

### Common Events

!!Note: Not all native JS events can be listened to on all components. Common events only include those listed in this document.

**The following common events apply to all UI components (frontend root components, basic UI components, layout containers, structural containers, extended UI components):**

* `@init()`: Triggered when component initialization completes
* `@click(event)`: Click event
* `@mouseOn(event)`: Mouse enter event
* `@mouseOut(event)`: Mouse leave event
* `@mouseDown(event)`: Mouse down event
* `@mouseUp(event)`: Mouse up event

**The following common events apply to layout containers, structural containers:**

* `@scrollToBottom()`: Triggered when container scrolls to bottom (or rightmost for horizontal layout)
* `@scrollToTop()`: Triggered when container scrolls to top (or leftmost for horizontal layout)
* `@clickOutside()`: Triggered when clicking outside container area

### Common Methods

**The following methods apply to all UI components (basic UI components, layout containers, structural containers, extended UI components)**:

* `focus()`, `blur()`: Gain/lose focus. When adding these methods to a component, system automatically sets its tabindex property to -1, no manual specification needed;
* `getBoundingClientRect()`: Returns an object containing element's real-time position and full size relative to **viewport**. Note: When clientWidth/Height or offsetWidth/Height can be used, prefer those to reduce overhead and simplify code.

**The following common methods apply to all layout containers, structural containers:**

* `scrollToBottom()`: Scroll to bottom. When direction is column, scrolls to bottom; when row, scrolls to rightmost, and so on;
* `scrollToTop()`: Scroll to top
* `scrollTo(left,top,speed)`: Scroll to position, left and top are distances from left and top, speed is value from 1-100 where 1 is fastest, 100 slowest

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

* **Conditions Array (conditions)**

  * **Format**: `[[field, operator, value], ...]`
  * **Operators**: `eq`(=), `neq`(!=), `gt`(>), `gte`(>=), `lt`(<), `lte`(<=), `in`, `nin`, `contains`(%val%), `startsWith`(val%), `endsWith`(%val)
  * **Null Value Ignore**: Add `*` prefix to operator (like `*eq`, `*contains`), automatically ignores condition when value is null
  * **OR Conditions**: `["OR", [condition1], [condition2], ...]`
* **Order Array (orderBy)**

  * **Format**: `[[field, "asc"/"desc"], ...]`
  * **No Sorting**: `null`
* **Pagination Array (outputRows)**

  * **Format**: `[offset, limit]`
  * **No Pagination**: `null`
* **Update Operation Array (set)**

  * **Format**: `[[field, operationType, value], ...]`
  * **Operation Types**: `"set"`(set value), `"inc"`(increment/decrement), `"mul"`(multiply), `"append"`(append to array)

**Quick Examples**

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

**Advanced Examples**

```vl
# OR condition + fuzzy search
<VirtualTable-Posts "postTable">.select([["OR", ["title","contains","VL"], ["content","contains","VL"]], ["status","eq","published"]], null, [0,10]) -> _result

# ✅ Dynamic filtering (recommended) - use * prefix to auto-ignore null
<VirtualTable-Users "userTable">.select([["name","*contains",$keyword], ["role","*eq",$role]], null, [0,20]) -> _result
# Automatically ignores when $keyword/$role is null

# ❌ Dangerous approach - manual condition concatenation (similar to SQL injection risk)
-_conditions([]) = [["role","eq","admin"]]
-IF $keyword != null
--_conditions.push(["name","contains",$keyword])
-<VirtualTable-Users "userTable">.select(_conditions, null, [0,20]) -> _result
```

**Notes**

* **Dynamic Filtering Safety Rule**: Use `*` prefix operators (like `*eq`, `*contains`) to auto-ignore `null` values. **Forbidden** to manually concatenate conditions array
* **null vs Empty Value**: `null` means "no value" and is ignored by `*` operators; `""` is a valid empty string that matches normally
* **System Fields**: `_id`, `_user`, `_create`, `_update` are auto-managed by system and cannot be specified during insert/update

#### ServerCache (Cache Table)

Cache table for cached data read/write. Unlike VirtualTable, cache table doesn't need to bind to an entity table; system automatically creates and maintains corresponding cache data storage.

**Methods**

**Single Value Cache Operations**

* `set(key, value, expire)`: Sets single value cache. Updates if record exists, creates if not. Return: `success(BOOL)`, `message(STRING)`
* `get(key)`: Gets single value type cache record. Return: `value(STRING | NUMBER)`, `success(BOOL)`, `message(STRING)`
* `getMultiple(keys)`: Gets multiple cache records. Return: `values([STRING | NUMBER])`, `success(BOOL)`, `message(STRING)`
* `insertIfNotExists(key, value, expire)`: Inserts single value cache. Fails if key already exists. Return: `success(BOOL)`, `message(STRING)`
* `updateIfExists(key, value, expire)`: Updates single value cache. Fails if key doesn't exist. Return: `success(BOOL)`, `message(STRING)`
* `incrBy(key, value)`: Increments numeric cache record value (can be negative, can include decimals). Return: `value(NUMBER)`, `success(BOOL)`, `message(STRING)`

**Expiration Time Management**

* `setExpire(key, expire)`: Sets cache record expiration duration (applies to single value or set types). Return: `success(BOOL)`, `message(STRING)`
* `getTimeToLive(key)`: Gets cache record's remaining time to live. Returns -1 if no expiration set. Return: `ttl(INT)`, `success(BOOL)`, `message(STRING)`

**Delete Operations**

* `delete(key)`: Deletes cache record (applies to single value or set types). Return: `success(BOOL)`, `message(STRING)`

**Set Cache Operations**

* `addToSet(key, values)`: Creates set type record or adds elements to existing set. All elements are auto-converted to string type after adding. Return: `addedNumber(INT)`, `success(BOOL)`, `message(STRING)`
* `removeFromSet(key, values)`: Removes one or more elements from set. Return: `removedNumber(INT)`, `success(BOOL)`, `message(STRING)`
* `getSetMembers(key)`: Gets all elements in set type record. Return: `members(ARRAY)`, `success(BOOL)`, `message(STRING)`
* `isSetMember(key, value)`: Checks if an element is in the set. Return: `isMember(BOOL)`, `success(BOOL)`, `message(STRING)`

**Cache Management**

* `scanKeys(cursor, limit)`: Gets list of all record key names in cache database, supports pagination. Return: `keys(ARRAY)`, `cursor(STRING)`, `success(BOOL)`, `message(STRING)`

### Backend Functional Components

#### ServerApi (Backend API Request Client)

Properties and methods are identical to Frontend API Request Client component.

#### MQ (Message Queue)

Receives messages sent by services/methods for asynchronous processing. Used for basic async processing and request peak shaving.

**Properties**

* struct({}): Message structure, a single-layer object, e.g., `<MQ-DataStream "dataStream" struct:{structField1:STRING,structField2:INT}>`

**Events**

* @Message(structField1, structField2...): Message received event, message processing logic can be defined in event body

**Methods**

* send(structField1, structField2...): Sends message to message queue. Note: Message structure must strictly follow message queue component's struct property. E.g., `-<MQ-DataStream "dataStream">.send(_data,_num)`

### Backend System Method Components (No Declaration Needed in Component Tree)

Backend system method classes have only one global instance and can be used without defining in component tree.

#### ServerUtils (Backend Utility Class)

System method class callable in backend services. Not allowed to call from frontend.

**Methods:**

- `getCurrentTime()`: Gets server's current time. Returns server system time, useful for time synchronization or getting standard time.
- `delay(second)`: Executes wait operation on server side, pausing for specified seconds.
