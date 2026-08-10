# VL Workflow Specification 4.16

> **Public specification release**: 4.16
> **Executable workflow schema**: 4.1 (comprehensive)
> **SysDoc source version**: 4.1.6
> **Engine release**: `VL-Workflow-Engine 4.16.2`
> **Event schema**: `vl.workflow.run-event.v4`
>
> Workflow JSON must continue to use `"version": "4.1"`. The current engine rejects `"version": "4.16"`; the public 4.16 release unifies documentation numbering without changing executable graph semantics.
>
> This is the consolidated comprehensive specification, merged from:
> - Spec 3.19 (comprehensive base)
> - Spec 4.0 (incremental: tools, toolScope, GraphPatch, Swarm, ResultEnvelope, Actor, Review, child-run)
> - Spec 4.1 (incremental: Swarm deprecation, Loop parallel branch control, Checkpoint v3)
>
> **Compatibility rules**: Spec upgrades precede all runtime upgrades. As long as a workflow does not use higher-version-exclusive features, existing workflows should continue to run. Runtimes should prefer capability-based detection over version string rejection.
>
> Use these companion docs for current host/runtime contract details:
>
> - `docs/workflow-capability-matrix.md`
> - `docs/workflow-event-schema.md`
> - `docs/workflow-execution-contracts.md`
> - `docs/workflow-runtime-extension-profiles-v4.md`
> - `docs/workflow-actor-profile-v1.md`

## 0.3 SysDoc and Resource Center Document Resolution Priority

For `registry.docs`, step `in.docs`, and any workflow-internal document references, the runtime must resolve in the following order:

1. Explicitly configured `Doc ID` in current project or IDE settings
2. Platform built-in official default `Doc ID`
3. SysDoc and Resource Center directory `path -> latest docId` fallback resolution

Constraints:

* Values like `"1"`, `"2"`, `"141"` in workflow JSON are semantically **reserved path slots**
* When actually reading document content, the runtime must resolve to a concrete `Doc ID` first
* When a path has an explicitly configured `Doc ID`, the runtime must not override with local old docs or path fallback
* UI may display `vl://doc/<id>` or `/doc-center.html?docId=<id>` as auxiliary references, but runtime configuration serialization must still use numeric `Doc ID`

---
# Chapter 1: Workflow Overview

## 1.1 Purpose and Goals

Workflow is a **general-purpose process orchestration mechanism** in the VL platform, used to describe and execute multi-step processes in a structured, executable manner. It can be used both during development for process constraints and as part of application runtime to provide capabilities externally.

Workflow is a **programmatically defined process** that can be triggered by frontend, backend services, or tool environments, and depending on the use case, manifests in different hosting forms and lifecycles.

In the VL platform, Workflow primarily covers four use cases:

| Scenario | Hosting Location | User | Is Project Asset | Participates in Compilation | Invocation Method | Primary Purpose |
| --- | --- | --- | --- | --- | --- | --- |
| IDE agent flow (no service calls) | Process/ | Developer | Yes | No | IDE internal | Constrain IDE development processes, support project-context-based Agent execution |
| Business workflow (can call services) | Workflows/ | App user | Yes | Yes | Frontend / service call | Orchestrate backend processes, provide API or automation services |
| Approval flow (can call services) | Workflows/ | App user | Yes | Yes | Frontend / service call | Support long-running processes with pause/resume and human intervention |
| Local personal workflow | LocalWorkflows/ (suggested) | Developer | No | No | Local tool call | Personal automation tools |

### IDE Scenario Hard Constraints (3.9+)

When workflow is used for IDE agent flow (typical path `Process/`):

* `steps` must not contain `Service_*` nodes; presence triggers compile/load error
* `registry.services` must be an empty array (or omitted if implementation allows)
* This constraint only applies to IDE workflows; business workflows (e.g., `Workflows/`) are not restricted

**Overall design goals:**

* Cover **development, business, and automation** scenarios with a unified orchestration model
* Clarify Workflow's **lifecycle and responsibility boundaries** through hosting location and compilation participation
* Support both frontend-triggered and service-internal invocation
* Distinguish "developer-internal processes" from "externally-provided application capabilities"
* Maintain structured, executable, and long-term maintainable process definitions

## 1.2 Workflow State

Workflow follows the **"stateless execution, externalized state"** principle.

From a definition perspective, Workflow itself contains no persistent state and does not describe how state is stored or evolves. The same Workflow definition can be executed multiple times in parallel without semantic differences caused by history.

At runtime, Workflow **can optionally mount an external state space** for execution context or artifacts such as files, intermediate results, or progress information.

State space characteristics:

* Mounting is optional
* State space is specified through runtime parameters
* State space is not part of the Workflow definition
* Different run instances can mount different state spaces

Without a mounted state space, Workflow still executes normally, depending only on runtime parameters and immediate computation results. When mounted, Workflow can read/write data in the state space while maintaining the execution engine's stateless nature.

## 1.3 Multi-step Output

Workflow supports **multi-step output** to expose intermediate progress, staged results, or execution events.

Core goal: **Allow external systems to perceive and respond to Workflow execution without blocking the process.**

Characteristics:

* Output is produced as an **event stream**, not a single final result
* Each output corresponds to a stage or node in the execution process
* For serial nodes, output order matches actual execution order
* For parallel nodes (`children` branches or `Loop_* mode:"parallel"`), output event order depends on actual completion time; no deterministic order guarantee

Multi-step output consumption is not limited to frontend scenarios:

* **Frontend**: Push via streaming interface for real-time progress display
* **Backend**: Send to message queue (MQ) for async processing

## 1.4 Control Flow Model

Control flow consists of two parts:

### A. Node Properties

For describing "how nodes connect, concur, and whether they execute":

* **`next`**: Serial successor node (explicit continuation signal)
* **`children`**: Parallel sub-branch entry list (parallel fan-out + join)
* **`if`**: Node execution condition (when false, skip node body and its children subtree)

Rules:

* **`next` must be explicitly written, supports special semantics like "RETURN" and "BREAK"**
* **Except `Stop_*` nodes, all nodes must have `next`, otherwise spec compilation error**
* **When `if` is false:**
  * Do not execute the node's actual action (no service/llm/component call, no `out` write)
  * **Also skip all `children` subtree of this node**
  * Proceed directly to `next`

Note:

* `next / children / if` are **universal properties** available on all nodes
* `next / children` express control flow exits on any node, not data ports

### B. Control Node Types

For expressing structured control semantics:

* `branch`: Conditional branching (selectively execute one branch)
* `loop`: Loop (repeat steps over collection data or conditions; supports array traversal mode and while condition mode)

### C. Entry and Termination

No explicit "start node". Entry rules and termination semantics:

#### Entry Nodes

**Entry node = nodes in `steps` not referenced by any other node's `next`, `children`, or `cases`.**

* Engine automatically identifies all entry nodes at startup
* Single entry node: workflow starts from that node
* Multiple entry nodes: treated as **parallel starting points** (implicit virtual `children` fan-out)
* Zero entry nodes: illegal workflow definition (cycle or structural error)

#### Termination, Pause, and Failure Semantics

Three **mutually exclusive** end states:

| End Type | Trigger | Status | Recoverable | Caller Meaning |
| --- | --- | --- | --- | --- |
| Completed (stopped) | Reaches `Stop_*` node | `stopped` | No | Process completed normally |
| Paused | Reaches `Pause_*` node | `paused` | Yes | Process awaiting external input |
| Failed | Node execution error | `failed` | No (needs rerun) | Process abnormally interrupted |

Notes:

* `Stop_*` represents **normal completion**; once reached, execution terminates and is not recoverable
* `Pause_*` represents **recoverable execution suspension**, not equivalent to process end
* `failed` represents execution exception, not part of normal control flow

Constraints:

* Every workflow **should have at least one `Stop_*` node**
* A workflow **can have multiple `Stop_*` nodes** for different branches or early completion paths

## 1.5 Variables

Variables carry **lightweight, structured data** such as parameters, identifiers, status flags, or small-scale results.

Characteristics:

* Key-value form
* Lifecycle limited to a single Workflow run
* Suitable for passing control information and small data
* Not suitable for large text or binary content

#### Global Variables (Workflow Globals)

* Use `$xxx` for workflow global variables
* `$vars` declared in registry
* Support inter-step data passing and serve as part of final output

#### Local Variables (Locals)

* Use `_xxx` for local/loop variables
* No declaration needed, used in loop bodies and temporary computation

#### System Variables (User Space Config)

* Use `SYSVAR.xxx` to reference user-preconfigured variables in system space
* `SYSVAR` is read-only, commonly used for keys, default config, preference parameters

## 1.6 Files and Workspace

Workflow can write results to file space via `out`. Files carry **intermediate artifacts or result output** such as code files, config files, documents, etc.

#### Workspace Basics

Workspace is an optional **external file and state hosting space** that Workflow can mount at runtime. When `workspaceId` is specified, all file write operations go to that workspace; otherwise, the system creates a temporary file space.

#### Workspace Composition

A workspace logically consists of two parts:

1. **System Space**: Stores system-level and process-level metadata (run state, pause/resume info, execution records). Managed automatically by the platform.

2. **Artifacts Space**: Stores business files and result files produced during Workflow execution. Files written via `out` with `/` paths land here.

#### Artifacts Space Version Management

The workspace provides **version management** for artifacts space (commit, rollback). These are workspace capabilities, not Workflow responsibilities.

