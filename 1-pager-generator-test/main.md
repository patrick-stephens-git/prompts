# Runbook:

## Instructions:
Most steps below run **directly in this conversation** — no subagent. The exceptions are Steps 5–7, where the research-and-tool-calling portion (Copilot/WebSearch lookups) is delegated to a subagent so its noisy tool output doesn't bloat this conversation's context. Even there, every user-facing interaction (canonical-freshness checks, scope questions, the review/refine/save loop) happens directly in this conversation, never inside the subagent — subagents do not reliably sustain multi-turn back-and-forth with the user, and each spawn starts with no memory of earlier steps beyond what is written in `./1-pager-output.md`.

When running a runbook directly, follow its prompts, output templates, and option lists exactly as written — do not summarize, reformat, or rewrite them.

### Canonical File Status ledger
Maintain a session-scoped **Canonical File Status** ledger in this conversation (not written to any file). Every runbook step's Task 1 (Confirm Canonical Freshness) resolves each canonical file it references to `yes` (up to date) or `ignore` (skip it) — record that resolution in the ledger keyed by file path. Any later Task 1, in any step for the rest of this session, must check the ledger first: if a file already has a recorded `yes` or `ignore`, reuse it silently and do not ask about that file again. Only files with no recorded status get asked. The ledger lives only in this conversation and does not carry over to a new session.

Subagents spawned for Steps 5–6 start with no memory of this ledger, so when a canonical file relevant to that step is marked `ignore`, tell the subagent explicitly in its spawn instructions to treat that file as blank and not read it — otherwise it may read the file directly per the runbook's own "also read" instructions, defeating the `ignore`.

### Consistency Check After Each Save
Run this check after any step writes new content to `./1-pager-output.md`, before moving to the next step. Skip this check when the step's Review & Confirm outcome was `skip` or `restart` — no new content was persisted.

1. Re-read the full `./1-pager-output.md` file.
2. Compare the new content against every earlier section.
3. Find any earlier claim, number, or scope statement that now conflicts with the new content, or that no longer applies because the scope has narrowed since it was written.
4. If you find no conflict, say so in one line and proceed to the next step.
5. If you find one or more conflicts, present them in this format (render as markdown, do not rewrite, summarize, or add your own option list):
   > **Consistency check:** the new content conflicts with earlier content.
   > - `{Section name}`: {quoted earlier content} — {why it conflicts}
   >   - Suggested update: {proposed edit}
   > - (repeat for each conflict)
   >
   > Choose:
   > - `apply` — apply all suggested updates exactly as proposed
   > - `edit` — I'll describe what to change instead
   > - `later` — keep the earlier content, log the conflict under "Open Inconsistencies," and continue
   > - `ignore` — keep the earlier content, don't log it, and continue
   >
   > Enter your choice:
6. Handle the user's choice:
   - `apply` — edit `./1-pager-output.md` exactly as proposed. Confirm the edit, then proceed.
   - `edit` — ask the user what to change. Apply that change. Confirm the edit, then proceed.
   - `later` — add or update a `## Open Inconsistencies` section at the end of `./1-pager-output.md` listing the conflict. Do not duplicate an entry already listed there. Proceed.
   - `ignore` — proceed without changing the file.
7. When an `apply` or `edit` resolves a conflict already logged under "Open Inconsistencies," remove that entry.

## Step 1 — Collect Initial Inputs

### Step 1a — Collect Problem Statement
Run the `collect-problem-statement.md` runbook directly in this conversation. After it confirms `./1-pager-output.md` is saved, proceed to the next step.

### Step 1b — Collect Identified First Principle
Run the `collect-identified-first-principle.md` runbook directly in this conversation. After it confirms `./1-pager-output.md` is saved, proceed to the next step.

## Step 2 — Breakdown Problem into First Principles
Run the `false-first-principles-synthesis.md` runbook directly in this conversation. After it confirms `./1-pager-output.md` is saved, proceed to the next step.

## Step 3 — Define Problem Hypothesis
Run the `problem-hypothesis.md` runbook directly in this conversation. After it confirms `./1-pager-output.md` is saved, proceed to the next step.

## Step 4 — Gather Evidence: Business Impact
**Gate:** read `./1-pager-output.md` and check whether the `- **Business impact:**` bullet is present under `### How do we know this is a problem?` AND its content contains no unresolved `{...}` template placeholders. If the bullet is absent or placeholder-only, skip this step entirely and proceed to Step 5.

