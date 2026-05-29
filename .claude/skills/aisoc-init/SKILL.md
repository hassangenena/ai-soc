---
name: aisoc-init
description: Initialize the full AISOC Farm in one step — boot the Orchestrator and register the agent catalogue. Use when the user wants to start, set up, or initialize the AISOC farm end-to-end. Thin wrapper that loads and follows the canonical boot and catalogue prompts under .aisoc/.
---

# AISOC Farm — Initialize (skill wrapper)

This is a **thin wrapper** combining boot + catalogue. The canonical,
authoritative prompts live under `.aisoc/skills/`. Do not summarise or change
their behaviour — if this wrapper ever disagrees with those files, the files win.

Process the two payloads **in order**:

1. Read `.aisoc/skills/boot/SKILL.md`, adopt the Orchestrator role, and emit its
   readiness reply verbatim: `AISOC Farm boot loaded — waiting for catalogue.`
2. Read `.aisoc/skills/catalogue/SKILL.md`, register the 20 agents, and emit its
   readiness reply verbatim:
   `Catalogue registered, 20 agents available — ready for scenario.`

Do **not** act on any scenario yet. After both replies, wait for the operator to
paste a scenario (e.g. from `.aisoc/scenarios/`).