#### Workspace Locking

Workspace supports locking for concurrent access and write conflict control.

#### Design Principles

* Workflow focuses on **how files are generated or modified**
* Workspace handles **file storage, versioning, and concurrency control**
* System space and artifacts space have clear, non-overlapping responsibilities

## 1.7 Runtime Parameters

Runtime parameters control execution behavior per run:

#### 1) `params` (optional: business inputs)

* Declared in `registry.params`
* Missing required params cause engine error
* Read-only inside workflow

#### 2) `workspaceId` (optional: mount file space)

* Unspecified: temporary file space with TTL
* Specified: files persist in the designated workspace

#### 3) `nodes` (optional: partial execution / rerun)

* Unspecified: full workflow execution
* Specified: only listed nodes execute

#### 4) `mode` (optional: run mode)

* `create`: Initialize or create artifacts
* `patch`: Incremental modification
* `regenerate`: Rebuild/rewrite
* `validate`: Validation only, no file writes

#### Example

```json
{
  "workflowId": "codegen_apply_v1",
  "runParams": {
    "params": {
      "userRequest": "Generate a login page",
      "targetLang": "zh-CN"
    },
    "workspaceId": "project_workspace_123",
    "nodes": ["blueprint", "contract", "file_backend"],
    "mode": "patch"
  }
}
```

## 1.8 Eager Execution and Parallel Scheduling

The execution engine uses an **"Eager Execution"** scheduling strategy:

**Core rule: When all input conditions (prerequisites) for a node are satisfied, the node can begin execution immediately without waiting for unrelated nodes.**

This means actual execution order is determined by **data dependency** (partial order), not linear sequence.

#### Workflow JSON Design Guidelines

1. **Parallelize when possible**: Use `children` fan-out or `Loop_* mode:"parallel"` for independent nodes
2. **Avoid unnecessary serialization**: Don't chain unrelated nodes via `next`
3. **Dependency criteria**: Node B depends on Node A only when B's `in` references A's `out`/`Set_*` output, B reads A's written files, or B explicitly depends on A via control flow

## 1.9 Error Handling

#### 1.9.1 Default Error Behavior

On unrecoverable node error, workflow enters **`failed`** state. Engine stops subsequent node execution.

#### 1.9.2 Failed Return Structure

* `status: "failed"`
* `failedAt: "<stepId>"`
* `error: { code: "<errorCode>", message: "<errorMessage>" }`
* `vars: {...}` (variable snapshot for debugging)

#### 1.9.3 Relationship with Runtime Parameters

In `failed` state, `nodes` parameter can specify failed node and successors for partial rerun.

#### 1.9.4 Design Principles

* **Fail-fast** strategy: node failure terminates entire workflow
* Retry and fault tolerance decided by external systems
* Support node-level `onError`: on failure, transfer to specified error handling node

## 1.10 Graph Simplicity Rules

#### 1.10.1 Core Principle: Inline When Possible

Call nodes (`Service_*/API_*/Component_*/LLM_*`) can write variables and files via `out`. When the write source is the current node's `_result`, do it inline in `out` rather than creating separate `Set_*`/`Write_*` nodes.

#### 1.10.2 When to Use Standalone `Set_*`/`Write_*` Nodes

1. Write content not from any node's `_result`
2. Need write at specific control flow position
3. Need special write strategies (`append`, `failIfExists`)
4. Need to set shared state before parallel fan-out

#### 1.10.3 Control Node Merge Principles

* Avoid empty shell nodes
* `Branch_*`/`Loop_*` should remain independent
* `Stop_*` should remain independent

#### 1.10.4 Parallel Structure Simplification

* Prefer `children` fan-out for independent parallel nodes
* Use lightweight `Set_*` as fan-out anchor when parent needs no computation

#### 1.10.5 Node Count Guidelines

* Target: **5-15 nodes** for typical business process
* Over 20 nodes: check for mergeable Set/Write nodes

---

# Chapter 2: Spec JSON Top-Level Structure

```json
{
  "version": "4.1",
  "name": "string",
  "registry": { },
  "subflows": { },
  "steps": [ ]
}
```

## 2.1 Field Definitions

### 2.1.1 `version` (required)

* Current value: `"4.1"`

### 2.1.2 `name` (required)

* Workflow name (for display, search, log positioning)
* Type: `string`

### 2.1.3 `registry` (required)

* External resource and global boundary declaration area (register before use)
* Type: `object`
* Structure defined in Chapter 3

### 2.1.4 `subflows` (optional, 4.0+)

* Embedded subflow definitions, keyed by subflow name
* Type: `object<string, Workflow>`
* Each subflow follows the same top-level Workflow structure (version, name, registry, steps)
* Referenced by `Subflow`/`SpawnChildRun` steps via `workflowRef`

### 2.1.5 `steps` (required)

* Node list (workflow body)
* Type: `array<Step>`

## 2.2 Top-Level Constraints

* `registry` and `steps` must both be present
* `steps` must be non-empty
* Node execution and connection relationships do not depend on array order, only on explicit `next/children/branch/loop` structures

## 2.3 Graph Validation Rules

#### 2.3.1 Entry Node Validation

* Entry node = not referenced by any other node's `next`, `children`, `cases`
* Entry node count must be >= 1

#### 2.3.2 Reachability Validation

* Every node must be reachable from at least one entry node
* Unreachable nodes are dead code; engine should error or warn

#### 2.3.3 Reference Integrity Validation

* All `next`, `children`, `cases` referenced step IDs must exist in `steps`
* No duplicate node IDs

#### 2.3.4 `Stop_*` Validation

* At least one `Stop_*` recommended (warning if missing)
* `Stop_*` must not have `next` or `children`

#### 2.3.5 Validation Levels

| Check | Level | Note |
| --- | --- | --- |
| Entry nodes >= 1 | ERROR | No entry = cannot start |
| All nodes reachable | ERROR | Unreachable = dead code |
| Referenced IDs exist | ERROR | Broken links cause runtime crash |
| No duplicate IDs | ERROR | Control flow ambiguity |
| At least one Stop_* | WARNING | Pure approval flows may lack Stop |
| Stop_* has no next/children | ERROR | Semantic conflict |

## 2.4 Compilation Checks

| Check | Rule | Level |
| --- | --- | --- |
| while/source mutual exclusion | Same `Loop_*` declares both `while` and `source` | ERROR |
| while required fields | `while` mode missing `maxIterations` | ERROR |
| maxIterations range | `maxIterations < 1` | ERROR |
| while mode constraint | `while` + `mode:"parallel"` | ERROR |
| BREAK scope | `BREAK` outside `Loop_*` children subtree | ERROR |
| BREAK reserved word | `BREAK` used as stepId | ERROR |
| EXIT_BRANCH scope (4.1) | `EXIT_BRANCH` outside source-mode parallel `Loop` children | ERROR |
| EXIT_BRANCH reserved word (4.1) | `EXIT_BRANCH` used as stepId | ERROR |
| LLM model empty | `LLM_*` node `model` is empty string | ERROR |
| LLM model segments | `model` contains `/` but either side is empty | ERROR |

# Chapter 3: Registry

`registry` declares workflow runtime **external dependencies** and **global boundaries**, enforcing **"register before use"**. Workflow only handles **execution logic** and does not embed any specific business closure (e.g., git commit, approval submission, etc.).

## 3.1 Registry Top-Level Structure

```jsonc
{
  "params": [ ... ],
  "services": [ ... ],
  "apis": [ ... ],
  "components": [ ... ],
  "vars": [ ... ],
  "files": {
    "inputs": [ ... ],
    "artifacts": [ ... ]
  },
  "docs": { ... },
  "schemas": { ... }
}
```

## 3.2 `params` (optional)

Declares workflow **runtime input parameters**. Passed by caller in `runParams.params`, accessed as read-only variables inside workflow.

### 3.2.1 Structure

String array, each element declares a parameter name and type, optionally with default value:

```
"params": [
  "userRequest(STRING)",
  "targetLang(STRING)",
  "maxRetries(INT) = 3"
]
```

### 3.2.2 Rules

* Declared **without `$` prefix**, aligned with VL service/method parameter style
* Parameters with defaults are optional; without defaults are required (engine error on missing)
* Read-only inside workflow, referenced by parameter name (e.g., `=userRequest`)
* Parameter names must not conflict with `registry.vars`

## 3.3 `services` (required)

Declares **project-internal services** with their **input/output contracts**.

### 3.3.1 Structure (VL Single-Line Signature)

```
"services": [
  "PlannerService(prd(STRING), rulesFile(FILE_REF)) RETURN plan(OBJECT)",
  "WriteFileService(path(STRING), content(STRING)) RETURN ok(BOOL)",
  "ApprovalService(form(OBJECT), policy(STRING)) RETURN decision(STRING), comment(STRING)"
]
```

### 3.3.2 Rules

* `ServiceName(...) RETURN ...` is the fixed format
* `ServiceName` must be unique
* `Service_*` node ID suffix must match a `registry.services` ServiceName

## 3.4 `apis` (optional)

Declares **third-party external APIs** with endpoint, method, and authentication info.

### 3.4.1 Structure

```jsonc
"apis": [
  {
    "id": "StripeCreateCharge",
    "method": "POST",
    "url": "https://api.stripe.com/v1/charges",
    "auth": "SYSVAR.stripeApiKey",
    "headers": { "Content-Type": "application/x-www-form-urlencoded" },
    "desc": "Stripe create charge"
  }
]
```

