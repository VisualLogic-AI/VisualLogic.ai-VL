<p align="center">
  <img src="https://raw.githubusercontent.com/VisualLogic-AI/VisualLogic.ai-VL/main/assets/logo.png" alt="VisualLogic.ai" width="420"/>
</p>

<h1 align="center">VisualLogic.ai — The AI-Native Programming System</h1>

<p align="center">
  <strong>Where Language, Visual Graphs, and AI Agents are fully reversible.</strong><br/>
  Build production-ready applications from natural language — deterministically.
</p>

<p align="center">
  <a href="https://editor.visuallogic.ai/">Website</a> &nbsp;|&nbsp;
  <a href="https://www.youtube.com/playlist?list=PLJE6c8wBknRnCZIRv_VFa1dYswTSqoW21">YouTube</a> &nbsp;|&nbsp;
  <a href="https://discord.com/invite/KdaVtR7pzv">Discord</a>
</p>

---

## What is VisualLogic?

VisualLogic is a **full-stack AI programming system** built on **VL (Visual Language)** — a minimal, deterministic language designed specifically for AI code generation.

It is **not** a low-code tool. It is **not** a prompt wrapper. It is a complete programming system where:

- **AI generates code** with near-perfect accuracy, thanks to VL's low-entropy grammar
- **Visual graphs** provide a lossless, bidirectional view of every program
- **Multi-agent workflows** orchestrate the entire development lifecycle

> VisualLogic is not a tool on top of code. VisualLogic *is the programming system itself.*

---

## The VL Language

VL (Visual Language) v2.91 is a declarative, component-oriented language covering the full stack through six file types:

| Extension | Purpose | Description |
|-----------|---------|-------------|
| `.vx` | App | Navigation, routing, configuration |
| `.sc` | Section | UI tree, state, events, styles |
| `.cp` | Component | Reusable props-driven UI blocks |
| `.vs` | Service | API contracts, domain logic |
| `.vdb` | Database | Tables, columns, types, relations |
| `.vth` | Theme | Colors, typography, spacing tokens |

### Why VL?

Traditional programming languages carry enormous entropy — formatting choices, import orderings, naming conventions, bracket styles. This makes AI generation unreliable and expensive.

VL eliminates this problem:

- **Stable grammar** — no formatting ambiguity, no hidden state
- **Dash-based tree structure** — clean hierarchy without brace/bracket noise
- **Ultra-low token cost** — compact representation means faster, cheaper AI generation
- **Massive parallelism** — independent components enable multi-agent concurrent generation

```
APP MyApp
--title: My Application
--theme: Theme-Default

SECTION Dashboard
--$userCount(INT) = 0

--HANDLER onLoad()
----CALL UserDomain.getStats()
------ON SUCCESS
--------SET $userCount = result.total

--<Column "root">
----<StatCard "users"> label:"Total Users" value:$userCount
```

---

## How It Works

### Natural Language to Production App

Give VisualLogic a product requirement in plain language. An automated **8-agent pipeline** handles everything:

| # | Agent | Output |
|---|-------|--------|
| 1 | Requirements Analyst | PRD document |
| 2 | Data Architect | Database schema (`.vdb`) |
| 3 | Service Designer | API contracts (`.vs`) |
| 4 | UI Architect | Screen/component breakdown |
| 5 | Component Builder | Reusable UI components (`.cp`) |
| 6 | Service Coder | Business logic (`.vs`) |
| 7 | Screen Builder | Screens with state & events (`.sc`) |
| 8 | App Assembler | App with routing (`.vx`) |

Stages 5-8 run **in parallel** using independent agents. The result: a fully compilable, deployable application.

### Bidirectional Code-Graph Transformation

Every VL program has a direct, lossless mapping to a visual graph and back:

```
Natural Language
       |
   AI IDE (Agents + Workflows)
       |
   VL Source Files (.vx .sc .vs .vdb)
       |
   Visual IDE (Graph Editor)
       |
   VL Compiler
       |
   JS + Java / Node.js (Full Stack)
```

Edit in code. Switch to the visual canvas. Drag, connect, rearrange. Switch back. **Nothing is lost.** This is the "graphical escape hatch" that makes VisualLogic accessible to everyone — from senior engineers to people who've never written a line of code.

---

## VL-Code: The AI IDE

**VL-Code** is the AI IDE for VisualLogic — a complete development environment where the LLM is not just an assistant, but the **core execution engine**.

### Non-Linear Workflow Engine

Unlike simple plan-then-execute tools, VL-Code uses **visual workflow DAGs** (Directed Acyclic Graphs). Each node can:

- Call an LLM with targeted context
- Spawn sub-agents for parallel tasks
- Branch conditionally on results
- Loop over collections (files, components, services)
- Fork into parallel execution paths

These workflows are fully visual. You see every node, every data flow, every decision point. They drive **every phase**: development, testing, debugging, and deployment.

### Semantic Intelligence

VL-Code doesn't just store files — it understands them:

- **Real-time symbol index** — every declaration, reference, and variable tracked across the project
- **Dependency graph** — automatic context loading based on file relationships
- **Blueprint architecture** — living PRD, service map, and data model injected into every AI interaction
- **Auto-fix engine** — syntax issues detected and repaired automatically

### Developer Experience

- Go-to-definition across all VL file types
- Find all references for any symbol
- Context-aware autocomplete
- Impact analysis before renaming or removing
- Session persistence across restarts
- Multi-workspace project management

---

## From Development to Production

VisualLogic applications aren't prototypes — they are production-ready:

1. **Develop** — full IDE experience with AI generation and visual editing
2. **Compile** — submit to the VL compiler, get instant preview URLs
3. **Test** — automated browser-based verification via Playwright integration
4. **Debug** — visual workflow tracing and impact analysis
5. **Deploy** — publish to the VL Platform for production access

---

## Open Source Strategy

| Layer | Status |
|-------|--------|
| VL Language Specification | Open |
| VL Grammar & Parser | Open |
| VL to Graph Mapping | Open |
| AI Optimization Patches | Closed |
| AI IDE (VL-Code) | Closed |
| Graphic IDE | Closed |
| Cloud Compiler | Closed |

The VL language specification and core parsing infrastructure are open, enabling the community to build tools, integrations, and extensions around the VL ecosystem.

---

## Technical Stack

- **Runtime**: Node.js + Express with SSE streaming
- **Editor**: CodeMirror 5 (locally served, no CDN dependency)
- **AI**: Anthropic Claude with prompt caching (47K-token VL spec)
- **Architecture**: 4-segment prompt design for optimal token efficiency
- **Integration**: MCP (Model Context Protocol) compatible, extensible skills system

---

## Who Is VisualLogic For?

| Audience | Value |
|----------|-------|
| **AI-native developers** | Leverage deterministic AI generation for rapid full-stack development |
| **Non-technical creators** | Build real applications through visual interfaces and natural language |
| **Enterprise teams** | Standardize on a structured, auditable, AI-native development platform |
| **Researchers** | Explore deterministic AI programming and language-graph reversibility |
| **Platform builders** | Build on open VL specifications for custom tooling and integrations |

---

## Get Started

- **Web Editor**: [editor.visuallogic.ai](https://editor.visuallogic.ai/)
- **Community**: [Discord](https://discord.com/invite/KdaVtR7pzv)
- **Tutorials**: [YouTube Playlist](https://www.youtube.com/playlist?list=PLJE6c8wBknRnCZIRv_VFa1dYswTSqoW21)

---

<p align="center">
  <strong>The future of programming is deterministic AI + visual systems.</strong><br/>
  <em>Build anything. Ship everything.</em>
</p>
