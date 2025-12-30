# Object Tree

## Overview

The **Object Tree** presents the internal structure of the **currently active application** in the VisualLogic IDE.

It provides a **hierarchical representation of objects within a single application**, allowing developers to understand, navigate, and modify structure as applications grow in size and complexity.

In the current IDE architecture, a **Project** is organized as a **Project Group** that contains:

- **One Backend Application**
   (databases, services, business logic, frontend–backend communication)
- **One or More Frontend Applications**
   (UI, layout, and client-side interaction logic, such as PC frontend, mobile frontend, admin frontend)

Each application maintains **its own independent Object Tree**.

Which Object Tree is displayed depends entirely on **which application is currently opened**:

- **Opening a Frontend Application shows a Frontend Object Tree**
- **Opening the Backend Application shows a Backend Object Tree**

Objects from different applications are never shown in the same tree.

------

## Where the Object Tree Fits in the Editor

The Object Tree panel is located on the **right side of the IDE workspace**.

It is designed for **structural navigation and editing**, including:

- Selecting objects precisely
- Adjusting hierarchy and containment
- Renaming and reorganizing components
- Copying or removing objects within the current application

A practical mental model when working in the editor is:

- **Stage** — visual appearance and spatial layout
- **Object Tree** — structural composition and hierarchy
- **Properties Panel** — configuration and data binding
- **Event Panel** — interaction and execution logic

As an application grows to include many pages, nested containers, and reusable modules, the Object Tree becomes the **primary tool for understanding and managing structure**.

------

## What the Object Tree Represents

The Object Tree represents the **structural composition** of the current application.

It explicitly describes:

- Which objects exist in the application
- How objects are nested and grouped
- Parent–child containment relationships
- Ordering of objects within the same scope

This structural information is **independent of visual styling and runtime behavior**.

The Object Tree does **not** describe:

- Colors, fonts, or visual effects
- Exact on-canvas appearance
- Execution order or runtime logic flow

Those aspects are handled by the **Stage**, **Properties Panel**, and **Event Panel** respectively.

------

## Frontend vs Backend Object Trees

VisualLogic separates frontend and backend development at the **application level**, and this separation is reflected in the Object Tree.

------

### Frontend Object Tree (Frontend Applications)

When a frontend application is active, the Object Tree displays frontend objects only, such as:

- Pages
- Layout containers (rows, columns, groups)
- Visible UI components (buttons, inputs, lists, media, etc.)
- Frontend modules and reusable UI structures
- Frontend-oriented bindings (component properties bound to variables or state)

The frontend object tree focuses on **how the interface is composed** and **how UI elements are organized**.

------

### Backend Object Tree (Backend Application)

When the backend application is active, the Object Tree displays backend objects only, such as:

- Databases, tables, and data models
- Backend services exposed to frontend applications
- Backend logic modules that process data and return results
- Reusable backend capabilities shared by multiple frontends

The backend object tree focuses on **data sources, processing logic, and service definitions**, rather than visual composition.

> The key idea is that **frontend objects describe user-visible structure**,
>  while **backend objects describe system capabilities and data flow**.
>  Both are edited using the same IDE concepts, but appear in separate object trees depending on the active application.

------

## Core Concepts: Hierarchy and Containment

In VisualLogic, structure is not only descriptive—it affects layout behavior, inheritance, and how developers reason about UI composition.

------

### Parent–Child Relationships (Containment)

Parent–child relationships represent ownership and containment:

- Container objects can hold multiple child objects and appear **indented** in the Object Tree
- Certain parent properties (such as visibility or positioning context) can influence child behavior
- Common parent objects include pages, layout containers, and grouped modules

**Practical implication:**
 If a component behaves unexpectedly (for example, appearing hidden, offset, or clipped), the first step is to inspect its **ancestor objects** in the Object Tree and reason about containment.

------

### Ordering Within the Same Scope

Objects under the same parent are ordered sequentially.

The effect of this ordering depends on the layout context:

- In **relative layout environments**, order influences layout flow and positioning
- In **absolute layout environments**, order influences visual stacking when objects overlap

Understanding object order is essential when diagnosing overlap or layout issues.

------

## Everyday Operations (Structural Editing)

The Object Tree is not only a navigation view—it is a **structural editor** for the current application.

------

### Rename

- Double-click an object name to rename it

**Recommendation:**
 Adopt a consistent naming convention early (for example: `PageOrders`, `BtnSubmit`, `ModalCreateUser`, `SvcUserQuery`) to keep large trees readable and searchable.

------

### Reordering Objects

- Drag objects within the same parent to adjust their order

This is commonly used to resolve overlap issues or clarify intended visual layering.

------

### Changing Parent Relationships

- Drag an object to a different parent to restructure containment

This allows layout refactoring without recreating components.

------

### Management Operations

- Copy, cut, paste, and delete objects
- Supports batch operations within the current application

These operations are especially useful when maintaining multiple frontend applications with similar structures.

------

## Binding and Association Indicators

As applications grow, understanding **how objects are connected** becomes more important than simply locating them.

The Object Tree provides visual indicators for this purpose.

------

### Event Binding Indicator

Objects with event bindings display an indicator in the Object Tree.

This allows developers to quickly identify which components have interaction logic attached and navigate to relevant event definitions.

------

### Property and Data Binding Indicators

Objects with bound properties display binding indicators.

These indicators signal that a property value is dynamically derived rather than statically configured, helping prevent accidental overrides.

------

## Recommended Structuring Practices

To keep large object trees readable and maintainable:

1. **Page → Layout Containers → Feature Modules → Leaf Components**
    Avoid placing too many leaf components directly under a page root.
2. **Group by feature, not by component type**
    For example, an `OrderListModule` groups table, filters, empty state, and actions together.
3. **Encapsulate repeated patterns as reusable modules**
    Especially important when maintaining multiple frontend applications within the same project.

------

## Summary

The Object Tree is an **application-scoped structural view** that:

- Represents the internal structure of the current application
- Separates frontend and backend objects by application
- Makes hierarchy, containment, and ordering explicit
- Supports precise structural editing and refactoring
- Helps developers manage complexity without conflating structure, appearance, or behavior