### 3.4.2 Field Definitions

| Field | Required | Type | Description |
| --- | --- | --- | --- |
| id | Yes | string | Unique API identifier |
| method | Yes | string | HTTP method: GET/POST/PUT/PATCH/DELETE |
| url | Yes | string | API endpoint URL (may contain path parameter placeholders like `{orderId}`) |
| auth | No | string | Authentication credential source (typically `SYSVAR.xxx`) |
| headers | No | object | Fixed request headers |
| desc | No | string | Human-readable description |

### 3.4.3 Rules

* `id` must be unique
* `API_*` node ID suffix must match `registry.apis` id
* `auth` credentials should be in `SYSVAR`, not plaintext in workflow

## 3.5 `components` (required)

Declares **system built-in component capabilities**.

```
"components": ["FileOps", "MCP_Search", "MCP_VectorDB"]
```

## 3.6 `vars` (required)

Declares workflow **global variable set** (VL style).

```
"vars": ["$keyword(STRING)", "$items([OBJECT])", "$result(OBJECT)", "$count(INT)"]
```

Rules:

* Variable names must start with `$`
* Must be unique
* Any `$xxx` in workflow must be declared here
* `set.target` and node `out` can only write to declared `$xxx`

## 3.7 `files` (required)

Declares workflow file read/write boundaries.

```jsonc
"files": {
  "inputs": ["Process/PRD.json", "Process/Rules/*"],
  "artifacts": ["Process/Artifacts/*"]
}
```

### 3.7.1 `inputs` (read-only)

All file read paths must be within `inputs` scope. Read-only during workflow execution.

### 3.7.2 `artifacts` (temporary writable)

All file write paths must be within `artifacts` scope. Run-scoped temporary file space.

## 3.8 `docs` (semantic document references)

`registry.docs` declares **semantic document identifiers** referenced by workflow.

```
"docs": {
  "11": "VL syntax and expression rules",
  "12": "Workflow v2.x design constraints",
  "20": "Frontend component generation spec"
}
```

Rules:

* `docId` must be unique within workflow
* Workflow only references `docId`, never paths or content
* Documents are read-only, do not participate in expression evaluation or control flow

## 3.9 Registry General Constraints (mandatory)

1. **Register before use**: services/apis/components/global vars/params/file paths must be declared in registry first. `LLM_*` is the exception (model resolved via `model` field).
2. **Uniqueness**: No duplicate IDs across services, apis, components, params, vars
3. **Clear read/write boundaries**: params read-only, inputs read-only, docs read-only, artifacts writable
4. **Maintain logical purity**: Registry only describes dependencies and boundaries

## 3.10 `schemas` (JSON Schema reuse, optional)

```jsonc
"schemas": {
  "SpecSchema": { "type": "object", "additionalProperties": false, "properties": { ... } },
  "PlanSchema": { "type": "object", "additionalProperties": false, "properties": { ... } }
}
```

Rules:

* `schemaId` (key) unique within workflow
* `schemaRef` must be findable in `registry.schemas`
* Declaring both `schema` and `schemaRef` should be treated as error

## 3.11 Model Configuration Boundary

Uses "local SDK built-in" mode: LLM provider called via engine built-in SDK, not through unified `LLM_URL` gateway.

Rules:

* No model lists or API keys in `registry`
* `LLM_*` node `model` field supports three forms:
  * No `model`: use environment `LLM_MODEL` (`<provider>/<modelId>`)
  * `model: "<provider>"`: only provider, `modelId` from `<PROVIDER>_MODEL`
  * `model: "<provider>/<modelId>"`: fully specified

# Chapter 4: Step Types

`steps` nodes fall into three categories: **Call Nodes**, **State Nodes**, **Control Nodes**.

## 4.1 Call Nodes

For calling external capabilities and producing results (may be streaming).

* **`Service_*`**: Call project-internal services
* **`API_*`**: Call third-party external HTTP APIs
* **`Component_*`**: Call system built-in component capabilities
* **`LLM_*`**: Call LLM inference (can stream)
* **`Actor_*`**: Actor-based execution unit (4.0+) -- domain-first bounded execution with ResultEnvelope normalization
* **`Download_*`**: Download single file from external source to artifacts
* **`Unzip_*`**: Read zip and extract entries to artifacts

## 4.2 State Nodes

For writing data into workflow internal state space.

* **`Set_*`**: Write/update global variables (`$vars`)
* **`Write_*`**: Write temporary files in workflow space (artifacts)

## 4.3 Control Nodes

For expressing process structure and execution strategy.

* **`Branch_*`**: Conditional branching
* **`Loop_*`**: Loop node (source or while mode)
* **`Stop_*`**: Termination node
* **`Pause_*`**: Pause node (suspends workflow, awaits resume)
* **`Noop_*`**: No-operation node (used as structural anchor, e.g., fan-out point)

## 4.4 Orchestration Nodes (4.0+)

These step families were formalized in Spec 4.0:

* **`Subflow_*`** / **`SpawnChildRun_*`**: Execute another workflow as a child run (see Chapter 15)
* **`Review_*`**: Evaluate a `ResultEnvelope` through human or verifier gate (see Chapter 16)
* **`GraphPatch_*`**: Mutate the future workflow graph for the current run (see Chapter 17)

### Aliases

* `Fork_* -> Noop_*`
* `Check_* -> Branch_*`
* `Done_* -> Stop_*`
* `ChildRun_* -> Subflow_*`
* `SpawnChildRun_* -> Subflow_*`

## 4.5 Legacy-Only Steps

* **`Swarm_*`**: Round-based multi-actor orchestration. **Validates and executes only for workflow versions `4.0` and earlier.** For `4.1`+ workflows, use native `Loop`/`Pause` orchestration instead (see Section 4.6).

## 4.6 Preferred Round-Orchestration Shape (4.1, replaces Swarm)

The recommended authoring model for round-based human-in-the-loop work in `4.1`:

```text
Set_Init
  -> Loop_Rounds (while: $done !== true)
       -> Loop_Actors (source: $actors, mode: parallel)
            -> LLM_ActorWork / Actor_ActorWork
       -> Set_CollectRound
       -> Pause_Human
       -> Set_ApplyDecision
  -> Stop_End
```

This shape uses:

* `Set` for initialization and state management
* `Loop` in `while` mode for rounds
* `Loop` in source-mode `parallel` for actors (with branch control via `spawnSibling`/`exitBranch`)
* `LLM` or `Actor` for work execution
* `Pause` for human steering
* `Stop` for termination

This shape is **normative for new `4.1` workflows** that previously would have used `Swarm`.

## 4.7 DAG-Complexity Steps (engine 4.12+)

Engine releases `4.12`–`4.13` added five additive **canonical** step families (they appear in `engine.getCapabilities().stepTypes`). They are not a new spec version — they validate and run inside ordinary spec-`4.1` graphs. Each writes its result to `_result` (plus a step-specific alias var) and supports `out` output-mapping.

* **`Score_*`**: Inference-time scoring + pruning decision. Computes a `0..1` score and a `keep`/`prune`/`expand` decision from weighted signals (`weights`/`thresholds`) over a `path` (trajectory), `candidates` array, or single `node`. Pair with a `Branch` on `=$score.decision === 'prune'` to prune. (`lib/scorer.js`)
* **`Grow_*`**: Scored dynamic graph growth. Proposes candidate nodes (rule-based from `context.gaps`, or a pre-made `candidates` var reference) and turns them into `GraphPatch` operations anchored at `anchorId`/`nextId`; with `apply: true` it applies them live (emitting `graph_patch_applied`), else returns `graphPatchOps` for a following `GraphPatch` step. (`lib/grower.js`)
* **`Channel_*`**: Bidirectional multi-DAG communication via named FIFO queues on the execution context, shared across parallel branches/sub-DAGs and checkpoint-serialized. `action`: `send` (enqueue `message`/`in`), `recv` (dequeue oldest; `wait:true` polls up to `maxWaitMs`, `required:true` throws on timeout, else `fallback`), `drain` (all remaining), `peek` (oldest without removing). Emits `channel_send`/`channel_recv`. The empty-value key is `fallback` (not `default`).
* **`Topology_*`**: Adaptive agent/edge pruning via the Scorer. `action`: `select` (rank candidate agents by `candidate.signals`, keep top-`keep`; `apply:true` GraphPatches them in as parallel children of `anchorId`) or `prune-edges` (keep high-value comm edges). (`lib/topology.js`)
* **`Iteration_*`**: Bounded `generate → test → fix` container — sugar that aliases `Loop`. Bound with `maxRounds`, `until`, and/or `while`; specifying both `while` and `until` is rejected at validation.

**Meta-flows**: `SpawnChildRun_*`'s `workflow` field may be a computed expression (`workflow: "=$generatedFlow"`) resolving a runtime-generated workflow object — a flow that builds and runs another flow (`features.workflowOfWorkflows`). See Chapter 15.

---

# Chapter 5: Step Properties

## 5.1 Properties Overview (Summary Table)

