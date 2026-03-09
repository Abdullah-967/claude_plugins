# Visual Patterns Reference

This document defines the visual types available for course diagrams and when to use each. Each pattern includes selection criteria, layout guidance, and a Mermaid example.

---

## Concept Map

**Use when**: The topic has 5+ interconnected ideas that don't follow a strict hierarchy or sequence.

**Mermaid type**: `graph TD` or `graph LR`

**Layout**: Radial or network. Place the central concept at the top/left, with related concepts branching out.

**Best for**: Showing how ideas relate to each other. Ideal for topic overviews, vocabulary webs, and "big picture" diagrams.

**Example**:
```mermaid
graph TD
    OOP[Object-Oriented Programming] --> Classes
    OOP --> Objects
    OOP --> Inheritance
    OOP --> Polymorphism
    Classes -->|creates| Objects
    Inheritance -->|enables| Polymorphism
```

---

## Hierarchy Diagram

**Use when**: The topic has categories, types, or levels. Information flows from general to specific.

**Mermaid type**: `graph TD`

**Best for**: Taxonomies, classification systems, abstraction layers, organizational structures.

**Example**:
```mermaid
graph TD
    ML[Machine Learning] --> Supervised
    ML --> Unsupervised
    ML --> Reinforcement
    Supervised --> Classification
    Supervised --> Regression
    Unsupervised --> Clustering
    Unsupervised --> DimReduction["Dimensionality Reduction"]
```

---

## Flowchart

**Use when**: The topic involves a process, decision-making, or sequential steps. Order matters.

**Mermaid type**: `flowchart TD` (use `{}` for decision diamonds)

**Best for**: Algorithms, workflows, decision trees, troubleshooting guides, step-by-step procedures.

**Example**:
```mermaid
flowchart TD
    Start([Need to store data]) --> D1{Need ordering?}
    D1 -->|Yes| P1[Use Array/List]
    D1 -->|No| D2{Need fast lookup?}
    D2 -->|Yes| P2[Use Hash Map]
    D2 -->|No| P3[Use Tree]
```

---

## Before/After Comparison

**Use when**: The topic involves transformation or contrasting approaches.

**Mermaid type**: `graph LR` with parallel branches

**Best for**: Paradigm shifts, improvement patterns, misconception correction.

**Example**:
```mermaid
graph LR
    subgraph Before ["Synchronous (Blocking)"]
        B1[Task 1 runs] --> B2[Wait] --> B3[Task 2 runs] --> B4[Wait]
    end
    subgraph After ["Asynchronous (Non-blocking)"]
        A1[Task 1 starts] --> A2[Task 2 starts immediately]
        A2 --> A3[Both complete in parallel]
    end
```

---

## System Diagram

**Use when**: The topic has interacting components or modules. Focus is on architecture and data flow.

**Mermaid type**: `graph TD` or `graph LR` with `subgraph` blocks

**Best for**: Architectures, ecosystems, multi-part systems, pipeline designs.

**Example**:
```mermaid
graph TD
    subgraph Frontend
        Browser --> UI["React UI"]
    end
    subgraph Backend
        API["API Server"] --> Auth["Auth Service"]
    end
    subgraph Data ["Data Layer"]
        DB[(Database)]
        Cache[(Cache)]
    end
    UI -->|HTTP| API
    API -->|check| Cache
    API -->|query| DB
```

---

## Causal Chain

**Use when**: The topic has cause-effect relationships. You want to show "why" sequences or chain reactions.

**Mermaid type**: `graph LR`

**Best for**: Explaining "why" sequences, domino effects, feedback loops, root cause analysis.

**Example**:
```mermaid
graph LR
    A["Money supply increases"] -->|excess liquidity| B["More dollars chase same goods"]
    B -->|"demand > supply"| C["Prices rise"]
    C -->|purchasing power falls| D["Workers demand higher wages"]
    D -->|labor costs rise| E["Production costs increase"]
    E -->|cost-push| C
```

---

## Selection Guide

| Learner Level | Recommended Visual Types | Max Nodes |
|---|---|---|
| Beginner | Concept Map (simple), Before/After | 5 |
| Novice | Concept Map, Hierarchy, Before/After | 7 |
| Intermediate | Flowchart, System Diagram, Concept Map | 10 |
| Advanced | System Diagram, Causal Chain, Flowchart | 14 |
| Expert | Causal Chain (with feedback), System Diagram (multi-layer) | 20 |

## Mermaid Syntax Reminders

- Use `<br>` for line breaks inside node labels (NOT `\n`)
- Quote node text with special characters: `A["Loss = f(x)"]`
- Decision diamonds use `{}`: `D{Is error low?}`
- Subgraphs for grouping: `subgraph Name ... end`
- Edge labels: `A -->|label text| B`
- Do NOT use `style` or `classDef` — keep diagrams clean
