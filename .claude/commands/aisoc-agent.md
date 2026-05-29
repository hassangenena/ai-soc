---
description: Print an AISOC agent prompt verbatim for dispatch (usage: /aisoc-agent <N>)
argument-hint: <agent number, e.g. 3>
allowed-tools: Read, Glob
---

The operator wants to dispatch AISOC agent **#$ARGUMENTS**.

1. Find the matching prompt file under `.aisoc/agents/`. Filenames are
   two-digit zero-padded, e.g. agent `3` → `.aisoc/agents/03-dns-sentinel.agent.md`.
   (List `.aisoc/agents/` if you need to resolve the short name.)
2. Output the file's **full contents verbatim**, inside a fenced block, so the
   operator can paste it as the agent's RICTOC prompt.
3. Do **not** execute the agent or invent input. After printing, remind the
   operator to paste the input block the Orchestrator requested for this agent.

This is a thin dispatch helper. It only loads the canonical prompt; it must not
alter it.