| Property | Description | Applicable Types | Notes |
| --- | --- | --- | --- |
| id | Node unique identifier (encodes type via prefix) | All | Required |
| if | Condition expression; false skips node and children | All except Stop_* | Optional |
| in | Call input parameters | Call nodes | Required for call nodes |
| out | Map node output to $vars or files | Call nodes | Optional |
| model | LLM selection | LLM_* only | Optional |
| source | Data source / loop source (mutually exclusive with while) | Download/Unzip/Loop | Conditional |
| while | Condition loop expression (Loop_* only) | Loop_* | Conditional |
| maxIterations | Iteration limit | Loop_* | Required for while mode |
| routeByExt | Route by file extension | Download/Unzip | Optional/Required |
| defaultDir | Fallback directory | Download/Unzip | Optional |
| overwrite | Unzip overwrite strategy | Unzip_* | Optional (default true) |
| target | Write target (variable path or file path) | Set_*/Write_* | Required |
| value | Write content/value (expression) | Set_*/Write_* | Required |
| children | Parallel sub-branch entry list | All except Stop_* | Optional (required for Loop_*) |
| next | Serial successor node | All except Stop_* | Required |
| cases | Branch rule list | Branch_* | Required |
| mode | Execution/write mode | Loop_*/Write_* | Required for Loop_* |
| meta | Metadata (display/tracking, not execution) | All | Optional |
| print | Custom message after node completion | All except Stop_* | Optional |
| reason | Pause wait reason text | Pause_* | Optional |
| resumeResultTarget | Resume payload $vars path | Pause_* | Required |
| timeout | Timeout config (sec, on) | Pause_* | Optional |
| tools | Tool declarations for this step (4.0+) | Call nodes | Optional |
| toolScope | Tool scope policy (4.0+) | Call nodes | Optional |
| resultEnvelope | ResultEnvelope configuration (4.0+) | Call nodes | Optional |
| onError | Error handler node | Call nodes | Optional |

## 5.2 Property Details

### 5.2.1 `id`

* Type: `string`
* Required: yes
* Purpose: Unique node identifier; prefix determines node type

**Prefix constraints (mandatory):**

* `Service_xxx`, `API_xxx`, `Component_xxx`, `LLM_xxx`
* `Download_xxx`, `Unzip_xxx`
* `Set_xxx`, `Write_xxx`
* `Branch_xxx`, `Loop_xxx`, `Stop_xxx`, `Pause_xxx`
* `Noop_xxx`, `Fork_xxx` (4.0+)
* `Actor_xxx` (4.0+)
* `Subflow_xxx`, `SpawnChildRun_xxx`, `ChildRun_xxx` (4.0+)
* `Review_xxx` (4.0+)
* `GraphPatch_xxx` (4.0+)
* `Tool_xxx` (3.17+)

### 5.2.2 `if`

* Type: `string` (expression)
* When `if` evaluates to `false`: skip node body + skip all `children` subtree, proceed to `next`

### 5.2.3 `in`

Standard input parameter object for call nodes. Rules vary by node type.

#### `Service_*` `in`

Fields must match `registry.services` parameter signatures.

#### `Component_*` `in`

Structure defined by system component (not repeated in registry).

#### `LLM_*` `model` (optional)

Three forms:
* No `model`: use `LLM_MODEL` env var
* `model: "<provider>"`: provider only, modelId from `<PROVIDER>_MODEL`
* `model: "<provider>/<modelId>"`: fully specified

#### `LLM_*` `in`

Supports structured output (`output_config`), streaming (`in.stream: true`).

##### `output_config` field

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `output_config` | object | No | Output format constraints |
| `output_config.format` | object | Yes (when output_config exists) | Output format config |
| `output_config.format.type` | string | Yes | `"text"`, `"json_object"`, or `"json_schema"` |
| `output_config.format.schema` | object | Yes (for json_schema) | Standard JSON Schema with `"additionalProperties": false` |
| `output_config.format.schemaRef` | string | Yes (for json_schema without schema) | Reference to `registry.schemas` |

##### LLM Node Runtime Context

| Field | Meaning | On Success | On Failure |
| --- | --- | --- | --- |
| `_result` | Business content body | Always present | N/A |
| `_meta` | Call metadata (usage/model/latency/request_id) | Always present | May exist |
| `_error` | Failure info | N/A | Always present |

#### `API_*` `in`

| Field | Type | Description |
| --- | --- | --- |
| body | object/string | Request body |
| query | object | URL query parameters |
| pathParams | object | Path parameters (replace `{xxx}` in URL) |
| headers | object | Additional request headers |
| timeout | number | Request timeout (ms) |

### 5.2.4 `out`

Two forms:

* **Shorthand**: `"out": "$plan"` (equivalent to `{ "$plan": "=_result" }`)
* **Full form**: `"out": { "$plan.status": "=_result.ok", "/path/file": "=_result" }`

Variable write (key starts with `$`): writes to variable space.
File write (key starts with `/`): writes to file space.

### 5.2.5 `Loop_*` `source` and `while`

Two mutually exclusive modes:

**A) `source` mode**: Array traversal

```json
{
  "id": "Loop_ForItems",
  "mode": "parallel",
  "source": "=$items",
  "children": ["LLM_GenOne", "Write_One"],
  "next": "Stop_End"
}
```

**B) `while` mode**:

```json
{
  "id": "Loop_review",
  "while": "=$review.approved != true",
  "maxIterations": 5,
  "mode": "serial",
  "children": ["LLM_Generate"],
  "next": "Write_final"
}
```

Constraints:

* `source` and `while` cannot coexist
* `while` mode requires `mode: "serial"` and `maxIterations`

#### Download_*/Unzip_* Constraints

* `Download_*`: `source` required, streaming download, zip-slip prevention
* `Unzip_*`: `source` required (zip file path), `routeByExt` required
* File content must not be stored in `$vars`; only paths/summaries

### 5.2.6 `target` & `value`

Core properties for state write nodes (`Set_*`, `Write_*`).

* `Set_*` target: global variable path (`$var`, `$var.field`, `$arr[_index]`)
* `Write_*` target: file path (expression string, within `registry.files.artifacts`)
* `value`: expression evaluated and written to target

### 5.2.7 `children`

* Type: `string[]` (step ID list)
* Parallel fan-out: engine starts all children nodes in parallel
* Join: parent waits for all children branches to complete (`RETURN`) before entering its own `next`
* Any child reaching `Stop_*`: entire workflow terminates
* Any child failure: entire workflow enters `failed`

### 5.2.8 `next`

* Type: `string`
* Required: yes (except `Stop_*`)
* Legal values:
  * `"<stepId>"`: proceed to successor
  * `"RETURN"`: end current branch, return to parent join
  * `"BREAK"`: end current iteration and exit entire `Loop_*` (only in Loop children)
  * `"EXIT_BRANCH"` (4.1): exit current parallel loop branch only (see Chapter 9)

`RETURN`, `BREAK`, and `EXIT_BRANCH` are reserved keywords.

### 5.2.9 `cases` (Branch_* only)

Two-dimensional array: `["<whenExpr>", "<stepId>"]`

* Evaluated in order; first true match wins
* `"ELSE"` must be last as fallback
* Selected case executes its entire branch chain until `RETURN`

### 5.2.10 `mode`

* `Loop_*`: `"parallel"` or `"serial"` (required)
* `Write_*`: `"overwrite"` (default), `"failIfExists"`, `"append"`, `"prepend"`

### 5.2.11 `meta`

Metadata for display/debugging, does not affect execution.

### 5.2.12 `print`

Custom message output after node completion. Triggers `step_print` event.

### 5.2.13 `tools` (4.0+)

Declares tool availability for a step. Accepted forms: string, array, object.

Normative behavior:
1. Values are expression-evaluated before dispatch
2. Validation accepts only string/array/object shapes
3. `step_start` may surface the evaluated value as `resolvedTools`
4. Runtime adapters may consume the normalized value directly

### 5.2.14 `toolScope` (4.0+)

Controls tool scope policy. Accepted forms:

* String mode: `inherit`, `append`, `replace`, `none`, `isolate`
* Object with: `mode?`, `allow?`, `deny?`

Normative behavior:
1. Values are expression-evaluated before dispatch
2. `step_start` may surface the evaluated value as `resolvedToolScope`
3. Child-run persistence stores the requested tool scope in metadata

### 5.2.15 `resultEnvelope` (4.0+)

Configures ResultEnvelope generation for a step. See Chapter 18.

Accepted forms:
* `true`
* Summary string
* Object

---

# Chapter 6: Variables and Scope

## 6.1 Variable Type Overview

| Notation | Type | Writable | Declaration |
| --- | --- | --- | --- |
| `paramName` | Input params | No | `registry.params` |
| `$xxx` | Global variables | Yes | `registry.vars` |
| `SYSVAR.xxx` | System variables | No | System preconfigured |
| `_item` / `_index` | Loop locals | No | Auto-injected |
| `_result` | Node output | No | Auto-injected |
| `_meta` | Node metadata | No | Auto-injected (LLM/Actor) |
| `_error` | Node error | No | Auto-injected (on failure) |
| `_resultEnvelope` | Result envelope (4.0+) | No | Auto-injected (when applicable) |
| `_childRun` | Child run result (4.0+) | No | Auto-injected (Subflow/SpawnChildRun) |
| `_graphPatch` | Graph patch result (4.0+) | No | Auto-injected (GraphPatch) |

## 6.2 Input Params

Declared in `registry.params` without `$` prefix. Read-only in all expression positions.

## 6.3 Global Variables `$vars`

Declared in `registry.vars`. Readable in all expressions, writable via `out` and `Set_*`.

