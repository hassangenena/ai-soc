---
mode: ask
description: Boot the AISOC Farm and register the catalogue in one step
---

Initialize the AISOC Farm in this chat. Process the two canonical payloads **in
order** and follow each exactly:

1. Read [`.aisoc/skills/boot/SKILL.md`](../../.aisoc/skills/boot/SKILL.md), adopt
   the Orchestrator role, and emit its readiness reply
   (`AISOC Farm boot loaded — waiting for catalogue.`).
2. Read [`.aisoc/skills/catalogue/SKILL.md`](../../.aisoc/skills/catalogue/SKILL.md),
   register the catalogue, and emit its readiness reply
   (`Catalogue registered, 20 agents available — ready for scenario.`).

Do not act on any scenario yet. After both replies, wait for the operator to
paste a scenario (e.g. from `.aisoc/scenarios/`).
