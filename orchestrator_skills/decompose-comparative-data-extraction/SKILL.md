---
name: decompose-comparative-data-extraction
description: Orchestrates parallel retrieval of distinct temporal or spatial data points followed by a centralized calculation and verification step.
---

## When to Use
Use this strategy when a query requires a mathematical comparison (delta, ratio, proximity, or ranking) between two or more specific entities, timeframes, or geographic locations. It is triggered by keywords indicating change over time, distance between points, or selection of an extreme value from a specific dataset.

## Decomposition Template
1.  **Identify Baseline & Target:** Define the specific parameters (Year A vs. Year B, Location X vs. Location Y) and the required metric.
2.  **Parallel Extraction:**
    *   **Step 1 (Worker A):** Retrieve Metric M for Parameter 1 using the primary authoritative source.
    *   **Step 2 (Worker B):** Retrieve Metric M for Parameter 2 using the *same* authoritative source.
3.  **Normalization:** Ensure both data points use identical units, scales, and measurement methodologies.
4.  **Computation (Calculator):** Perform the mathematical operation (Subtraction, Division, or Comparison) using raw, unrounded values.
5.  **Final Synthesis (Judge):** Apply required rounding, format the result, and verify the logic against the original prompt.

## Worker Assignment Rules
*   **Primary Workers (2+):** Assign one worker per data point. Each worker must be instructed to cite the specific source/document title to ensure cross-worker consistency.
*   **Calculator Worker (1):** A dedicated step to perform the math. This worker should not perform search; it only processes the outputs of the Primary Workers.
*   **Verification Worker (Optional):** If the data source is a dense report or catalog, add a "Source Auditor" to confirm both Primary Workers extracted data from the same table or page.

## Answer Format
The final output must clearly state the retrieved values for each parameter before showing the calculation.
End with: `最终答案:你的答案`

## Anti-Patterns
*   **Source Fragmentation:** Using different publications or databases for the two comparison points, leading to "apples-to-oranges" errors due to varying methodologies.
*   **Premature Rounding:** Rounding individual data points before the final calculation, which compounds errors in the final result.
*   **Implicit Selection Bias:** Failing to scan the entire requested range (e.g., a century or a full catalog) before selecting the "closest" or "highest" value.
*   **Unit Mismatch:** Neglecting to convert different units (e.g., meters vs. feet) before performing the comparison.