---
name: publish-course
description: "This skill should be used when publishing completed course content to Notion, creating a structured page hierarchy for lessons and supporting materials, or when the user says 'publish to Notion', 'push the course', or 'create the workspace'. Requires output from build-course."
version: 2.0.0
---

# Publish Course

Take the complete course content from build-course and publish everything to Notion as a structured page hierarchy. Mermaid diagrams are embedded directly as code blocks (Notion renders them natively). The result is a self-contained course workspace the learner can navigate, study from, and return to.

## Inputs

Accept the following:

1. **Course content** — the full structured output from build-course, including all lessons with Mermaid diagrams embedded in the markdown.
2. **Notion parent page or database** (optional) — if the learner specifies where to publish, use that location. Otherwise, create the course at the top level of the connected Notion workspace.

## Step 1 — Create the Course Home Page

Use the Notion MCP tools to create a top-level page that serves as the course entry point.

### Home Page Content Structure

The home page content should include ONLY:

1. **Description**: a paragraph with 2-3 sentences summarizing the course.
2. **Learner level**: a callout block indicating the target level.
3. **Learning Objectives**: a heading 2 + numbered list of objectives.
4. A divider.
5. **"## Lessons"** heading — this is a label only. Do NOT populate it with links.
6. **"## Supporting Materials"** heading — same, label only.

**CRITICAL — Avoiding Double-Listing:**

When you create child pages under a parent, Notion automatically adds `<page>` blocks to the parent's content. These blocks ARE the clickable lesson navigation the user sees.

**DO NOT also add `<mention-page>` links, numbered lists with links, or any other navigation pointing to the same child pages.** This causes every page to appear twice — once as your link, once as Notion's auto-generated `<page>` block.

The correct flow:
1. Create home page with overview content + section headings ("## Lessons", "## Supporting Materials").
2. Create lesson child pages — they automatically appear under "## Lessons" as `<page>` blocks.
3. Create supporting child pages — they automatically appear under "## Supporting Materials".

No manual navigation list needed. The `<page>` blocks handle it.

## Step 2 — Create Lesson Pages

For each lesson, create a child page under the course home page. **Process lessons sequentially (Lesson 1 first, then 2, etc.)** to maintain correct page ordering in Notion's sidebar.

Each lesson page must contain:

### Page Title
Set the page title to "Lesson N: [Title]" where N is the lesson number.

### Navigation
At the top: a link back to the course home page.
At the bottom: "Previous Lesson" and "Next Lesson" text links.

### Content Sections
Convert the lesson content into Notion blocks:

1. **Overview** — heading 2 + paragraph.
2. **Key Concepts** — heading 2 + bulleted list.
3. **Mermaid Diagram** — a `mermaid` code block placed directly after Key Concepts. Add a brief italic caption below. Do NOT add a section heading for the diagram.
4. **Intuitive Analogy** — heading 2 + paragraph (or toggle block for optional expansion).
5. **Concrete Examples** — heading 2 + bulleted list or numbered examples.
6. **Why This Matters** — callout block with lightbulb icon.
7. **Mini Checkpoint** — heading 2 "Check Your Understanding" + blockquote with the question.

### Mermaid in Notion — Best Practices

Notion renders ` ```mermaid ` code blocks as interactive diagrams. To keep it clean:

- Place the mermaid block with no heading above it — it flows naturally after Key Concepts.
- Add only a brief italic caption below: `*Caption describing the diagram.*`
- Do NOT describe the diagram content in text — the visual IS the description.
- Use `<br>` for line breaks in node labels (not `\n`).
- Quote node text containing special characters: `A["MSE = (1/n) sum(y - y-hat)^2"]`

## Step 3 — Create Supporting Material Pages

Create child pages under the course home page, **after all lesson pages** (to maintain ordering):

### Glossary Page
- Page title: "Glossary".
- For each term (alphabetically sorted): a toggle block with the term as the title and the definition as the body.
- Include a "Back to Course Home" link at the top.

### Cheat Sheet Page
- Page title: "Cheat Sheet".
- Use heading 3 blocks for categories, bulleted lists for key facts, and table blocks for comparisons.
- Include a "Back to Course Home" link at the top.

### FAQ Page
- Page title: "FAQ & Common Misconceptions".
- For each question: a toggle block with the question as the title and the corrective answer as the body.
- Include a "Back to Course Home" link at the top.

## Step 4 — Verify and Return

Perform a quick verification:

1. Confirm all lesson pages were created and are accessible.
2. Confirm all supporting pages exist.
3. Confirm the course home page does NOT have duplicate listings (no `<mention-page>` links alongside `<page>` blocks).

Return the following:

```
PUBLISH RESULT
==============
Course Home URL:    [Notion URL]
Pages Created:      [total count]
Lessons:            [count]
Supporting Pages:   [count]
Mermaid Diagrams:   [count]
Status:             [success / partial — list any issues]
```

## Guidelines

- **Create pages sequentially**, not in parallel, to ensure correct ordering in Notion's sidebar.
- If a Notion API call fails, retry once. If it fails again, log the error and continue. Report failures in the final status.
- Do not add emoji prefixes to page titles unless the Notion workspace already uses them.
- Keep block count per page reasonable. If a lesson would exceed 100 blocks, use toggle blocks to collapse detail.
- Never delete or modify existing pages in the learner's Notion workspace. Always create new pages.
- If the Notion MCP tools are unavailable, output the complete course content as structured markdown instead.
