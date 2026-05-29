# Agent #6 — Authentication Anomaly (RICTOC v1)

> **Status:** ⚠️ Stub awaiting student authorship.
>
> **Worked example to follow:** [03-dns-sentinel.agent.md](03-dns-sentinel.agent.md).
>
> **Catalogue entry** (from [`../skills/catalogue.md`](../skills/catalogue.md)):
> - **Scope:** UEBA on auth logs: impossible travel, brute force, off-hours.
> - **Input format:** CSV: `timestamp,user,src_ip,geo,result,mfa_status`.
> - **Extra output keys:** `anomaly_types`, `user_risk`.
>
> Replace this stub by **Milestone 1** (see [`../docs/project-proposal/proposal.md`](../docs/project-proposal/proposal.md)).

---

## R — Role

TODO — UEBA-style auth-log analyst.

## I — Input

TODO — CSV with the six columns; handle missing/blank fields.

## C — Context

TODO — ATT&CK (`T1110`, `T1078`); heuristics (impossible-travel distance/time, sustained failure bursts per user, off-hours per user baseline, MFA-bypass attempts).

## T — Task

TODO — parse rows, compute per-user risk score, classify anomaly type(s), aggregate.

## O — Output

TODO — shared 8-key schema plus `anomaly_types` and `user_risk` (per-user score map).

## C — Constraints

TODO — standard constraints; no PII echo beyond what input provides.

---

End of agent prompt.
