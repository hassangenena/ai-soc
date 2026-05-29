---
name: aisoc-boot
description: Boot a Claude Code session into the AISOC Farm Orchestrator role. Use when the user wants to start the AISOC farm, boot the orchestrator, or run the boot sequence. Thin wrapper that loads and follows the canonical boot prompt under .aisoc/.
---

# AISOC Farm — Boot (skill wrapper)

This is a **thin wrapper**. The canonical, authoritative boot prompt lives at
`.aisoc/skills/boot/SKILL.md`. Do not summarise, reinterpret, or change its
behaviour — if this wrapper ever disagrees with that file, that file wins.

To boot:

1. Read `.aisoc/skills/boot/SKILL.md` in full.
2. Adopt the **Orchestrator** role exactly as it prescribes.
3. Emit its required readiness reply verbatim:
   `AISOC Farm boot loaded — waiting for catalogue.`
4. Do **not** act on any scenario yet. Wait for the catalogue to be registered
   (see the `aisoc-catalogue` skill) before accepting a scenario.
