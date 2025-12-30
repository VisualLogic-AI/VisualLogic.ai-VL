# AI Files (Project File Structure)

## 1. Overview

The **AI Files panel** exposes the complete **file-based project definition** of a VisualLogic project.

This panel represents the **source-of-truth layer** of the system.
 All frontend applications, backend services, data structures, and visual specifications are defined as **explicit VL files** and organized in a fixed directory structure.

Unlike the Graphic IDE (Stage, Object Tree, Event Panel), which presents **derived visual representations**, the AI Files panel shows the **actual project files** that are:

- Parsed by the VL compiler
- Validated against language and structural rules
- Used to generate runtime code
- Exported as final source code

Any change at this level directly affects compilation results.

------

## 2. Project File Tree Structure

> ⚠️ **The following structure is normative and must not be altered.**

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

All files under this structure participate in **final project compilation**.

There is no concept of disabled, optional, or ignored files.

------

## 3. File Type Semantics and Responsibilities

This section defines **what each file type represents**, **what it is allowed to do**, and **what it must not do**.

------

### 3.1 `.vx` — App (Application Entry)

An `.vx` file defines **one frontend application entry**.

#### Responsibilities

- Defines application-level configuration
- Declares the root frontend tree
- Coordinates Sections and Components
- Owns global frontend variables
- Handles application-level event bindings

Each `.vx` file corresponds to **exactly one application**.
 Multiple `.vx` files under `Apps/` represent multiple frontend applications sharing the same backend.

#### Required Sections (Strict Order)

- `# SysConfig`
- `# Frontend Tree`
- `# Frontend Event Handlers`

Each section:

- Must appear **once**
- Must appear **in order**
- Missing or reordered sections cause **compilation failure**

#### Engineering Rationale

The compiler relies on fixed ordering to:

- Resolve runtime environment
- Build the frontend object tree
- Bind event handlers deterministically

------

### 3.2 `.sc` — Section (Frontend Business Module)

A `.sc` file defines a **frontend business module**.

#### Responsibilities

- Encapsulates a complete business interaction
- Connects UI structure with backend services
- Acts as a reusable logic unit inside Apps

#### Capabilities

- Can be referenced by `.vx`
- Can call `.vs` (ServiceDomain)
- Can use `.cp` (Components)
- Can define public props, events, and methods

#### Prohibitions (Strict)

- Sections **cannot nest other Sections**
- Sections **cannot act as application roots**
- Sections **cannot be used outside Apps**

#### Engineering Rationale

These constraints prevent:

- Recursive frontend structures
- Implicit application entry points
- Unclear ownership of business logic

------

### 3.3 `.cp` — Component (Pure UI Component)

A `.cp` file defines a **pure UI component**.

#### Responsibilities

- UI rendering only
- Visual composition
- Stateless interaction handling

#### Allowed

- Props for input
- Events for output
- Derived variables for presentation logic

#### Forbidden

- Calling ServiceDomain
- Business logic
- Nesting Components
- Cross-component imports

#### Engineering Rationale

Components are intentionally restricted to ensure:

- Predictable reuse
- No hidden side effects
- Clear separation from business logic

------

### 3.4 `.vs` — ServiceDomain (Backend Service Domain)

A `.vs` file defines a **backend service domain**.

#### Responsibilities

- Encapsulates backend logic
- Accesses database schema
- Exposes callable services to frontend

#### Required Sections

- `# Backend Tree`
- `# Services`

#### Prohibitions

- Cannot reference frontend files
- Cannot reference other ServiceDomains
- Cannot depend on UI constructs

#### Engineering Rationale

This ensures backend services remain:

- Deterministic
- Reusable across multiple frontends
- Isolated from UI concerns

------

### 3.5 `.vdb` — Database (Database Structure)

A `.vdb` file defines the **authoritative database schema**.

#### Responsibilities

- Table definitions
- Field types and constraints
- Indexes and relations

The database file is loaded during backend compilation and defines the persistent data model.

------

### 3.6 `.vth` — Theme (Theme Configuration)

A `.vth` file defines **project-wide visual specifications**.

#### Rules (Strict)

- Exactly one theme file per project
- Existing tokens **must not be modified**
- New tokens may only be appended

#### Engineering Rationale

This guarantees:

- Visual consistency
- Safe incremental design evolution
- Backward compatibility

------

## 4. File Cross-References (Strict Rules)

VL files support **limited cross-referencing**.

