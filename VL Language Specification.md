# VL Language Specification (Standard Edition)

**Version: 1.92**

---

## Table of Contents

1. [Version Declaration](#1-version-declaration)
2. [File Types and Project Structure](#2-file-types-and-project-structure)
3. [Core Syntax Rules](#3-core-syntax-rules)
4. [Naming Conventions](#4-naming-conventions)
5. [Components](#5-components)
6. [Variables](#6-variables)
7. [Database Tables and Virtual Table Field Types](#7-database-tables-and-virtual-table-field-types)
8. [Logic Definition](#8-logic-definition)
9. [Expressions](#9-expressions)
10. [Logic Writing Fundamentals](#10-logic-writing-fundamentals)
11. [Style Definition](#11-style-definition)
12. [Database File Specifications](#12-database-file-specifications)
13. [App File Specifications](#13-app-file-specifications)
14. [Section and Component File Specifications](#14-section-and-component-file-specifications)
15. [ServiceDomain File Specifications](#15-servicedomain-file-specifications)
16. [Section and App Responsibilities](#16-section-and-app-responsibilities)
17. [User Authentication and Access Control](#17-user-authentication-and-access-control)

---

## 1. Version Declaration

```vl
// VL_VERSION:1.92
```

The version declaration must appear in the first line comment of all files to ensure the parser uses the correct version rules.

> **Note:** The version declaration is not a VL code section. Use `//` instead of `#`.

---

## 2. File Types and Project Structure

### 2.1 Project File Tree Structure

```
Workspace/
├─ Services/              # ServiceDomain files (service domain definitions, callable in Sections)
│   ├─ Catalog.vs
│   └─ Order.vs
├─ Database/              # Database files (database structure definitions)
│   └─ MyProject.vdb
├─ Sections/              # Section files (frontend view modules, usable only in Apps)
│   ├─ NavHeader.sc
│   ├─ UserProfile.sc
│   ├─ ProductCard.sc
│   └─ OrderList.sc
├─ ExtComponents/         # Component files (internal encapsulated components, usable in Section/App)
│   ├─ InputField.cp
│   └─ DataAuth.cp
├─ Theme/                 # Theme file (project visual specifications: colors, fonts, spacing tokens, and preset component styles; one Theme file per project)
│   └─ MyProject.vth   
└─ Apps/                  # App files (one file per application)
    ├─ ShopApp.vx   
    └─ AdminApp.vx
```

All files participate in the final project code compilation.

### 2.2 File Cross-References

VL code files support limited cross-referencing. Follow these rules strictly:

|                             | App | Section | ServiceDomain | Component |
| --------------------------- | --- | ------- | ------------- | --------- |
| App can reference           | ❌  | ✅      | ❌            | ✅        |
| Section can reference       | ❌  | ❌      | ✅            | ✅        |
| Component can reference     | ❌  | ❌      | ❌            | ❌        |
| ServiceDomain can reference | ❌  | ❌      | ❌            | ❌        |

### 2.3 File Sections and Structure

VL files follow strict section ordering. **All sections must appear in order and only once**, or compilation will fail.

#### 2.3.1 `.vx` — App (Application Entry)

**Required sections:** `# SysConfig`, `# Frontend Tree`, `# Frontend Event Handlers`

```vl
// VL_VERSION:1.92
<App-MarkdownEditApp "root">

# SysConfig
DEVICE_TARGET:"PC"
SCREEN_RESOLUTION:"1920x1080"

# Frontend Global Vars
$currentDocId(INT) = -1

# Frontend Derived Vars
$docIdString(STRING) = ("Doc ID " + $currentDocId)

# Frontend Tree
<Page-Home "homePage"> path:"home" width:"100%" height:"100vh"
-<Section-DocumentList "docListView">

# Frontend Event Handlers
<Section-DocumentList "docListView">.@docSelected(docId, title)
-$currentDocId = docId

# Frontend Internal Methods
# Frontend Pipeline Funcs
</App-MarkdownEditApp>
```

#### 2.3.2 `.sc` — Section (Business Module)

**Core features:** Can call ServiceDomain, use Components, contains complete business logic.
**Prohibited:** Nesting Sections.

**Required sections:** `# Frontend Tree`, `# Frontend Event Handlers`

```vl
// VL_VERSION:1.92
<Section-NavHeader>

# Frontend Public Props
$userId(INT) = 0

# Frontend Global Vars
$userInfo({name:STRING,avatar:STRING}) = {name:"",avatar:""}

# Frontend Derived Vars
$displayName(STRING) = ($userInfo.name != "" ? $userInfo.name : "Guest")

# Frontend Tree
<ServiceDomain-User "userService">
-<Service-GetUserInfo> params:(userId(INT)) returns:(success(BOOL),data({}))
<Row-HeaderContainer> width:"100%" height:"64px"
-<Component-UserAvatar> userId:$userId avatar:$userInfo.avatar
-<Button-Logout "logoutButton"> value:"Logout"

# Frontend Public Events
EVENT @userLoggedOut()

# Frontend Public Methods
METHOD_PUB RefreshUserInfo();RETURN {success:BOOL}
-<ServiceDomain-User "userService">.GetUserInfo($userId) -> _result
-RETURN {success:_result.success}

# Frontend Event Handlers
<Button-Logout "logoutButton">.@click()
-@userLoggedOut()

# Frontend Internal Methods
# Frontend Pipeline Funcs
</Section-NavHeader>
```

#### 2.3.3 `.cp` — Component (Pure UI Component)

**Core features:** UI reuse only, data passed via Props, communication via Events.
**Prohibited:** Importing ServiceDomain, nesting Components, complex business logic.

**Required sections:** `# Frontend Tree`, `# Frontend Event Handlers`

```vl
// VL_VERSION:1.92
<Component-UserCard>

# Frontend Public Props
$userId(INT) = 0
$userName(STRING) = ""

# Frontend Global Vars

# Frontend Derived Vars
$displayName(STRING) = ($userName != "" ? $userName : ("User " + $userId))

# Frontend Tree
<Block-CardContainer> width:"280px" padding:"16px"
-<Text-UserName> value:$displayName
-<Button-Action "actionButton"> value:"View"

# Frontend Public Events
EVENT @cardClick(userId(INT))

# Frontend Public Methods

# Frontend Event Handlers
<Button-Action "actionButton">.@click()
-@cardClick($userId)

# Frontend Internal Methods

# Frontend Pipeline Funcs
</Component-UserCard>
```

#### 2.3.4 `.vs` — ServiceDomain (Backend Service Domain)

**Required sections:** `# Backend Tree`, `# Services`

```vl
// VL_VERSION:1.92
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

#### 2.3.5 `.vdb` — Database (Database Structure)

```vl
// VL_VERSION:1.92
<Database-ProjectName>
<Table-Documents> data:[{"_id":1,"title":"Doc1","_create":"2024-01-15 09:30:00"}]
-<Field-title> type:STRING notNull:true
-<Field-content> type:STRING
-<Index-TitleIdx> type:UNIQUE fields:["title"]
<Relation-User&Docs> users._id<<documents._user
</Database-ProjectName>
```

#### 2.3.6 `.vth` — Theme (Theme Configuration)

**Rules:** Singleton file; only add new tokens, do not modify or delete existing ones.

```vl
// VL_VERSION:1.92
<Theme-ProjectName>
# Design Tokens
<Color-Brand> colorBrandPrimary:#0052D9
<Spacing-Base> spacing4:16px

# Component Styles
<Button-Primary> background-color:--colorBrandPrimary padding:--spacing4
-<StateStyle-Hover> trigger:hover background-color:--colorBrandHover
</Theme-ProjectName>
```

---

## 3. Core Syntax Rules

### 3.1 Single-Line Rule (Mandatory)

Many definitions and statements **must** be completed on a single line without line breaks, including but not limited to:

- Variable definitions (global `$var(...) = ...`, local `_var(...) = ...`, `_var({}) = {}`, `_var([]) = []`)
- Method, service, event, and pipe definition lines (the first line including all parameters and `RETURN` declaration)
- All `RETURN {...}` (for METHOD/SERVICE) or `RETURN value` (for PIPE) statements
- All assignment statements (`$var = ...`, `_var = ...`)
- Object and array initialization assignments (even with complex structures)
- Component property definitions on a single line (multiple properties separated by spaces)

### 3.2 No Leading Spaces Rule (Mandatory)

- All `-` (hyphen) symbols representing hierarchy must be **flush left** with no preceding spaces.
- In VL syntax, any line with content should have **no leading spaces** (except JSON code blocks or specific embedded languages).

### 3.3 Semicolon (`;`) Usage (Strict)

- Semicolons are used as separators in `FOR` loop control parts (e.g., `FOR (_i(INT)=0; _i<10; _i++)`).
- **Prohibited** at the end of normal code lines (variable assignments, method calls, RETURN statements).
- **Prohibited** between or after style properties.

### 3.4 String Escaping

**Strictly prohibit any `\` escape characters. Do not use `\` escape characters anywhere.**

### 3.5 Indentation

VL uses `-` without spaces to represent indentation:

- **Top-level definitions (no indentation):** Root components (`<Section-..>`), section headers (`# Frontend Global Vars`, `# Services`, `# Frontend Tree`, etc.), and method/service/event/pipe definition lines (`METHOD...`, `SERVICE...`, `EVENT...`, `PIPE...`).
- **Frontend Tree:**

  - Components added directly under the root are written without leading `-`.
  - Child components add one `-` per depth level.

  ```vl
  <Block-Header>
  -<Text-Title> value:"Title"
  <Block-Content>
  -<Button-Submit> value:"Submit"

  <VirtualTable-Users> ..
  -<Field-_id> ... // Field is a child of VirtualTable
  ```
- **Method/Event Handler internals:**

  - All lines in method/event handler bodies must be indented at least one level (`-`).
  - Control flow structures (`IF`, `FOR` blocks) introduce deeper indentation levels.

### 3.6 Section Markers and Comments

`#` marks standard section structure markers (e.g., `# Frontend Tree`, `# Frontend Default Styles`). Follow the predefined section structure and order strictly based on the current file's development specifications.

`//` represents inline and line comments.

`/*` and `*/` represent block comments.

```vl
// VL_VERSION:1.92

<Section-antUploadDragger>
/*
This module simulates the Ant Design UploadDragger component.
*/

# Frontend Global Vars
$uploadedFiles([{uid:STRING,name:STRING,status:STRING,url:STRING,size:INT}]) = []

</Section-antUploadDragger>
```

---

## 4. Naming Conventions

### 4.1 File Naming

All file names use PascalCase and match the root component:

`UserAuth.vs` file has root component:

```vl
<ServiceDomain-UserAuth>
file body
</ServiceDomain-UserAuth>
```

### 4.2 Component Instance Naming

- Component names use **PascalCase** (e.g., `UserCard`, `LoginButton`, `NoteItemCard`, `AddButton`).
- Component names should reflect the component's purpose.
- Component IDs use camelCase and should reflect the component's purpose and type (e.g., `<Button-Submit "submitButton">`, `<Block-ModuleRoot "root">`, `<VirtualTable-Users "userTable">`).

### 4.3 Variable Naming

- **Frontend global variables** (`# Frontend Global Vars`): `$` prefix + **camelCase** (e.g., `$currentPage`, `$userData`).
- **Derived variables** (`# Frontend Derived Vars`): `$` prefix + **camelCase** (e.g., `$fullName`, `$canSubmit`).
- **Local variables in methods** (`_varName(TYPE) = ...` or `_varName({}) = {}` or `_varName = []`): `_` prefix + **camelCase** (e.g., `_result`, `_tempData`). **Must** declare type or infer from empty structure. **No** `let` keyword.
- **Loop variables:**
  - Loop variables are a **special type of local variable** that can be used directly without prior definition.
  - `FOR...IN` loops: Loop item and index variables **must** use `_` prefix + **camelCase**. Index variables should use `_indexX` (X starts from 0, e.g., `_index0`, `_index1`). Loop item variables can be named contextually (e.g., `_user`, `_product`, `_noteItem`) or use `_itemX` for generic iteration.
  - `FOR (...)` counting loops: Loop control variables **must** use `_` prefix + **camelCase** (e.g., `_i`, `_j`, `_k`, `_loopCounter`).

### 4.4 Method/Function Naming

- **Frontend public methods** (`METHOD_PUB`): **PascalCase** (e.g., `ProcessData`, `ValidateForm`, `ReloadList`)
- **Frontend/backend internal methods** (`METHOD`): **camelCase** (e.g., `loadData`, `loadNotesFromServer`, `validateBackendInput`)
- **Backend services** (`SERVICE`): **PascalCase** (e.g., `UserLogin`, `GetDocList`)
- **Frontend/backend pipeline (PIPE) functions** (for pure data transformation): `_` prefix + **camelCase** (e.g., `_formatCurrency`, `_formatDate`)

### 4.5 Event Naming

Event names use camelCase (e.g., `@click`, `@init`, `@tick`, `@keyDown`).

### 4.6 Method/Function Parameters

- All method/function **parameter names use camelCase without `_` prefix**.
- Example: `METHOD myMethod(userId(INT), configData({}))`, parameters are `userId`, `configData`.
- Example: `PIPE _formatDate(dateValue(TIMESTAMP), formatString(STRING))`.
- **Note:** When referencing these parameters inside the method, use their defined names directly (e.g., `userId`, `configData`).

### 4.7 Tables and Fields

- All table names use PascalCase, including Database Tables and ServiceDomain VirtualTables.
- All field names use camelCase.

### 4.8 Other

- **Public event definitions** (`EVENT @eventName`): **camelCase** (e.g., `itemSelected`, `formSubmitted`).

---

## 5. Components

### 5.1 Component Instance Creation

#### 5.1.1 Creation Location

| Applicable Files | Definition Section | Core Purpose                                            |
| ---------------- | ------------------ | ------------------------------------------------------- |
| vx, sc, cp       | # Frontend Tree    | Frontend UI layout and functional component declaration |
| vs               | # Backend Tree     | Backend component declaration                           |

#### 5.1.2 Component Definition Format

```vl
<ComponentClass-ComponentName "componentId"> functionalProp1:value1 functionalProp2:value2 StyleClass:($selected ? "TextSelected" : "TextNormal") cssOverride1:value1...
```

##### Elements

| Element                       | Description                                                | Naming Convention | Example                               | Notes                                                                                     |
| ----------------------------- | ---------------------------------------------------------- | ----------------- | ------------------------------------- | ----------------------------------------------------------------------------------------- |
| ComponentClass                | System-predefined component type                           | Fixed             | `Button`, `Text`, `Input`       | Cannot be modified, defined by system                                                     |
| ComponentName                 | Developer-defined descriptive name reflecting purpose      | PascalCase        | `SubmitButton`, `UserStatus`      | Must be meaningful                                                                        |
| componentId                   | Unique identifier within the file                          | camelCase         | `"submitBtn"`, `"userStatus"`     | Optional, only specify when reference needed;**pure static string, no expressions** |
| Functional Properties         | Properties defined in component documentation              | -                 | `value`, `disabled`               | Strictly follow component documentation                                                   |
| StyleClass (frontend only)    | Preset component style class, string, supports expressions | PascalCase        | `"ButtonPrimary"`, `"TextTitle1"` | Must be predefined in project's Theme file                                                |
| CSS Overrides (frontend only) | Standard CSS properties to override StyleClass defaults    | -                 | `color`, `font-size`              | Only define if StyleClass presets don't meet requirements                                 |

### 5.2 Component Tree Structure

#### 5.2.1 Root Component

The root component is a special component serving as the starting marker for the entire file. Besides serving as a file header, its usage rules are the same as regular components:

- Frontend root components (`<App>`, `<Section>`, `<Component>`) also function as layout containers. All components in `# Frontend Tree` are added under this container. Their layout function is similar to a `Col` container. Style properties can be added: `<Section-ProductCard "root"> width:"320px" min-height:"400px" background-color:"#ffffff"`
- Frontend root components can add common layout container events like `@init()`: `<App-MyApp "root">@init()`, `<Section-MySection "root">@init()`
- `<ServiceDomain>` root component currently has no properties or methods, serving only as a file starting declaration.

#### 5.2.2 Component Creation Order

Frontend component tree component creation order:

1. Functional components (no UI): FrontendApi, Trigger, WindowEventListener, ClientUserCenter, etc.
2. UI and container components

Backend component tree component creation order:

1. Functional components: ServerApi, etc.
2. Backend data components: VirtualTable, etc.

#### 5.2.3 Component Parent-Child Relationships

When component B is a child of component A, add B below A with one additional indentation level. Parent-child relationships exist in these scenarios:

**Allowed to add child components:**

- All container components (UI containers, logical containers) can have other frontend components (containers, basic UI components, extended UI components, etc.) as children.
- Non-UI functional components (FrontendApi, Trigger, etc.) can only be direct children of root components.
- Any UI component (UI containers, UI components) can add widgets to extend functionality (StateStyle, Animation, UseDrag, etc.).

**Strictly restricted or prohibited from adding children:**

- Non-UI functional components are strictly prohibited from adding any children.
- Non-container UI components (basic/extended UI components) are prohibited from adding any children except widgets.
- Section/Component instances referenced in another file cannot contain child nodes (except widgets); their internal tree is defined in their own file.

### 5.3 Component Usage and Access

#### 5.3.1 Component Usage Scenarios

After defining components in the component tree, they can be used in subsequent events/methods/functions/expressions:

| Usage                                                                   | Events | Methods | PIPE Functions/Expressions |
| ----------------------------------------------------------------------- | ------ | ------- | -------------------------- |
| Listen to component events, e.g.,`.@init()`                           | ✅     | ❌      | ❌                         |
| Call component methods, e.g.,`.scrollToBottom()`                      | ✅     | ✅      | ❌                         |
| Access component read-only properties, e.g.,`_position = .offsetLeft` | ✅     | ✅      | ✅                         |

#### 5.3.2 Correct Access Method

Reference using `<ComponentClass-ComponentName "componentId">`. Component class, name, and ID must strictly match the component tree definition. **Do not confuse component name with component ID.**

```vl
# Frontend Tree
<Button-Next "nextButton"> disabled:$status

# Frontend Event Handlers
<Button-Next "nextButton">.@click() // Content in angle brackets strictly matches component tree
```

#### 5.3.3 Define Before Use Principle

Except for system method components, all components must be defined in the component tree before being referenced in subsequent methods/functions.

Note the following non-UI functional components also need prior definition: `Trigger`, `FrontendApi`, `WindowEventListener`, `ClientUserCenter`

#### 5.3.4 System Method Components

System method components are special components with only one global instance, providing convenient access to system methods. They don't need definition in the component tree and can be used directly:

`<ClientUtils>.consoleLog(123)`

### 5.4 Component Properties

#### 5.4.1 Property Order

**Strictly specify component properties in this order:**

- Functional properties: e.g., value, type, disabled - strictly follow component documentation
- StyleClass: Current component's preset style class - strictly follow project's `.vth` file definitions
- CSS override properties: Additional CSS styles that override StyleClass presets

#### 5.4.2 Read-Only and Non-Read-Only Properties

- **Non-read-only properties** define component behavior, data, logic, core features, and non-visual state.
- **Read-only properties** are mainly DOM element read-only properties like offsetWidth. These don't need definition in `# Frontend Tree` and can be accessed directly in methods/functions later.
- Whether read-only or not, **strictly follow component documentation**; do not invent properties.

#### 5.4.3 Property Read/Write Rules (Unidirectional Data Flow)

VL uses **Unidirectional Data Flow** architecture. **There is no v-model syntactic sugar.**

##### Non-Read-Only Property Mutability

1. **Prohibited** to directly read or modify component non-read-only properties in any method/event/function/expression (like `Button.color` or `Modal.show = false`).
2. For dynamic property control, follow the **"variable intermediary"** pattern:
   - **Bind** a variable to the property (e.g., `show:$isModalVisible`).
   - In logic, **only modify the variable** (`$isModalVisible = false`), letting reactive binding drive component refresh.

##### Read-Only Property Access

Read-only properties can be read in methods/functions (not written) without breaking unidirectional data flow:

```vl
# Frontend Event Handlers

<Button-CheckSize "checkButton">.@click()
// ✅ Allowed: Read size information
-_buttonWidth(FLOAT) = <Button-Submit "submitButton">.offsetWidth
-_buttonHeight(FLOAT) = <Button-Submit "submitButton">.offsetHeight
-_rect({}) = <Block-MainContainer "mainContainer">.getBoundingClientRect()

// ✅ Allowed: Read position information
-_scrollTop(FLOAT) = <Block-Content "docContent">.scrollTop
-_offsetLeft(FLOAT) = <Image-Logo "logo">.offsetLeft
```

---

## 6. Variables

### 6.1 Basic Types

- **STRING**: Text string, e.g., "Hello". String values are usually wrapped in double quotes `"`. When the string value needs to contain double quotes, use single quotes `'`. **Strictly prohibit any `\` escape characters in VL.**
- **INT**: Integer, e.g., 42
- **FLOAT**: Floating point, e.g., 3.14
- **BOOL**: Boolean, true or false
- **TIMESTAMP**: Internally stores standard time string format, e.g., `2024-12-12 11:11:11.342`. Can be defined without milliseconds, e.g., `2024-12-12 11:11:11`. Used as "time currency" for all time-related scenarios.

### 6.2 Composite Types

- **Array**: `[elementType]`, e.g., `[INT]`, `[STRING]`, `[{id:INT,name:STRING}]`. Use `[]` when internal structure is uncertain.
- **Object**: `{field:type}`, e.g., `{id:INT,name:STRING,items:[{id:INT}]}`. Use `{}` when internal structure is uncertain.
  - **Note**: When an object's fields are uncertain at definition time or will be dynamically added at runtime, initialize as empty object `{}` (e.g., `_seen({}) = {}`). VL will infer it as a base object type. Properties can be added dynamically later.
- **JSON**: Define JSON type first, then use, e.g., define `_myJsonVar(JSON) = null`, then assign later in methods/events as `_myJsonVar(JSON) = _apiResult`. JSON type is typically used for data whose structure is **not fully determined** or **variable** at compile time. Note: If a data type is a definite object or array with unknown internal structure, define it more explicitly as `[]` or `{}` rather than simply JSON.
- **Composite type initialization** must include all required fields or use valid empty structures (empty object `{}` or empty array `[]`); **JSON type only allows initialization to `null`** as a placeholder for "unknown structure, to be filled later."

```vl
// JSON type - for external API interaction or completely uncertain structure data
_apiResponse(JSON) = null
_flexibleConfig(JSON) = null

// Dynamic object type - for scenarios with dynamically added properties
_lookup({}) = {}
_cache({}) = {}
```

### 6.3 Timestamp Type

#### 6.3.1 Timestamp Variable Assignment

VL's TIMESTAMP type uses a unified string format as "time currency", like `2024-12-12 11:11:11.342` or `2024-12-12 11:11:11` (milliseconds can be omitted). When assigning to a TIMESTAMP global or local variable with `=`, the following input types are accepted:

- **System variables**: e.g., `$serverDate = SYSENV.currentTime`
- **Unix timestamp**: Millisecond-level; use `setUnixS()` method for second-level timestamps
- **Strings** (five formats, all case-sensitive):
  - `"YYYY-MM-DD HH:mm:ss"`
  - `"YYYY-MM-DD HH:mm:ss.SSS"`
  - `"YYYY-MM-DD"`
  - `"YYYY-MM-DDTHH:mm:ssZ"`
  - `"YYYY-MM-DDTHH:mm:ss.SSSZ"`
- **Recommended initialization format**: Use complete format when defining: `$createTime(TIMESTAMP) = "2024-12-12 11:11:11.342"`. System variable `SYSENV.currentTime` returns standard time string format (with milliseconds).

#### 6.3.2 Timestamp Variable Usage

TIMESTAMP variables can be used directly in most scenarios without explicit conversion:

- All time-related variable assignments: `$eventTime(TIMESTAMP) = "2024-12-12 11:11:11"`
- Parameter passing: `processEvent(eventTime)` where eventTime is TIMESTAMP type
- Database time comparison: `[["createTime", "gt", $startTime]]`

**Example:**

```vl
$eventTime(TIMESTAMP) = "2024-12-12 15:30:00"
$displayTime(STRING) = $eventTime.format("YYYY-MM-DD HH:mm")
<Text-EventTime> value:($eventTime.format("MM/DD HH:mm"))
```

#### 6.3.3 Timestamp Methods and Functions

**Immutable methods:**

- **Format output**: Use `.format(outputFormat)` method. Output formats include: YYYY (4-digit year), YY (2-digit year), M (1-digit month 1-12), MM (2-digit month 01-12), MMM (short month name, e.g., Jan), MMMM (full month name, e.g., January), D (1-digit day 1-31), DD (2-digit day 01-31), d (day of week 0-6), H (24-hour 1-digit hour 0-23), HH (24-hour 2-digit hour 00-23), h (12-hour 1-digit hour 1-12), hh (12-hour 2-digit hour 01-12), m (1-digit minute 0-59), mm (2-digit minute 00-59), s (1-digit second 0-59), ss (2-digit second 00-59), SSS (3-digit millisecond 000-999), A (AM/PM uppercase), a (am/pm lowercase)
- **Timestamp retrieval**: Use `.unixS()` and `.unixMS()` methods

**Mutable functions:**

- `variable.setUnixS(secondTimestamp)`
- `variable.setUnixMS(millisecondTimestamp)`
- `variable.setCustomFormat("timeString", "correspondingFormatString")`

**Prohibited operations:**

- **Prohibited from directly calling JavaScript Date API**: Not supported to directly call `new Date(...)`, `.getTime()`, `.getFullYear()`, `.getMonth()`, `.getDate()`, etc.

### 6.4 Variable Definition

#### 6.4.1 Frontend Global Variable Definition (`# Frontend Global Vars`)

Frontend global variables are accessible at file-level frontend environment, using `$` prefix:

```vl
$variableName(type) = initialValue
```

**Examples:**

```vl
$userName(STRING) = ""
$userCount(INT) = 0
$isLoggedIn(BOOL) = false
$userData({id:INT,name:STRING,age:INT}) = {id:0,name:"",age:0}
$items([{id:INT,title:STRING}]) = []
$genericArray([{}]) = [{}]
```

#### 6.4.2 Derived Variable Definition (`# Frontend Derived Vars`)

Derived variables are read-only variables automatically computed from other variables:

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

**Important rules:**

- Can only contain pure computation; cannot call METHOD/SERVICE actions, but can use PIPE functions and variable immutable functions
- Derived variables are read-only; cannot be assigned in code
- **Important: Derived variables must define types**

#### 6.4.3 Local Variable Definition

##### Local Variables in Methods/Functions

Local variables are used inside methods with `_` prefix. They **must** explicitly declare type (e.g., `_varName(TYPE) = ...`) or infer type through empty structure initialization (e.g., `_varName({}) = {}`, `_varName([]) = []`). **No** `let` keyword:

```vl
_variableName(type) = value
// or
_variableName({}) = {}
_variableName([]) = []
```

**Example:**

```vl
METHOD processData()
-_result({type:STRING}) = {}
-_count(INT) = 0
-_userList([{id:INT,name:STRING}]) = []
-_lookupTable({}) = {}
-_genericItems([{}]) = [{}]
```

- Local variables, like global variables, need type declaration or type inference through empty structures at initialization. (Loop control variables are an exception to this rule; see Loop section)
- **Do not omit the equals sign and initial value** when declaring local variables, or the definition will be mistaken as a public method call:

```vl
METHOD processData()
-_condition([]) = [] // Define a local variable named _condition
-_condition([]) // Call a public method named _condition
```

##### Loop Container Local Variables

In For, Tree, and other loop container component properties, you can declare local variables for the current loop container:

```vl
<For-UserList> sourceArray:$users,loopVar:[_item0,_index0] // _item0, _index0 are loop variable and loop index for the current structure loop; they are local variables declared in the component tree, use _ prefix
```

#### 6.4.4 System Variables (`SYSENV`, `WINDOW`)

System variables can be used directly to obtain current system environment information:

| Variable Name      | Applicable Scope                       | Internal Properties                                                                                                                                                                                                                                                                                                                     |
| :----------------- | :------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SYSENV.currentUser | Frontend and backend                   | **isLogin**: BOOL, **userId**: STRING, **userInfo**: {id:INT, username:STRING, avatar:STRING, deleted:INT, deptId:INT, disableStatus:INT, email:STRING, lastTimeLogin:TIMESTAMP, fullName:STRING, nickname:STRING, phoneNumber:STRING}, **tenantId**: INT                                                       |
| SYSENV.currentTime | Frontend and backend                   | None. Can be assigned directly to a time variable:`$currentTime(TIMESTAMP) = SYSENV.currentTime`                                                                                                                                                                                                                                      |
| SYSENV.requestInfo | Backend only                           | **ip**: STRING, **userAgent**: STRING, **headerInfo**: {}, **cookie**: {}                                                                                                                                                                                                                                       |
| WINDOW             | Frontend only (expressions and events) | Read-only access to browser `window` object properties. Used with components to respond to window events (resize, scroll). **Prohibited from calling** `WINDOW` object methods. Accessible properties: `innerWidth`, `innerHeight`, `outerWidth`, `outerHeight`, `devicePixelRatio`, `scrollX`, `scrollY`, etc. |
| --themeTokenName   | Frontend                               | All Token variables defined in Theme.vth can be directly referenced in frontend code:`font-size:--fontSizeLg color:--colorTextDefault`                                                                                                                                                                                                |

### 6.5 Variable Assignment and Modification

#### 6.5.1 Variable Assignment

Variable assignment uses equals `=`, no need to redeclare type:

```vl
$variableName = newValue
_variableName = newValue
```

- The `=` operation follows JS rules: when the new value is a simple value (STRING, INT, FLOAT, BOOL), it assigns the value itself; when the new value is a complex data structure (array, object, JSON), it assigns a reference to that data structure.

#### 6.5.2 In-Place Modification of Complex Data Structures

VL **does not require** the **data immutability** of React/Vue frameworks for complex data structures (objects, arrays, JSON). The framework automatically handles data updates and performance optimization after in-place modifications. VL **supports and recommends direct modification of parts of complex structures**:

```vl
# Frontend Global Vars
$users([{name:STRING}]) = []

# Frontend Tree
-<For-UserList> sourceArray:$users loopVar:[_item0,_index0]
--<Text-Name> value:_item0.name

# Frontend Event Handlers
<Button-Change "changeButton">.@click()
$users[0].name = "adam" // Directly modify a value in the array; bound UI updates automatically
$users.push("alice") // Modify array in-place via mutable method; bound UI also updates automatically
```

#### 6.5.3 Variable Mutable Methods

In methods and events, you can call variable **mutable** methods (which modify the original variable). These operation statements follow the indentation rules of their code block (usually at least one level `-`).

**Important: Mutable methods can only be called directly in methods. In any expression (including property bindings, `conditions`, `IF/FOR` conditions/loop bodies, etc.), only immutable functions can be used.**

| **Variable Type** | **Mutable Methods Allowed in Methods**                                             |
| :---------------------- | :--------------------------------------------------------------------------------------- |
| Array                   | `push`, `pop`, `shift`, `unshift`, `splice`, `sort`, `reverse`, `delete` |
| Object                  | `Object.assign`, `delete`                                                            |
| Timestamp               | `setUnixS`, `setUnixMS`, `setCustomFormat`                                         |

> **⚠️ Usage Restriction**: Mutable operations can only appear as standalone statements in method bodies; they cannot be used in expressions, conditional judgments, or chained calls.

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

#### 6.5.4 Variable Built-in Immutable Functions/Properties

Variable built-in immutable functions can be called in expressions:

| Category                | Immutable Functions                                                                                                                                                                |
| :---------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Array                   | `map()`, `filter()`, `reduce()`, `indexOf()`, `includes()`, `slice()`, `concat()`, `join()`, `length` (property)                                                 |
| String                  | `split()`, `trim()`, `toLowerCase()`, `toUpperCase()`, `replace()`, `replaceAll()`, `substring()`, `slice()`, `includes()`, `indexOf()`, `length` (property) |
| Number                  | `toFixed()`, `toPrecision()`, `toExponential()`                                                                                                                              |
| Object                  | `Object.keys()`, `Object.values()`, `Object.entries()`                                                                                                                       |
| Timestamp               | `unixS()`, `unixMS()`, `format()`                                                                                                                                            |
| Type Check              | `typeof`                                                                                                                                                                         |
| Math                    | `Math.random()`, `Math.floor()`, `Math.ceil()`, `Math.round()`, `Math.max()`, `Math.min()`, `Math.abs()`, `Math.pow()`                                             |
| JSON Handling           | `JSON.parse()`, `JSON.stringify()`                                                                                                                                             |
| Regex                   | `match()`, `test()`                                                                                                                                                            |
| Type Conversion & Check | `String()`, `Number()`, `parseInt()`, `parseFloat()`, `Boolean()`, `isNaN()`, `isFinite()`                                                                           |

- Variable built-in functions can be called directly without prior definition.
- When calling, use the function name directly **without `_` prefix**:
  - Correct (direct use): `_num.toFixed()`
  - Wrong (confused with PIPE function, added `_`): `_num._toFixed()`

#### 6.5.5 Prohibited JS Methods

- `toString()`: Use `String()` instead for broader applicability

#### 6.5.6 Callback Function Parameter Usage Rules

##### Hard Rules (Must / Must Not)

- **MUST**: In map/filter/reduce/some/every/sort callback function parameters, **must use arrow functions, and the function body must be a single expression (concise body).**
- **MUST**: **Callbacks must be pure synchronous with no side effects** (cannot modify external/upper-level variables, cannot call asynchronous METHOD/component methods)
- **MUST NOT**: Use statements with side effects in callbacks (like `acc[k]=...`, push/splice write operations)
- **MUST NOT**: Write return statement blocks.

✅ **Correct (pure expression):**

```vl
arr.reduce((acc, x) => acc + x, 0)
arr.map(x => ({ id: x.id, name: x.name?.trim() }))
params.reduce((acc, p) => ({ ...acc, [p.name]: p.def ?? "" }), {})
```

❌ **Wrong (statement block + side effects):**

```vl
arr.reduce((acc, p) => { acc[p.name] = p.def || ""; return acc; }, {})
JSON.parse(...)._reduce((acc, p) => { ... }, {})
```

---

## 7. Database Tables and Virtual Table Field Types

Backend Database tables and virtual tables support the following field types: STRING, INT, FLOAT, BOOL, TIMESTAMP, JSON, VEC

Note: Backend table field types differ from variable types as follows:

- Backend has no object or array type fields; use JSON type uniformly for complex structures
- Backend has additional VEC (vector) type field for vector search; VL currently doesn't support VEC type variables

Except for these two differences, other field types have identical data definitions to variable data types.

### 7.1 Vector Field Usage

Vector fields are used for vector search. In VL, you only need to define a vector field's vecSource (vectorization data source field), and the system will automatically vectorize the source field data during table insert/update.

#### 7.1.1 Vector Field Definition

**In VirtualTable:**

```vl
<ServiceDomainRoot-Root>
-<VirtualTable-Documents "docTable"> mockData:[{...}]
--<Field-title> type:STRING
--<Field-content> type:STRING
--<Field-contentVec> type:VEC vecSource:["title","content"] // Vector field data source is title and content
```

- In most cases, one VirtualTable only needs one vector field. Add all fields that may participate in content search as the vector field's vecSource, rather than having one vector field per content field.

**In Database Table:**

```vl
<Table-documents> data:[{"_id":1,"title":"VL Framework Guide","content":"VL is an innovative visual programming language...","_user":"1","_create":"2024-01-15 09:30:00","_update":"2024-01-15 09:30:00"}]
-<Field-_id> type:INT
-<Field-title> type:STRING
-<Field-content> type:STRING
-<Field-contentVec> type:VEC vecSource:["title","content"]
```

#### 7.1.2 Vector Search

Vector search is implemented through the `select` method's `conditions` (filtering) and `orderBy` (sorting) parameters:

**Vector sorting (orderBy):**

- Format: `[["vectorFieldName","searchText"],...]`
- Automatically sorts by similarity from high to low (vector distance from small to large)
- Can mix with regular field sorting: `[["contentVec","AI Tech"],["_create","desc"]]`

**Vector filtering (conditions):**

- Format: `["vectorFieldName","l2str","searchText",maxDistance]`
- maxDistance is the maximum distance from search text; value ≤ 1
- Can combine with regular filter conditions: `[["status","eq","published"],["contentVec","l2str","keyword",0.4]]`
- Supports use in OR conditions: `[["OR",["titleVec","l2str","keyword",0.3],["contentVec","l2str","keyword",0.3]]]`

**Example:**

```vl
-<VirtualTable-Documents "docTable">.select([["status","eq","published"],["contentVec","l2str",keyword,0.4]],[["contentVec",keyword],["_create","desc"]],[_offset,pageSize],null) -> _result
```

**Notes:**

- Vector fields don't need manual assignment during `insert`/`update`; system generates automatically based on `vecSource` fields
- Vector fields don't support `eq`, `contains`, and other regular operators; only support `l2str` filtering and similarity sorting
- Search text is automatically vectorized by the system for matching

---

## 8. Logic Definition

### 8.1 Distinguishing Events, Methods, Functions, and Expressions

#### 8.1.1 Core Comparison Table

| Type                 | Definition                                                                       | Side Effects | Call Location                                                        | Can Use Internally              | Typical Use                                        |
| -------------------- | -------------------------------------------------------------------------------- | ------------ | -------------------------------------------------------------------- | ------------------------------- | -------------------------------------------------- |
| **Event**      | Passively triggered logic responding to user interaction or system state changes | ✅ Yes       | `# Frontend Event Handlers` section, listening to component events | Methods, functions, expressions | Handle clicks, inputs, initialization, etc.        |
| **Method**     | Actively callable unit executing complete business logic                         | ✅ Yes       | Event handlers, other methods                                        | Methods, functions, expressions | Data processing, service calls, state modification |
| **Function**   | Pure data transformation computation unit                                        | ❌ No        | Events, methods, functions, expressions                              | Functions, expressions          | Formatting, validation, array transformation       |
| **Expression** | Computation expression returning a single value                                  | ❌ No        | Property binding, conditional judgment, derived variables            |                                 | UI display, condition control, dynamic computation |

#### 8.1.2 Mutual Call Rules (Strict)

|                         | Call Event | Call Method | Use Function | Use Expression |
| ----------------------- | ---------- | ----------- | ------------ | -------------- |
| **In Event**      | ❌         | ✅          | ✅           | ✅             |
| **In Method**     | ❌         | ✅          | ✅           | ✅             |
| **In Function**   | ❌         | ❌          | ✅           | ✅             |
| **In Expression** | ❌         | ❌          | ✅           | ✅             |

### 8.2 Methods

#### 8.2.1 Method Definition Location and Format

| Method Type                   | Applicable Files | Definition Section          | Definition Prefix | Method Name Format |
| ----------------------------- | ---------------- | --------------------------- | ----------------- | ------------------ |
| Frontend internal method      | vx, sc, cp       | # Frontend Internal Methods | METHOD            | camelCase          |
| Frontend module public method | sc, cp           | # Frontend Public Methods   | METHOD_PUB        | PascalCase         |
| Service                       | vs               | # Services                  | SERVICE           | PascalCase         |
| Backend internal method       | vs               | # Backend Internal Methods  | METHOD            | camelCase          |

**Definition Format**

The method definition line is at the top level, no indentation. All code lines in the method body need at least one level of indentation (`-`).

```vl
<definitionPrefix> <methodName>(param1(Type1),param2(Type2),...);RETURN {returnVal1:Type1,returnVal2:Type2,...}
-method body
```

#### 8.2.2 Method Parameter Definition

Method parameters use camelCase and need type specification: `rawEmail(STRING), tags([FLOAT])`

#### 8.2.3 Method RETURN Structure

For all methods, **`RETURN` structure must be an object `{}`**, even with only one return value.

**To work with the system's default error handling mechanism, it's recommended that the return object always include a success:BOOL field and optional message:STRING field:**

```vl
METHOD calculateTotal(prices([FLOAT]),discount(FLOAT));RETURN {total:FLOAT,discounted:FLOAT,success:BOOL}
-_sum(FLOAT) = 0
-FOR (_price,_index0) IN prices
--_sum = _sum + _price
-_discountedValue(FLOAT) = _sum * (1 - discount)
-RETURN {total:_sum,discounted:_discountedValue,success:true}
```

#### 8.2.4 Method Invocation

**Invocation Methods**

- Internal custom method calls within file: Use `<methodName>(parameters)` directly; frontend files can call frontend public and internal methods; ServiceDomain files can call services and backend internal methods
- System/component method calls: Use `<ComponentClass-ComponentName "componentId">.methodName(parameters)`
- Variable method calls: Use `<variableName>.methodName(parameters)`

**Handling Method Return Values**

Use `-> variableName` to store the method's returned data in a specified variable. `variableName` can be a pre-declared local variable (starting with `_`) or a pre-declared global variable (starting with `$`, depending on current environment). When there's no return value or you don't need to receive the return value, `->` can be omitted.

```vl
-methodName(paramValue1,paramValue2,...) -> _apiResponse // Pass value to a pre-defined or undefined local variable
-<ServiceDomain-DomainName>.ServiceName(paramValue1,param2,...) -> $globalStatus // Pass value directly to global variable
-<ClientUtils>.delay(1500) // Method has no return value
```

**Best Practice**: Recommend storing method/service call results in local variables (`_variableName`) first to maintain clear scope and data flow. Only use global variables (`$variableName`) as receivers when you genuinely need to directly update an existing global state.

#### 8.2.5 Internal Method Usage Boundaries

In frontend and backend development, internal methods are only used for **reusable logic** definition, with strict usage scenarios:

- When a piece of logic is only used in one event method/service/public method, write the logic directly there. Prohibited to define an internal method first then call it.
- When a piece of logic is used in 2 or more event methods/services/public methods, it must be encapsulated as an internal method to avoid duplicate code.

### 8.3 Functions

#### 8.3.1 Pipeline Function Definition File and Section

In VL, you can define custom data transformation pipeline functions in code files:

| Function Type              | Applicable Files | Definition Section        |
| -------------------------- | ---------------- | ------------------------- |
| Frontend pipeline function | vx, sc, cp       | # Frontend Pipeline Funcs |
| Backend pipeline function  | vs               | # Backend Pipeline Funcs  |

#### 8.3.2 Pipeline Function Definition Format

```vl
PIPE <functionName>(param1(Type1),param2(Type2),...);RETURN returnVal1
-function body
```

- Function names use `_` prefix + camelCase naming, e.g., `_calculateTotal`

PIPE function parameter format is identical to method parameter format.

#### 8.3.3 Pipeline Function RETURN Structure

PIPE functions are only for data processing transformation; their RETURN structure can be flexibly handled based on current needs, returning any type supported by the data type system:

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

#### 8.3.4 Function Invocation

All functions **can only be called in chain form within expressions**, using the `.` operator.

### 8.4 Events

#### 8.4.1 Event Concept

Events are the core mechanism in VL for responding to user interaction or system state changes. Unlike methods, events are passively triggered, automatically executing their internal logic when specific conditions are met. Events define response behavior through Event Handlers.

#### 8.4.2 Event Handler Definition

All event handlers are uniformly defined in `# Frontend Event Handlers` / `# Backend Event Handlers` sections, consisting of two parts:

- Event listening statement
- Event handling body

**Definition Format:**

```vl
<ComponentClass-ComponentName "componentId">.@eventName(param1, param2...) // Event listening statement
-event handling body
```

**Format Notes:**

- Event listening statement has no indentation
- All code in event handling body is indented at least one level (`-`)
- Event name must be prefixed with `@`

**Naming Convention:**

- Event names uniformly use **camelCase**
- Event parameter names use **camelCase**
- Parameter names must strictly follow definitions in component documentation

**Event Body Writing Rules**

Event handling body is essentially a method; its writing rules (indentation, variable operations, method calls, control flow, error handling, etc.) are identical to methods.

#### 8.4.3 RETURN Statement

- Event handlers can use `RETURN` to terminate execution early
- Event handlers **don't need return values**; `RETURN` is not followed by anything

```vl
<Button-Submit "submitButton">.@click()
-IF !$canSubmit
--<SysUI>.showToast("Please complete required fields first", "warning")
--RETURN
-submitForm()
```

#### 8.4.4 Event Parameters

Event parameters are defined in the event listening statement:

**When to declare parameters:**

- When the event handling body needs to use event parameters, they must be declared in parentheses of the event listening statement
- Parameter names use **camelCase**

**When parameters can be omitted:**

- When the event handling body doesn't need any event parameters, parentheses can be empty
- Example: `@click()` listens to click event but doesn't use event object

**Example:**

```vl
# Frontend Event Handlers

// Need to use event parameters
<Input-Username "usernameInput">.@change(newValue, oldValue)
-$username = newValue
-<SysUI>.consoleLog("Old value: " + oldValue)

// Don't need event parameters
<Button-Submit "submitButton">.@click()
-submission logic, no need for @click parameters
```

**Notes:**

- Event parameters don't need type declaration (unlike method parameters)
- Event parameter names must exactly match definitions in component documentation
- Even without using parameters, parentheses `()` cannot be omitted

#### 8.4.5 Event Triggering Mechanism

**Prohibited from manually triggering events or calling DOM-style methods through component instances**

- Event handlers are **automatically triggered** by user interaction or component state changes; **cannot be manually called in code**.
- The following are all **wrong usages, strictly prohibited**:

```vl
# Frontend Event Handlers

// ❌ Wrong: Attempting to trigger button click via DOM-style method
<Button-Submit "submitButton">.click()

// ❌ Wrong: Manually calling event as method
<Button-Submit "submitButton">.@click()
```

- In VL, Button, Input, and other UI components **do not expose any `.click()` / `.focus()` / `.blur()` browser DOM methods**;
- Only component methods explicitly declared in component documentation can be called (e.g., some list components' `.scrollToBottom()`, etc.).

**Events Don't Support Propagation**

Events in VL don't support event bubbling or capture mechanisms. Each event is triggered independently.

#### 8.4.6 Event Handling in Loops

**Component id property in loops**

When listening to events on components within a loop container, no need to worry about correspondence between multiple loop-created components; use static ID uniformly.

❌ Strictly prohibit: `<Button-Action ("btn_" + _index0)> value:_item0.name`

✅ Use static ID directly: `<Button-Action "btn"> value:_item0.name`

**Loop Variable Usage**

In event handlers for components defined within `<For>` or `<TreeFor>` loop containers, loop variables can be used directly:

```vl
# Frontend Tree
<For-UserList> sourceArray:$users loopVar:[_user0, _index0]
-<Block-UserItem>
--<Text-UserName> value:_user0.name
--<Button-Select "selectButton"> value:"Select"

# Frontend Event Handlers

<Button-Select "selectButton">.@click() // id is static value
-$selectedUserId = _user0.id
-$selectedUserName = _user0.name
-<SysUI>.showToast(("Selected: " + _user0.name), "success")
```

**Loop Variable Scope**

Loop variables (`_itemX`, `_indexX`) are only valid in event handlers for direct children and descendant components of the `<For>` container.

---

## 9. Expressions

### 9.1 Allowed Operations in Expressions

1. **Arithmetic operators**: `+`, `-`, `*`, `/`, `%`, `**`
2. **Comparison operators**: `==`, `!=`, `>`, `<`, `>=`, `<=`
3. **Logical operators**: `&&`, `||`, `!` (for boolean values)
4. **Ternary operator**: `condition ? trueValue : falseValue`

```vl
$message = ($age >= 18) ? "Adult" : "Minor"
$complexResult = (($valueA > 10 && $valueB < 20) || $isOverride ? "Case1" : "Case2")
```

5. **Variable access**: `$globalVar`, `_localVar`, `_item0.field1`, `SYSENV.currentUser.userId`
6. **String concatenation**: Using `+` operator.
   **Important rule**: To ensure correct types and avoid potential issues, when performing string concatenation, if the expression doesn't start with a literal string (`"..."` or `'...'`), **must** add `"" +` at the beginning.

```vl
$fullName = ($firstName + " " + $lastName)
$priceDisplay = ("" + "¥" + $price)
```

7. **Calling PIPE functions**:

- Example: `_inputValue._trim()._toUpperCase()`

8. **Calling variable built-in immutable methods**:
   These methods **cannot modify original data**, but return new processed results.
9. **Using arrow functions (`=>`) as callbacks**:
   When system default immutable methods (like `filter`, `map`) need callback functions, **must** use arrow function (`=>`) syntax. **Strictly prohibited** to use `function` keyword to define callbacks in expressions, **limited to JS built-in immutable prototype method callbacks**;

   ```vl
   // Correct:
   -_filteredOptions([{value:INT,label:STRING}]) = $stockOptions.filter( (item) => item.value > 10 )
   -_firstMatchingOption({value:INT,label:STRING}) = $stockOptions.filter( (option) => option.value === 100 )[0]
   ```

### 9.2 Core Prohibition Rules

**1. Absolutely prohibited to call METHOD in expressions**

Any method defined with `METHOD` or `METHOD_PUB` cannot be called in expressions:

```vl
# ❌ Wrong examples
<If-Check> conditions:(validateData())  # validateData is METHOD
<Text-Result> value:calculateTotal()    # calculateTotal is METHOD
$summary(STRING) = generateReport()      # generateReport is METHOD

# ✅ Correct: Use PIPE function instead
PIPE _validateData(); RETURN BOOL
-RETURN ($formData.name != "" && $formData.email != "")

<If-Check> conditions:$formData._validateData()
```

**Reason**: METHOD may contain side effects (like modifying variables, calling services); calling in expressions leads to unpredictable behavior and performance issues.

### 9.3 Parentheses Rules in Expressions

**General Rule:**

All expressions with operators must have parentheses. Otherwise, no parentheses.

```vl
$message = ($age >= 18) ? "Adult" : "Minor"
<StateStyle-EmptyState> conditions:(!$hasData)
<Text-Time> value:_item0.time
```

### 9.4 Complex Expression Examples (Including Loop Variables)

```vl
# Accessing loop item properties
_item0.name
_user.profile.avatar

# Conditional judgment on loop item properties
_item0.status == "active" ? "Online" : "Offline"

# Combining loop index and loop item
("" + "Index:" + (_index0 + 1) + ", Name:" + _item0.name)
```

---

## 10. Logic Writing Fundamentals

### 10.1 Unidirectional Data Flow Rules (Strict)

In the following scenarios, **downstream data is uniquely determined by upstream data**; downstream data cannot be modified independently of upstream data, **and upstream data cannot be modified through downstream data**.

#### 10.1.1 Variable Bound to Property (Upstream) -> Component Property (Downstream)

When a component's property is bound to a variable, that variable becomes the sole control factor for that component property. To modify the component property, you must modify its upstream variable.

For input components, this rule equally applies: If an input component's value property is bound to a variable, when the user inputs, you must assign the current user input content to its bound variable through @change(), @blur(), @selected(), @input() events. Note: **There is no v-model syntactic sugar** in VL; all data modifications must be explicit:

```vl
# Frontend Tree
-<Input-Name "nameInput"> value:$name // $name only initializes the input display; won't change automatically with user input
-<Button-Submit "submitButton">

# Frontend Event Handlers

<Input-Name "nameInput">.@change(newValue,oldValue) // Critical!! Must use input event to reassign user input value to variable
-$name = newValue

<Button-Submit "submitButton">.@click()
-submitName($name)
```

#### 10.1.2 Global Variable (Upstream) -> Derived Variable (Downstream)

Derived variables' sole computation basis is the global variables they depend on at definition time; they cannot be independently assigned in subsequent operations.

#### 10.1.3 Loop Data Source (Upstream) -> Loop Variable (Downstream)

Loop variables are references to loop data source; their values are entirely determined by the loop data source and cannot be independently modified.

#### 10.1.4 Method/Function/Event Parameter Declaration (Upstream) -> Parameter Usage in Method/Function Body (Downstream)

Method and event parameters, once declared in method signature/event definition, their subsequent usage is a unidirectional reference to their declaration; parameters cannot be modified in event/method body.

### 10.2 Conditional Judgment Usage Rules (`IF / ELSE IF / ELSE`)

`IF` statements are used for conditional branching based on **boolean expressions**.

**Indentation Rules:**

- `IF booleanExpression` statement maintains **same indentation** as current line in its code block (e.g., `-`).
- `ELSE IF booleanExpression` and `ELSE` keywords also maintain **same indentation** as `IF` (e.g., `-`).
- Execution code blocks inside `IF`, `ELSE IF`, `ELSE` are **one level deeper** than their keywords (e.g., `--`). Nested `IF` continues increasing indentation.

```vl
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

### 10.3 Loop Usage Rules (`FOR`)

VL supports three `FOR` loop structures:

- **In frontend component tree (# Frontend Tree):** Use `<For>`, `<TreeFor>` loop structure containers to loop-create their child components
- **In method/function body:** Use loop syntax `FOR...IN` and `FOR(...)` for array loops and counting loops

##### 10.3.1 Loop Structure Containers in Component Tree (`<For>`, `<TreeFor>`)

Use `<For/TreeFor-containerName sourceArray:iterableArray loopVar:[_itemN,_indexN]>` to create; see component documentation for details.

- Note: `loopVar` property's local loop variables declared in loop structure container properties don't need prior definition.
- Loop variables should uniformly use _itemN and _indexN format, where N represents nesting level, starting from 0.

##### 10.3.2 Array Loop in Methods/Functions (`FOR ... IN`)

Use `FOR ... IN ...` keyword to iterate arrays.

**Indentation Rules:**

- `FOR (_itemVarN, _indexVarN) IN arrayVariable` statement maintains **same indentation** as current line in its code block (e.g., `-`).
- Code blocks inside loop body are **one level deeper** than `FOR` statement (e.g., `--`).
- Loop variables can be used directly without prior definition
- Loop item and index variables **must** use `_` prefix + **camelCase**.

**Syntax:**

```vl
-FOR (_itemVarN, _indexN) IN arrayVariable
--loop execution code
--...
```

##### 10.3.3 Counting Loop in Methods/Functions (`FOR (...)`)

Provides C/Java/JavaScript-like `for` loop structure with initialization, condition check, and increment/decrement expressions.

**Indentation Rules:**

- `FOR (initialization; condition; increment/decrement)` statement maintains **same indentation** as current line in its code block (e.g., `-`).
- Code blocks inside loop body are **one level deeper** than `FOR` statement (e.g., `--`).
- Loop control variable is declared in initialization part using `_varName(TYPE) = initialValue`. **Must** declare type, **no** `let` keyword. **Must** use `_` prefix + **camelCase** name (e.g., `_i`, `_j`, `_k`, `_loopCounter`).

**Syntax:**

```vl
-FOR (_loopVar(TYPE) = initialValue; loopCondition; incrementExpression)
--loop execution code
--...
```

### 10.4 Form Building Standards

Forms in VL follow **unidirectional data flow** principles, implementing real-time data collection by writing back to corresponding global variables through input component events. Form building has two scenarios; choose different implementation strategies based on field count.

#### 10.4.1 Core Principles

**Data Binding Rules:**

- Input component's `value` property binds global variable as initial value
- Write back to global variable through input component events (like `@change()`, `@input()`, etc.)
- There is no v-model syntactic sugar in VL; all data modifications must be explicit

**Form Validation Rules:**

- Validation logic goes in derived variables (use PIPE functions if expressions are too complex) or frontend internal methods
- Validation results control submit button's `disabled` property

#### 10.4.2 Scenario 1: Field Count ≤ 3 (Direct Building)

Declare input components directly in component tree, each corresponding to one global variable.

#### 10.4.3 Scenario 2: Field Count > 3 (Loop Building + Custom Component)

- Use `<For>` and `$formFields` to dynamically create input components
- Create a custom dynamic field component (switch displayed input component type via type property) to uniformly manage field input and events
- Dynamic field's input completion event writes current input value back to `$formData`, then calls a `validate` method to write all current validation errors to `$errors`
- Use `$errors` to display real-time error prompts after each field and control whether submit button can submit

### 10.5 Error Handling

**Use system default error handling** (recommended):

- Standard CRUD operations
- Simple data fetching
- No special error recovery logic needed

**Use `IF` custom judgment**:

- Need different handling based on specific error types
- Need to perform cleanup operations on error
- Need to log detailed error logs

After **method calls** or **service calls**, there are two error handling approaches:

**1. System Default Error Handling Mechanism**

No explicit error handling logic declaration needed; for any method call, as long as the previous step's return result's success field doesn't equal true, default system error handling occurs:

- Current method/event RETURNs
- If current method/event's return parameters have success and message fields, returns success:false, message:previousStepResult.message
- If current method/event is frontend-called, automatically calls SysUI method to handle error message prompting
- All three points are handled automatically by system; user doesn't need to declare IF statements and subsequent handling
- Without special requirements, directly use system default error handling to simplify code
- Recommend using system default error handling; only use IF special judgment when user feels need to handle errors separately

**2. Manual Error Handling Mechanism**

Use `IF` to manually determine error handling logic.

---

## 11. Style Definition

### 11.1 Overall Style Configuration Rules

**1. StyleClass takes priority over individual property overrides**

- Recommend first defining `# Component Styles` (component preset styles) in project Theme file, then using StyleClass property to reference in `# Frontend Tree`
- If component preset styles don't meet requirements, override individually in component properties under `# Frontend Tree`

**2. ThemeToken takes priority over literal values**

- Whether in Theme file's `# Component Styles` or frontend file's `# Frontend Tree`, prefer using ThemeTokens like `--colorBrandPrimary`, `--spacing4`
- Only use literal values when a style property isn't defined in `Theme.vth`

**3. CSS Literal Value Writing Format**

- When property value is **pure number**: No quotes, e.g., `line-height:2.5 font-weight:600 opacity:0.5 z-index:10`
- When property value is **not pure number**: Always add double quotes, e.g., `color:"#ff0000" background-color:"transparent" cursor:"pointer" width:"100px" height:"auto" max-width:"calc(100% - 20px)" background-image:"url('https://example.com/image.jpg')"`

**4. Dynamic Style Specifications (Strict)**

- Prefer using dynamic StyleClass property, e.g., `StyleClass:($selected?"TextSelected":"TextNormal")`
- If dynamic StyleClass doesn't meet requirements, use VL expressions in component override CSS style properties, e.g., `<Block-ProgressBar> width:("" + $progress*300 +"px")`. Note: VL expressions only supported in regular component instances; **`StateStyle` widget is static style configuration, VL expressions not allowed**
- **Strictly prohibited to directly access/modify component styles in events/methods**

### 11.2 Compound Property Usage Rules

- **Allowed compound value properties (keep complete)**: `box-shadow`, `transform`, `clip-path`, `filter`, `text-shadow`, and `background-image` (for gradients) and `grid-template-columns/rows/areas` property values are inherently compound structures; **must** keep their complete string form.
- **Prohibited multi-property shorthand (must split)**:
  - `background` (split into `background-color`, `background-image`, `background-repeat`, etc.)
  - `border` (split into `border-width`, `border-style`, `border-color`)
  - `font` (split into `font-size`, `font-family`, `font-weight`, etc.)
  - `transition` (split into `transition-property`, `transition-duration`, etc.)
  - `flex` (split into `flex-grow`, `flex-shrink`, `flex-basis`)
  - And other similar bundled shorthands.
- **Allowed multi-direction shorthands (follow "either-or" rule)**:
  - `padding`, `margin`, `overflow`, `border-radius`, `gap`
  - **"Either-or" principle**: If using these shorthand forms (like `padding:"10px"` or `padding:"10px 20px"`), **prohibited** to also use corresponding single-side properties (like `padding-left`) in the same style rule set.

### 11.3 Prohibited CSS Properties in VL

- **Media queries (strictly prohibited)**: **Strictly prohibited** to use `@media` queries in any style definition. VL's design philosophy is to create independent applications for each target device (PC, Phone, Pad), not responsive adaptation within a single application.
- **Animation keyframes**: Only **@keyframes** animation, **animation** property VL syntax doesn't support; other CSS styles are all supported.
- **display**: `display` property is entirely managed internally by components; not allowed to directly write `display:"xxx"`

### 11.4 `<StateStyle>` Conditional Style Widget

The conditional style widget, through its Conditions and Trigger properties, can clearly specify a UI component's styles in different scenarios. Note: StateStyle widget's style definition only supports literals and calc expressions; VL expressions are strictly restricted.

#### Components That Cannot Add Styles

The following components are pure logic containers with no DOM elements. Therefore, no style definitions can be added to these components (i.e., nothing outside angle brackets `<...>`). Consider these components "transparent" in layout, not participating in UI element layout at all: `If`, `For`, `TreeFor`, `AnimationGroup` (and other similar pure logic loop or conditional rendering components).

---

## 12. Database File Specifications

### 12.1 Table Definition

**Example:**

```vl
<Table-Users> data:[{"_id":1,"username":"user1","email":"user1@example.com","role":"Admin","status":1,"_user":"1","_create":"2024-01-15 09:30:00","_update":"2024-01-15 09:30:00"}...]
-<Field-username> type:STRING
-<Field-email> type:STRING
-<Field-role> type:STRING
-<Field-status> type:INT
```

**Rules:**

- Table names **PascalCase**, field names **camelCase**
- `default` supports literals: strings need quotes (e.g., `default:""`), booleans (`true|false`), numbers, time strings, etc.
- The following system fields are **automatically added during system table creation**, **prohibited from explicitly declaring same-meaning fields**:
  - `_id(INT)`: Auto-increment primary key, starting from 1
  - `_user(STRING)`: Records submitting user's ID
  - `_create(TIMESTAMP)`: Creation time
  - `_update(TIMESTAMP)`: Update time
- `vecSource` is vector type field specific property, specifying vector field's source data fields.
- `Index` is optional child component; index field order takes effect.
- `data` is optional property; used to initialize some test data. Initialize 3-7 rows of data based on actual needs. Unlike normal insert/update in services, test data needs to declare system fields for testing convenience:
  - `_id` field: Required, start from 1
  - `_user` field: Required, if no special requirements, use "1"
  - `_create`/`_update` fields: Required, if no special requirements, use current time

### 12.2 Relation Definition

**Format:**

```vl
<Relation-RelationName relation1 relation2 ..>
```

**Examples:**

```vl
<Relation-User&Profile> users._id--userProfiles.userId // One-to-one single relation
<Relation-User&Posts> users._id<<posts.authorId  // One-to-many single relation
<Relation-User&Messages> users._id<<messages.senderId users._id<<messages.receiverId // One-to-many multi-line relation
<Relation-Departments&Employees> departments._id<<employees.departmentId departments.managerId>>employees._id // One-to-many + many-to-one dual relation
<Relation-Warehouse&Inventory> warehouses.(code,region)<<inventory.(warehouseCode,warehouseRegion) // One-to-many single relation composite key
```

- One Relation describes all relationships between two tables; relation name strictly follows table names. E.g., `<Relation-TableName1&TableName2>`, two table names connected with `&`.
- Two tables may have multiple relationships
- Each relationship contains table1's field, relation symbol, and table2's field
- Relation symbols `--`, `<<`, and `>>` represent one-to-one, one-to-many, and many-to-one respectively
- A relationship may contain composite keys; note that multiple fields in a composite key actually represent one direction, so use `table1.(compositeField1,compositeField2...)` format.

---

## 13. App File Specifications

### 13.1 `#SysConfig` (Application System Configuration)

`.vx` application files must declare system configuration parameters in `#SysConfig`:

```vl
# SysConfig

DEVICE_TARGET:"PC"
SCREEN_RESOLUTION:"1920x1080"
```

**Required Configuration Items:**

- **DEVICE_TARGET**: Target device type, options: "PC", "Phone", "Pad"
- **SCREEN_RESOLUTION**: Target screen resolution

---

## 14. Section and Component File Specifications

### 14.1 `# Frontend Public Props` (Public Property Definition)

Use frontend global variables (`$`) to define public property variables. These variables are used the same way as regular global variables within the file, but during application compilation, public property variable values can be passed in by upper-level files (App or Section).

**Definition Method:**

```vl
# Frontend Public Props
$propertyName(type) = initialValue
```

### 14.2 `# Frontend Tree`

**Prohibited**: Absolutely prohibited to add another Section within a Section.

### 14.3 `# Frontend Public Events` (Frontend Public Event Definition)

In Section files, public events can be defined—events sent from inside the module to outside. These events can be listened to at the application layer (.vx file) to complete interaction between Section modules. Definition consists of two parts:

1. First in `# Frontend Public Events` section, top-level, no indentation, define event method name using `@`:

```vl
EVENT @eventName(param1(Type1),param2(Type2),...)
```

- **Parameter naming**: All parameters use **camelCase**, **without `_` prefix**.

2. Then inside Section file's program logic (usually in event handlers or methods), define its trigger logic:

```vl
<Button-Submit "submitButton">.@click()
-validateForm() -> _validation
-IF _validation.success
--@formSubmitted($formData)
-ELSE
--<SysUI>.showToast(_validation.message, "error")
```

### 14.4 `# Frontend Public Methods` (Frontend Public Method Definition)

In .sc/.cp files, you can define a method as public using `METHOD_PUB MethodName` format.

These methods can be called by upper-level application files (.vx/.sc).

Public methods can also be called within the file; the calling method is identical to internal methods:

```vl
# Frontend Public Methods

METHOD_PUB Hide();RETURN {success:BOOL}
-$isVisible = false
-@modalClosed()
-RETURN {success:true}

# Frontend Event Handlers
<Button-Close "closeButton">.@click()
-Hide() // Call method directly
```

Note: When calling public methods inside a file, don't mistake public methods as root component methods. For example, **there is no `<Section-MyModule>.Hide()` calling method** inside a Section.

### 14.5 Calling Services in Sections

Section files can call services defined in any ServiceDomain file under the same project:

1. Add the needed ServiceDomain in the component tree as a functional component:

```vl
# Frontend Tree

<ServiceDomain-ServiceDomainName "serviceId"> // First import service domain component
-<Service-ServiceName1> params:(param1(type),param2(type)...) returns:(out1(type),out2(type)...) // Import needed services with params and returns as component properties
-<Service-ServiceName2> ... // Another service under service domain
```

2. In Section's event handlers or methods, call services defined in this ServiceDomain:

```vl
<ServiceDomain-ServiceDomainName "serviceId">.ServiceName()
```

---

## 15. ServiceDomain File Specifications

### 15.1 VirtualTable Definition

VirtualTable is the "intermediate client" through which backend service logic operates real database tables. VL adheres to the principle of separating code logic from data source definitions; database tables cannot be operated directly in service code and must use VirtualTable as a bridge. The sourceField property specifies virtual table-entity table correspondence.

**Format:**

```vl
<VirtualTable-TableName "componentId"> extraSpecs?:{specName1(type):"propertyDescription"..} sourceTable:correspondingEntityMainTable
-<Field-fieldName> type:fieldType vecSource?:["virtualField1","virtualField2"] sourceField?:correspondingEntityTableField extraProperty1:value.. // Field as VirtualTable child must have one extra indentation
```

### 15.2 `# Services` (Backend Service Definition)

In vs files, all externally callable services should be defined under `# Services` section, prefixed with `SERVICE ServiceName`.

Each service acts as a public method of the current ServiceDomain. These methods can be called in Section files.

### 15.3 `# Backend Event Handlers` (Backend Event Listening)

Backend message component message handling logic definitions. E.g., `<ServerWSClient>`, `<MQ>` message events.

### 15.4 `# Transactions` (Database Transaction Definition)

Define a database operation transaction; rollback operations are supported within transactions:

```vl
# Transactions
TRANSACTION txTransferMoney(fromId(INT), toId(INT), amount(FLOAT)); RETURN {success:BOOL, message:STRING}
-<VirtualTable-Accounts "accountTable">.select([["_id","eq",fromId]]) -> _fromAccount
-IF _fromAccount.dataArray[0].balance < amount
--ROLLBACK {success:false, message:"Insufficient balance"}
-<VirtualTable-Accounts "accountTable">.update([["_id","eq",fromId]], [["balance","inc",-amount]], 1) -> _updateFrom
-<VirtualTable-Accounts "accountTable">.update([["_id","eq",toId]], [["balance","inc",amount]], 1) -> _updateTo
-RETURN {success:true, message:"Transfer successful"}
```

- Names use camelCase starting with `tx`
- Use ROLLBACK keyword to rollback transaction
- Transaction usage is identical to internal methods; cannot be called directly from frontend like services

### 15.5 `# Backend Internal Methods` (Backend Internal Method Definition)

Define backend internal methods prefixed with METHOD; format identical to frontend internal methods.

---

## 16. Section and App Responsibilities

In VL application systems, follow these principles for App and Section usage:

### 16.1 Independent Section Principle

- Each Section is an independent entity that can be previewed and debugged alone; its interior is a complete system, so it cannot embed other Sections
- All Sections are equal; interaction relationships between Sections are entirely implemented by App code. **Strictly prohibited to embed another Section or call another Section's methods** within a Section
- When developing Sections, they're unaware of other Sections' existence; route-related functionality should all be at the App layer

### 16.2 Functional Cohesion Principle

- Sections carry main frontend application functionality, including core UI and backend interaction, all concentrated in Sections
- App only handles frontend assembly of all Sections; it doesn't carry main functionality. All features requiring backend interaction (lists, details, etc.) go in Sections
- Apps are prohibited from adding these logic components: ServiceDomain, For, TreeFor; can only arrange Sections and complete Section-to-Section interactions
- Apps can add global variables to store cross-Section state

---

## 17. User Authentication and Access Control

### 17.1 User Center

VL system uniformly provides User Center application; every project automatically binds to a User Center without specifying in VL. User Center provides:

- User registration, login
- User information editing
- Admin user management (user list view, user disable/deactivate)
- Admin user permission configuration (ABAC combined with RBAC permission management system; configurable resource groups, access rules, etc.)

### 17.2 Application and User Center Interaction

In frontend applications, `<ClientUserCenter>` component can call User Center interface methods to implement frontend custom login interface functionality.

Both frontend and backend applications can use `SYSENV.currentUser` to get current logged-in user information.

### 17.3 Logic Prohibited in Applications

Entity table, virtual table, service, and frontend element access permissions in applications will be configured by administrators or an independent AI agent in User Center's admin interface. Therefore, **do not hardcode any user permission-related logic in code**, including:

- Whether a service can be accessed
- Data access scope restrictions (e.g., whether only own-created data can be accessed, or department data, or global data)
- Whether a page can be accessed
- Whether a table or field can be accessed or updated

**Example:**

```vl
// Wrong: Hardcode user-related filtering logic in code
<messageTable>.select([["_user","eq",SYSENV.currentUser.userId]],[["_update","desc"]],[offset,limit],null) -> _result

// Correct: Code only handles business logic; user-related data scope permissions left to permission configuration module
<messageTable>.select(null,[["_update","desc"]],[offset,limit],null) -> _result
```
