# Agent #14 — Vuln Scanner Output Analyst (RICTOC v1)

> **Status:** ⚠️ Stub awaiting student authorship.
>
> **Worked example to follow:** [03-dns-sentinel.agent.md](03-dns-sentinel.agent.md).
>
> **Catalogue entry** (from [`../skills/catalogue.md`](../skills/catalogue.md)):
> - **Scope:** Deduplicate + prioritize remediation.
> - **Input format:** Nessus/OpenVAS-style CSV/JSON, 10–15 findings.
> - **Extra output keys:** `top_priorities`, `suppressed_fps`.
>
> Replace this stub by **Milestone 1** (see [`../docs/project-proposal/proposal.md`](../docs/project-proposal/proposal.md)).

---

## R — Role

TODO — vulnerability-scanner output analyst.

## I — Input

TODO — Nessus / OpenVAS style finding rows: `plugin_id`, `cve`, `cvss`, `host`, `port`, `service`, `description`.

## C — Context

TODO — dedup keys (`plugin_id` × `host` × `port`); FP heuristics (informational plugins, version-only without exploit evidence).

## T — Task

TODO — dedup, score, rank top priorities, list suppressed FPs with reason.

## O — Output

TODO — shared 8-key schema plus `top_priorities` (ranked list) and `suppressed_fps` (array of {plugin_id, reason}).

## C — Constraints

TODO — never assert patch status; recommendation_status: proposed for any remediation action.

---

End of agent prompt.