### Concurrency Note (Loop parallel)

In `mode:"parallel"`, write by `_index` slot to avoid race conditions: `$generated[_index] = ...`

## 6.4 System Variables `SYSVAR.xxx`

Read-only. No declaration needed. Available in all expressions.

## 6.5 Loop Locals `_item` / `_index` / `_iterDir`

Injected per iteration in Loop children subtree:

* `_item`: current iteration element (source mode only; not available in while mode)
* `_index`: current iteration index (0-based, both modes)
* `_iterDir`: iteration temp directory name `{loopNodeId}_{_index}`

## 6.6 `_result`

Call node output, available only during `out` mapping evaluation. Does not cross node boundaries.

## 6.7 `_meta`

LLM/Actor node metadata object. Available only during `out` evaluation.

Recommended fields: `provider`, `model`, `model_resolved`, `request_id`, `response_id`, `latency_ms`, `finish_reason`, `usage.input_tokens`, `usage.output_tokens`, `usage.total_tokens`, `cost`

## 6.8 `_error`

Injected on node failure. Available in error paths (`onError` branch).

Standard type values: `auth_error`, `bad_request`, `rate_limit`, `timeout`, `context_length_exceeded`, `content_policy_violation`, `service_unavailable`, `connection_error`, `json_parse_error`, `schema_validation_error`, `internal_error`, `unknown_error`

---

# Chapter 7: Expressions and Path Syntax

## 7.1 Expression Prefix Rule (`=` prefix)

* Starts with `=`: expression, engine evaluates content after `=`
* Does not start with `=`: literal string
* Starts with `==`: literal string whose content starts with `=`

## 7.2 Expression Positions

Expressions used in: `if`, `in` field values, `out` right-side values, `Set_*` value, `Write_*` target/value, `Loop_*` source/while, `Branch_*` cases when expressions, `print`.

Not expressions: `out` left-side (target paths), `Set_*` target, `next`/`children`/`cases[*][1]`, `mode`, `model`.

## 7.3 Minimum Expression Capabilities

* Variable references: params, `$xxx`, `SYSVAR.xxx`, `_item`, `_index`, `_result`, `_meta`, `_error`
* Boolean logic: `&&`, `||`, `!`
* Comparison: `==`, `!=`, `>`, `>=`, `<`, `<=`
* String concatenation: `+`
* Arithmetic: `+ - * /`
* Parentheses: `(...)`
* Property access: `.field`
* Array/dict indexing: `[expr]`

## 7.4 Path Syntax

* `.` (dot): always literal field name
* `[...]` (brackets): always expression index
* `["literal"]`: literal key access

## 7.5 `out` Path Write Semantics (deep-set)

Engine performs deep-set write. Auto-creates intermediate objects when possible.

---

# Chapter 8: Temporary File Artifacts

## 8.1 Definition

Artifacts are temporary files in the workflow run instance's isolated space, for storing large text/objects and multi-file intermediate products.

## 8.2 Path Declaration and Boundaries

All artifact write paths must be within `registry.files.artifacts`.

## 8.3 Write Rules (`Write_*`)

Use `Write_*` node with `target` (path) and `value` (content).

## 8.4 Write Strategy (`Write.mode`)

* `"overwrite"` (default), `"failIfExists"`, `"append"`, `"prepend"`

## 8.5 Read Rules

Artifacts read via `Component_*` (e.g., FileOps.Read) or `Service_*`. File paths stored in `$vars`.

## 8.6 Artifacts and Variables Relationship

* Store large content in artifacts, store paths/summaries/hashes in variables

## 8.7 Temporary Directory (`.tmp`) and Lifecycle

* `.tmp/` prefix: temporary file path
* Engine resolves to `<artifactsRoot>/.tmp/{runId}/xxx`
* Isolation: `workspaceId + runId`
* Cleanup after run reaches terminal state; TTL fallback for orphaned directories

## 8.8 Download/Unzip Execution Constraints

* Stream downloads, per-entry extraction, zip-slip prevention
* Large file content must not go into `$vars`

## 8.9 Concurrency and Conflict Constraints

Parallel execution requires different paths per iteration. Use `_index` or `_item.name` for path segmentation.

---

# Chapter 9: Loop Complete Execution Semantics (`Loop_*`)

## 9.1 Node Structure

`source` mode:
```json
{
  "id": "Loop_xxx",
  "mode": "parallel | serial",
  "source": "<expr>",
  "children": ["<stepId>"],
  "next": "<stepId>"
}
```

`while` mode:
```json
{
  "id": "Loop_xxx",
  "mode": "serial",
  "while": "<expr>",
  "maxIterations": 5,
  "children": ["<stepId>"],
  "next": "<stepId>"
}
```

Constraints:

* `mode` required; `while` mode must be `"serial"`
* `source` and `while` mutually exclusive
* `children` required

## 9.2 Data Source Evaluation (`source`)

* `items = eval(source)` must be array
* `null/undefined` treated as `[]` (empty loop)
* Non-array: runtime error

## 9.2.A While Mode Evaluation

* Each round evaluates `while` before executing children
* Round 0 false: zero executions, proceed to `next`
* `_index` incremented each round; `maxIterations` forces exit
* `_item` not available; `_index` and `_iterDir` available

## 9.3 Empty Loop Behavior

If `items.length == 0`: no loop body execution, proceed to `Loop_*` `next`.

## 9.4 Loop Body

### 9.4.1 Entry

`children` defines loop body entry nodes. Recommended: single entry node.

### 9.4.2 Local Variable Binding

Per iteration `i`:
* `_item = items[i]` (source mode)
* `_index = i`
* Scoped to iteration subtree, isolated between iterations

## 9.5 Execution Mode

### 9.5.1 `mode: "serial"`

Sequential per-iteration execution. Iteration 0 completes before iteration 1 starts.

### 9.5.2 `mode: "parallel"`

All iterations can run in parallel. Loop completes (join) when all iterations finish.

### 9.5.3 while mode constraint

`while` + `mode:"parallel"` is a compilation error.

## 9.6 Iteration Completion

An iteration ends when:

* `next: "RETURN"`: normal completion, return to Loop join
* `next: "BREAK"`: complete and exit entire Loop, jump to `Loop_*.next`
* `next: "EXIT_BRANCH"` (4.1): exit current branch only (parallel loops only, see 9.10)
* `Pause_*`: workflow enters `paused`
* `Stop_*`: workflow terminates (`stopped`)
* Failure: workflow enters `failed`

## 9.7 Loop Join Rules

* Loop enters `next` only when all iterations complete (`RETURN`)
* `BREAK`: exit loop, enter `next`; in parallel mode, started iterations continue to natural end
* `Pause_*`: entire workflow pauses
* `Stop_*`: entire workflow terminates
* Failure: entire workflow fails

## 9.8 Interaction with `if`

* `Loop_* if=false`: skip entire loop and children, proceed to `next`
* Loop body node `if`: independent per iteration, does not affect other iterations

## 9.9 Variable/Artifact Interaction

* Serial: safe to write shared variables (structured recommended)
* Parallel: write by `_index` slot: `$generated[_index] = ...`
* Parallel artifacts: ensure different paths per iteration

## 9.10 Parallel Loop Branch Control (4.1)

Source-mode parallel loops may dynamically change their active branch set while running.

### 9.10.1 `spawnSibling(item)`

Available inside a parallel loop child context.

Normative behavior:

1. Engine appends a new sibling branch to the same loop
2. New branch receives the same loop children
3. New branch gets a fresh `ChildExecutionContext`
4. New branch receives `_item = item`
5. Loop does not complete until every initial branch and every spawned branch reaches terminal state

Constraints:

* Nested dynamic spawn is not supported
* A dynamically spawned branch cannot itself spawn another branch

### 9.10.2 `exitBranch(reason?)`

Available inside a parallel loop child context.

Normative behavior:

1. Only the current branch exits
2. Sibling branches continue unaffected
3. Loop still waits for the rest of the branch set
4. Runtime emits `loop_branch_exited`

### 9.10.3 `next: "EXIT_BRANCH"`

Inside a parallel loop child step, `next: "EXIT_BRANCH"` is a declarative alias for `ctx.exitBranch()`.

Validation rule: `EXIT_BRANCH` is only valid inside children of a source-mode parallel `Loop`.

### 9.10.4 `BREAK` Semantics in Parallel Loops

In parallel loops, `BREAK` prevents not-yet-started branches from launching. Already-started iterations continue to natural end.

### 9.10.5 Adapter Runtime Visibility

When `Actor` or `LLM` runs inside a parallel loop branch, the runtime passes branch control to the adapter:

* `loopId`
* `branchIndex`
* `branchId`
* `spawnSibling(item)`
* `exitBranch(reason)`

This enables hosts to upgrade from "round-to-round actor adjustment" to "in-round self-splitting / self-exit".

## 9.11 Loop Output and Data Aggregation

Loop itself does not produce `_result`. Results via:
* Loop body call node `out` writes to `$vars[_index]`
* Loop body `Write_*` writes to artifacts, paths stored in `$vars[_index]`

---

# Chapter 10: Branch Complete Execution Semantics (`Branch_*`)

## 10.1 Node Structure

```json
{
  "id": "Branch_xxx",
  "cases": [
    ["<whenExpr1>", "<stepId1>"],
    ["<whenExpr2>", "<stepId2>"],
    ["ELSE", "<stepIdElse>"]
  ],
  "next": "<stepId>"
}
```

