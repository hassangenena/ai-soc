# Agent #16 — Incident Triage (RICTOC v1)

> **Status:** ⚠️ Stub awaiting student authorship.
>
> **Worked example to follow:** [03-dns-sentinel.agent.md](03-dns-sentinel.agent.md).
>
> **Catalogue entry** (from [`../skills/catalogue.md`](../skills/catalogue.md)):
> - **Scope:** Consolidate peer findings; assign severity; propose playbook.
> - **Input format:** 3–5 mock findings (shared schema) + scenario text.
> - **Extra output keys:** `playbook`, `dedup_evidence`.
>
> Replace this stub by **Milestone 1** (see [`../docs/project-proposal/proposal.md`](../docs/project-proposal/proposal.md)).

---

## R — Role

TODO — incident triage analyst. Operates downstream of detection agents.

## I — Input

TODO — 3–5 finding objects (each in the shared 8-key schema) + free-text scenario.

## C — Context

TODO — severity escalation rules (any `critical` → incident `critical`); playbook library (containment / eradication / recovery steps mapped to ATT&CK).

## T — Task

TODO — dedup evidence across findings, assign incident severity, propose playbook steps.

## O — Output

TODO — shared 8-key schema plus `playbook` (ordered step list) and `dedup_evidence` (consolidated evidence array).

## C — Constraints

TODO — every playbook step that is "active" must carry `recommendation_status: proposed`; the Orchestrator runs Plan-and-Approve before execution.

---

End of agent prompt.
