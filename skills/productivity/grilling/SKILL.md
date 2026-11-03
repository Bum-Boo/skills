---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled: the questions you can ask _now_ without guessing at answers you haven't heard yet. Take up to five consequential independent frontier questions in one batch and give a recommended answer where useful. Wait for the batch's answers before reasoning about the next round.

## Structured interview rounds

When the host provides `clarify` or an equivalent structured question tool, collect **up to five independent frontier decisions in one tool call**. Prepare the batch before asking; do not call the tool separately for each independent question and do not run question calls in parallel. The interface may show one question at a time, but selecting or typing an answer is local form progress: wait for the complete batch result before reasoning about the next round.

- Use `questions=[...]`, with 1–5 entries. Prefer one batch of five when five consequential independent decisions are ready; do not invent filler to reach five.
- Put a concise question in `question` and selectable options only in `choices` (up to four). Put the recommended option first. Do not repeat the same question/options in assistant prose or print a separate Markdown questionnaire before calling the tool.
- Omit `choices` for an open-ended answer; use `multi_select=true` only when multiple choices can jointly answer the question. Users may select options or type their own answer within the same batch.
- Never interpret an intermediate selection as the whole interview answer. Process every returned entry by its answered/skipped/unanswered status. A cancelled or timed-out form is not consent and missing answers must not be invented.
- After the batch returns, consolidate the decisions once and advance only the genuinely unresolved frontier. Do not re-ask settled decisions, restart an already prepared batch, or spend a model turn acknowledging each field.
- Questions that depend on an answer still pending belong to a later batch. If more than five independent decisions are ready, take the five with the greatest effect on scope, risk, or design first and retain the rest for the next batch.
- Numeric navigation and Enter confirmation are UI operations, not additional model calls. Do not bypass the structured form by asking the user to type numbered answers into ordinary chat when the form is available.

If the host has no structured question tool, put the same 1–5-question batch in one message, with concise numbered questions and a recommended answer for each, then wait for one consolidated reply. Do not fall back to one model round trip per independent question.

Each round the user answers reshapes the tree: settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier and ask the next round. A question whose answer depends on another question still open in this round belongs to a _later_ round, not this one.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent to find it; don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report; ask the rest of the frontier now. The _decisions_ are the user's: put each to them and wait.

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Do not act on it until the user confirms you have reached a shared understanding.
