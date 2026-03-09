---
name: mastery-check
description: "This skill should be used when a learner requests a quiz or test on material they have studied, saying things like 'test me', 'quiz me', 'check my understanding', or 'run a mastery check'. Administers an adaptive 5-question assessment covering recall, explanation, application, misconception detection, and confidence calibration."
version: 1.0.0
---

# Mastery Check

Administer an adaptive 5-question assessment to evaluate a learner's understanding of a previously studied topic. This skill activates only when the learner explicitly requests testing — never trigger it automatically after course delivery. The assessment follows a structured question sequence designed to probe different depths of understanding, from basic recall through application and critical evaluation.

For the detailed scoring rubric with per-question point values, see `references/rubric.md`.

## Activation Guard

Before proceeding, confirm that the learner has explicitly asked for a mastery check, quiz, test, or assessment. Valid triggers include phrases like:

- "Test me on this"
- "Check my understanding"
- "I want a mastery check"
- "Quiz me"
- "Am I ready?"

If the learner has not explicitly requested testing, do not proceed. Instead, ask: "Would you like me to test your understanding of [topic]? I can run a 5-question mastery check whenever you are ready."

## Inputs

Accept or reconstruct the following:

1. **Course topic** — the subject the learner studied.
2. **Learning objectives** — the specific objectives from the learner profile. If the original profile is not available in context, reconstruct reasonable objectives from the topic.
3. **Lesson summaries** — brief descriptions of what each lesson covered. Used to map gaps back to specific lessons for remediation.
4. **Notion course URL** (optional) — if available, used to link remediation recommendations to specific lesson pages.

## Step 1 — Prepare the Assessment

Generate all five questions before presenting any of them. This ensures the full assessment covers the breadth of the course material. However, present questions one at a time to the learner — never reveal all questions upfront, as this allows the learner to preview and pre-plan answers, which undermines authentic assessment.

Design each question to target a different lesson or learning objective when possible. Avoid testing the same concept twice.

## Step 2 — Administer Questions

Present each question in sequence. After the learner answers each one, provide brief feedback before moving to the next question. Use this exact sequence:

### Question 1 — Recall
Test whether the learner can retrieve a key concept from memory without any hints or context.

Format: "From what you studied about [topic], [ask them to state, list, or define a core concept]."

Do not provide multiple choice options. The learner must produce the answer from memory. Accept answers that are substantively correct even if they use different wording than the course material.

Feedback: State whether the answer is correct or incorrect. If incorrect, provide the correct answer in one sentence. If partially correct, acknowledge what they got right and fill in what was missing.

### Question 2 — Explain It Simply
Test whether the learner has internalized the concept well enough to teach it. This is the core Feynman test.

Format: "Explain [concept] as if you were teaching a friend who has never heard of it. Use plain language — no jargon."

Evaluate the explanation on three criteria:
- **Accuracy**: are the core facts correct?
- **Clarity**: would a non-expert understand this?
- **Completeness**: are the essential elements present?

Feedback: Rate their explanation (strong / adequate / needs work) and note any inaccuracies or missing elements. If they used jargon without explaining it, point that out — it signals they may be reciting rather than understanding.

### Question 3 — Application
Test whether the learner can apply their knowledge to a concrete, realistic scenario they have not seen before.

Format: Present a short scenario (2-4 sentences) that requires the learner to use a concept from the course to solve a problem, make a decision, or predict an outcome. The scenario should be different from any examples used in the course material.

Feedback: Evaluate the reasoning process, not only the final answer. A learner who reaches the wrong conclusion through sound reasoning has more mastery than one who guesses the right answer. Explain the correct approach step by step.

### Question 4 — Misconception Trap
Test whether the learner has overcome the common misconceptions identified in the learner profile.

Format: Present a statement that sounds plausible but is incorrect (drawn from the mapped misconceptions). Ask: "True or false: [plausible but incorrect statement]. Explain your reasoning."

The statement must be genuinely tempting — it should be something that someone with a surface-level understanding would accept as true. Requiring them to explain their reasoning prevents lucky guesses from inflating the score.

