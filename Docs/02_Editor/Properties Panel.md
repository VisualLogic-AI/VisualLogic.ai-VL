# Properties Panel

## Property Overview

In VisualLogic, every time you add an object, it appears both on the **Stage** and in the **Object Tree**. Click the object to select it, and the **Properties Panel** will appear on the right side of the editor, showing all configurable properties for that object.  

By redefining properties, you can adjust an object’s **size**, **position**, **appearance**, **styles**, **data content**, and **behavior**, enabling fine-grained control over how it looks and works.  

------

## How Properties Are Defined

Different property types use different UI controls. Common patterns include:

### Content Field Input

For **quantitative** properties (e.g., width/height), you can enter a fixed value or bind a variable/expression in the content field. In many numeric fields, you can also fine-tune values with the control handle on the right.  

### Toggle Switch

For **boolean** properties (on/off), the panel uses a toggle switch (e.g., visibility, text wrapping). Orange indicates enabled; gray indicates disabled.  

### Slider

For **continuous** values (e.g., opacity, volume), the panel provides a slider. You can drag it or type a numeric value directly.  

### Dropdown Menu

For **enumerated** options (e.g., border type: solid/dashed/dotted), select from a dropdown list.  

------

## Tabs in the Properties Panel

For visual components such as an **Image**, the Properties Panel typically contains three tabs:

- **Basic**: Common essentials like width, height, position, visibility. All objects have this tab.  
- **Style**: Advanced visual styling such as shadow, blur, borders. Most objects include style properties and sections can usually be collapsed/expanded.  
- **Custom Style**: Add custom key-value styles (CSS-like syntax). Empty by default and filled by developers as needed. All objects have this tab.  

------

## Property Categories

In VisualLogic, object properties can be grouped into two major categories: **Basic Properties** and **Style Properties**.  

### Basic Properties

#### Dimensions

- **Relative positioning objects**:
   Width/Height default to **Original Width/Height**, meaning the system uses the asset’s native pixel size. You can set:
  - **Fixed pixels** (e.g., `20`, `30`), or
  - **Percentages** relative to the parent (e.g., `20%`, `30%`).
     Switching between `px` and `%` is done by typing the unit once; the panel updates accordingly.  
- **Absolute positioning objects**:
   Width/Height are typically fixed pixel values and do not allow percentage-based sizing.  

#### Visible

A toggle controlling whether the object is shown on the stage. Turning it off hides the object but does not delete it; it can be shown again via interaction logic. In relative layout, visibility changes can affect layout flow.  

#### Opacity

Controls how transparent the object is. Default is `100%`. Setting opacity to `0` makes it fully transparent, but it may still be “visible” in the sense that the visibility toggle remains on.  

#### Margin

A property commonly available in **relative layout**, defining spacing around the object (top/bottom/left/right). It can typically be expressed in pixels or percentages.  

#### Background Color

Sets the object’s background color via a color picker or direct color code entry. For non-transparent images, this may be hidden behind the asset.  

#### Material Resource Address

Defines the image source. If uploaded locally, VisualLogic stores it and generates a URL; you can also provide a web URL. Clicking the field typically enables re-upload or editing.  

------

### Style Properties

Beyond basic properties, many objects provide richer visual styling:

#### Rotation

Rotation is determined by:

- **Origin X / Origin Y**: The pivot point used for rotation (and, in absolute layout, also relates to positioning reference).
- **Angle**: Default `0°`. Positive values rotate clockwise; negative values rotate counterclockwise.  

#### Border Style

Borders are defined by:

- **Border Radius** (px)
- **Border Width** (px)
- **Border Type** (none/solid/dashed/dotted)
- **Border Color**  

#### Shadow

Shadow is defined by:

- **Offset X**, **Offset Y**
- **Blur**
- **Color**
   If both offsets are `0`, the shadow overlaps the object and may appear invisible depending on blur/color.  

#### Effects and Filters

Common filter-style controls include:

- **Blur**
- **Brightness / Contrast / Saturation / Grayscale**
- **Invert**
- **Hue Rotate**  

Custom Style is an advanced feature and is documented separately.  

------

## Property Data Binding

VisualLogic supports binding an object property to a **variable**. When the variable changes, the property updates immediately. This is typically a **one-way** data flow: property updates do not automatically write back to the variable.  

This lets you shift from “manually adjusting many UI objects” to “driving UI from data structures,” improving reuse and maintainability—especially when dealing with lists, uncertain counts, or large groups of elements.  

### Example: Binding a Slider’s Default Value to a Variable

1. Add a **Slider**, an **Input**, and a **Numeric Variable**.
2. Select the slider, and bind its **Default Value** to the variable using the binding mode button in the property field.
3. On the input’s blur event, assign the input value to that variable.
    Now the input controls the slider value through the variable.  

### Q&A

**Q: Why can’t I drag the slider after binding it to a variable?**
 A: Because the variable drives the slider value one-way, and the slider can’t write back to the variable. To achieve two-way behavior, add a reverse assignment: when the slider changes, assign its value back into the variable.  

------

## Formula Editor and Code Mode Equivalence

In VisualLogic, any field that accepts values (property values, action parameters, condition operands, etc.) uses the **Formula Editor**. It supports:

- Direct literals (numbers, strings, colors)
- Referencing object properties or variables
- Basic operators and functions (math, string, array operations)
- Preset blocks (in newer versions) for conditions, mappings, color blocks, and more  

### Visual Blocks and VS Code Expressions

The content you build in the Formula Editor has a **corresponding textual expression** in code mode (VS Code / IDE code view). This means the IDE supports **two-way conversion** between:

- **Visual editing mode** (Formula Editor blocks, pickers, bindings), and
- **Code mode** (text expressions),

consistent with how event logic can be represented both visually and as code.  

Practical mapping examples (conceptual):

- **Literal** in Formula Editor → the same literal in code mode
  - `20`, `30%`, `"#FFAA00"`
- **Reference** (select a variable/object property via UI) → a textual reference path in code mode
- **Expression** (e.g., arithmetic) → the same operator expression in code mode
  - `a + 1`, `Math.floor(x / y)`

This design allows teams to work in the style they prefer while keeping the project consistent and convertible across both modes.

------

## Summary

The Properties Panel is the primary way to define how objects look and behave in VisualLogic. By combining structured property controls (inputs, toggles, sliders, dropdowns) with data binding and a Formula Editor that maps to code-mode expressions, you can build UI and logic that is both fast to edit visually and cleanly represented as code when needed.  