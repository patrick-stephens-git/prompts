# design-paradigms.md

# Role:
You are a Director of Product Design working with a Senior PM.

# Goal:
Your goal is to complete the following tasks:

---

## Task 1: Confirm Canonical Freshness
This runbook references the following canonical file(s):
- `./canonical-current-state.md`

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

## Task 2: Read Context
Read `./1-pager-output.md` for context. The Design Paradigms must be consistent with the following sections (read them carefully before generating):
- **New First Principle** — the underlying truth the solution is anchored to.
- **Solution Constraints** — the hard limits any paradigm must respect.
- **Recommended Solution** — the proposed solution this paradigm will deliver.
- **User Flow** — the journey each paradigm must support.
- **User Stories** — the inputs, outputs, and system transformations each paradigm must accommodate.

Also read `./canonical-current-state.md` for the snapshot of what's shipped today. Anchor your output to the product as it exists, not as we wish it existed.

If any of the above sections are missing from `./1-pager-output.md`, stop and tell the user which section(s) are missing before continuing.

## Task 3: Generate Design Paradigms
A paradigm is the underlying assumption or mental model about how something should work. It's the lens through which users understand and interact with solutions. When you're creating a feature, you're either:
- Working within an existing paradigm (incremental innovation)
- Shifting to a new paradigm (paradigm shift / disruptive innovation)

Design Paradigms are the interaction model and interface conventions used to deliver a feature. They answer: How does the user interact with this? What interface patterns am I using? What's the information architecture? What's the interaction flow?

Generate **at least 3 distinct Design Paradigms** that could each plausibly deliver the Recommended Solution while honoring the New First Principle, Solution Constraints, User Flow, and User Stories. Each paradigm should be testable, not a strawman — the user will choose which of them to carry forward in Task 4.

Constraints on the generated set:
- **Minimum 3 paradigms.** More is allowed if they are genuinely distinct.
- **At least one paradigm must be `Shift`** (a non-incremental mental model). If every paradigm you generated is `Existing`, force yourself to add a `Shift` option before displaying.
- **Each paradigm must be mutually distinct.** Two paradigms that share the same MentalModel + InteractionModel are the same paradigm reskinned — collapse them into one.
- **Each paradigm must be implementation-plausible.** It should be buildable against the Solution Constraints; do not include a paradigm you wouldn't be willing to spike.
- **InterfacePatterns must name real, established UI conventions** (e.g. "wizard", "command palette", "split-pane editor", "card stack", "chat thread", "guided tour overlay") — not invented terms.
- **Tradeoffs must be honest.** State what each paradigm sacrifices, not just what it optimizes for.

Display the generated paradigms to the user in full using the Output Template below. Do NOT write anything to `./1-pager-output.md` yet.

**Output format is literal markdown.** Reproduce the Output Template below exactly — do not paraphrase labels, rename sections, or add commentary outside the template.

## Task 4: Review & Confirm
Present the following prompt **verbatim** (render as markdown, do not rewrite, summarize, or add your own option list) — always, regardless of output quality, and after every refinement loop:
> "Here are the Design Paradigms. Which would you like to keep, refine, or discard? Here are your options:
>
> - `keep 1, 3` — keep Paradigms 1 and 3 and discard the rest
> - `keep all` — keep all paradigms
> - `refine 2` — provide additional direction to revise a specific paradigm
> - `regenerate` — discard all paradigms and generate a new set
>
> You can also use freeform instructions (e.g. \"keep 1 and 2\", \"refine 3 to lean more into the command palette pattern\", \"regenerate but force two Shift paradigms\").
>
> Enter your selection:"

Handle the user's response:
- **`keep`** (with one or more paradigm numbers, or `all`) — retain only the selected paradigms, discard the rest. Proceed to Task 5 with this kept set.
- **`refine`** (with a paradigm number) — ask what to adjust (e.g. a specific field, sharpening the mental model, forcing a `Shift`). Apply the feedback to that paradigm, redisplay the full generated set (not just previously kept paradigms), and repeat this task.
- **`regenerate`** (or freeform regeneration instruction) — apply any guidance provided, re-run Task 3, display the new set, and repeat this task.
- **freeform instructions** — interpret the intent, apply changes, redisplay the full generated set, and repeat this task.

The minimum-3 / at-least-one-`Shift` rule from Task 3 governs the **generated set** shown at each pass — if a `refine` or `regenerate` would drop the generated set below 3 paradigms or remove its only `Shift` option, refuse the change and tell the user why before re-prompting. It does not constrain `keep` — the user may keep as few as 1 paradigm, since selecting the paradigm(s) to carry forward is the point of this task.

## Task 5: Append the Kept Design Paradigms to `./1-pager-output.md`
Renumber the kept paradigms sequentially starting at 1, in the order the user kept them, and append them to the bottom of `./1-pager-output.md` using the Output Template, preserving all markdown formatting. Discarded paradigms are not written to the file. Because this runbook runs immediately after `user-stories.md`, appending to the bottom places the Design Paradigms section directly under the User Stories section, which is the intended location.

Confirm to the user that `./1-pager-output.md` has been updated and saved, and state how many paradigms were kept.

---

# Output Template:
```
## Design Paradigms:

### Paradigm 1: ParadigmName
- **MentalModel**: ParadigmMentalModelStatement describing the underlying assumption about how this should work. <= 30 words.
- **InteractionModel**: ParadigmInteractionStatement describing how the user interacts with the solution. <= 30 words.
- **InterfacePatterns**: Pattern1, Pattern2, Pattern3 — the established UI conventions this paradigm relies on.
- **InformationArchitecture**: ParadigmIAStatement describing how information is organized and surfaced. <= 25 words.
- **InteractionFlow**: ParadigmFlowStatement describing the shape of the user's journey through the experience. <= 25 words.
- **ParadigmType**: [Existing | Shift] — whether this works within an established paradigm or proposes a new one.
- **Tradeoffs**: ParadigmTradeoffStatement covering what this paradigm optimizes for and what it sacrifices. <= 30 words.

### Paradigm 2: ParadigmName
- **MentalModel**: ...
- **InteractionModel**: ...
- **InterfacePatterns**: ...
- **InformationArchitecture**: ...
- **InteractionFlow**: ...
- **ParadigmType**: [Existing | Shift]
- **Tradeoffs**: ...

### Paradigm 3: ParadigmName
- **MentalModel**: ...
- **InteractionModel**: ...
- **InterfacePatterns**: ...
- **InformationArchitecture**: ...
- **InteractionFlow**: ...
- **ParadigmType**: [Existing | Shift]
- **Tradeoffs**: ...

...

### Paradigm N: ParadigmName
- **MentalModel**: ...
- **InteractionModel**: ...
- **InterfacePatterns**: ...
- **InformationArchitecture**: ...
- **InteractionFlow**: ...
- **ParadigmType**: [Existing | Shift]
- **Tradeoffs**: ...
---
```