Feedback: If they correctly identified the statement as false and gave a sound explanation, confirm and reinforce why it is false. If they fell for the trap, explain the misconception clearly and reference which lesson addressed it.

### Question 5 — Confidence Rating
Shift from testing knowledge to testing metacognition — the learner's awareness of what they do and do not know.

Format: List the 3-5 main subtopics from the course. Ask the learner to rate their confidence on each from 1 (not confident at all) to 5 (could teach this to someone else). Then ask them to identify which single subtopic they feel least confident about and explain why.

This question is not scored as correct/incorrect. Instead, use the learner's self-assessment to:
- Identify mismatches between confidence and actual performance (e.g., high confidence but wrong answers on Questions 1-4 suggests overconfidence on that subtopic).
- Identify areas the learner knows they need to revisit.

Feedback: Acknowledge their self-assessment. If you noticed a mismatch between their confidence and their performance, gently flag it: "You rated [subtopic] as a 4, but your answer to Question 3 suggests there may be a gap in [specific area]. Consider reviewing Lesson N."

## Step 3 — Score and Summarize

After all five questions are complete, compile the results.

### Scoring
Score each question on a 100-point scale with the following point values:
- **Question 1 (Recall)**: 20 points
- **Question 2 (Explain Simply)**: 25 points
- **Question 3 (Application)**: 25 points
- **Question 4 (Misconception Trap)**: 20 points
- **Question 5 (Confidence Rating)**: 10 points
- **Total**: 100 points

For each question, award full points for a correct answer with sound reasoning, partial points for a partially correct answer or correct conclusion with flawed reasoning, and zero points for an incorrect answer.

### Mastery Levels
Map the total score to a level:
- **90-100**: Mastery achieved. The learner has internalized the material.
- **70-89**: Strong understanding with minor gaps.
- **50-69**: Developing. Key concepts need reinforcement.
- **Below 50**: Needs significant review. Re-study recommended.

### Results Report

Present the results in this format:

```
MASTERY CHECK RESULTS
=====================
Topic:               [topic]
Score:               [X / 100]
Mastery Level:       [level]

STRENGTHS
- [specific concept or skill they demonstrated well]
- [another strength]

GAPS IDENTIFIED
- [specific concept or skill that needs work]
- [another gap]

RECOMMENDATIONS
- [specific action, e.g., "Revisit Lesson 3 on [topic] — focus on [specific section]"]
- [another recommendation]
- [link to Notion lesson page if available]
```

## Step 4 — Remediation Guidance

Based on the mastery level, provide tailored next steps:

- **90-100%**: Congratulate the learner. Suggest advanced topics or related subjects they could explore next. No remediation needed.
- **70-89%**: Identify the specific gaps and recommend re-reading the relevant lesson sections. Suggest they try the practice exercises related to their weak areas.
- **50-69%**: Recommend re-studying the specific lessons where gaps were found. Suggest they focus on the analogies and examples in those lessons before attempting the technical content again. Offer to re-test after they have reviewed.
- **Below 50%**: Recommend restarting from the lesson where the first gap appeared. Suggest working through the course more slowly, pausing after each lesson to self-test with the checkpoint questions. Offer encouragement — struggling is a normal part of learning, not a sign of inability.

## Guidelines

- Keep the tone encouraging throughout. Testing is a learning tool, not a judgment. Use phrases like "Let's see where you are" rather than "Let's see if you pass."
- Never present all five questions at once. The sequential presentation with feedback after each question turns the assessment into a learning experience, not just an evaluation.
- Adapt question difficulty based on the learner's stated level. A beginner should get application questions that are straightforward. An advanced learner should get application questions that involve edge cases or trade-offs.
- If the learner asks to skip a question, allow it but score it as 0 points. Do not penalize them emotionally — simply note the skipped topic as an area to review.
- If the learner disputes a scoring decision, engage with their reasoning. If they make a valid point, adjust the score. The goal is accurate assessment, not rigid grading.
- Time spent on the mastery check should not exceed 15 minutes. Keep questions concise and feedback focused.
- If course context is unavailable (the learner asks for a mastery check on a topic without having used the course builder), construct reasonable questions from general knowledge of the topic. Note in the results that the assessment was based on general coverage rather than a specific course.
