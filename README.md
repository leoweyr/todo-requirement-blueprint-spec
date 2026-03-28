# TODO Requirement Blueprint (TRB) Specification

[![Version](https://img.shields.io/badge/version-1.1.0-blue.svg)](#) [![License](https://img.shields.io/badge/license-MIT-green.svg)](#)

**Eradicate over-engineering. A declarative, executable blueprint specification to align business strategy with system architecture through a strict Demand-Pull model.**

## ⚠️ The Problem: Why Do We Need TRB?

We are exhausted by the mountains of fragmented tasks in Jira, and we are completely done with static architecture diagrams (UML / C4) that hopelessly decouple from the actual codebase the moment they are drawn.

In traditional development paradigms, it is perilously easy for engineers to fall into the **"Technology Push"** trap: blindly constructing highly-available microservices or complex event buses, only to realize later that no upstream business logic actually needs them to generate cash flow.

The TRB (TODO Requirement Blueprint) specification was forged in the extreme survival environment of the Solopreneur and agile teams. It is not merely a JSON/YAML structure; it is the ultimate embodiment of the **"Architecture-as-Code"** philosophy at the commercial strategy layer.

## 🧠 Core Philosophy

The TRB specification is built upon immutable laws of architectural physics:

### 1. The Demand-Pull Model

In the TRB universe, **infrastructure nodes are absolutely forbidden to exist in a vacuum**. The architectural perspective must always **"look upstream from the downstream."** In practice, demand is only generated from downstream business touchpoints (the nodes closest to the customer and revenue), which "pull" requirements from underlying infrastructure. If an infra node loses all incoming edge connections, it is immediately classified as a "zombie asset" and must be terminated.

### 2. The Value Chain Gravity

Every node must be anchored within a strict `layer` coordinate system:

- **`touchpoint`**: Closest to the customer and cash flow. Highly volatile and uncertain.
- **`domain`**: Core business logic and rules. Relatively stable.
- **`infra`**: Pure cost centers. Must pursue absolute automation and determinism. This guarantees that when you look at the blueprint, you aren't just seeing API calls—you are directly observing the **distribution of Return on Investment (ROI)**.

### 3. Eternal Demand, Evolving Edges

In traditional graph databases, lines merely represent dependencies. In the TRB specification, **Edges are composed \*inside\* the downstream node as first-class citizens.** The "demand description" of an edge is **strictly immutable** once initialized. However, the technical implementation used to satisfy that demand (evolving from a manual script to a cloud-native service) is meticulously recorded in the edge's **`history` array**.

### 4. Developer Experience (DX) First

Engineered for human architects, TRB natively embraces YAML anchors (`&`) and aliases (`*`). Through the `node_statuses` and `edge_evolution_reasons` global dictionaries, TRB achieves "define once, reference everywhere" for domain-specific vocabularies, ensuring the blueprint remains DRY (Don't Repeat Yourself) and beautifully readable.

## 📐 The Specification Anatomy

### I. The Global Dictionaries

The ontology of your architecture. It defines the reusable enumeration items for the entire blueprint.

```yaml
# yaml-language-server: $schema=https://raw.githubusercontent.com/leoweyr/todo-requirement-blueprint-spec/master/schemas/v1.1.0/trb.schema.json

node_statuses:
    automated: &NODE_STATUS_AUTOMATED
        name: "AUTOMATED"
        description: "Fully automated cash-printing node, zero human intervention."
    scripted: &NODE_STATUS_SCRIPTED
        name: "SCRIPTED"
        description: "Semi-automated via scripts. Carries technical debt."

edge_evolution_reasons:
    initial_mvp: &EDGE_EVOLUTION_REASON_INITIAL_MVP
        name: "INITIAL_MVP"
        description: "Fast iteration to validate market fit using cheapest tools."
    cost_reduction: &EDGE_EVOLUTION_REASON_COST_REDUCTION
        name: "COST_REDUCTION"
        description: "Architectural pivot to reduce server or API costs."
```

### II. Node Anatomy

A strategic/technical module with a strong evolutionary lifecycle. Notice the use of `description` instead of `name`, and the strictly static `metadata`.

```yaml
nodes:
  - id: "touchpoint-discord-growth-bot"
    description: "Core business engine: Discord automated community engagement."
    version: "v1.2.0"
    updated_at: "2026-02-25T14:00:00Z"
    
    # Referenced from global dictionary.
    status: *NODE_STATUS_AUTOMATED
    
    # Flexible but strictly for STATIC architectural context. No runtime data!
    metadata:
        layer: "touchpoint"
        # This is a sample URL for demonstration purposes only.
        repo_url: "https://github.com/leoweyr/discord-growth-bot"
    
    edges: [ /* See Section III */ ]
```

### III. Edges & Evolutionary History

The soul of TRB. Edges represent the downstream node's "demand" from upstream components. Enums are strictly `SCREAMING_SNAKE_CASE`.

```yaml
    edges:
      - id: "edge-storage-demand"
        # Immutable demand: The eternal business pain point.
        demand_description: "Requires high-availability event storage for user profiles."
        
        # Evolutionary History: How this pain point was solved over time.
        history:
          - version: "v1.0.0"
            updated_at: "2026-01-01T10:00:00Z"
            type: "REQUIRES"
            status: "CUT"
            target_upstream_id: "infra.db.local_sqlite"
            evolution_reason: *EDGE_EVOLUTION_REASON_INITIAL_MVP
              
          - version: "v2.0.0"
            updated_at: "2026-02-25T14:00:00Z"
            type: "REQUIRES"
            status: "ACTIVE"
            target_upstream_id: "infra.db.pg_event_store"
            # Reusing the dictionary definition!
            evolution_reason: *EDGE_EVOLUTION_REASON_COST_REDUCTION
```

## 🚀 Use Cases: What Can You Build With TRB?

Once your architecture is governed by the TRB specification, your system unlocks formidable engineering automation capabilities:

- **Auto-Rendering Strategic Maps:** Feed TRB files into front-end IDE workspaces (like React Flow) to automatically render vertical heights based on `layer` metadata, and node traffic lights based on `status`.
- **"Zombie Infra" Alarms:** Write a minimalist CI/CD pipeline to scan your blueprints. If an infra node has zero `ACTIVE` edges pointing to it, instantly trigger alerts to cut cloud billing, mercilessly intercepting capital bleed caused by over-engineering.
- **Architectural Time Travel:** By parsing the timestamps in the `edges.history` array, you can drag a slider in your UI to watch your architecture evolve—playing back the exact moments your system transformed from a pile of manual scripts (v1.0) into a decoupled, event-driven matrix (v3.0).

## 🧩 Build Tooling with [`@todo-requirement-blueprint/domain`](https://github.com/leoweyr/todo-requirement-blueprint-domain)

> [!TIP] 
>
> You can use this repository as the specification and schema source of truth, while using `@todo-requirement-blueprint/domain` as a TypeScript domain layer in your own TRB tooling stack.

If you're building TRB applications in TypeScript (CLI tools, visual editors, CI validators, architecture bots), [`@todo-requirement-blueprint/domain`](https://github.com/leoweyr/todo-requirement-blueprint-domain) is a recommended option:

```bash
npm install @todo-requirement-blueprint/domain
```

Using this package can help your app treat TRB files as strongly-typed domain objects instead of ad-hoc JSON/YAML structures. In practice, this may give you:

- **Type-safe modeling** for nodes, edges, history, and enums across your codebase.
- **A shared domain language** across parsers, validators, renderers, and automation workflows.
- **Lower maintenance cost** by avoiding repeated hand-written interfaces in each tool project.

Recommended integration flow:

1. Parse YAML/JSON input from TRB files.
2. Convert or map data into the domain package's model types.
3. Implement your tooling features (graph generation, audits, migration checks, CI guards) against that domain model.
4. Keep schema validation (`schemas/<version>/trb.schema.json`) as the contract boundary for external blueprint files.

## 📜 Schema Validation

The rigorous JSON Schema enforcing these rules is located at `schemas/<version>/trb.schema.json`.

By adding the `$schema` comment to the top of your `.yaml` files, you instantly unlock flawless IntelliSense, auto-completion, and syntax validation in VS Code, WebStorm, and other modern IDEs.

> *Built with ❤️ for solitary developers and those who refuse to over-engineer.*
