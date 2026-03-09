---
name: diagnose-and-map
description: "This skill should be used when a user wants to learn a new topic, says 'teach me about X', 'I want to learn Y', or asks for prerequisite analysis, learning path planning, or knowledge gap assessment. It diagnoses the learner's current level, maps prerequisites, identifies misconceptions, and generates learning objectives."
version: 1.0.0
---

# Diagnose and Map

Perform a rapid learner diagnosis and prerequisite mapping for a given topic. The goal is to produce a structured learner profile that downstream skills (build-course, design-visuals) can consume directly. Keep the process fast and assumption-driven rather than interrogative — infer what you can from the stated level and topic, and only ask the learner for clarification when the ambiguity would materially change the course.

For detailed learner level descriptions and teaching depth calibration, load `references/learner-profiles.md`.

## Inputs

Accept two required inputs and one optional input:

1. **Topic** — the subject the learner wants to study (e.g., "Docker networking", "Bayesian inference", "React state management").
2. **Learner level** — one of: beginner, novice, intermediate, advanced, expert.
3. **Goal** (optional) — what the learner wants to accomplish or build with this knowledge. If not provided, infer a reasonable default goal from the topic.

If the learner supplies only a topic with no level, default to "beginner" and note the assumption in the output.

## Step 1 — Determine Teaching Depth

Map the learner level to a teaching depth. Use this table as a baseline, then adjust if the topic's inherent complexity warrants it:

| Learner Level  | Default Depth | Description                                      |
|----------------|---------------|--------------------------------------------------|
| Beginner       | Surface       | Core ideas only, heavy use of analogy, no math   |
| Novice         | Surface+      | Core ideas with light technical vocabulary        |
| Intermediate   | Working       | Mechanisms and trade-offs, moderate detail        |
| Advanced       | Deep          | Edge cases, internals, formal definitions         |
| Expert         | Expert        | Research-level nuance, open questions, caveats    |

If the topic is unusually simple (e.g., "what is a variable"), cap the depth at Working regardless of stated level. If the topic is unusually complex (e.g., "quantum error correction"), raise the floor to Working even for beginners, but compensate with more analogies.

## Step 2 — Map Prerequisites

Enumerate the prerequisite concepts the learner needs before engaging with the topic. Organize them into two lists:

- **Likely known** — concepts a person at the stated level can be assumed to understand. List 3-8 items.
- **Likely gaps** — concepts a person at the stated level probably has not yet mastered but will need. List 2-6 items.

Order both lists in dependency sequence: foundational concepts first, derived concepts later. Each item should be a short phrase (3-8 words) with an optional one-sentence clarification if the concept name alone is ambiguous.

When in doubt about whether something is known or a gap, place it in the gaps list. It is cheaper to briefly review a known concept than to skip a needed one.

## Step 3 — Identify Misconceptions

List 3-5 misconceptions that are common at the stated learner level for this topic. For each misconception:

1. State the incorrect belief in one sentence, phrased the way a learner would say it.
2. State the correction in one sentence.
3. Explain briefly why this misconception forms (what intuition leads people astray).

Prioritize misconceptions that, if left unaddressed, would cause the learner to misunderstand later material. Rank them from most damaging to least damaging.

## Step 4 — Generate Learning Objectives

Produce 3-5 learning objectives that are specific, measurable, and appropriately scoped for the teaching depth. Follow these rules:

- Start each objective with a verb from Bloom's taxonomy appropriate to the depth:
  - Surface: define, identify, describe, list, recognize
  - Working: explain, compare, distinguish, illustrate, predict
  - Deep: analyze, evaluate, justify, construct, derive
  - Expert: critique, synthesize, design, formulate, hypothesize
- Each objective must be testable — it should be clear how you would verify whether the learner achieved it.
- Avoid vague objectives like "understand X" or "learn about Y". Replace with concrete actions: "Explain X in terms of Y", "Predict what happens when Z changes".
- Scope objectives to be achievable within a short course (3-7 lessons, 15-45 minutes each).

## Step 5 — Assemble the Learner Profile

Compile all outputs into a single structured profile using this format:

```
LEARNER PROFILE
===============
Topic:               [topic]
Stated Level:        [level]
Teaching Depth:      [depth]
Goal:                [goal or inferred goal]

PREREQUISITES
Known:
  1. [concept]
  2. [concept]
  ...

Gaps:
  1. [concept]
  2. [concept]
  ...

MISCONCEPTIONS TO ADDRESS
  1. Misconception: [incorrect belief]
     Correction:    [correct understanding]
     Why it forms:  [explanation]
  2. ...

LEARNING OBJECTIVES
  1. [verb] [specific outcome]
  2. [verb] [specific outcome]
  ...

RECOMMENDED APPROACH
  [1-3 sentences on how to structure the course for this learner —
   e.g., "Start with analogy-heavy overview, then introduce formal
   definitions in lesson 3. Address misconception #1 early as it
   blocks understanding of later material."]
```

## Guidelines

- Complete the entire diagnosis in a single pass. Do not ask the learner multiple rounds of questions — that slows the process and adds friction.
- If the learner provides extra context (e.g., "I'm a frontend dev learning backend"), use that context to refine the known-prerequisites list and tailor analogies.
- Keep the profile concise. The downstream build-course skill will expand on everything. The profile is a blueprint, not a textbook.
- When the topic is ambiguous (e.g., "networking" could mean computer networking or professional networking), ask one clarifying question before proceeding. Do not guess.
- Never skip misconception identification. Misconceptions are the single highest-leverage thing to address in teaching — they cause more confusion than missing knowledge does.
- If the learner is at expert level, shift the misconceptions section to "common subtle errors" or "frequently overlooked nuances" — experts have misconceptions too, but they manifest differently.

## Output

Return the completed learner profile as the primary output. This profile will be consumed by the build-course skill to generate lesson content, by design-visuals to determine which diagrams to create, and by mastery-check to generate assessment questions. Ensure it is complete enough to drive all three downstream processes without requiring the learner to re-state their inputs.
