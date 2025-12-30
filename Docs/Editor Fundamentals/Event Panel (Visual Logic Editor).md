# Event Panel (Visual Logic Editor)

## 1. What the Event Panel Is

The **Event Panel** is the **primary logic editor** in VisualLogic.

It is designed to edit **interaction logic** and **service logic** in a visual, structured way:

- **Frontend event logic** (UI interactions, state updates, navigation, UI updates)
- **Backend service logic** (service workflows, database operations, backend pipelines)

The Event Panel is **Turing-complete**: it can express complex control flow including:

- sequential execution
- branching (conditions)
- loops (multiple loop types)
- data flow via inputs/outputs between actions

### Why VisualLogic Uses a Visual Logic Editor

VisualLogic projects exist in **two equivalent representations**:

- **VL Code in AI Files**: formal, compilable source
- **Visual editing in the IDE**: human-friendly structure + logic editing

Event definitions and logic exist in **VL code**, but are **converted into the Event Panel** where they become:

- easier to read
- easier to refactor
- safer to edit (structure and ordering are explicit)

In typical workflow, developers **edit logic in Event Panel**, and the IDE keeps VL code consistent.

------

## 2. Core Model: Event → Actions

An **interaction logic** consists of two parts:

1. **Trigger Event** (what happened)
2. **Response Actions** (what to do)

Example:

- Trigger: “Button is clicked”
- Response: “Image toggles visibility”

Here:

- “button clicked” = event source + event type
- “toggle image visibility” = action on a target object

### Thinking in Interaction Logic

Any application can be viewed as a collection of “event → actions” chains:

- E-commerce app: search / view / add-to-cart / checkout → update UI + call services + write data
- Chat app: input / send / switch contact → update state + request data + refresh view

This is why configuring interactions between components can implement arbitrarily complex applications.

------

## 3. Events and Actions Are Component-Specific

Each component type provides **predefined events** and **predefined actions (methods)**.

Examples:

- Button: click, long-press… (events)
- Variable: value changed… (events)
- Image: toggle show/hide… (actions)

Important implications:

- **Different component types expose different event lists**
- **Different component types expose different action lists**
- A “valid” interaction always means:
   **choose an event supported by the trigger component** + **choose an action supported by the target component**

This is one of the biggest reasons to prefer Event Panel over raw code: the editor only offers valid options in context.

------

## 4. Event Panel Entry: Creating an Event from Object Tree

A typical flow starts in the **Object Tree**:

### Step 1 — Select the trigger component in Object Tree

Pick the component that should “emit” the trigger, such as a Button.

### Step 2 — Add an event to that component

Use the logic tool area (the **event icon / exclamation mark**) to add an event.

After adding:

- the new event is automatically selected
- the **Event Panel** opens showing this event

This establishes a key model:

> **Object Tree defines which object owns the event.**
>  **Event Panel edits the event’s logic.**

------

## 5. Building a Basic Interaction (the Minimal Working Chain)

A minimal interaction includes four decisions:

1. **Trigger object** (already chosen in Object Tree)
2. **Trigger event type**
3. **Target object**
4. **Target action**

### Step 1 — Choose the trigger event type

In Event Panel, use the Trigger Event dropdown and select (e.g.) `click`.

### Step 2 — Choose the target object

Click the “select object” icon to enter Object Tree selection mode, then select the target component (e.g. an image).

This is important: target selection is not just picking a name—
 it binds the action to a specific node in the Object Tree.

### Step 3 — Choose the target action (method)

Pick the action from the action dropdown (e.g. “toggle visible”).

At this point you have a complete chain:

> Trigger event happens → Target action runs

------

## 6. Multiple Actions: Building Real Logic Chains

In real apps, one event usually triggers **multiple actions**.

### 6.1 How to add more actions

You can insert additional action blocks by:

- hovering the `+` beneath an action and clicking an “Action” option
- using toolbar buttons
- using right-click menu

### 6.2 Execution order and the “synchronous” rule

