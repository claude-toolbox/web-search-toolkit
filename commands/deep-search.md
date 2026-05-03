---
description: Deep research using Exa with multi-agent orchestration (comprehensive, multi-angle)
---

Invoke the `exa:search` skill for deep research on the following topic. This triggers multi-pass parallel subagent search with deduplication and source quality evaluation.

If the `exa:search` skill is not available (Exa plugin not installed), fall back to:
1. Break the topic into 2-3 sub-questions
2. Dispatch parallel Agent subagents, each running WebSearch on a sub-question
3. Compile, deduplicate, and present with sources

$ARGUMENTS
