# Development Interaction Model

This document explains how developers interact with the VisualLogic IDE and how different interaction methods operate on the same underlying full-stack system.

---

## VisualLogic as a Full-Stack Development System

VisualLogic is not a UI builder, page editor, or script generator.

It is a complete full-stack development system that includes:

- Frontend interface structure
- Frontend interaction logic
- Backend services and data models
- Runtime execution and compilation
- Source code generation and export

All development activities—regardless of interaction method—operate on a unified project model and execution architecture.

---

## Project-Centric Interaction

In VisualLogic, all development starts from a **project**.

A project is a logical container that includes:

- One backend application (data and services)
- One or more frontend applications (user interfaces and frontend logic)

This structure reflects real-world application architecture, where backend services are shared across multiple frontend clients.

---

## Interaction Through the Visual IDE Workspace

The primary interaction surface of VisualLogic is the integrated visual IDE.

Within this workspace, developers can explicitly define:

- Component structure
- Data flow
- Interaction logic
- Event orchestration
- Backend service calls

This is achieved through coordinated panels including the stage, object tree, properties panel, event panel, and logic panels.

All interactions performed visually are directly mapped to executable logic structures.

---

## Interaction Through AI-Assisted Development

VisualLogic also provides an integrated AI interaction window.

Through natural language input, developers can describe:

- Business requirements
- Application roles and responsibilities
- Interface layout intentions
- Data models and logic behaviors

The AI does not generate isolated code fragments.  
Instead, it generates **structured project content** that conforms to the same project model used by the visual editor.

This may include:

- Creation of frontend applications
- Page and component structure
- Event logic and data bindings
- Backend service scaffolding

All generated results become native project content and can be inspected, modified, and extended visually.

---

## Relationship Between Interaction Methods

Visual interaction and AI interaction are not separate development paths.

They are **two ways of operating the same system**.

A typical workflow may include:

1. Creating a project
2. Describing requirements through AI interaction
3. Reviewing generated frontend and backend structures
4. Adjusting layout, logic, and data visually
5. Continuing iterative development through either method

There is no mode switching, conversion, or migration.

---

## Logic Construction and Execution Model

Regardless of interaction method, application logic in VisualLogic is constructed using:

- Events as execution entry points
- Actions as execution steps
- Conditions for decision control
- Loops for repeated execution
- Formula expressions for data transformation

These structures correspond directly to traditional programming concepts such as functions, conditionals, and loops, but are expressed graphically.

Execution order, dependencies, and parameters are all explicit.

---

## Engineering Transparency and Control

VisualLogic emphasizes transparency rather than abstraction.

Developers can always:

- Inspect all frontend components
- Trace logic execution paths
- View backend service definitions
- Control data structures and permissions
- Export complete source code

This ensures that applications remain understandable, maintainable, and extensible as they grow.

---

## Summary

- VisualLogic supports multiple interaction methods
- All interactions operate on a unified project model
- Visual and AI interactions are fully interoperable
- Logic is explicit and traceable
- Full-stack control and code export are always available