When multiple action blocks exist under one event:

- actions execute **top → bottom**
- an action must **finish** before next action begins
- if an action takes time (API request, delay), the chain waits for it

So the rule is:

> In the new Event Panel, action execution is **synchronous by default**.

### 6.3 Why this matters

This synchronous semantics is what enables the next sections:

- return values are always available
- “action output” can be used by later actions reliably
- you can reason about logic as a deterministic sequence

### 6.4 When you need async behavior

If you need asynchronous behavior, you should use:

- frontend triggers (to decouple timing)
- backend message queue patterns (to decouple processing)

(You already提到这两条，我这里保持原意，不扩写成“系统一定有某功能”，只解释为何需要。)

------

## 7. Action Inputs (Parameters): How Actions Receive Data

Most actions require **input parameters**.

Examples:

- paging action needs a target page index
- open external link needs a URL + open mode
- assign text needs a value expression
- database output needs filters / conditions

### 7.1 Parameter UI is action-specific

Different actions show different parameter editors:

- simple list-style parameters (URL + toggle option)
- structured editors for complex inputs (filters, query conditions)

This is not cosmetic—this is part of the “type-safe visual logic” design.

### 7.2 Formula Editor: the universal expression tool

Most parameter inputs use a universal expression tool called the **Formula Editor**.

The Formula Editor is not a plain text box. It is the standard way to define:

- constants (`"hello"`, `80`, `true`)
- variables / state
- object property references
- expressions and computed values
- action return values (outputs) from previous blocks

You should treat it as a “typed expression builder”.

> If the Formula Editor is central to your workflow, it deserves its own document (you already有计划写 “公式编辑器”，我建议独立成篇)。

### 7.3 Tooltips / parameter help

For complex actions and parameters:

- hovering the `?` icon provides description of the action/parameter

This becomes important especially for database and service operations, where parameter structure is not obvious.

------

## 8. Action Outputs (Return Values): Passing Data Downstream

Some actions return results, such as:

- API calls returning response objects
- database queries returning arrays/rows
- backend service calls returning structured objects

These returned values are called:

- return parameters / outputs

### 8.1 How to reference outputs

In later blocks, open the Formula Editor and select from **Return Results** category.

Rules:

- you can only reference outputs from blocks **above** the current position
- outputs are available because execution is synchronous

### 8.2 Common pattern: “request → transform → use”

A typical pipeline pattern looks like:

1. call service / API
2. take returned data
3. bind into UI / store / another request

Event Panel is designed to make this pipeline readable and maintainable.

------

## 9. Conditional Execution: Condition Blocks

Condition blocks allow branching behavior.

### 9.1 Adding a condition block

Two common methods:

- select an event or block, then click toolbar **Condition** button (adds at the end)
- hover the `+` under a block and choose **Condition** (inserts at that position)

When added, a condition block comes with:

- **Case 1** (a branch)

### 9.2 Branch evaluation semantics (very important)

In the new panel version:

- branches are evaluated top → bottom
- **only the first matching branch executes**
- if no branch matches, execution goes to the **Else** branch (if created)

This is closer to an `if / else if / else` chain than “evaluate all”.

### 9.3 Operators and expressions

Each branch condition uses:

- left expression (Formula Editor)
- operator
- right expression (Formula Editor)

Operators include:

- equals, greater-than, contains, in, type checks…

### 9.4 Multiple conditions within a branch

Within a single branch you can add multiple conditions.

Default relationship:

- **AND** (all must be satisfied)

You can switch to:

- **OR** (any satisfied enters branch)

### 9.5 Nested conditions

Condition blocks can be nested:

- branch inside branch
- multiple-condition nesting (a nested block treated as a grouped expression)

Mental model:

> A condition block is like parentheses in code: it groups logic into a single boolean result.

------

## 10. Loops: Repeating Actions

Loops exist because manually duplicating actions is not scalable and sometimes impossible (unknown iteration count).

