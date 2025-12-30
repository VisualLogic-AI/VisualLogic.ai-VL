# Formula Editor

## Overview

The Formula Editor is VisualLogic’s unified expression input system. You will encounter it anywhere the IDE needs a value or an expression—whether you are configuring UI properties or building complex logic in the Event Panel.

It is more than a text box. The Formula Editor is an expression builder that supports:

- Literal values (string, number, boolean, null)
- References to objects and variables from the Object Tree
- Visual preset blocks for common structures and patterns
- Math expressions and built-in math functions
- Deep JSON/array access through type-aware menus
- Tool functions with chained calls using `.`

## Where You Use the Formula Editor

The Formula Editor appears in both the **Properties Panel** and the **Event Panel**.

### Properties Panel usage

Typical uses include:

- Setting a property value (text content, color, visibility, width/height)
- Binding a property to dynamic data (another component’s value, loop data, computed values)
- Styling mappings for lists and repeated UI (selected state, conditional colors, etc.)

In the Properties Panel, expressions are primarily **declarative**: the UI property evaluates to the expression result whenever the relevant data changes.

### Event Panel usage

Typical uses include:

- Action parameter fields in an event chain
- Complex action parameter areas such as database output filters, pagination, and field selection
- Condition block inputs and comparisons
- Loop configuration (loop count, target array, loop conditions)
- Referencing outputs from earlier action blocks

In the Event Panel, expressions are primarily **execution-context aware**: they evaluate at runtime as the event logic executes, and can access action outputs and event parameters.

## Two synchronized representations

Formula expressions are represented in two equivalent forms across the IDE:

- **Visual form** in the Formula Editor
- **Code form** in VL Code

Anything you build in the Formula Editor has a corresponding representation in VL Code, and VL Code expressions can be transformed back into the visual Formula Editor structure. This is consistent with VisualLogic’s overall design:

- AI Files provide a code-level definition
- Visual editors provide a human-friendly editing surface
- The IDE keeps both representations semantically equivalent

This enables “code mode” and “visual mode” to remain interchangeable for expressions, just like events and logic.

## Core input methods

You can build a formula in four main ways:

1. Entering literal values directly
2. Referencing objects, properties, and variables from the Object Tree
3. Using preset input blocks
4. Using math expressions and tool functions

The Formula Editor supports unlimited nesting: any parameter slot inside a block can itself be another formula.

## Literal values

Literal values are typed values entered directly.

### Strings

If you enter plain text such as `hello` and the input loses focus, the editor automatically wraps it in quotes to mark it as a string:

- Input: `hello`
- After blur: `"hello"`

### Numbers

Numbers remain unchanged after blur:

- `80`
- `3.14`

### Booleans and null

Supported special literals:

- `true`
- `false`
- `null`

Different literal types are visually distinguished with different colors to reduce mistakes.

## Referencing Object Tree items

Referencing is one of the most powerful capabilities of the Formula Editor: you can point directly to entities defined in the Object Tree.

### Referencing an object property

You can reference a component’s property to drive another value.

Example: bind a text component’s content to an input component’s current value. The result is a live binding:

- user input changes
- the bound text updates automatically

This works in both the Properties Panel binding field and the Event Panel formula fields, as long as the reference is meaningful in that context.

### Referencing variable computed methods

Variables provide type-specific computed methods that transform data:

- string methods such as substring and replace
- array methods such as length, sorting, filtering, mapping
- object-array methods for data shaping

These are “computed functions” because they derive a new value from the variable’s current value using a defined rule.

### Referencing object methods

In the Event Panel, you can reference object methods, such as:

- input widgets exposing “current character”
- containers exposing “current scroll distance”
- action groups or functions exposing output parameters

This often appears when you call a group/function and then use its output as another action’s input.

Important rule:

- Object methods are meaningful only in the Event Panel.
- They should not be used in Properties Panel bindings, because bindings do not define when a method should be invoked.

## The default dropdown menu

When you focus a Formula Editor field, a default dropdown appears. It contains both:

- context-aware reference sources
- preset input blocks

The available sections depend on where the editor is used.

### Return results

In the Event Panel, you can reference outputs of action blocks executed earlier in the same event chain, such as:

- API responses
- database operation outputs

Because action execution is synchronous and sequential in the Event Panel, outputs from earlier blocks are always available to later blocks.

### Receive parameters and event parameters

In backend services, action groups, or transactions, you can reference input parameters passed into the current unit, plus system-provided parameters.

Examples include:

- submitted payload data
- visiting user information when user context is enabled
- request information such as headers and IP

In frontend event logic, if a trigger event exposes additional parameters, you can reference them here as well (for example click coordinates).

### System parameters

System parameters expose environment information such as:

- system time
- environment variables if configured
- current application context

Frontend and backend environments provide different system parameters because they run in different contexts.

### Loop variables

In the Event Panel, when a formula is inside a loop block, loop variables become available:

- count loop: current index starting from 0
- array loop: current element, including field selection for object arrays
- conditional loop: no loop variable is provided

### Loop objects

In the Properties Panel, when a component is inside a repeating container such as a for-loop container, table container, or tree container, binding fields can access loop objects such as:

