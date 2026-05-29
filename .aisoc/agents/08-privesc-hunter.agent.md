# Agent #8 — Privilege Escalation Hunter (RICTOC v1)

> **Status:** ⚠️ Stub awaiting student authorship.
>
> **Worked example to follow:** [03-dns-sentinel.agent.md](03-dns-sentinel.agent.md).
>
> **Catalogue entry** (from [`../skills/catalogue.md`](../skills/catalogue.md)):
> - **Scope:** Token manipulation, sudo abuse, group drift.
> - **Input format:** Windows Security 4672/4673 + Linux sudo log + group-add events.
> - **Extra output keys:** `escalation_events`, `attack_subtechniques`.
>
> Replace this stub by **Milestone 1** (see [`../docs/project-proposal/proposal.md`](../docs/project-proposal/proposal.md)).

---

## R — Role

TODO — privilege-escalation hunter specialization.

## I — Input

TODO — accept Windows Security 4672/4673 records + Linux sudo log lines + group-add events; allow mixed input.

## C — Context

TODO — ATT&CK (`T1134`, `T1548`, `T1098`); heuristics (special privileges assigned to unusual accounts, sudo NOPASSWD adds, group membership drift to privileged groups).

## T — Task

TODO — parse multi-source events, correlate by user/time, identify escalation chains.

## O — Output

TODO — shared 8-key schema plus `escalation_events` (array) and `attack_subtechniques` (ATT&CK sub-IDs).

## C — Constraints

TODO — single-function; no account lockout without Plan-and-Approve.

---

End of agent prompt.