### 10.1 Adding loops

Use the toolbar **Loop** button and place actions inside the loop body.

Loop type can be selected from a dropdown.

### 10.2 Loop types

VisualLogic provides multiple loop types:

1. **Count loop**
   - runs a fixed number of times
   - provides loop index (0…count-1) in Formula Editor
2. **Conditional loop**
   - runs while condition is true
   - requires careful design to avoid infinite loops
3. **Array loop**
   - iterates through an array
   - provides current item reference in Formula Editor
   - supports primitive arrays and object arrays

### 10.3 Breaking out of a loop (break)

Inside loop body:

- choose an action targeting “current loop”
- select “terminate loop”

When break executes:

- loop ends immediately
- subsequent blocks after the loop do not execute within that loop iteration

Typically used together with conditions.

### 10.4 Infinite loop warning (critical)

In conditional loops:

- if you never update the condition variable inside the loop,
- the condition may remain true forever,
- causing lock-up / freezing / event chain never completing.

Recommended practice:

- always update condition variables inside the loop body
- or include a safety count / timeout strategy where applicable

------

## 11. Execution Model Summary (Deterministic and Composable)

Within a single event:

- blocks execute sequentially
- execution is synchronous
- outputs from earlier blocks are available to later blocks
- control flow blocks (conditions / loops) partition the chain deterministically

This is what makes Event Panel both:

- human-readable
- capable of complex logic
- safe for AI generation + human refinement

------

## 12. Automatic Log Tracing (Debugging)

Event Panel supports automatic tracing for both frontend and backend.

### 12.1 Enabling logging

On the right side of the Event Panel:

- click the **log icon** to enable tracing for selected event blocks

### 12.2 Frontend log tracing

After enabling:

- open browser DevTools console
- you can view logs including:
  - component name (name)
  - action inputs (input)
  - action outputs (output)

This is especially useful for debugging:

- input expression values
- service call parameters
- unexpected UI state

### 12.3 Backend log tracing

Similarly for backend:

- enable log icon on backend logic blocks
- check backend logs for additional info

This is useful when debugging:

- service execution
- database operations
- unexpected return structures

------

## 13. Relationship to AI Files, Object Tree, Stage

### 13.1 AI Files ↔ Event Panel

- AI Files contains VL code definitions of events and logic
- Event Panel transforms them into visual blocks for easier reading/editing
- edits in Event Panel are serialized back into VL code (equivalent representation)

### 13.2 Object Tree ↔ Event Panel

- events belong to specific objects (frontend components or backend service objects)
- object selection defines event context
- logic is always anchored to object identity

### 13.3 Stage ↔ Event Panel

- Stage is preview only: it shows runtime effect of logic
- interaction on Stage triggers events; Event Panel defines what happens afterward

------

## 14. Best Practices for Large Projects

### 14.1 Prefer “business logic in Sections / services”, not scattered UI nodes

Even though any component can have events, maintainability improves when:

- UI components emit events
- Sections orchestrate business logic
- backend services encapsulate data operations

### 14.2 Use return values instead of duplicated requests

If an API call returns data:

- reuse outputs downstream
- avoid calling the same API multiple times

### 14.3 Keep condition branches mutually exclusive where possible

Because only the first matching branch executes:

- design branch conditions so intent is clear
- put most-specific conditions above broader ones

### 14.4 Always guard loops

For conditional loops:

- ensure exit condition changes
- consider adding a max-iteration strategy in design

### 14.5 Use logging strategically

Enable logging on:

- service calls
- complex parameter expressions
- blocks whose outputs feed many downstream blocks

------

## 15. Summary

The Event Panel is the **logic engine editor** of VisualLogic:

- Visual, Turing-complete logic editing
- Frontend + backend logic in one unified paradigm
- Deterministic synchronous execution
- Strong support for data flow via action outputs
- Condition and loop blocks for complex control flow
- Automatic log tracing for debugging
- Bidirectional mapping with VL code in AI Files