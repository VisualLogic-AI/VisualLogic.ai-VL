# Stage

The stage is the central working area of the VisualLogic IDE.  
It represents the visual and interactive surface of frontend applications and serves as the primary entry point for interface construction and inspection.

---

## Position of the Stage in the IDE

In the VisualLogic IDE, the stage is located at the center of the editor workspace.

It is commonly referred to as:

- The frontend workspace
- The visual editor window
- The interface rendering area

Unlike a static preview, the stage is a **fully interactive development surface**.  
It reflects the real structure and runtime behavior of the frontend application during development.

---

## What the Stage Represents

The stage visually represents:

- All visible frontend components
- Their spatial layout and hierarchy
- Their current state during development

Every component that is rendered and visible to end users must appear on the stage.

Internally, the stage reflects the same component tree that is shown in the object tree panel, but from a **visual and spatial perspective**.

---

## Core Responsibilities of the Stage

The stage fulfills several essential responsibilities during development.

---

## Selecting Components on the Stage

All visible components on the stage are selectable.

When a developer clicks a component on the stage:

- The corresponding object is highlighted in the object tree
- The properties panel switches to the selected object
- The event panel context updates accordingly

This bidirectional synchronization allows developers to locate components efficiently, especially in deeply nested object hierarchies.

Using the stage to select components avoids the need to manually traverse complex object trees.

---

## Spatial Editing and Position Adjustment

The stage supports direct manipulation of component positions, depending on the layout environment.

---

### Relative Positioning Environment

In a relative positioning environment:

- Component positions are determined by layout rules
- Direct dragging is not available
- Position adjustments are performed through:
  - Margins
  - Layout-related properties

This approach ensures structural consistency in responsive layouts.

---

### Absolute Positioning Environment

In absolute positioning environments (such as H5 applications or canvas-based scenes):

- Components have explicit X and Y coordinates
- Components can be directly dragged on the stage
- Position changes are reflected immediately in the properties panel

This direct manipulation provides fine-grained spatial control and is especially useful for canvas and free-layout scenarios.

---

## Interaction Feedback and Real-Time Preview

The stage provides real-time feedback during development.

As developers modify:

- Component layout
- Properties
- Event logic

The stage updates immediately to reflect the changes.

This enables rapid iteration and reduces the need for separate preview or build steps.

---

## Relationship Between Stage and Object Tree

The stage and the object tree represent the same system from different perspectives:

- The stage focuses on **visual appearance and spatial relationships**
- The object tree focuses on **hierarchy, scope, and structure**

Actions performed in one are reflected in the other:

- Selecting on the stage highlights the object tree
- Reorganizing the object tree updates the stage rendering

This dual representation allows developers to manage both complexity and clarity.

---

## Role of the Stage in Component-Oriented Programming

VisualLogic is built on component-oriented programming principles.

The stage is a direct embodiment of this approach:

- Components are visually composed rather than written as code
- Layout and structure are explicit
- Relationships between components are visible

Through the stage, developers can design interfaces and behaviors without writing traditional frontend code.

---

## Supporting Logic Construction Through Visual Context

While logic is primarily defined in the event panel, the stage provides essential visual context:

- Developers can reason about logic based on component position and structure
- Interaction flows are easier to understand when tied to visual elements
- UI-driven logic becomes intuitive

This tight coupling between visual structure and logic definition improves correctness and maintainability.

---

## Stage as a Development Accelerator

Combined with other IDE panels, the stage supports rapid development cycles:

- Immediate visual feedback
- Precise component selection
- Efficient spatial adjustment
- Reduced cognitive load when managing complex interfaces

This enables developers to focus on application behavior rather than low-level rendering details.

---

## Summary

- The stage is the central visual workspace of the VisualLogic IDE
- It represents the real frontend structure of the application
- Components are selected, positioned, and inspected directly on the stage
- Relative and absolute positioning environments are supported
- The stage is synchronized with the object tree and properties panel
- It embodies VisualLogic’s component-oriented programming model
- Real-time feedback enables fast iteration and development