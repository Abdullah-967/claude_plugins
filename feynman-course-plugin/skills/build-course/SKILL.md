---
name: build-course
description: "This skill should be used when generating lesson content from a learner profile, building a structured course with analogies and exercises, or when the user says 'build the course', 'create lessons', or 'write the course content'. Typically invoked after diagnose-and-map produces a learner profile."
version: 1.0.0
---

# Build Course

Generate a complete, structured course from a learner profile and topic. Apply the Feynman method throughout: explain every concept as if teaching a bright 12-year-old, using plain language, concrete analogies, and real-world examples before introducing formal terminology. The output is a full content package ready for the publish-course skill to push to Notion.

For the lesson page template, see `references/lesson-template.md`. For the course home page template, see `references/course-template.md`.

## Inputs

Accept the following:

1. **Learner profile** — the structured output from the diagnose-and-map skill, containing topic, level, depth, prerequisites, misconceptions, and learning objectives.
2. **Topic decomposition** (optional) — if the caller provides a breakdown of subtopics, use it. Otherwise, decompose the topic yourself based on the learner profile.

## Step 1 — Determine Lesson Count and Structure

Choose a lesson count between 3 and 7 based on these factors:

- **Topic breadth**: more subtopics warrant more lessons.
- **Teaching depth**: deeper depth means more time per concept, which may mean fewer but longer lessons.
- **Learner level**: beginners benefit from smaller steps (more lessons, each covering less). Advanced learners can handle denser lessons.

Map out lesson titles and a one-sentence scope statement for each. Ensure each lesson has a clear, single focus — never combine two unrelated concepts in one lesson. Order lessons so that each one builds on the previous. The first lesson should always ground the learner in the big picture before diving into specifics.

## Step 2 — Write Each Lesson

For every lesson, generate the following sections in order:

### Title and Overview
Write a plain-English overview (3-5 sentences) that tells the learner what they will understand by the end of this lesson and why it matters. Address the learner directly using "you".

### First-Principles Breakdown
Decompose the lesson's core concept into its fundamental building blocks. Start from what the learner already knows (reference the prerequisites-known list) and build upward. Each step should follow logically from the previous one. Use short paragraphs (2-4 sentences each). Never make logical leaps without bridging them explicitly.

### Analogies
Provide at least one analogy per lesson. Good analogies:
- Map the unfamiliar concept to something from everyday life.
- Preserve the structural relationship between parts, not just surface similarity.
- Include a brief note on where the analogy breaks down, so the learner does not over-extend it.

For complex lessons, provide two analogies approaching the concept from different angles.

### Concrete Examples
Include at least one real-world, specific example per lesson. Avoid abstract or hypothetical scenarios when a real one exists. If the topic is technical, show a minimal code snippet, configuration, or calculation — but always explain what the example demonstrates in plain language before and after the technical content.

### Why This Matters
Write 2-3 sentences connecting this lesson's content to the learner's goal (from the profile). Answer the question: "Why should I care about this specific concept?" Make the connection practical, not philosophical.

### Checkpoint Question
Write one question that tests whether the learner understood the lesson's core concept. The question should:
- Be answerable in 2-3 sentences.
- Require understanding, not just recall (prefer "explain why" over "what is").
- Include a brief model answer in a collapsed/toggle section.

## Step 3 — Address Misconceptions

Weave misconception corrections into the relevant lessons rather than dumping them in a separate section. For each misconception from the learner profile:

1. Identify which lesson is the natural place to address it.
2. Present the misconception as a "common trap" or "watch out" callout within that lesson.
3. Explain why the incorrect belief is wrong and what the correct understanding is.
4. If a misconception does not fit naturally into any lesson, include it in the FAQ page instead.

## Step 4 — Generate Supporting Materials

### Glossary
Compile every technical term introduced across all lessons into an alphabetical glossary. Each entry has:
- The term in bold.
- A plain-English definition (one sentence).
- The lesson number where it was first introduced.

### Cheat Sheet
Create a one-page quick reference containing:
- The 5-10 most important facts or rules from the course.
- Any key formulas, commands, or patterns.
- A decision flowchart or checklist if applicable to the topic.

Keep it scannable — use bullet points, short phrases, and tables. The learner should be able to tape this to their wall.

### FAQ / Misconceptions Page
List 5-8 frequently asked questions about the topic. Include the mapped misconceptions rephrased as questions (e.g., "Isn't X just the same as Y?"). Provide concise, clear answers. Cross-reference relevant lesson numbers.

### Practice Exercises
Create 3-5 exercises of escalating difficulty:
1. **Warm-up**: a straightforward application of a single concept from the course.
2. **Integration**: combine two or more concepts from different lessons.
3. **Challenge**: a problem that requires the learner to extend what they learned to a new situation.

For each exercise, provide:
- The problem statement.
- Hints (in a collapsible section).
- A full solution with explanation (in a collapsible section).

## Step 5 — Final Review Pass

Before outputting the course, verify:

- Every learning objective from the profile is addressed by at least one lesson.
- Every prerequisite gap is either covered in lesson content or flagged as "review this before starting."
- No lesson uses a term before it has been defined.
- Progressive complexity is maintained — a random reader dropping into lesson 4 should not encounter unexplained concepts from lesson 2.
- The total estimated reading time is reasonable: 5-15 minutes per lesson for surface/working depth, 10-25 minutes for deep/expert.

## Output Format

Return the complete course as a structured document with clearly separated sections. Use markdown headings to delineate lessons and supporting materials. The publish-course skill will parse this structure to create individual Notion pages, so maintain consistent heading levels:

- `# Course Title` — top level
- `## Lesson N: Title` — one per lesson
- `### Section Name` — sections within lessons
- `## Glossary`, `## Cheat Sheet`, `## FAQ`, `## Practice Exercises` — supporting materials

## Guidelines

- Write in second person ("you") throughout lesson content. Use present tense.
- Keep sentences short. Aim for a Flesch-Kincaid grade level of 8-10 for surface depth, 10-12 for working, 12-14 for deep.
- Never say "simply" or "just" — these words make learners feel bad when they struggle.
- Bold key terms on first use and define them inline before adding them to the glossary.
- Use numbered lists for sequential processes and bullet lists for non-ordered collections.
- If the topic involves code, use fenced code blocks with language hints. Always explain the code; never let code stand alone without commentary.
- Estimate and note the reading time at the top of each lesson.
