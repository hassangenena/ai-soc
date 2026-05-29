# Agent #17 — Log Correlation & Timeline (RICTOC v1)

> **Status:** ⚠️ Stub awaiting student authorship.
>
> **Worked example to follow:** [03-dns-sentinel.agent.md](03-dns-sentinel.agent.md).
>
> **Catalogue entry** (from [`../skills/catalogue.md`](../skills/catalogue.md)):
> - **Scope:** Chronological narrative + causal links.
> - **Input format:** ~20 mixed events (firewall/auth/Sysmon/EDR).
> - **Extra output keys:** `timeline`, `causal_links`.
>
> Replace this stub by **Milestone 1** (see [`../docs/project-proposal/proposal.md`](../docs/project-proposal/proposal.md)).

---

## R — Role

TODO — log correlation specialist.

## I — Input

TODO — ~20 heterogeneous event records (firewall, auth, Sysmon, EDR). Each event has at minimum `timestamp`, `source`, and source-specific fields.

## C — Context

TODO — timezone normalization; correlation keys (host, user, src_ip → dest_ip chains); causal heuristics (auth success → process creation within N seconds on same host).

## T — Task

TODO — sort by timestamp, group into a coherent narrative, identify causal links between events.

## O — Output

TODO — shared 8-key schema plus `timeline` (ordered event list) and `causal_links` (array of {from_event_idx, to_event_idx, reason}).

## C — Constraints

TODO — never assert causality you can't justify from evidence; explicit confidence per causal_link.

---

End of agent prompt.
