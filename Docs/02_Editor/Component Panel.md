# Component Panel

## Overview

The **Component Panel** is located on the **far left side of the VisualLogic IDE**.
 It displays a vertical list of component icons, where each icon represents a **distinct component type** that can be used to build applications.

Components are the **fundamental building blocks** of VisualLogic.
 All user interfaces, interactions, animations, and data logic are constructed by **adding, combining, and configuring components**.

Examples of components include:

- Layout components for page structure (rows, panels, containers)
- Media components such as images, video, audio, and text
- Interaction components such as buttons and input fields
- Animation and effect components
- Data- and logic-related components (depending on frontend or backend context)

Every visible element and logical unit in a VisualLogic application ultimately originates from the Component Panel.

------

## Components as the Core of VisualLogic

VisualLogic follows a **component-based development model**.

This means:

- UI structure is composed of layout and visual components
- User interaction is handled through interactive components and events
- Data flow and logic are expressed through components combined with event logic
- Complex behavior emerges from **component composition**, not manual coding

Some components require **external resources** to function:

- **Image components** require an image asset
- **Audio components** require an audio file
- **Video components** require a video source

Understanding what each component does, what data it accepts, and how it interacts with other components is essential for effective development.
 Detailed usage and configuration of each component are documented in the **Components** section of this documentation.

------

## Component Panel Organization

The Component Panel is **grouped by functionality**, allowing developers to quickly locate the components they need.

Typical categories include:

- **System Components** – foundational components provided by the platform
- **Layout Components** – structure, containers, and positioning
- **Media Components** – image, video, audio, text
- **Data Components** – variables, data binding helpers, data display components
- **Communication Components** – frontend-backend interaction related components
- **Logic-related Components** – components that support conditional logic, loops, or triggers (context-dependent)

The exact set of available components may vary depending on:

- Whether you are editing a **Frontend App** or the **Backend App**
- The current application type and runtime environment
- The layout and positioning mode being used

Frontend and Backend applications expose **different component sets**, reflecting their different responsibilities.

------

## Panel Display Modes

The Component Panel supports two display modes:

### Compact Mode

- Shows only component icons
- Maximizes workspace for the Stage and editor panels
- Suitable for experienced users who recognize components visually

### Full Mode

- Displays icons with expanded grouping and labels
- Easier for discovering and browsing components
- Recommended for new users or when exploring unfamiliar components

You can switch between **Compact** and **Full** mode using the **Collapse / Expand** button at the top-left of the panel.

If the panel content exceeds the vertical height of your screen, you can **scroll using the mouse wheel** while hovering over the Component Panel.

------

## Context-Aware Component Availability

VisualLogic automatically adjusts available components based on context:

- **Frontend Apps** focus on UI, interaction, and client-side logic components
- **Backend App** focuses on data, services, and backend logic components

This design prevents invalid usage (for example, adding UI-only components to backend logic) and keeps the component list relevant to the current task.

For detailed distinctions, refer to:

- *Frontend Components*
- *Backend Components*

in the **Components** documentation section.

------

## Component Documentation Access

VisualLogic provides **inline documentation access** directly from the Component Panel.

By **hovering over any component icon**, you can quickly view:

- The component’s name
- Its basic purpose
- A shortcut to detailed documentation

This allows you to explore components without leaving the editor or searching manually through documentation.

------

## How Components Are Used in Practice

A typical workflow involving the Component Panel looks like this:

1. Select a component from the Component Panel
2. Place it onto the Stage or into the Object Tree
3. Configure its properties in the Properties Panel
4. Define its behavior using the Event Panel
5. Bind it to data or expressions using the Formula Editor when needed

Through this workflow, VisualLogic enables building **complex, full-stack applications without writing traditional code**, while still allowing seamless mapping to VL code when needed.

------

## Summary

The Component Panel is the **entry point for building everything** in VisualLogic.

It provides:

- Access to all available frontend and backend components
- Structured, categorized component discovery
- Context-aware component filtering
- Tight integration with the Stage, Object Tree, Properties Panel, and Event Panel

Mastering the Component Panel is the foundation of efficient and scalable VisualLogic development.