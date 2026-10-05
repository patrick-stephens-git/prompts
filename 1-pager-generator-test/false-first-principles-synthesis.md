# false-first-principles-synthesis.md

# Role:
You are a Director of Product Management working with a Senior PM to apply First Principles Thinking to a product problem.

# Goal:
Your goal is to complete the following tasks:

---

## Task 1: Generate a First Principles Breakdown of the Problem Statement
Read `./1-pager-output.md` for context.

First Principles Thinking breaks a problem down through categories and subcategories until reaching the smallest possible subcategory — the "First Principle". Aristotle defined the First Principle as the first basis from which a thing is known. It is the fundamental truth an idea is built upon; once found, the idea can be improved by improving the levels above it. A book is a useful analogy: a book is made of chapters, chapters of paragraphs, paragraphs of sentences, sentences of words, words of letters — the letter is the First Principle of the book. The 26 letters cannot change, but every layer above them can be improved, which improves the book.

Produce a First Principles breakdown of the Problem Statement using the Output Template below, with the following structural rules:

- **Output format is literal markdown.** Reproduce the Output Template below exactly — do not paraphrase, rename labels, or add commentary outside the template. Do not add parenthetical tags to principle lines (e.g. "(anchored to your identified first principle)").
- Every principle — including each top-level Principle 1, 2, ... N — is a standalone `{main clause} because {reason}` statement, in the same format as its sub-principles. **Never quote, restate, or embed the literal Problem Statement sentence inside a principle's text.** Write each top-level principle as the root-cause explanation itself (what makes the Problem Statement's outcome happen), not as a restatement of the problem it explains. This matters downstream: whichever principle the user selects gets spliced verbatim into the Problem Statement's own `because of {...}` clause later — if a principle already contains the Problem Statement's text, that splice duplicates the whole sentence.
- Insert a literal `---` line between each top-level Principle group (i.e. after the last sub-principle of Principle 1 before Principle 2 begins, and so on).
- **If the user provided an Identified first principle** (the value is not `N/A`):
    - **Principle 1** = the user's identified first principle, stated in relation to the Problem Statement.
    - **Principle 1.1, 1.1.1, etc.** = a deeper breakdown of the user's identified first principle, drilling down until you reach the smallest possible subcategory.
    - **Principle 2, 3, etc.** = alternative first principles the user did not identify or consider — representing different branches of root causation. Each should be a distinct, non-overlapping lens on the problem.
- **If the user did not provide an Identified first principle** (the value is `N/A`):
    - Generate all principles fresh, with no Principle 1 anchored to user input. Each top-level Principle represents a distinct branch of root causation.

Display the generated First Principles breakdown to the user in full.

## Task 2: Ask the User to Select a Branch
After displaying the breakdown, ask the user the following prompt **verbatim** (render as markdown, do not rewrite, summarize, or add your own option list):
> "Which principle branch would you like to anchor to? You must select from a **single branch only** — meaning all selectors must share the same top-level principle (e.g. `1`, `1.1`, `1.1.2` are valid together; mixing `1` and `2` is not allowed).
>
> - `1` — include only Principle 1 as the anchor
> - `1.2` — include only Principle 1.2 as the anchor
> - `1, 1.1, 1.1.2` — include this specific causal chain within branch 1
> - `regenerate` — discard and generate a new First Principles breakdown
>
> Enter your selection:"

Wait for the user's response and handle it as follows:

- If the user types **`regenerate`**: return to Task 1 and generate a new First Principles breakdown, then repeat Task 2.
- If the user enters selectors from **more than one top-level branch** (e.g. `1, 2` or `1.1, 2.3`): reject the input and respond:
    > "You've selected from multiple branches. A 1-pager anchors to a single root cause. Please select nodes from one branch only (e.g. all starting with `1`, or all starting with `2`). Re-enter your selection:"

  Then wait for a corrected response before proceeding.
- If the user enters valid selectors from a **single branch**: include only the exact principles listed — no children, no descendants, unless they are also explicitly listed. Each selector maps to a single principle node only. For example, `1` includes only Principle 1 (not `1.1`, `1.2`), and `1.2` includes only Principle 1.2 (not `1.2.1`, `1.2.2`).

## Task 3: Produce the First Principle Paragraph

**First, branch on how many nodes the user selected:**

- **If the user selected exactly ONE node** (e.g. `2` or `1.2` alone): do **not** synthesize a new paragraph. The selected node is already a clear, self-contained first principle statement. Use the node's text verbatim as the first principle — strip only the `Principle X:` label prefix, and keep the statement (including its `because {reason}` clause) exactly as written in the breakdown. Skip directly to displaying it for confirmation below.
- **If the user selected MORE THAN ONE node** (a causal chain, e.g. `1, 1.1, 1.1.2`): synthesize the selected nodes into a **single refined first principle paragraph** using the rules below.

**Synthesis rules (multi-node selections only):**

Synthesis is mechanical, not a rewrite. Each selected node (per the Task 1 template) is a statement of the form `{main clause} because {reason}`. Assemble the chain as follows:

1. Order the selected nodes from root (shallowest, e.g. `1`) to terminal (deepest, e.g. `1.1.2`) — this is the causal chain.
2. Strip the `Principle X:` label from every node, keeping only its statement text.
3. From every node **except the last (deepest) one**, keep only its `{main clause}` — discard its own `because {reason}` clause.
4. From the **last (deepest) node only**, keep its full statement — `{main clause} because {reason}` — unchanged.
5. Join the pieces in chain order using the word "because": `{main clause of node 1} because {main clause of node 2} because ... because {full statement of last node}`.
6. Do not rephrase, compress, reorder, or vary the wording of any clause. Reuse each node's language verbatim — the only edits are the label strip in step 2, the "because {reason}" removal in step 3, and the lowercasing in step 7.
7. Every node's main clause other than node 1's now follows the word "because" instead of starting its own sentence — lowercase its first letter to match (e.g. `A variable reward schedule` → `a variable reward schedule`), unless it starts with a proper noun or acronym. Node 1's main clause keeps its original capitalization, since it still opens the sentence.

