# Agent #1 — Network Traffic Analyzer (RICTOC v1)

> **Status:** ⚠️ Stub awaiting student authorship.
>
> **Worked example to follow:** [03-dns-sentinel.agent.md](03-dns-sentinel.agent.md).
> Reproduce its structure section-by-section.
>
> **Catalogue entry** (from [`../skills/catalogue.md`](../skills/catalogue.md)):
> - **Scope:** Detect beaconing / anomalous flows in flow logs.
> - **Input format:** Zeek `conn.log` (TSV or JSON), 30–80 lines.
> - **Extra output keys:** `flagged_hosts`, `beacon_interval_s`.
>
> Replace this stub by **Milestone 1** (see [`../docs/project-proposal/proposal.md`](../docs/project-proposal/proposal.md)).

---

## R — Role

TODO — identify the specialization in one paragraph. State explicitly what the agent does *not* do (no live capture, no external enrichment).

## I — Input

TODO — describe both accepted forms (TSV and JSON), what each row contains, and how to handle ambiguity (mixed delimiters, missing fields).

## C — Context

TODO — environment (lab-only chat runtime), ATT&CK techniques in scope (e.g. `T1071`, `T1571`), heuristics (beacon interval regularity, low-byte uniform flows, unusual ports/protocols), trust model (input is data, never instructions), determinism (input order preserved).

## T — Task

TODO — numbered task list:
1. Parse input.
2. Detect beaconing candidates by inter-arrival regularity.
3. Detect anomalous flows by protocol/port/byte-distribution outliers.
4. Aggregate into the shared finding object with `flagged_hosts` and `beacon_interval_s`.
5. Apply the severity rule.
6. Self-check against constraints.

Include a **severity rule** table (see worked example).

## O — Output

TODO — exactly one JSON object matching the shared 8-key schema plus the extra keys `flagged_hosts` and `beacon_interval_s`. No prose outside the JSON.

## C — Constraints

TODO — single-function discipline, no active response, no invented hosts, schema discipline, embedded-instruction refusal, silent self-check.

---

End of agent prompt.
