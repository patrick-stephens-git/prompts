# gather-behavior-signals.md

# Role:
You are a Product Analyst working with a Director of Product Management to validate whether a suspected product problem is real and worth solving.

# Goal:
Your goal is to complete the following tasks:

---

## Scope
This runbook validates **one** signal hypothesis only: **Behavioral Signals**. Other runbooks validate the other hypotheses (Business Impact, Voice-of-Customer Signals, Market Signals). They are out of scope here.

Evidence comes from meeting transcripts and chat transcripts. Copilot gathers it. Look for users who describe their own workarounds, avoidance patterns, or behaviors without being asked.

For every claim you make, produce a transcript link that directly supports it. A claim without a link is not a valid finding.

**Reference:** Read `canonical-transcripts.md` before you start. It contains the Speaker Attribution rule, the Relevance Gate (3 tests), and the Editorial Brackets rule. Apply all three to every candidate quote.

---

## Task 1: Confirm Canonical Freshness
This runbook references the following canonical file(s):
- `./canonical-transcripts.md`

For each canonical file referenced, first check this session's Canonical File Status ledger (see `main.md`'s Instructions). If a file already has a recorded `yes` or `ignore` status from earlier in this session, reuse it silently — do not ask about that file again — and move to the next file. Only files with no recorded status get asked.

Ask the user the question below for each remaining file — **one file at a time, sequentially**. Send the prompt for the first file, wait for the user's answer, handle it, and only then send the prompt for the next file. Do **not** batch the prompts; do **not** display multiple file prompts in a single message. Substitute `{file}` with the file path and `{today}` with today's date, and ask **verbatim**:

> "Is `{file}` up to date as of {today}? Choose:
>
> - `yes` — proceed
> - `no` — I'll update `{file}` first; tell me when I'm done
> - `ignore` — skip this file; don't use what's in it, I'll fill in the blanks myself
>
> Enter your choice:"

For every `no`, wait for the user to confirm they've finished updating the file before continuing. Re-read the canonical file after the user confirms, then record `yes` in the ledger for that file. For every `yes`, record `yes` in the ledger. For every `ignore`, do not read or rely on that canonical file's contents — treat it as if it were blank and proceed; the user will fill in the relevant details manually. Record `ignore` in the ledger for that file. Do not proceed past this task until every referenced canonical has a `yes` or `ignore` status, whether just recorded or reused from the ledger.

## Task 2: Parse the Problem Statement into Search Targets
Read `./1-pager-output.md` for context.

Extract the following:
- **Target user** — who has the problem?
- **Trigger / context** — when does the problem occur?
- **Behavioral Signals hypothesis** — which user action or avoidance pattern should be observable? Take the hypothesis verbatim from `- **Behavioral signals:** ...` in the Problem Hypothesis.
- **Key vocabulary** — which words or phrases would a user say when they describe this behavior? Generate at least 5 candidate search terms.
- **First principles core** — write 1–2 sentences that state the most fundamental reason this problem exists. Take it from the first principles breakdown in `./1-pager-output.md`. Every quote you consider in later tasks must map back to this statement.

Do NOT extract the Business Impact, Voice-of-Customer, or Market Signal hypotheses. They are out of scope for this runbook.

## Task 3: Search for Relevant Meetings and Chats
Users often describe their own workarounds and avoidance patterns in conversation.
- Use Copilot to search meeting transcripts and chat transcripts with the key vocabulary from Task 2.
- Run multiple searches with different keyword combinations.
- Note the title, date, and participants of each result.
- Prioritize conversations with the target user type (customer calls, onboarding, sales, support, user interviews).
- If you find no relevant conversations after you exhaust your keyword list, record a research gap. Report it in the output.

## Task 4: Extract Quotes Describing the Behavioral Signal
For each relevant conversation found in Task 3, search the transcript for quotes where speakers describe workarounds, manual processes, tools used outside the product, or moments of abandonment.
- Listen for these patterns: a speaker does something by hand that the product should handle; a speaker names a competitor or alternative tool; a speaker leaves the product to find something elsewhere; a speaker is surprised that a feature does not exist.
- Apply the Speaker Attribution rule, the Relevance Gate, and the Editorial Brackets rule from `canonical-transcripts.md` to every candidate quote before you include it.
- For each quote that passes the relevance gate, use Copilot to link the quote at its location in the transcript. Capture the transcript link.
- Use the speaker's exact words. Do not paraphrase.
- If a quote supports the hypothesis, label it "Supports". If it contradicts the hypothesis, label it "Contradicts". If it is ambiguous, label it "Inconclusive".
- Each quote needs its own transcript link.

## Task 5: Cluster Quotes into Themes
Group the collected quotes into 2–4 distinct themes that describe what the behavior looks like in practice.
- A theme is one specific behavior pattern, not a broad topic.
- Each quote belongs to one theme only. Choose the theme it speaks to most directly.
- Discard any theme that has no supporting quote.
- Write down the themes and their supporting quotes before you start Task 6.

