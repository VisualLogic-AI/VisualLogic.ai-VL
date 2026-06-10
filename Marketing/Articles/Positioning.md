# VisualLogic Positioning

VisualLogic is a practical AI software generation system for real projects.

Most AI coding tools are optimized for producing a code fragment, a file, or a quick prototype. VisualLogic is designed around the full delivery loop: define intent, structure the project, generate modules, validate each step, compile, preview, refine, and deploy.

## VL: A Language Designed for AI

VL is an AI-native programming language. It is structured, compact, and contract-oriented, so both humans and AI agents can understand what each file and component is responsible for.

The language is organized across clear layers:

- apps
- sections
- components
- services
- database models
- themes

Each layer has a focused responsibility. That decoupling makes the system easier to reason about and much easier to parallelize. Once a service contract is stable, service domains can be generated in parallel. Once component contracts are stable, frontend modules can be generated, checked, and refined independently.

## Component-First Software Generation

In VL, software is assembled from components. A component exposes a clear surface:

- properties
- methods
- events

That small public interface is exactly what AI agents need. They can inspect the contract, call the right method, wire the right event, or adjust the right property without guessing through a large unstructured codebase.

This component-first model also creates a long-term advantage: reusable capability. Components, business modules, UI blocks, service templates, and workflow nodes can become shared engineering assets instead of one-off generated code.

## DAG Harness Engineering

VisualLogic uses DAG workflows as a core Harness Engineering mechanism.

A DAG is a directed acyclic graph. In VisualLogic, the DAG is the visible process that controls the AI work: planning, contract generation, service generation, frontend generation, component search, validation, repair, compile, preview, and packaging.

This is more powerful than asking a single agent to solve one isolated task. A DAG represents the whole engineering process. Each node has a role, each edge defines dependency, and each checkpoint can be inspected. When the graph completes, the project has moved through a complete, auditable workflow rather than a single unpredictable prompt result.

## VLC: VL + DAG + AI Assistant

VLC brings the language, workflow, and assistant into one local IDE.

The built-in AI assistant is project-aware. It can work with VL files, understand project context, operate inside DAG workflows, and help users move from intent to a real generated application.

This combination is the key product idea:

- VL gives AI the right representation.
- DAG gives AI the right process.
- The AI assistant gives users an interactive control surface.
- VLC puts all three into one IDE for real project delivery.

## Who It Helps

VisualLogic is designed for professional teams, enterprise projects, and non-programmers who still need real software.

A project manager or domain expert can describe what should be built, inspect the generated structure visually, guide the DAG, and refine the result with the assistant. Developers can still inspect and improve the generated project, but writing code is no longer the only entry point to software creation.

## Current Access

The current IDE is free to use. Users can try the system, inspect the language, reuse open resources, and build with the available public assets.
