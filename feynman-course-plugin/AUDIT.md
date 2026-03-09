# Audit: feynman-course-plugin

Scope: static audit of prompt/config assets. No runtime tests were run; the repo is prompt-heavy and does not include automated tests.

## Findings

### Critical

1. Hardcoded Notion API key is committed
- Evidence: `.mcp.json:6-7` contains a live-looking `NOTION_API_KEY`, `.gitignore:1-2` does not exclude `.mcp.json`, and `README.md:32,38` says the key should come from the environment.
- Why it matters: this leaks a workspace credential to anyone with repo access and creates two competing auth sources. Rotate the exposed key and load it from the environment only.

### High

2. The advertised `/feynman-check` command does not exist
- Evidence: `commands/feynman.md:182` tells users to run `/feynman-check [topic]`, but the repo only ships `commands/feynman.md`.
- Why it matters: the documented post-course flow is broken unless the host has undocumented routing from plain text back into the `mastery-check` skill.

3. The lesson schema produced by `build-course` does not match what downstream skills expect
- Evidence: `commands/feynman.md:79-84` and `skills/publish-course/SKILL.md:62-68` require `Key Concepts`, a Mermaid block after that list, `Intuitive Analogy`, and `Mini Checkpoint`. `skills/build-course/SKILL.md:37-58` instead defines `First-Principles Breakdown`, `Analogies`, and a checkpoint with a model answer, while `skills/build-course/references/lesson-template.md:15-35` uses `The Big Idea`, `Think of It Like...`, `In Practice`, and `Key Terms`.
- Why it matters: `design-visuals` and `publish-course` are being asked to place or parse sections that `build-course` is not actually instructed to emit. That makes the orchestration brittle and highly sensitive to prompt interpretation.

4. The Notion publishing workflow is internally inconsistent
- Evidence: `commands/feynman.md:120-144` says child pages appear at the bottom of the home page and therefore `## Supporting Materials` should be added only after lesson pages exist. `skills/publish-course/SKILL.md:30-42` instead says to create both headings up front and assumes lessons/support pages will land under the correct heading automatically. The same workflow also requires sequential creation plus `"Previous Lesson"` and `"Next Lesson"` links (`commands/feynman.md:146-152`, `skills/publish-course/SKILL.md:48-57`) without a later update pass.
- Why it matters: one of these instructions is wrong. As written, section placement and next-link generation are not reliably satisfiable in a single publish pass.

### Medium

5. Teaching-depth calibration already drifts between the main skill and its reference file
- Evidence: `skills/diagnose-and-map/SKILL.md:29-33` maps novice/intermediate/advanced to `Surface+`/`Working`/`Deep`, while `skills/diagnose-and-map/references/learner-profiles.md:25,37,49` maps the same levels to `Working`/`Deep`/`Expert`.
- Why it matters: the skill explicitly says to load the reference file, so the same learner input can yield materially different lesson depth and visual complexity depending on which instruction wins.

6. Supporting-material contracts drift across files
- Evidence: `skills/build-course/SKILL.md:88-97` and `skills/build-course/references/course-template.md:32-36` require `Practice Exercises`, but `commands/feynman.md:88-92` and `skills/publish-course/SKILL.md:84-97` only create Glossary, Cheat Sheet, and FAQ pages.
- Why it matters: either exercises are silently dropped at publish time or the generated course no longer matches the published workspace shape.

## Recommended Fixes

1. Remove the committed Notion secret, rotate it, and read credentials from env only.
2. Either add `commands/feynman-check.md` or change the user-facing follow-up to a plain-language mastery-check request that the existing agent/skill can handle.
3. Pick one canonical lesson/page schema and make `build-course`, `design-visuals`, `publish-course`, and the command prompt all consume that same contract.
4. Make publishing explicitly two-pass if you want ordered child pages plus previous/next links: create pages first, then patch navigation and any section headings based on the created URLs.
