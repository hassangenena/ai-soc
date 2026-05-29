---
name: aisoc-catalogue
description: Register the AISOC Farm agent catalogue after the orchestrator has booted. Use when the user wants to load or register the 20-agent catalogue. Thin wrapper that loads and follows the canonical catalogue under .aisoc/.
---

# AISOC Farm — Catalogue (skill wrapper)

This is a **thin wrapper**. The canonical, authoritative catalogue lives at
`.aisoc/skills/catalogue/SKILL.md`. Do not summarise or change it — if this
wrapper ever disagrees with that file, that file wins.

Run this only after the Orchestrator has booted (see the `aisoc-boot` skill).

1. Read `.aisoc/skills/catalogue/SKILL.md` in full.
2. Register the 20 agents it lists as read-only data for the Orchestrator.
3. Emit its required readiness reply verbatim:
   `Catalogue registered, 20 agents available — ready for scenario.`
4. Then wait for the operator to paste a scenario.
