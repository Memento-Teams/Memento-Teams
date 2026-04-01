---
name: decompose-multimedia-source-verification
description: A two-stage strategy to isolate specific visual or bibliographic details from social media and video content by separating source identification from contextual extraction.
---

## When to Use
This strategy is triggered when a query requires identifying a specific object, title, or detail contained within a piece of multimedia content. It is applicable when the user provides metadata signals such as a platform-specific title, an uploader's handle, or a precise upload timeframe. Use this when the answer cannot be found in general knowledge but resides within the visual or auditory stream of a specific digital asset.

## Decomposition Template
1. **Source Authentication & Retrieval**: Locate the primary source URL and retrieve the most granular metadata available (full transcript, OCR data, or frame-by-frame descriptions). The goal is to move beyond search engine snippets to the actual content body.
2. **Targeted Contextual Extraction**: Within the retrieved source, isolate the specific segment referenced. Distinguish between the *subject matter* (what the video is about) and the *target object* (the specific item, text, or title requested).
3. **Cross-Reference Verification**: Compare the extracted detail against the video’s official description or pinned comments to ensure the item is not a misidentified prop or a generic placeholder.

## Worker Assignment Rules
- **Worker 1 (The Scout)**: Responsible for finding the exact video link and the full transcript/metadata.
- **Worker 2 (The Analyst)**: Responsible for scanning the transcript or visual descriptions to find the specific timestamp and extracting the requested detail.
- **Worker 3 (The Auditor)**: Required if the target is a "title" or "name," to ensure the worker hasn't substituted a descriptive summary for a formal name.

## Answer Format
The final output must clearly state the specific detail found, followed by the timestamp or source segment where it was located.
最终答案: [Specific Detail/Title] (Found at [Timestamp/Context])

## Anti-Patterns
- **Topic-Object Confusion**: Substituting a general description of the video's theme for the specific bibliographic or physical title requested.
- **Snippet Reliance**: Formulating an answer based on search engine preview text which often truncates or summarizes the actual data.
- **Temporal Drift**: Failing to verify the upload date, leading to the selection of a similar video from the same creator but a different timeframe.
- **Metadata Blindness**: Ignoring the video description or pinned comments which often contain the precise names of items shown in the video.