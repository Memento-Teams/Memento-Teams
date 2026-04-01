---
name: decompose-linear-multi-hop-dependency
description: Decomposes queries where each answer serves as the mandatory search key for the subsequent step in a sequential chain.
---

## When to Use
Use this skill when the query contains a "possessive chain" or a series of nested relationships. This is indicated by phrases like "the [Attribute] of the [Entity] who [Action]," or when the final answer depends on a specific property of an intermediate entity that is not mentioned in the original prompt. If the query requires identifying Entity A to find Entity B, and Entity B to find Entity C, it is a linear multi-hop dependency.

## Decomposition Template
1.  **Step 1: Identify Anchor Entity.** Extract the primary known entity and the specific constraint (date, location, or title) to find the first hidden link.
2.  **Step 2: Extract Bridge Attribute.** Using the result from Step 1, search for the specific property or relative required to move to the next hop.
3.  **Step 3: Final Constraint Application.** Using the result from Step 2, apply the final filter or count requested in the original prompt.
4.  **Step 4: Verification.** Re-trace the chain from Step 3 back to Step 1 to ensure no "entity drift" occurred (e.g., ensuring a date constraint from Step 1 still applies to the final result).

## Worker Assignment Rules
- **Single Worker Execution:** Assign to a single "Chain-of-Thought" worker. Do not parallelize, as Step N cannot begin without the output of Step N-1.
- **Sequential Verification:** If the chain exceeds three hops, insert a verification step after the second hop to confirm the "Verified Entity" before proceeding.

## Answer Format
The output must show the reasoning chain for each hop.
End with: `最终答案:你的答案`

## Anti-Patterns
- **Premature Parallelization:** Attempting to search for the final entity and the bridge entity simultaneously, leading to "hallucinated" connections.
- **Constraint Leaking:** Forgetting a specific year or numerical constraint mentioned in the first hop by the time the agent reaches the final hop.
- **Entity Ambiguity:** Failing to resolve a common name in the middle of the chain, causing the search to pivot to a different person/object with the same name.
- **Counting Logic Errors:** Confusing "number of occurrences" or "number of terms" with "number of unique individuals" when the final hop involves a list.