- current index
- current data item

This is fundamental for list rendering and item-level styling.

## Preset input blocks

Preset blocks provide visual tools for entering complex values without writing raw syntax. They are available from the default dropdown.

### Select from Object Tree

This activates Object Tree selection and inserts a reference to the chosen object or property.

### Color block

A color is ultimately a string such as `"#FF1100"`. The color block provides a visual picker that generates that string value.

### Array, object, and vector blocks

These blocks construct structured JSON values:

- arrays like `[1, 2, 3]`
- objects like `{ key: value }`

Each element slot is itself a Formula Editor, enabling deep nesting:

- arrays of arrays
- arrays mixing numbers, strings, and objects
- objects whose fields are computed expressions

This is commonly used for:

- API request bodies
- database filter definitions
- complex component parameters

### String concatenation block

This block concatenates multiple parts into a single string.

Important rule:

- `+` is reserved for numeric math.
- For strings, always use the string concatenation block to avoid ambiguity.

### Value mapping block

Value mapping maps a single input expression to different output values by case.

Example:

- if status is 1 → “Success”
- if status is 2 → “Failure”
- otherwise → “Unknown”

A common UI use is style mapping in property bindings, such as changing background colors based on whether an item is selected.

### Conditional value block

Conditional value is a more flexible mapping system where each case can contain multiple conditions, evaluated with a consistent logical relationship.

Each case can contain multiple conditions, combined using either:

- AND
- OR

A single case cannot mix both relationships.

### Condition judgment block

This block returns a boolean result from a condition expression. It is commonly used alongside array search, filtering, and mapping.

### Null coalescing block

This block is a simplified conditional expression:

- if the first expression is not null or empty, return it
- otherwise return the fallback expression

It is widely used for default values and UI fallback display.

### Math functions block

Math functions provide built-in functions for numeric processing. You can select a function from the menu and fill parameters visually.

Example use: applying `floor` to 1.75.

### Custom expression block

Custom expressions allow inserting code-style expressions for advanced cases that cannot be expressed cleanly with preset blocks.

Use this sparingly. Prefer preset blocks and tool functions whenever possible.

## Math expressions

The Formula Editor supports composing numeric expressions using:

- parentheses
- `+`, `-`, `*`, `/`
- `%` modulo
- commas for function argument separation

Key constraint:

- All operands in a math expression must be numeric.
- If an operand is not numeric, the editor will prevent adding math operators.

For string concatenation, use the string concatenation block.

## Any-type extra menu for deep data access

Real-world data often involves nested JSON structures. VisualLogic provides a type-aware extra menu for deep access and transformation.

### How it works

1. Select a variable or expression first
2. Click the same Formula Editor field again
3. The any-type extra menu appears
4. Choose a target data type category such as object, array, object array, string, number
5. Select a method or access pattern
6. Provide keys or indices in the provided parameter fields

You can repeat this process multiple times to access deeper levels of the structure.

This replaces free-form code access like `obj.list[0].change` with a structured, type-aware selection flow.

## Tool functions and chained calls

Tool functions provide a large library of data processing utilities.

### Typical use cases

- data cleaning and transformation
- string and text processing
- date and time operations
- array and JSON manipulation

### Calling tool functions with `.`

You can type `.` after a value to bring up tool function suggestions, then select a function and fill parameters visually.

You can chain multiple calls:

- call a function
- type `.` again on its result
- call another function

This enables complex pipelines, for example:

- parse a string into JSON
- take the first array element
- access a specific field
- format the result to two decimals

### Smart search

After typing `.`, you can type part of a function name (for example `toFixed`) and select from auto-matched results.

### Visual parameter filling

Function calls are configured visually: each parameter is a Formula Editor field, so parameters can themselves be references or computed expressions.

### Function categories

Tool functions are organized into categories covering common needs, such as:

- type checks and conversion
- math operations
- array operations
- string operations
- boolean operations
- date and time
- regular expressions
- JSON processing
- encoding and decoding
- generators
- global functions

## Nesting and composition

The Formula Editor can be nested without practical limit:

- an action parameter can be an object block
- that object can contain arrays
- each element can contain tool function calls
- each tool function parameter can itself be a formula

This composability is what makes the Formula Editor suitable for both simple bindings and complex full-stack data pipelines.

## Key rules and common pitfalls

- Do not use object methods in Properties Panel bindings. Method calls need an execution moment, which bindings do not provide.
- Do not use `+` for string concatenation. Use the string concatenation block.
- Numeric operators require numeric operands. If you need conversion, use a tool function first.
- For deeply nested JSON, prefer stepwise access using the any-type menu and tool functions, instead of trying to build one dense expression.

## Summary

The Formula Editor is VisualLogic’s unified expression system:

- it appears in both Properties Panel and Event Panel
- it supports literals, references, preset blocks, math, deep data access, and tool functions
- it supports chained processing using `.`
- it can be nested arbitrarily for complex structured data
- it maintains a synchronized code representation in VL Code, enabling interchange between code and visual editing

This makes expressions in VisualLogic fully compatible with the platform’s dual representation philosophy: **code and visual editors are interchangeable views of the same project semantics.**