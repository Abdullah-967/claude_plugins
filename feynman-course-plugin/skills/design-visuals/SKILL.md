---
name: design-visuals
description: "This skill should be used when creating visual diagrams for a course, designing concept maps or flowcharts for educational content, or when the user says 'add visuals', 'create diagrams', or 'make a concept map'. Generates one Mermaid diagram per lesson for native rendering in Notion."
version: 3.0.0
---

# Design Visuals (Mermaid)

Generate **one Mermaid diagram per lesson** to maximize visual explanability. Each diagram is embedded directly in Notion as a `mermaid` code block, which Notion renders natively as a visual diagram — no external tools, screenshots, or image uploads needed.

For detailed visual type specifications, see `references/visual-patterns.md`.

## Inputs

1. **Course content** -- the full structured course output from build-course, including all lessons.
2. **Learner profile** -- the output from diagnose-and-map, particularly teaching depth and learner level.

## Step 1 -- Plan One Visual Per Lesson

For each lesson, determine the single most impactful visual:

1. Read the lesson content and identify the core concept that benefits most from a diagram.
2. Assign a visual type and matching Mermaid diagram kind:
   - **Concept Map** → `graph TD` or `graph LR` (interconnected ideas)
   - **Hierarchy Diagram** → `graph TD` (categories, layers, parent-child)
   - **Flowchart** → `flowchart TD` (sequential process, decision points with `{}`)
   - **Before/After Comparison** → `graph LR` (side-by-side contrasting approaches)
   - **System Diagram** → `graph TD` with subgraphs (interacting components)
   - **Causal Chain** → `graph LR` (cause-effect sequences)
3. Keep to node limits by level: beginner=5, novice=7, intermediate=10, advanced=14, expert=20.
4. Never use the same visual type for consecutive lessons unless the concepts are fundamentally different.

Output: an ordered list mapping each lesson to its visual type, Mermaid kind, and planned nodes.

## Step 2 -- Write the Mermaid Diagrams

For each lesson, write a clean Mermaid diagram following these rules:

### Syntax Rules
- Use `<br>` for line breaks inside node labels (NOT `\n` — Mermaid doesn't interpret `\n` in node text).
- Enclose node text in double quotes when it contains special characters (parentheses, commas, equals signs):
  ```
  A["Loss = f(predicted - actual)"]
  B["Continuous Output<br>(price, temp, score)"]
  ```
- For decision diamonds, use `{}`: `D{Is error low enough?}`
- Prefer descriptive node IDs when possible: `bias[High Bias]` over `A[High Bias]`
- Keep labels concise — max ~6 words per node.

### Layout Rules
- Use `graph TD` (top-down) for hierarchies and processes.
- Use `graph LR` (left-right) for comparisons and timelines.
- Use `flowchart TD` when you need decision diamonds with multiple paths.
- Use `subgraph` blocks to group related concepts (advanced/expert levels only).
- Avoid crossing arrows — restructure the graph to minimize visual clutter.

### Style Rules
- Do NOT add Mermaid `style` or `classDef` directives — keep diagrams clean and let Notion's default rendering handle styling.
- Avoid `:::` class annotations.
- Keep it simple: nodes + arrows + labels. The hand-drawn feel comes from Notion's renderer.

## Step 3 -- Embed in Lesson Content

Place each Mermaid diagram in the lesson markdown at the correct position:

1. After the "Key Concepts" bulleted list.
2. Before the "Intuitive Analogy" section.
3. With only a brief italic caption below the code block (e.g., `*Concept map showing how regression fits within supervised learning.*`).

**DO NOT:**
- Add a "## Diagram" section heading — the mermaid block should flow naturally.
- Add explanatory text above the diagram describing what it shows.
- Duplicate the diagram content as a text description.

The mermaid code block IS the visual. Notion renders it as a diagram. One representation only.

## Guidelines

- **One diagram per lesson, no exceptions.** Every lesson gets its own visual.
- **Diagrams must be standalone comprehensible.** A learner should grasp the core idea from the diagram alone.
- **Match complexity to level.** Beginners see 5 labeled nodes. Experts see 20 with subgraphs.
- **Use consistent node naming across a course.** If "Regression" is a node in Lesson 1, use the same label in Lesson 5 if it reappears.
- **Test Mermaid syntax mentally.** Common pitfalls: unquoted special chars, missing `end` for subgraphs, `\n` instead of `<br>`.
- Every diagram must be understandable without reading the lesson text. Include enough labels.
- For beginners, annotate heavily with descriptive labels. For experts, let structure speak.
- Never use decorative elements that do not carry information.