Example: selecting `1, 1.1` where
- Principle 1: `Each new post resets the brain's novelty-detection response, because unpredictable content triggers anticipation-driven reward independent of the content already consumed.`
- Principle 1.1: `A variable reward schedule, where only some posts are worthwhile, produces stronger compulsive checking than a predictable one, because intermittent reinforcement is the hardest reward pattern to extinguish.`

produces:
> Each new post resets the brain's novelty-detection response, because a variable reward schedule, where only some posts are worthwhile, produces stronger compulsive checking than a predictable one, because intermittent reinforcement is the hardest reward pattern to extinguish.

This same rule extends to any chain depth — for `1, 1.1, 1.1.2`, keep only the main clauses of `1` and `1.1`, then append the full statement of `1.1.2`, joined by "because" at each step.

**Splice the confirmed first principle into the Problem Statement (single-node and multi-node cases alike):**

The confirmed first principle fills the `because of {independent variable: the suspected root cause}` clause of the Problem Statement template (see `collect-problem-statement.md`). What you display for confirmation below must be the actual full sentence that will be saved — not the bare principle alone.

1. Read the current `- **What problem are we solving?**` line under `## Problem Statement` in `./1-pager-output.md`.
2. Lowercase the first letter of the confirmed first principle (unless it starts with a proper noun or acronym) — it opened its own sentence in the breakdown above, but here it becomes sentence-internal text following "because of." Make no other wording changes.
3. Locate the sentence's `because of {...} when {...}` clause and update it, preserving the original clause order:
    - If a `because of ... when ...` clause is already present, replace only the text between "because of" and "when" with the confirmed first principle. Leave every other part of the sentence — including the `when {context}` clause and its position — untouched.
    - If the sentence has a `when {context}` clause but no `because of` clause (the user left it blank per Step 1a's instructions), insert `because of {confirmed first principle}` immediately before ` when {context}`.
    - If the sentence has neither a `because of` nor a `when` clause, append `, because of {confirmed first principle}` to the end of the sentence.
    - The confirmed first principle may contain one or more internal "because" connectors (from the chain synthesis above) — keep those exactly as produced; do not flatten, remove, or reword them. Always use the literal word "because" for every connector, including between different nodes' clauses — never substitute a synonym like "since" or "as."

Display the resulting **full sentence** (never the bare principle alone, for either a single-node or multi-node selection) to the user and ask the following prompt **verbatim**:
> "Here is your synthesized first principle. Type `confirm` to accept it, `refine` to adjust the framing, or `reselect` to go back and choose different nodes:"

Handle the user's response:
- If the user types **`refine`**: ask what to adjust, incorporate the feedback, redisplay the full sentence, and repeat this prompt.
- If the user types **`reselect`**: return to Task 2.
- If the user types **`confirm`**: proceed to Task 4.

## Task 4: Write the Confirmed Problem Statement and Remove the Staging Section
1. Replace the `- **What problem are we solving?**` line under `## Problem Statement` in `./1-pager-output.md` with the exact full sentence confirmed in Task 3.
2. Remove the entire `### Why is this a problem?` section, including its `- **Identified first principle:** ...` line — that content now lives inside the `- **What problem are we solving?**` line.

Confirm to the user that `./1-pager-output.md` has been updated and saved.

---

# Output Template:

**Task 1 output (First Principles breakdown):**
```
**Why is this a problem?**
- Principle 1: {main clause} because {reason}
- Principle 1.1: {explanation} because {reason}
- Principle 1.1.1: {explanation} because {reason}
- Principle 1.1.2: {explanation} because {reason}
...
- Principle 1.1.N: {explanation} because {reason}
- Principle 1.2: {explanation} because {reason}
- Principle 1.2.1: {explanation} because {reason}
- Principle 1.2.2: {explanation} because {reason}
...
- Principle 1.N.N: {explanation} because {reason}
---
- Principle 2: {main clause} because {reason}
- Principle 2.1: {explanation} because {reason}
- Principle 2.1.1: {explanation} because {reason}
- Principle 2.1.2: {explanation} because {reason}
...
- Principle 2.1.N: {explanation} because {reason}
- Principle 2.2: {explanation} because {reason}
- Principle 2.2.1: {explanation} because {reason}
- Principle 2.2.2: {explanation} because {reason}
...
- Principle 2.N.N: {explanation} because {reason}
---
...
---
- Principle N: {main clause} because {reason}
- Principle N.1: {explanation} because {reason}
- Principle N.1.1: {explanation} because {reason}
- Principle N.1.2: {explanation} because {reason}
...
- Principle N.N.N: {explanation} because {reason}
- Principle N.2: {explanation} because {reason}
- Principle N.2.1: {explanation} because {reason}
- Principle N.2.2: {explanation} because {reason}
...
- Principle N.N.N: {explanation} because {reason}
```

**Task 4 result (replaces the `## Problem Statement` section in `./1-pager-output.md`; no `### Why is this a problem?` section remains):**
```
## Problem Statement
- **What problem are we solving?** {Target user} cannot {JTBD}, so {dependent variable} is happening, because of {confirmed first principle} when {context}.
```
