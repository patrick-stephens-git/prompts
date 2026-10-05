# canonical-current-state.md

Canonical snapshot of **what the product is today**. Read directly by most 1-pager-generator steps (solution proposals, constraints, user stories, project phases, event tracking, design paradigms, etc.) to ground their output in shipped reality, not aspiration. Update this file at the start of each 1-pager cycle — or whenever a major surface ships or is removed.

Anything listed here is, by definition, what the product is today. Deciding whether to build on each item, optimize it, double down on it, or rip it out and start over is the calling runbook's job — this file's job is just to state what's shipped.

If no product exists yet, say `_No product exists yet._`

---

## What's Shipped Today

- **Elasticsearch**: System used to store product documents and return them ranked by relevance to a search query.
- **Spelling Correction**: Data science model that uses search behavior and language patterns to correct misspelled queries.
- **Keyword Suggestions**: Data science model that populates keyword suggestions based on historical searches.
- **User Behavior Bias**: Data science model that ranks products in search results based on historical engagement for the search term. Optimized for profit and revenue.
- **Semantic Filtering**: LLM-driven process that narrows search results to the intended product type for a more precise search experience.
- **LTR**: Data science model that predicts the optimal ranking of products in search results based on athlete behavior and product signals.
- **Vector Search**: A series of data science models that work together to expand search results with products that are semantically similar to a user's query.
- **Reciprocal Rank Fusion**: Combines lexical search and vector search ranked lists of items into one final ranking.
- **Query Intent Classifier**: A classifier that determines whether to utilize vector search or not for a search query.
- **Named Entity Recognition**: Identifies entities in the search query and filters results to ensure they are aligned with intent.
- **Personalized LTR**: Incorporates user-specific browse and purchase signals into raking to tailor results based on personal preference.
- **LTR Next Period Predictor**: Predicts how result should be ranked in the upcoming period in anticipation of demand shifts.
- **Human-Curated Synonym Map**: Human-curated set of synonyms for head queries.

- **Core Product Attributes**: Attributes in product document used for matching.
  - **Brand**: Captures brand names in our product catalog.
  - **Gender (by age)**: Captures words like "male", "womens", "youth", etc.
  - **Activity**: Captures words like "basketball" or "tennis".
  - **Product Type**: Captures categories like "bikes" or "gloves".

