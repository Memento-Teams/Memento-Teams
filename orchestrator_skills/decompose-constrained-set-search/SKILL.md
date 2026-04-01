---
name: decompose-constrained-set-search
description: Orchestrates a multi-stage filter-and-verify process to find a specific entity satisfying three or more independent constraints.
---

## When to Use
Use this skill when the user query defines a "needle in a haystack" search requiring the intersection of multiple distinct attributes. Indicators include queries for a single entity (person, place, object, or document) that must satisfy a specific timeframe, a quantitative threshold, a negative constraint (exclusion), and a specific affiliation or category simultaneously.

## Decomposition Template
1. **Constraint Extraction**: Parse the query into a checklist of independent constraints (e.g., Constraint A: Timeframe, Constraint B: Quantitative Metric, Constraint C: Exclusion/Negative, Constraint D: Affiliation).
2. **Primary Candidate Generation**: Identify the "most restrictive" or "easiest to index" constraint. Generate a broad list of candidates that satisfy this single primary filter.
3. **Sequential Filtering**: Pass the candidate list through the remaining constraints one by one.
4. **Deep Verification**: For the final 1-2 candidates, perform a dedicated "fact-check" pass against every original constraint, specifically looking for "hidden" requirements or negative exclusions.

## Worker Assignment Rules
- **Worker 1 (The Scout)**: Responsible for Step 1 and 2. Must prioritize recall to ensure the "needle" is in the initial set.
- **Worker 2 (The Auditor)**: Responsible for Step 3. Cross-references the candidate list against secondary data points.
- **Worker 3 (The Critic)**: Responsible for Step 4. Acts as a "Red Team" to try and disqualify the final candidate based on the most easily overlooked constraint (e.g., the negative constraint).

## Answer Format
The final output must clearly state how the chosen entity satisfies every constraint listed in the prompt.
End with: `最终答案:你的答案`

## Anti-Patterns
- **The "Close Enough" Fallacy**: Selecting a candidate that meets 3 out of 4 criteria because it is a high-probability match in other contexts.
- **Primary Filter Failure**: Starting the search with the broadest constraint (e.g., "a person") rather than the most specific one (e.g., "published in October 2023"), leading to an unmanageable candidate pool.
- **Negative Constraint Oversight**: Forgetting to check "Except X" or "Not including Y" during the final verification phase.
- **Hallucinated Aggregation**: Combining attributes from two different entities to create a "perfect" but non-existent match.