## 10.2 Case Selection Rules

Sequential evaluation; first true match wins. `"ELSE"` is fallback (must be last).

## 10.3 Single Entry Rule

Each case specifies one entry node.

## 10.4 Branch Execution Semantics

Selected case executes its entire branch chain (following `next/children/Branch_*/Loop_*`).

## 10.5 Branch Completion

Branch chain ends when:
* `next: "RETURN"`: normal completion, return to Branch join
* `Pause_*`: workflow pauses
* `Stop_*`: workflow terminates
* Failure: workflow fails

## 10.6 Branch Join and `next`

Branch enters its own `next` only when the selected branch chain completes normally (`RETURN`).

## 10.7 Unselected Cases

All unselected cases do not execute at all.

## 10.8 Interaction with `if`

* `Branch_* if=false`: skip entire Branch (no case evaluation), proceed to `next`
* Internal node `if`: affects only that branch chain

## 10.9 Branch and Parallel (children)

Branch is mutually exclusive selection. If a case entry node contains `children`, that branch can still have internal parallel execution.

## 10.10 Missing ELSE Behavior (not recommended)

If ELSE is missing and no case matches: Branch enters `next` without executing any branch.

---

# Chapter 11: Pause_* Node and Pause/Resume Protocol

## 11.1 Purpose

`Pause_*` explicitly expresses "wait for external result before continuing". Engine enters `paused` state and continues only after legal `resume` request.

## 11.2 Node Structure

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | Yes | Must start with `Pause_` |
| `reason` | string | No | Wait reason text for display |
| `resumeResultTarget` | string | Yes | `$vars` target path for resume payload |
| `timeout` | object | No | Timeout config |
| `next` | string | Yes | Successor after successful resume |

### `timeout` Structure

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `sec` | number | Yes | Timeout seconds (must be > 0) |
| `on` | string | Yes | Node to jump to on timeout |

### Pause and Human Steering (4.1 recommended pattern)

* Gather round outputs into normal workflow variables
* Pause on a regular `Pause_*` node
* Resume with `resumeResultTarget`
* Apply the decision in `Set_*`
* Let the next `while` evaluation decide whether another round is needed

## 11.3 Execution Semantics

### 11.3.1 Entering Pause

1. Set run status to `paused`
2. Persist wait context (runId, pauseNodeId, waitToken, expireAt, resumeResultTarget)
3. Emit `pause_start` event

### 11.3.2 Resuming Execution

1. Validate run is `paused`
2. Validate `waitToken` and `pauseNodeId` correspondence
3. Write `payload` to `resumeResultTarget`
4. Emit `pause_resumed` event
5. Continue from `Pause_*` node's `next`

### 11.3.3 Timeout Handling

1. Emit `pause_timeout` event
2. Jump to `timeout.on`
3. Without timeout config: implementation-defined behavior

## 11.4 Resume Interface Protocol

### Minimum Request Fields

```json
{
  "runId": "run_xxx",
  "token": "wait_token_xxx",
  "payload": { "approved": true, "comment": "ok" },
  "requestId": "req_xxx"
}
```

### Constraints

1. `resume` must be idempotent (suggest `requestId`-based dedup)
2. Same wait token can only succeed once
3. Non-`paused` run should reject resume
4. External cannot specify `nextStep`; resume path is fixed to `Pause_*` `next`

## 11.5 Pause Events

* `pause_start`: entering Pause_* (runId, nodeId, reason, waitToken, expireAt, resumeResultTarget)
* `pause_resumed`: resume successful (runId, nodeId, requestId, resumedAt)
* `pause_timeout`: wait timed out (runId, nodeId, expiredAt, timeoutAction)
* `pause_rejected`: resume rejected (runId, nodeId, requestId, reasonCode)

## 11.6 Service Responsibility Boundary

1. Workflow: "when to wait / when to continue"
2. Business Service: "who processes / how rules compute / when to form final result"
3. Complex approval logic (countersign/or-sign/rollback) not built into workflow engine
4. Business Service calls `resume` when final result is ready

---

# Chapter 12: IDE Workflow Interaction

> Workflow is part of the project, stored in `/config/workflow.json`. New projects auto-generate default workflow. IDE always runs in known `tenantId + projectId` context.

## 12.1 Frontend Core Interaction Windows

| Window | Content | User Action | Notes |
| --- | --- | --- | --- |
| Dialog window | User natural language input | Input requirements | Sole entry for all workflow execution |
| Workflow run view | Current project workflow execution | None | Auto-display current run status |
| Output console | Logs / node status / errors | View | Auto-pushed from backend |
| Workspace view | Current workspace file status | commit / rollback | Decoupled from workflow |
| Status indicator | Run status | View | running / finished / failed |

## 12.2 Backend Interfaces

| Interface | Purpose | Input | Output |
| --- | --- | --- | --- |
| LoadProjectContext | Load project context | tenantId, projectId | Project info, default workspaceId |
| LoadWorkspace | Load workspace | tenantId, projectId, workspaceId | File tree summary, workspace status |
| AnalyzeIntent | Generate run params (Agent) | tenantId, projectId, workspaceId, userInput | runMaps, notes |
| RunProjectWorkflow | Run project workflow | tenantId, projectId, workspaceId, userInput, runMaps? | runId + SSE connection |
| GetRunStatus | Query run status | tenantId, projectId, runId | Run status, error info |
| CommitWorkspace | Commit artifacts | tenantId, projectId, workspaceId | commitId |
| RollbackWorkspace | Rollback artifacts | tenantId, projectId, workspaceId | Rollback result |

---

# Chapter 13: Runtime Event Specification (`run_events`)

## 13.1 Overview

Events are produced as structured messages during execution, for callers (frontend, MQ, logging) to track progress.

Design principles:
* `run_events` describes process only; final conclusions in `workflow_runs`
* Each event independently parseable
* Event stream is best-effort; consumer errors do not affect execution

## 13.2 Event Base Structure

```json
{
  "run_id": "run_xxx",
  "seq": 1,
  "ts": "2026-02-21T10:00:00.123Z",
  "type": "step_start",
  "step_id": "LLM_Answer",
  "payload": {}
}
```

| Field | Type | Description |
| --- | --- | --- |
| `run_id` | string | Unique execution ID |
| `seq` | integer | Monotonically increasing sequence number (from 1) |
| `ts` | string | ISO 8601 timestamp (ms precision) |
| `type` | string | Event type |
| `step_id` | string/null | Triggering node ID; null for workflow-level events |
| `payload` | object | Event payload |

## 13.3 Event Types

### Run-event schema

`vl.workflow.run-event.v4`

### Workflow Level

* `workflow_start`
* `workflow_done`
* `workflow_failed`
* `workflow_cancelled`

### Step Level

* `step_start` -- payload may include `resolvedTools` and `resolvedToolScope` (4.0+)
* `step_print`
* `step_done`
* `step_error`
* `step_skipped`

### LLM Specific

* `llm_token` (only when `in.stream: true`)
* `llm_done`

### File Status

* `file_start` (declares file path to be written)
* `file_done` (file content fully written)

### Pause Specific

* `pause_start`
* `pause_resumed`
* `pause_timeout`
* `pause_rejected`

### Tool Runtime Events (3.17+)

* `tool_start`
* `tool_message`
* `tool_done`
* `tool_error`

### ResultEnvelope Events (4.0+)

* `result_envelope_stored`

### GraphPatch Events (4.0+)

* `graph_patch_applied`

### Swarm Events (4.0, legacy-only)

* `swarm_started`, `swarm_round_started`, `swarm_actor_dispatched`, `swarm_contribution_recorded`
* `swarm_human_gate_requested`, `swarm_frontier_updated`, `swarm_node_promoted`, `swarm_conflict_detected`
* `swarm_round_completed`, `swarm_done`, `swarm_aborted`

### Loop Branch Events (4.1)

* `loop_branch_spawned` -- emitted when `spawnSibling` adds a new branch
* `loop_branch_exited` -- emitted when `exitBranch` or `EXIT_BRANCH` terminates a branch

These events are additive and continue to use event schema `vl.workflow.run-event.v4`.

### Event Ordering

* `step_start` -> `llm_token`(xN) -> `llm_done` -> `step_print`(0..1) -> `step_done`
* `file_start` events batch-emitted after `step_start`, before `llm_token`
* `file_done` events after `llm_done`, before `step_print`/`step_done`

## 13.4 Event Payload Specifications

### `workflow_start`
```json
{ "params": {} }
```

### `workflow_done`
```json
{ "stop_id": "Stop_End", "duration_ms": 4821 }
```

### `workflow_failed`
```json
{
  "failed_step_id": "LLM_Analyze",
  "error": { "type": "json_parse_error", "message": "Unexpected token", "retryable": true },
  "duration_ms": 1203
}
```

### `workflow_cancelled`
```json
{ "reason": "user_request", "duration_ms": 800 }
```

### `step_start`
```jsonc
{ "step_type": "LLM_*", "resolvedTools": [...], "resolvedToolScope": {...} }
```

### `step_done`
```json
{ "step_type": "LLM_*", "duration_ms": 2341 }
```

### `step_print`
```json
{ "message": "Version check complete: 3.13" }
```

### `step_error`
```json
{ "step_type": "LLM_*", "error": { "type": "rate_limit", "message": "Rate limit exceeded", "retryable": true, "status_code": 429 }, "duration_ms": 312 }
```

