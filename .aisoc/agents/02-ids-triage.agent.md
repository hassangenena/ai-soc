---
name: aisoc-agent-02-signature-ids-triage
description: RICTOC prompt for AISOC Farm Agent #2, the Signature IDS Triage. Deduplicates and rates a Suricata eve.json alert array (10–15 alerts), separating likely true positives from false positives. Emits the shared 8-key finding plus extra keys dedup_count, tp_sids, and fp_sids. Paste when the Orchestrator dispatches Agent #2.
---

# Agent #2 — Signature IDS Triage (RICTOC v1)

> **Status:** ⚠️ Stub awaiting student authorship.
>
> **Worked example to follow:** [03-dns-sentinel.agent.md](03-dns-sentinel.agent.md).
>
> **Catalogue entry** (from [`../skills/catalogue.md`](../skills/catalogue.md)):
> - **Scope:** Deduplicate & rate IDS alerts; separate likely TP/FP.
> - **Input format:** Suricata `eve.json` array, 10–15 alerts.
> - **Extra output keys:** `dedup_count`, `tp_sids`, `fp_sids`.
>
> Replace this stub by **Milestone 1** (see [`../docs/project-proposal/proposal.md`](../docs/project-proposal/proposal.md)).

---

## R — Role

TODO — specialization paragraph.

## I — Input

TODO — Suricata `eve.json` array; key fields (`sid`, `signature`, `src_ip`, `dest_ip`, `proto`, `severity`).

## C — Context

TODO — ATT&CK mapping per signature class; FP heuristics (low-severity ET-INFO, internal scanners, known business apps); dedup keys.

## T — Task

TODO — numbered tasks: parse, dedup by `(sid, src_ip, dest_ip)`, classify TP/FP, aggregate, apply severity rule.

## O — Output

TODO — shared 8-key schema plus `dedup_count`, `tp_sids` (array of SIDs judged TP), `fp_sids` (array of SIDs judged FP).

## C — Constraints

TODO — standard constraints (see worked example).

---

End of agent prompt.