Otherwise, run `gather-business-impact.md` directly in this conversation — no subagent. The runbook uses no tools, and Task 4 asks the user a question.
1. Run Tasks 1–5 in order.
2. Run Task 6's Review & Confirm prompt:
   - `refine` — ask the user what to adjust, re-run Tasks 3–5, redisplay the report, and repeat this task.
   - `save` — append the Business Impact Summary section to `./1-pager-output.md`, following Task 6's save rules exactly. Confirm to the user, then proceed to the next step.
   - `skip` — do not modify the file. Proceed to the next step.
   - `restart` — do not modify the file. Return to Step 2.

## Step 5 — Gather Evidence: Behavioral Signals
**Gate:** read `./1-pager-output.md` and check whether the `- **Behavioral signals:**` bullet is present under `### How do we know this is a problem?` AND its content contains no unresolved `{...}` template placeholders. If the bullet is absent or placeholder-only, skip this step entirely and proceed to Step 6.

Otherwise:
1. Run Task 1 (Confirm Canonical Freshness) of `gather-behavior-signals.md` directly in this conversation.
2. Spawn a subagent with the following instructions:
   #### Subagent Instructions
   - Run Tasks 2–6 of `gather-behavior-signals.md` (Parse the Problem Statement through Summarize Findings Using Footnoted Citations). Canonical file status from this session's ledger: `{canonical-transcripts.md: yes/ignore}`. If marked `ignore`, treat that file as blank — do not read it even where the runbook says to.
   - Do not ask the user any questions. Do not modify `./1-pager-output.md`. Return only the full Behavioral Signals Evidence Report, in the exact shape of the Output Template, as your final message.
3. Back in this conversation: display the returned report wrapped in a ```markdown code block, then run Task 7's Review & Confirm prompt directly:
   - `refine` — ask the user what to adjust, then repeat step 2 with a new subagent spawn that includes the refinement direction; redisplay the new report and repeat this task.
   - `save` — append the Behavioral Signals Summary section to `./1-pager-output.md` yourself, following Task 7's save rules exactly. Confirm to the user, then proceed to the next step.
   - `skip` — do not modify the file. Proceed to the next step.
   - `restart` — do not modify the file. Return to Step 2.

## Step 6 — Gather Evidence: Voice-of-Customer Signals
**Gate:** read `./1-pager-output.md` and check whether the `- **Voice-of-customer signals:**` bullet is present under `### How do we know this is a problem?` AND its content contains no unresolved `{...}` template placeholders. If the bullet is absent or placeholder-only, skip this step entirely and proceed to Step 7.

Otherwise:
1. Run Task 1 (Confirm Canonical Freshness) of `gather-voice-of-customer-signals.md` directly in this conversation.
2. Spawn a subagent with the following instructions:
   #### Subagent Instructions
   - Run Tasks 2–6 of `gather-voice-of-customer-signals.md` (Parse the Problem Statement through Summarize Findings Using Footnoted Citations). Canonical file status from this session's ledger: `{canonical-transcripts.md: yes/ignore}`. If marked `ignore`, treat that file as blank — do not read it even where the runbook says to.
   - Do not ask the user any questions. Do not modify `./1-pager-output.md`. Return only the full Voice-of-Customer Evidence Report, in the exact shape of the Output Template, as your final message.
3. Back in this conversation: display the returned report wrapped in a ```markdown code block, then run Task 7's Review & Confirm prompt directly:
   - `refine` — ask the user what to adjust, then repeat step 2 with a new subagent spawn that includes the refinement direction; redisplay the new report and repeat this task.
   - `save` — append the Voice-of-Customer Summary section to `./1-pager-output.md` yourself, following Task 7's save rules exactly. Confirm to the user, then proceed to the next step.
   - `skip` — do not modify the file. Proceed to the next step.
   - `restart` — do not modify the file. Return to Step 2.

## Step 7 — Gather Evidence: Market Signals
**Gate:** read `./1-pager-output.md` and check whether the `- **Market signals:**` bullet is present under `### How do we know this is a problem?` AND its content contains no unresolved `{...}` template placeholders. If the bullet is absent or placeholder-only, skip this step entirely and proceed to Step 8.

Otherwise:
1. Spawn a subagent with the following instructions:
   #### Subagent Instructions
   - Run Tasks 1–5 of `gather-market-signals.md` (Parse the Problem Statement through Summarize Findings with Inline Citations).
   - Do not ask the user any questions. Do not modify `./1-pager-output.md`. Return only the full Market Signals Evidence Report, in the exact shape of the Output Template, as your final message.
