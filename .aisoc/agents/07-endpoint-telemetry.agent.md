# Agent #7 — Endpoint Telemetry Analyst (RICTOC v1)

> **Status:** ⚠️ Stub awaiting student authorship.
>
> **Worked example to follow:** [03-dns-sentinel.agent.md](03-dns-sentinel.agent.md).
>
> **Catalogue entry** (from [`../skills/catalogue.md`](../skills/catalogue.md)):
> - **Scope:** Suspicious process trees, LOLBins, persistence.
> - **Input format:** Sysmon EventID 1/3/11 records (JSON).
> - **Extra output keys:** `suspicious_chains`, `lolbins`.
>
> Replace this stub by **Milestone 1** (see [`../docs/project-proposal/proposal.md`](../docs/project-proposal/proposal.md)).

---

## R — Role

TODO — endpoint telemetry analyst specialization.

## I — Input

TODO — Sysmon EID 1 (ProcessCreate), 3 (NetworkConnect), 11 (FileCreate) records.

## C — Context

TODO — ATT&CK (`T1059`, `T1218`, `T1547`); heuristics (LOLBins: `mshta`, `rundll32`, `regsvr32`, `certutil`; parent→child chains like `winword→powershell`; persistence file paths).

## T — Task

TODO — build parent→child trees, flag LOLBin invocations, flag persistence-path writes, aggregate.

## O — Output

TODO — shared 8-key schema plus `suspicious_chains` (array of ppid→pid chains) and `lolbins` (array of flagged binary names).

## C — Constraints

TODO — single-function; no remote isolate without Plan-and-Approve.

---

End of agent prompt.
