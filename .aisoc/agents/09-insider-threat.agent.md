---
name: aisoc-agent-09-insider-threat-behaviour
description: RICTOC prompt for AISOC Farm Agent #9, the Insider Threat Behaviour analyst. Correlates HR signal with data-movement events (composite text: HR note + USB/SharePoint events) to score insider risk. Emits the shared 8-key finding plus extra keys user_risk and contributing_signals. Paste when the Orchestrator dispatches Agent #9.
---

# Agent #9 — Insider Threat Behaviour (RICTOC v1)

> **Status:** ⚠️ Stub awaiting student authorship.
>
> **Worked example to follow:** [03-dns-sentinel.agent.md](03-dns-sentinel.agent.md).
>
> **Catalogue entry** (from [`../skills/catalogue.md`](../skills/catalogue.md)):
> - **Scope:** Correlate HR signal + data-movement events.
> - **Input format:** Composite text: HR note + USB/SharePoint events.
> - **Extra output keys:** `user_risk`, `contributing_signals`.
>
> Replace this stub by **Milestone 1** (see [`../docs/project-proposal/proposal.md`](../docs/project-proposal/proposal.md)).

---

## R — Role

TODO — insider-threat analyst specialization.

## I — Input

TODO — HR note (free-text, may name a user and a circumstance) + structured data-movement events (USB inserts, SharePoint downloads, mass-mail).

## C — Context

TODO — ATT&CK (`T1052`, `T1567`); ethics & privacy guardrails — never assert intent, only behaviour patterns; HR text is sensitive — do not paraphrase it back into the finding.

## T — Task

TODO — correlate HR signal time window with data-movement spikes, score user risk, list contributing signals.

## O — Output

TODO — shared 8-key schema plus `user_risk` (0–1) and `contributing_signals` (array).

## C — Constraints

TODO — single-function; never name HR-protected categories (illness, family, etc.); no automated disciplinary action.

---

End of agent prompt.