2. Back in this conversation: display the returned report wrapped in a ```markdown code block, then run Task 6's Review & Confirm prompt directly:
   - `refine` — ask the user what to adjust, then repeat step 1 with a new subagent spawn that includes the refinement direction; redisplay the new report and repeat this task.
   - `save` — append the Market Signals Summary section to `./1-pager-output.md` yourself, following Task 6's save rules exactly. Confirm to the user, then proceed to the next step.
   - `skip` — do not modify the file. Proceed to the next step.
   - `restart` — do not modify the file. Return to Step 2.

## Step 8 — Synthesize Combined Summary
Run the `combined-synthesis.md` runbook directly in this conversation. After it confirms `./1-pager-output.md` is saved (or reports `skip`), proceed to the next step.

## Step 9 — Re-evaluate the Problem Statement
Run the `problem-statement-refinement.md` runbook directly in this conversation. After it confirms `./1-pager-output.md` is saved (or that no changes were needed), proceed to the next step.

## Step 10 — Propose New First Principles
Run the `new-first-principles.md` runbook directly in this conversation. After it confirms `./1-pager-output.md` is saved, proceed to the next step.

## Step 11 — Define Solution Hypothesis
Run the `solution-hypothesis.md` runbook directly in this conversation. After it confirms `./1-pager-output.md` is saved, proceed to the next step.

### Sanity Check-in — Loop Bryan In
Run the `sanity-check-in.md` runbook directly in this conversation. After it returns, proceed to the next step.

## Step 12 — Capture Solution Constraints
Run the `solution-constraints.md` runbook directly in this conversation. After it confirms `./1-pager-output.md` is saved, proceed to the next step.

## Step 13 — Propose Simple Solution
Run the `propose-simple-solution.md` runbook directly in this conversation. After it confirms `./1-pager-output.md` is saved (or reports `skip`), proceed to the next step.

## Step 14 — Propose Start a Movement Solution
Run the `propose-start-a-movement-solution.md` runbook directly in this conversation. After it confirms `./1-pager-output.md` is saved (or reports `skip`), proceed to the next step.

## Step 15 — Propose Agentic Solution
Run the `propose-agentic-solution.md` runbook directly in this conversation. After it confirms `./1-pager-output.md` is saved (or reports `skip`), proceed to the next step.

## Step 16 — Generate Success Metrics
Run the `success-metrics.md` runbook directly in this conversation. After it confirms `./1-pager-output.md` is saved (or reports `skip`), proceed to the next step.

## Step 17 — Recommended Solution
Run the `recommended-solution.md` runbook directly in this conversation. After it confirms `./1-pager-output.md` is saved, proceed to the next step.

## Step 18 — Generate Inputs, Operation, and Outputs for the Recommended Solution
Run the `inputs-operations-outputs.md` runbook directly in this conversation. After it confirms `./1-pager-output.md` is saved, proceed to the next step.

## Step 19 — Build vs Buy Evaluation
Run the `build-vs-buy.md` runbook directly in this conversation. After it confirms `./1-pager-output.md` is saved (or reports `skip`), proceed to the next step.

### Sanity Check-in — Loop Bryan In
Run the `sanity-check-in.md` runbook directly in this conversation. After it returns, proceed to the next step.

## Step 20 — Outline User Flow
Run the `user-flow.md` runbook directly in this conversation. After it confirms `./1-pager-output.md` is saved, proceed to the next step.

### Sanity Check-in — Loop Bryan In
Run the `sanity-check-in.md` runbook directly in this conversation. After it returns, proceed to the next step.

## Step 21 — Write User Stories
Run the `user-stories.md` runbook directly in this conversation. After it confirms `./1-pager-output.md` is saved, proceed to the next step.

## Step 22 — Categorize User Stories (Must Have vs Nice-to-Have)
Run the `categorize-user-stories.md` runbook directly in this conversation. After it confirms `./1-pager-output.md` is saved, proceed to the next step.

### Sanity Check-in — Loop Bryan In
Run the `sanity-check-in.md` runbook directly in this conversation. After it returns, proceed to the next step.

## Step 23 — Define Project Phases
Run the `project-phases.md` runbook directly in this conversation. After it confirms `./1-pager-output.md` is saved, proceed to the next step.

### Sanity Check-in — Loop Bryan In
Run the `sanity-check-in.md` runbook directly in this conversation. After it returns, proceed to the next step.

## Step 24 — Define Event Tracking Plan
Run the `event-tracking.md` runbook directly in this conversation. After it confirms `./1-pager-output.md` is saved, proceed to the next step.

### Sanity Check-in — Loop Bryan In
Run the `sanity-check-in.md` runbook directly in this conversation. After it returns, proceed to the next step.

## Step 25 — Explore Design Paradigms
Run the `design-paradigms.md` runbook directly in this conversation. After it confirms `./1-pager-output.md` is saved, proceed to the next step.

### Sanity Check-in — Loop Bryan In
Run the `sanity-check-in.md` runbook directly in this conversation. After it returns, end the runbook.
