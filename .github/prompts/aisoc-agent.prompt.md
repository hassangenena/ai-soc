---
mode: ask
description: Print an AISOC agent prompt verbatim for dispatch (provide the agent number)
---

The operator wants to dispatch an AISOC agent. Ask for the agent number if it
was not supplied with this prompt.

1. Find the matching prompt file under `.aisoc/agents/`. Filenames are two-digit
   zero-padded, e.g. agent `3` → `.aisoc/agents/03-dns-sentinel.agent.md`.
2. Output that file's **full contents verbatim**, inside a fenced block, so the
   operator can paste it as the agent's RICTOC prompt.
3. Do **not** execute the agent or invent input. After printing, remind the
   operator to paste the input block the Orchestrator requested for this agent.

This is a thin dispatch helper. It only loads the canonical prompt; it must not
alter it.