| Source \ Target   | App  | Section | ServiceDomain | Component |
| ----------------- | ---- | ------- | ------------- | --------- |
| **App**           | ❌    | ✅       | ❌             | ✅         |
| **Section**       | ❌    | ❌       | ✅             | ✅         |
| **Component**     | ❌    | ❌       | ❌             | ❌         |
| **ServiceDomain** | ❌    | ❌       | ❌             | ❌         |

Violations cause **compilation failure**, not runtime warnings.

------

## 5. VL File Sections and Ordering

All VL files follow **strict section ordering rules**.

- Each section appears exactly once
- Order is fixed per file type
- Missing or duplicated sections invalidate compilation

This enables deterministic parsing and code generation.

## 6. AI Files Panel Operations 

The AI Files panel is not a simple file browser.
 It is the **operational interface for managing the project’s source-of-truth files** and controlling how AI-generated structures are persisted, validated, and converted into visual applications.

All operations in this panel act **directly on the project file set**, and therefore have immediate impact on compilation and visual rendering.

------

### 6.1 Viewing and Editing VL Files

#### Selecting Files

- Clicking a file selects it as the **active source file**
- The file content is displayed in the central **VL Code editor**
- The editor shows the complete VL structure of the file

This allows developers to **inspect exactly what AI has generated**, rather than treating AI output as opaque.

------

#### Editing VL Code

- VL code can be edited directly in the code editor
- All edits apply to the underlying project files
- Section ordering and structural rules are enforced

This enables developers to:

- Correct AI-generated details
- Refine logic
- Adjust structure at the source level

------

### 6.2 Save and Update Operations

#### Save

The **Save** action persists all current file changes to the project workspace.

This operation:

- Writes changes to disk
- Makes them eligible for compilation
- Does not alter project structure

Save should be used frequently when iterating on VL code.

------

#### Update VL Code

**Update VL Code** re-synchronizes the internal IDE model with the current file contents.

This operation is required when:

- Files are edited manually
- Files are uploaded or replaced
- Structural changes occur that affect parsing

It ensures that:

- The compiler sees the latest file definitions
- The Graphic IDE reflects current source state

------

### 6.3 Exporting to the Graphic IDE

#### Export to Graphic IDE

This operation converts the **file-based project definition** into a **graphical IDE representation**.

During export:

- `.vx` files generate frontend applications
- `.sc` files generate reusable frontend sections
- `.cp` files generate reusable UI components
- `.vs` and `.vdb` files generate backend structure
- `.vth` files apply visual tokens and styles

After export:

- Applications appear in the Graphic IDE
- Object Trees and Stages are created
- Visual editing becomes available

This operation is the **bridge between AI-generated files and visual development**.

------

### 6.4 File and Folder Management

The AI Files panel supports direct manipulation of the project file system.

#### Rename

- Double-click a file or folder to rename it
- Renaming affects references during compilation

Clear naming improves readability and long-term maintainability.

------

#### Delete

- Removes files or folders from the project
- Deleted files are removed from compilation input

Deleting required files may cause compilation failure.

------

#### Download

- Downloads individual files or folders
- Useful for inspection, sharing, or backup of specific artifacts

------

### 6.5 Project Import and Export

#### Download Zip

Packages the **entire project workspace** into a ZIP archive.

Typical use cases:

- Backup
- Version snapshots
- Transfer between environments

------

#### Upload Zip

Imports a ZIP archive as a VisualLogic project.

Rules:

- The archive must follow the standard project structure
- All required directories and files must exist

After upload, **Update VL Code** is typically required to refresh the IDE state.

------

### 6.6 Creating New Files

The AI Files panel provides explicit actions to create new VL files:

- `+ .vdb` — Database definition
- `+ .vs` — ServiceDomain definition
- `+ .cp` — UI Component
- `+ .sc` — Frontend Section
- `+ .vx` — Application entry

Each creation action:

- Places the file in the correct directory
- Initializes a valid structural template
- Prevents invalid file placement

This reduces the risk of structural errors.

------

## 7. Summary 

The AI Files panel defines **how a VisualLogic project actually exists**.

It is not merely a file list, but the **authoritative operational layer** where:

- AI-generated project structures are materialized
- VL source files are inspected and edited
- Compilation inputs are managed explicitly
- File-based definitions are converted into visual applications

In the VisualLogic workflow:

- **AI plans and generates files**
- **AI Files makes them explicit and editable**
- **Graphic IDE renders them visually**
- **Compilation consumes them as source**

Understanding the AI Files panel is therefore essential for:

- Verifying AI output
- Maintaining architectural correctness
- Controlling the full lifecycle from requirements to runnable applications