### `step_skipped`
```json
{ "step_type": "Set_*", "reason": "if_false" }
```

### `file_start`
```json
{ "path": "src/components/Header.tsx" }
```

### `file_done`
```json
{ "path": "src/components/Header.tsx", "size_bytes": 2048 }
```

### `llm_token`
```json
{ "delta": "Hello" }
```

### `llm_done`
```json
{ "finish_reason": "stop", "usage": { "input_tokens": 312, "output_tokens": 428, "total_tokens": 740 }, "model": "claude-sonnet-4-5", "latency_ms": 2180 }
```

### `result_envelope_stored` (4.0+)
Emitted when a stored envelope changes.

### `graph_patch_applied` (4.0+)
Emitted after successful graph mutation.

### `loop_branch_spawned` (4.1)
Emitted when `spawnSibling` adds a new branch to a parallel loop.

### `loop_branch_exited` (4.1)
Emitted when `exitBranch` or `EXIT_BRANCH` terminates a branch.

## 13.5 Parallel and Order Guarantees

* Serial nodes: event order matches execution order
* Parallel nodes: intra-node events ordered; inter-node order not guaranteed
* Consumers should use `step_id` + `seq` for reassembly

---

# Chapter 14: Extension Profiles and Compatibility

## 14.1 Purpose

Formalize high-value capabilities that were previously scattered in implementation:

* Workflow calling host tools
* Outer window full tool event visibility
* Parallel runs with stable attribution from first step
* Checkpoint-based continuation in complex fork/loop structures

## 14.2 `Tool_*` Node

### 14.2.1 Purpose

`Tool_*` calls host environment tools (files, search, compile, browser control, SysDoc and Resource Center, sub-workflow scheduling).

### 14.2.2 Recommended Structure

```json
{
  "id": "Tool_ReadBlueprint",
  "tool": "ReadFile",
  "input": { "file_path": "docs/blueprint.md" },
  "timeout": 15000,
  "allowError": false,
  "out": {
    "$blueprint": "_result",
    "$readMeta": "_toolResult"
  },
  "next": "LLM_AnalyzeBlueprint"
}
```

| Field | Type | Description |
| --- | --- | --- |
| `tool`/`toolName` | string | Host tool name |
| `input`/`in` | object | Tool input |
| `timeout` | integer | Optional, ms timeout |
| `allowError` | boolean | When true, tool failure doesn't fail workflow |

### 14.2.3 `Subflow_*` Semantic Alias

`Subflow_*` serves as semantic alias for `WorkflowRun`, highlighting sub-workflow boundaries without introducing new execution primitives.

## 14.3 Tool Runtime Events

* `tool_start`, `tool_message`, `tool_done`, `tool_error`

Must enter workflow standard event stream and be perceivable by outer window, SSE clients, logging systems.

## 14.4 Run Identification and Parallel Attribution

### `runID` and `clientRunToken`

`clientRunToken` is a stable, pre-assigned run attribution identifier.

Requirements:
* Present from `workflow_start` in all events
* New token for each `Run`, `rerunFromStep`, `executeFrom`
* Parallel runs maintain independent run traces

## 14.5 Checkpoint, Resume, and Complex Branches

### `rerunFromStep` / `executeFrom`

Runtime must:
* Preserve dependent artifacts and variables
* Normalize completed branch info, loop progress, step pointer
* Allow caller `overrides` for input variables
* Prevent stale sibling branch completion from blocking downstream

### Fork/Loop Minimum Requirements

* Fork resume: continue from fork successor without reusing unrelated sibling branch completion
* Loop resume: continue from mid-iteration, rebuild loop progress as needed
* Parallel loop resume: continue remaining iterations from checkpoint

## 14.6 Workflow-of-Workflows

Recognized as recommended pattern:
* Parent workflow calls `WorkflowRun` via `Tool_*` or host capability
* Parallel child workflow dispatch
* Child workflow events bridged to parent via `tool_message` or host bridge

---

# Chapter 15: Child-Run Semantics (4.0+)

`Subflow`, `ChildRun`, and `SpawnChildRun` execute another workflow as a child run.

## 15.1 Accepted Workflow References

* `workflowRef`: reference to embedded `subflows` definition
* `workflowPath`: external workflow path
* `subflow`: alias
* `workflow`: alias

## 15.2 Common Fields

* `alias`
* `in` or `input`
* `options`
* `tools`
* `toolScope`
* `inheritAdapters`
* `shareArtifacts`
* `importResultEnvelopes`

## 15.3 Normative Behavior

1. A normalized child-run request is built
2. Child workflow dispatched through `workflowAdapter.call()` when present, or executed locally for embedded definitions
3. `tools` and `toolScope` travel with the child-run request
4. Child-run record persisted into `ctx.childRuns`
5. `_result`, `_childRun`, and `_meta` exposed during output mapping
6. Imported child result envelopes re-stored in parent context when enabled

## 15.4 Minimum Persisted Child-Run Record

* `childRunId`
* `workflowRef`
* `workflowName`
* `workflowVersion`
* `status`
* `artifacts`
* `resultEnvelopes`
* `snapshot`
* `checkpoint`
* `metadata.requestedTools`
* `metadata.requestedToolScope`
* `completedAt`

## 15.5 Example

```json
{
  "id": "SpawnChildRun_Finalize",
  "type": "SpawnChildRun",
  "workflowRef": "FinalizeService",
  "alias": "finalize_service",
  "tools": ["VLValidate"],
  "toolScope": "replace",
  "in": {
    "brief": "=brief",
    "note": "=$auditNote || \"audit:default\""
  },
  "out": {
    "$childRun": "=_childRun"
  },
  "next": "Stop_End"
}
```

---

# Chapter 16: Review Step (4.0+)

`Review` evaluates a `ResultEnvelope` through either a `human` or `verifier` gate.

## 16.1 Common Fields

* `source` or `envelope`: the ResultEnvelope to review
* `gate` or `acceptance`: gate type (`human` or `verifier`)
* `decisionTarget`
* `statusTarget`
* `envelopeTarget`
* `review`
* `timeout`

## 16.2 Gate Types

* **`human`**: Opens a pending review state and emits `human_gate_opened`. Workflow pauses until `engine.review()` resolves it or timeout fires.
* **`verifier`**: Executes in-process and returns reviewed envelope without human pause.

## 16.3 Normative Behavior

1. Human gate opens pending review state, emits `human_gate_opened`
2. Workflow pauses inside the step until resolved
3. Reviewed envelope normalized, stored, emitted through `review_completed`
4. Verifier gate executes in-process without pause

## 16.4 Output Mapping Locals

* `_result`, `_resultEnvelope`, `_meta`

---

# Chapter 17: GraphPatch Step (4.0+)

`GraphPatch` mutates the future workflow graph for the current run.

## 17.1 Accepted Patch Containers

* `patch`
* `graphPatch`
* `operations`

## 17.2 Accepted Operations

* `upsertStep`
* `removeStep`
* `setNext`
* `setField`
* `appendChildren`
* `replaceChildren`

## 17.3 Normative Behavior

1. Patch expressions evaluated before normalization
2. Patch normalized into `{ patchId, stepId, operations }`
3. Active workflow graph mutated immediately
4. Persisted graph-patch record stored in `ctx.graphPatches`
5. `graph_patch_applied` emitted after success
6. `_result`, `_graphPatch`, `_meta` available during output mapping

## 17.4 Patch Replay Behavior

* Fresh `engine.execute()` starts from baseline workflow graph
* `engine.executeFrom()` replays checkpointed graph patches before resolving resume step
* Resuming into a dynamically inserted step is valid

Patch authors are responsible for semantic graph integrity after mutation. Engine guarantees structural op normalization, not full ahead-of-time validation.

## 17.5 Example

```json
{
  "id": "GraphPatch_Prepare",
  "type": "GraphPatch",
  "if": "=applyPatch",
  "patch": {
    "operations": [
      {
        "op": "upsertStep",
        "step": {
          "id": "Set_AuditNote",
          "target": "$auditNote",
          "value": "=\"audit:patched\"",
          "next": "Stop_End"
        }
      },
      { "op": "setNext", "stepId": "SpawnChildRun_Finalize", "next": "Set_AuditNote" }
    ]
  },
  "out": { "$graphPatch": "=_graphPatch" },
  "next": "SpawnChildRun_Finalize"
}
```

---

# Chapter 18: ResultEnvelope Contract (4.0+)

`ResultEnvelope` is a **generic** normalized output contract -- no longer actor-only in 4.0+.

## 18.1 `step.resultEnvelope`

Accepted forms:
* `true`
* Summary string
* Object

## 18.2 Normative Behavior

1. If a step already produced `_resultEnvelope`, that envelope remains authoritative unless `override` is set
2. Otherwise engine materializes a normalized envelope from the step result
3. `result_envelope_stored` emitted when stored envelope changes
4. Checkpoints persist `ctx.resultEnvelopes`

## 18.3 Supported Step Families

* `Service`, `API`, `Component`, `Actor`
* `Subflow` / `SpawnChildRun`
* `Review`, `GraphPatch`
* `LLM`, `Download`, `Unzip`

## 18.4 Output-Mapping Locals by Step Family

