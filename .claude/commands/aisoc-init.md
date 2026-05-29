---
description: Boot the AISOC Farm and register the catalogue in one step
---

Initialize the AISOC Farm in this session. Process the two canonical payloads
below **in order** and follow each exactly:

1. First, adopt the Orchestrator role from the boot skill and emit its required
   readiness reply (`AISOC Farm boot loaded — waiting for catalogue.`).
2. Then, register the agent catalogue and emit its readiness reply
   (`Catalogue registered, 20 agents available — ready for scenario.`).

Do not act on any scenario yet. After both replies, wait for the operator to
paste a scenario.

--- BOOT SKILL ---

@.aisoc/skills/boot/SKILL.md

--- AGENT CATALOGUE ---

@.aisoc/skills/catalogue/SKILL.md
