# canonical-transcripts.md

Canonical transcript reference material. Hypothesis runbooks read this file when they gather evidence from meeting transcripts and chat transcripts. Copilot gathers this evidence. This file contains the rules that apply to all Copilot-backed evidence gathering.

---

## Speaker Attribution
Any direct statement in a meeting transcript or chat transcript is a valid quote. The speaker can be a user, customer, prospect, or team member. Do not discard a quote because of who said it.

Attribute each quote to the speaker by name and role.

---

## Relevance Gate — Mandatory Before Any Other Check
Before you apply any other rule to a candidate quote, confirm that all three tests pass:

1. **Topical match** — the speaker discusses the specific product area, workflow, or situation named in the problem statement. A quote that only mentions a related feature or adjacent frustration does not pass.
2. **First-principles alignment** — the quote speaks to the root cause or its direct downstream effect. The calling runbook documents the first principles core. A quote that is related but does not reflect that core mechanism must be discarded.
3. **Unambiguous subject** — the quote and its immediate context show exactly what product, feature, or situation the speaker means. If you cannot state in one sentence what the speaker describes, discard the quote.

If the quote fails any one of these three tests, discard it immediately. Do not resolve editorial brackets. Do not label it. Do not include it in any form. Move on.

---

## Editorial Brackets — Mandatory Resolution Rule
Before you include a quote, scan it for ambiguous pronouns and demonstratives. Examples are "it", "that", "this", "they", "there", "those", and "them". For each one, read the surrounding transcript context to find the referent.

- If the context shows the referent clearly, add the referent inline in square brackets. Example: `"it [the export feature] never works the way I expect"`.
- If the context does not show the referent with confidence, **discard the quote entirely.** Do not use `[referent unclear]` as a placeholder. An unresolved quote is not a valid finding.

A quote has all ambiguous terms resolved, or you do not use it.
