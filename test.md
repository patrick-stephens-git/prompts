## Problem Statement
- **What problem are we solving?** Our company does not know whether Google's standalone Vector Search is differentiated from or a migration of our existing Vector Search capabilities we've built in-house on Elasticsearch when we're in the process of Q4 planning, which is blocking the Q4 decision on whether we move forward and build on Google's platform, because no one has completed a structured technical evaluation of Google's standalone Vector Search against the in-house Elasticsearch capabilities.

### How do we know this is a problem?
- **Voice-of-customer signals:** We would expect to hear leadership say they cannot make the Q4 decision without a recommendation. We would expect to hear the Search Relevance Data Science team say they cannot make a recommendation without a structured technical evaluation of Google's standalone Vector Search against the in-house Elasticsearch capabilities.
---
- **New First Principle:** A technical capability comparison of Google's standalone Vector Search against our existing Elasticsearch implementation shows whether Google is differentiated or a migration.
---

## Hypothesis
A technical capability comparison of Google's standalone Vector Search against our existing Elasticsearch implementation will result in a recommendation on whether we move forward and build on Google's platform so that leadership can make a decision before the Q4 planning deadline.
---

- **Solution Constraints:**
1. Timeline: Needs to be completed in Sprint 5.
---

### Idea 1: Replay our existing evaluation on Google
- **"What if ...?"** What if we received our existing search evaluation queries and relevance judgments, ran them through Google's standalone Vector Search, and scored the results with our existing Elasticsearch evaluation metrics, so that leadership gets a recommendation based on measured quality?
- **InflectionCategory:** "How might we ...?" How might we compare Google's results with our current results using the measures our team already trusts? We score both on the same queries.
- **Imagine this:** The Search Relevance Data Science team gets one set of scores for the current system and one set for Google. The team no longer argues from opinion. Leadership sees the same measures that the team already uses.

### Idea 2: Map our shipped capabilities to Google's published capabilities
- **"What if ...?"** What if we received the list of capabilities in `canonical-current-state.md` and Google's published product documentation, and compared them one by one, so that we know which parts of Google's product overlap with ours and which parts are new?
- **InflectionCategory:** "How might we ...?" How might we tell "differentiated" from "migration" without running any system? We compare the written capability lists.
- **Imagine this:** Leadership gets a short list of overlaps and differences. The team finds out early which questions need a measured test. Nobody builds a test for a capability that is clearly the same.
---

## Success Metrics:
- [Business Outcome] Q4 build decision made by the deadline: Measures whether leadership makes the decision on whether to build on Google's platform on or before the Q4 planning deadline. This traces to claim 3. The canonical Success Metrics are not enough for this Hypothesis. They measure customer results of search, such as CVR and Revenue, and they move only after a build. They cannot show whether the decision happened on time.
  - Success Metric Target: A decision recorded on or before the Q4 planning deadline, measured at the deadline via the leadership decision record. No instrumentation is required, because the check is manual.
  - Kill Threshold: Leadership has the recommendation and still says it cannot decide by the deadline. That result would show that the recommendation was not the missing input.

- [Product Outcome] Recommendation delivered: Measures whether the Search Relevance Data Science team delivers a recommendation on whether to build on Google's platform. This traces to claims 1 and 2. It is an Input Metric. Leadership needs the recommendation to make the decision, so on-time delivery leads to the Business Outcome above.
  - Success Metric Target: One recommendation delivered by the end of Sprint 5, measured at the end of Sprint 5 via the sprint record. No instrumentation is required.
  - Kill Threshold: The comparison is complete and still cannot say whether Google is differentiated or a migration. That result would show that a technical capability comparison does not answer the question.

## Excluded Metrics (Vanity):
- Number of capabilities compared, and number of queries evaluated: These show effort, and they do not show whether the decision happened. Replace them with the Business Outcome "Q4 build decision made by the deadline" and the Product Outcome "Recommendation delivered."
---