* `Actor`: `_result`, `_resultEnvelope`, `_meta`
* `Subflow`: `_result`, `_childRun`, `_meta`
* `Review`: `_result`, `_resultEnvelope`, `_meta`
* `GraphPatch`: `_result`, `_graphPatch`, `_meta`
* Any step with `resultEnvelope`: `_resultEnvelope`

---

# Chapter 19: Actor Step (4.0+)

`Actor` is the bounded execution unit introduced in 3.20 and formalized in 4.0.

## 19.1 Key Points

* Actor identity is domain-first, not model-first
* Adapter result normalized into a `ResultEnvelope`
* Actor lifecycle events emitted in workflow stream
* `_result`, `_resultEnvelope`, `_meta` available during output mapping
* `step.tools` and `step.toolScope` forwarded with actor request

---

# Chapter 20: Swarm Step (Legacy, 4.0 only)

> **Deprecation notice**: `Swarm` is removed from `4.1` validation and authoring. Runtime compatibility remains for workflow versions `4.0` and earlier only. New `4.1` workflows should use native `Loop`/`Pause` orchestration instead (see Section 4.6).

`Swarm` is a round-based orchestration step for multi-actor collaboration on one problem root.

## 20.1 Common Fields

* `problem`
* `actors`
* `scheduler`
* `exit`
* `contextPolicy`
* `promotionPolicy`
* `knowledgeRoot` / `knowledgeTree`
* `resultPolicy`

## 20.2 Normative Behavior (4.0)

1. Engine creates or reuses a swarm knowledge tree keyed to the workflow run and step
2. Each round evaluates knowledge-driven exit conditions before scheduling work
3. Scheduler selects assignments from strategy rules or actor defaults
4. Each assignment dispatches through Actor execution path with injected context
5. Actor dispatches inside the same round may run in parallel
6. Actor results may contribute `knowledgeDrafts`
7. Verifier or human review may promote contributed nodes to `verified`
8. `swarmState` persists round progress in checkpoints and snapshots
9. `swarm_*` events surface start, round, contribution, promotion, conflict, and completion milestones

## 20.3 Knowledge-Sharing Contract

* Verified facts, ruled-out paths, dead-ends, and conflicted nodes always eligible for shared context
* Peer drafts stay private by default unless `contextPolicy.sharePeerDrafts === true`
* An actor always receives its own prior contributions

## 20.4 Legacy Compatibility

* Use workflow `version: "4.0"` if a graph still contains `Swarm`
* Upgrade to `4.1` only after replacing `Swarm` with native `Loop`/`Pause` orchestration
* The host-side swarm knowledge adapter remains exported only so older `4.0` flows can still be replayed

---

# Chapter 21: Checkpoint / Resume / Rerun

## 21.1 Formal Guarantees

* Checkpoints remain JSON-serializable
* `resultEnvelopes`, `childRuns`, `reviewState`, and `graphPatches` persist in checkpoints (4.0+)
* Rerun from any step remains supported through `engine.executeFrom()`
* `clientRunToken` survives into actor, child-run, review, and workflow events
* Graph patches are replayed before resume step lookup

## 21.2 Checkpoint Version 3 (4.1)

Checkpoint version `3` adds `loopState`.

Normative behavior:

1. Active parallel loops persist their branch set in `checkpoint.loopState`
2. Dynamically spawned branches survive JSON round-trips
3. In-flight branches resume as pending work
4. The resume entry point is the loop node, not an individual branch child
5. `loopProgress` remains present for compatibility with pre-`4.1` checkpoints and static-loop resumes

---

# Chapter 22: Capability Manifest (4.0+)

Consumers should prefer `engine.getCapabilities()` over inferring support from `workflow.version`.

## 22.1 Important 4.0+ Capability Flags

* `features.stepTools`
* `features.toolScope`
* `features.subflowStep`
* `features.childRun`
* `features.spawnChildRun`
* `features.graphPatch`
* `features.resultEnvelopeContract`
* `features.reviewStep`
* `features.humanGate`

## 22.2 Legacy 4.0 Flags (deprecated in 4.1)

* `features.swarmStep` -- only meaningful for `version <= 4.0` workflows
* `features.swarmEvents` -- only meaningful for `version <= 4.0` workflows
* `features.swarmAdapter` -- only meaningful for `version <= 4.0` workflows

---

# Chapter 23: Compatibility Notes

## 23.1 Unchanged from 3.22

* Explicit `type`
* Actor lifecycle events
* Human and verifier review gates
* Child-run persistence
* `clientRunToken`
* Checkpoint / rerun behavior
* Loop `while`
* `BREAK`
* Pause / resume

## 23.2 Formalized under 4.0

* First-class `GraphPatch`
* First-class `Swarm` (now legacy-only in 4.1)
* First-class `step.tools`
* First-class `step.toolScope`
* Event schema v4
* Generic `ResultEnvelope` contract
* Replay-safe graph patch restore

## 23.3 New in 4.1

* `Swarm` deprecated from canonical step families (legacy-only for versions <= 4.0)
* Parallel loop branch control: `spawnSibling(item)`, `exitBranch(reason)`, `next: "EXIT_BRANCH"`
* Checkpoint v3 with `loopState`
* New events: `loop_branch_spawned`, `loop_branch_exited`
* Preferred round-orchestration shape (Loop/Pause replacing Swarm)
* Adapter runtime visibility for loop branch control

---

# Chapter 24: Agent App Host Profile (DAG Shell / Node Capsule)

Agent App authoring may wrap an ordinary Workflow Spec `4.1` graph with a host-owned Shell and per-step Capsules. This is an additive host profile, not a core step family. It does not change engine execution semantics and should be ignored by hosts that do not implement Agent App authoring.

The authoring source of truth is:

* `app.surface.dagShell` for Shell props, params, permissions, and Shell Event Panel logic
* `workflow.steps[]`, `next`, `children`, `in`, and `out` for topology and actual step wiring
* `step.capsule` for a node-local declaration block plus one node-local Event Panel logic block
* control-plane run records for runtime evidence, artifacts, errors, checkpoints, and node result history

Canonical authoring shape:

```json
{
  "app": {
    "permissions": { "tools": ["ReadFile", "WriteFile"] },
    "surface": {
      "dagShell": {
        "schemaVersion": "vl.agentos.dag-shell.v1",
        "props": {},
        "params": {},
        "logic": { "kind": "eventPanel", "events": [] }
      }
    }
  },
  "workflow": {
    "version": "4.1",
    "steps": [
      {
        "id": "LLM_GeneratePlan",
        "type": "LLM",
        "in": { "request": "=$params.request" },
        "out": { "$plan": "=_result" },
        "capsule": {
          "schemaVersion": "vl.agentos.node-capsule.v1",
          "contract": {
            "schema": {
              "inputs": { "request": { "type": "object" } },
              "outputs": { "$plan": { "type": "object" } }
            },
            "sideEffects": "workspace",
            "toolScope": { "mode": "replace", "allow": ["ReadFile", "WriteFile"] },
            "retry": {}
          },
          "llmRuntime": {
            "model": {},
            "outputSchema": {},
            "agenticLoop": {
              "enabled": false,
              "tools": [],
              "maxIterations": 1,
              "stopWhen": ""
            }
          },
          "resources": {
            "skills": [{ "name": "Planner", "content": "Plan with explicit assumptions and produce JSON." }],
            "docs": [],
            "tools": ["ReadFile", "WriteFile"]
          },
          "logic": { "kind": "eventPanel", "events": [] },
          "samples": []
        }
      }
    ]
  }
}
```

Normative host-profile rules:

1. Shell and Capsule `logic` must persist Event Panel `events` / AST as source of truth. A string field such as `"vl": "..."`, or source-only logic without events/AST, is not canonical. `compiledJs` is a derived cache only.
2. Step `in` / `out` wiring is the runtime fact. `capsule.contract.schema.inputs` and `outputs` declare the expected shape; host validators must fail when required wiring is missing or when strict schemas reject undeclared wired keys.
3. Node skills are inline authoring data under `capsule.resources.skills[]` as `{ "name", "content" }`. This profile does not define `skillRefs` or a package registry pointer.
4. `capsule.samples[]` may carry author-provided sample inputs for dry-run. Runtime `evidence`, `artifact`, `error`, and node result history belong to the host control plane, not the authoring document.
5. `contract.toolScope` and `resources.tools` must be a subset of `app.permissions.tools` before execution. Hosts should fail closed before compiling or running Event Panel logic.
6. Capsules must not embed internal DAGs, workflow steps, `children`, or hidden `WorkflowRun` chains. Logic requiring internal evidence, checkpoint, single-step rerun, HumanGate, irreversible external side effects, parallel governance, or multi-LLM orchestration must be promoted to an external `Subflow` / `WorkflowRun`.
7. LLM generate -> verify -> repair loops should be represented as an `llmRuntime.agenticLoop` with verifier tools and bounded `maxIterations`; deterministic input shaping, output parsing, artifact writes, and local glue remain in the single Capsule Event Panel logic block.

Host write gates:

1. Validate the ordinary Workflow Spec graph.
2. Validate `app.surface.dagShell`.
3. Validate every `step.capsule`.
4. Check step wiring against `contract.schema`.
5. Check tool declarations against app permissions.
6. Run WorkflowDryRun plus Shell/Capsule dry-run samples when samples exist.

Only after all gates pass should the host write a successful authoring state.

---

This specification's canonical copy is published in **SystemDoc (key: WorkflowSpec)**. If the local mirror and the document center disagree, the document center's latest version takes precedence.
