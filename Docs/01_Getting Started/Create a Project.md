# Create a Project

> This document explains how to create a project in VisualLogic and how projects evolve through different development interactions inside the IDE.

---

## What a Project Represents in VisualLogic

In VisualLogic, a **project** represents a complete application system.

A project defines:

- The boundary of data and business logic
- The scope of frontend applications
- The collaboration and versioning context

All development activities—whether related to UI, logic, or data—take place within a project.

---

## Creating a Project

Projects are created from the **top toolbar** of the IDE.

### Steps

1. Open the VisualLogic IDE
2. Click the **+ Project** button in the top navigation bar
3. Enter a project name
4. Confirm creation

After confirmation, the project is created and opened immediately.

---

## Default Project Initialization

When a project is created, the IDE initializes a **base system structure**.

### Backend Application (Created Automatically)

Each project contains **one backend application**, which is created by default.

The backend application is responsible for:

- Data models and databases
- Business logic and rules
- Services and APIs
- Frontend–backend communication

The backend application has no visual interface and serves as the shared foundation for all frontend applications in the project.

This design ensures that data and business logic remain centralized and consistent.

---

## Frontend Applications in a Project

Frontend applications are not limited in number.

A project may contain multiple frontend applications, for example:

- A PC-oriented frontend and a mobile-oriented frontend
- A client-facing frontend and an admin/management frontend
- Independent frontends sharing the same backend logic

All frontend applications operate within the same project and connect to the same backend application.

---

## Creating Frontend Applications Through AI Interaction

VisualLogic provides an integrated AI interaction window, displayed by default in the **bottom-right corner** of the IDE.

Through natural language interaction, developers can describe:

- The purpose of the application
- User roles and usage scenarios
- Required pages and functionality
- Interface characteristics (such as PC or mobile usage)

Based on this input, the AI creates frontend applications that:

- Are bound to the project’s backend application
- Include pages, components, and interaction logic
- Are immediately runnable and previewable in the IDE

The generated frontend applications become first-class parts of the project and can be freely inspected, modified, and extended.

---

## Manual Creation of Frontend Applications

In addition to AI-assisted creation, frontend applications can also be created manually.

Developers can:

- Click the **+** button within the project
- Add a new frontend application
- Build its structure using the visual IDE tools

Manual creation is often used to:

- Extend an existing project with new frontends
- Introduce specialized interfaces
- Refine system structure as requirements evolve

---

## Working Within a Unified Development Model

Regardless of how a frontend application is created:

- All applications share the same backend foundation
- All logic is visible and editable
- All data flow is explicit
- All components operate within the same project model

AI interaction and visual editing are **different ways of interacting with the same system**, not separate development modes.

Developers may switch freely between these interactions as the project evolves.

---

## From Creation to Iteration

After frontend applications are created:

- The application can be previewed immediately
- UI layout and logic can be adjusted visually
- Backend data and services can be refined
- Additional frontend applications can be added as needed

This supports an iterative development process while preserving architectural clarity.

---

## Common Misunderstandings

- A project is not a single page or a single application
- Backend logic is created once and shared
- Frontend applications are independent but connected
- AI-generated structures are fully editable
- Visual development does not hide system architecture

---

## Summary

- Projects define the complete system boundary
- A backend application is created by default
- Frontend applications can be created via AI interaction or manually
- Multiple frontends can share one backend
- All development interactions operate on the same project model
- Full control over structure, logic, and source code is preserved