## Recommended Solution:
- **Recommended Solution:** Idea 2: Map our shipped capabilities to Google's published capabilities
- **Hypothesis Fit:** The Hypothesis says a technical capability comparison will produce a recommendation. Idea 2 is that comparison. It compares our shipped capabilities with Google's published capabilities one by one. The result shows which parts overlap and which parts are new. It also fits the Sprint 5 constraint, because it needs no new test setup.
- **Evidence Anchor:** The voice-of-customer hypothesis says the Search Relevance Data Science team cannot recommend without a structured technical evaluation. This is a hypothesis only. Step 6 was skipped, so no transcript evidence supports it. That is a gap.
- **Key Assumption:** Our existing Elasticsearch evaluation metrics and golden dataset can help score Google's embeddings, and Google's documentation plus a Google technical resource can answer the capability questions. The cheapest test is to score a small sample of the golden data set on Google's embeddings with our existing metrics.
- **Why Not the Alternatives:**
   - Idea 1: Replay our existing evaluation on Google: It needs a test setup and sample runs, which cost more time in Sprint 5. It also measures result quality only. It does not show which capabilities overlap.
- **Kill Condition:** The Product Outcome "Recommendation delivered" fails. The comparison is complete by the end of Sprint 5 and still cannot say whether Google is differentiated or a migration. Pivot to a measured test (Idea 1).
---

## Recommended Solution Mechanic:
- **Input(s):**
    - Our existing Vector Search capabilities built in-house on Elasticsearch.
    - Google's published documentation, limits, and release notes for its standalone Vector Search.
    - Our existing Elasticsearch evaluation metrics and the golden data set that we use for evaluation, which Destiny Marrero can help provide.
    - A technical resource at Google, engaged to probe deeper when the documentation is not enough.
- **Operation:**
    - List each capability of our existing Vector Search implementation in detail.
    - List each capability of Google's standalone Vector Search in the same level of detail.
    - Compare the two lists in a full detailed analysis, one capability at a time, and record what is different or differentiated in Google's offer.
    - Engage the technical resource at Google when the documentation does not answer a question.
    - Evaluate embedding quality for Google's embeddings and for our Elasticsearch embeddings on the golden data set, using our existing Elasticsearch evaluation metrics.
    - Estimate the effort to build multimodal search in-house and the effort to build it with Google, factoring in what we have already learned, and decide whether Google accelerates it.
    - Identify the embedding model types and capabilities that Google offers and that we do not have.
    - Decide which of those embedding models, if any, warrant a deeper follow-up assessment.
    - Combine the results into one technical capability comparison against our existing Elasticsearch implementation.
    - Write a recommendation on whether to build on Google's platform, based on the comparison.
- **Output(s):**
    - A full technical capability comparison against our existing Elasticsearch implementation, as a feature-by-feature matrix or capability checklist that shows what Google offers that is different or differentiated from what we have today — it gives the Search Relevance Data Science team the structured technical evaluation that it lacked.
    - An embedding quality evaluation, as a quantitative scorecard that compares embedding quality metrics side by side for our Elasticsearch embeddings and Google's embeddings, built on the golden data set — it shows whether Google's embeddings match, beat, or trail ours on the measures that the team already trusts.
    - A finding on whether Google accelerates multimodal search versus building it in-house, as a direct yes or no with supporting rationale and an effort comparison in estimated engineering weeks. Our current assumption is that it does not accelerate, because we still produce the embeddings ourselves and move them into Elasticsearch — it shows whether Google saves work that we would otherwise build ourselves.
    - A list of embedding model types or capabilities that are unique to Google compared with what we have, with a recommendation on which, if any, warrant a deeper follow-up assessment — it shows whether Google adds capability that Elasticsearch cannot supply, and what to assess next.
    - A recommendation on whether to build on Google's platform, with a verdict of "differentiated" or "migration" — it gives leadership the input needed to make the Q4 decision.
---