## Task 6: Summarize Findings Using Footnoted Citations
Produce the final output using the Output Template below.

**Output format is literal markdown.** Reproduce the Output Template below exactly. Do not paraphrase labels. Do not rename sections. Do not add commentary outside the template.

- The `### Behavioral Signals Hypothesis` section keeps the claim + link bullet format. Use one quote per bullet, with the speaker attribution and the transcript link.
- The Summary is a bulleted list. Each bullet states one theme from Task 5 in plain language and ends with a footnote marker `[^1]`, `[^2]`, and so on. Include one bullet per theme.
- **Attribution lives only in footnotes.** A Summary bullet must not contain direct quotes, speaker names, roles, or dates. Put every quote, speaker, role, and date only in the `**Sources:**` footnote definitions. Anti-pattern: `Nigel told Patrick on 2026-04-20 "we can't find that effect"[^1]`.
- At the bottom of the Summary, under a `**Sources:**` line, list each footnote definition in the exact shape shown in the Output Template. A footnote definition contains only markdown links — no quote text, no bare URLs, no full names. Anti-pattern: `[^1]: "quote text" — Full Name, Role, YYYY-MM-DD — bare_url`.
- If several quotes support one theme, put all of them under the same footnote, separated by ` · `.
- Do not reuse quotes across footnotes.
- Do not add claims that the quotes in the hypothesis section do not support.

## Task 7: Review & Confirm
Display the full generated report to the user for review, wrapped in a ```markdown code block so the raw footnote formatting is inspectable. Do NOT write anything to `./1-pager-output.md` yet.

Then present the following prompt **verbatim** (render as markdown, do not rewrite, summarize, or add your own option list) — always, regardless of whether evidence was found, and after every refinement loop:
> "Does this evidence report look good, or would you like to refine it? Choose an option:
>
> - `refine` — provide more context or additional direction to keep digging for supporting data
> - `save` — accept the current report as-is and append it to the 1-pager
> - `skip` — discard this report entirely (nothing will be added to the 1-pager)
> - `restart` — discard this report and re-approach the problem from first principles
>
> Enter your choice:"

Handle the user's response:
- **`refine`** — ask what additional context or direction to provide, re-run Tasks 3–6 with the new direction applied, display the revised output, and repeat this task.
- **`save`** — append ONLY the `### Behavioral Signals Summary` section (the heading and every line below it in the report, including the `**Sources:**` block and all footnote definitions, up to the closing `---`) to the bottom of `./1-pager-output.md`, preserving all markdown formatting. Do not append the `## Behavioral Signals Evidence Report` header, the `### Behavioral Signals Hypothesis` section, the `#### Qualitative Evidence (Transcripts)` block, or the `### Research Gaps` section — those are display-only and must not be persisted. Confirm to the user that `./1-pager-output.md` has been updated and saved, and mention that only the summary was persisted. Report `save` as the terminal outcome.
- **`skip`** — do not modify `./1-pager-output.md`. Inform the user that the Behavioral Signals evidence section has been omitted. Report `skip` as the terminal outcome.
- **`restart`** — do not modify `./1-pager-output.md`. Report `restart` as the terminal outcome.

---

# Constraints:
- See `canonical-transcripts.md` for the Copilot rules that apply to every task above.
- If Copilot returns no results for a search query, retry with at least two alternative keyword combinations before you conclude that no evidence exists.
- If you find no relevant quotes, report that outcome explicitly. "No evidence found" is a valid and complete finding.
- The Summary must use footnote syntax (`[^1]`, `[^2]`, and so on). Do not use inline hyperlinks or parenthetical citations.

---

# Output Template:
````
## Behavioral Signals Evidence Report
### Behavioral Signals Hypothesis
> {Restate the behavioral signals hypothesis from ./1-pager-output.md verbatim}

#### Qualitative Evidence (Transcripts)
{If quotes were found:}
- "{Exact quote describing the behavior, with all ambiguous pronouns resolved via [bracketed clarifications]}" — {Speaker first name and last name, role}, {Date in YYYY-MM-DD format} — {transcript link url} — {Supports / Contradicts / Inconclusive}
- ...

{If no quotes were found:}
- No evidence found in meeting transcripts and chat transcripts.

### Research Gaps
{If gaps exist:}
- {Gap description}: No meetings of type {meeting type} found. To close this gap, record and transcribe {meeting type}.

{If no gaps exist:}
- No gaps identified. All search targets mapped to existing conversations.

### Behavioral Signals Summary
{If evidence was found:}
{A bulleted list. Each bullet states one theme from Task 5 in plain language and ends with a footnote marker in the form [^1], [^2], and so on. Include one bullet per theme. Do not add themes or claims that the quotes in the hypothesis section do not support. Do not count verdicts or tally labels.}

**Sources:**

[^1]: [First-name, role — YYYY-MM-DD](transcript_link_url) · [First-name, role — YYYY-MM-DD](transcript_link_url)
[^2]: [First-name, role — YYYY-MM-DD](transcript_link_url)

{If no evidence was found:}
No evidence found.
---
````
