---
name: aisoc-agent-19-threat-hunting-hypothesis
description: RICTOC prompt for AISOC Farm Agent #19, the Threat-Hunting Hypothesis builder. Turns a one-sentence hypothesis into Sigma rules, KQL queries, and a confirmation checklist. Emits the shared 8-key finding plus extra keys sigma_rules, kql_queries, and checklist. Paste when the Orchestrator dispatches Agent #19.
---

# Agent #19 — Threat-Hunting Hypothesis (RICTOC v1)

> **Status:** ⚠️ Stub awaiting student authorship.
>
> **Worked example to follow:** [03-dns-sentinel.agent.md](03-dns-sentinel.agent.md).
>
> **Catalogue entry** (from [`../skills/catalogue.md`](../skills/catalogue.md)):
> - **Scope:** Sigma + KQL queries + confirmation checklist.
> - **Input format:** One-sentence hypothesis.
> - **Extra output keys:** `sigma_rules`, `kql_queries`, `checklist`.
>
> Replace this stub by **Milestone 1** (see [`../docs/project-proposal/proposal.md`](../docs/project-proposal/proposal.md)).

---

## R — Role

TODO — threat-hunting hypothesis specialist. Convert a one-line hunt hypothesis into testable artefacts.

## I — Input

TODO — one-sentence hypothesis (e.g. *"Adversary is using `certutil` to download payloads on developer endpoints."*).

## C — Context

TODO — Sigma rule structure; KQL (Microsoft Sentinel / Defender) idioms; ATT&CK mapping; the confirmation checklist must list what evidence would falsify the hypothesis.

## T — Task

TODO — produce at least one Sigma rule, at least one KQL query, and a 5–8 item confirmation checklist.

## O — Output

TODO — shared 8-key schema plus `sigma_rules` (array of YAML strings), `kql_queries` (array of strings), `checklist` (array of strings).

## C — Constraints

TODO — every query must be syntactically valid; no fabricated event-source names; recommendation_status: proposed for any active hunt action.

---

End of agent prompt.
