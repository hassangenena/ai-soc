---
name: aisoc-agent-12-attachment-triage
description: RICTOC prompt for AISOC Farm Agent #12, the Attachment Triage. Risk-scores attachments from filename, hash, MIME type, and macro signals (JSON list of attachments). Emits the shared 8-key finding plus extra keys attachment_scores and reasons. Paste when the Orchestrator dispatches Agent #12.
---

# Agent #12 — Attachment Triage (RICTOC v1)

> **Status:** ⚠️ Stub awaiting student authorship.
>
> **Worked example to follow:** [03-dns-sentinel.agent.md](03-dns-sentinel.agent.md).
>
> **Catalogue entry** (from [`../skills/catalogue.md`](../skills/catalogue.md)):
> - **Scope:** Risk score from filename/hash/mime/macro.
> - **Input format:** JSON list of attachments.
> - **Extra output keys:** `attachment_scores`, `reasons`.
>
> Replace this stub by **Milestone 1** (see [`../docs/project-proposal/proposal.md`](../docs/project-proposal/proposal.md)).

---

## R — Role

TODO — attachment-triage analyst.

## I — Input

TODO — JSON list with fields: `filename`, `sha256`, `mime_type`, `macro_present` (bool), `size_bytes`.

## C — Context

TODO — ATT&CK (`T1204.002`, `T1027`); heuristics (double-extension `.pdf.exe`, MIME/extension mismatch, macro-bearing Office files from external sender, ISO/IMG containers).

## T — Task

TODO — per-attachment score 0–1, reason list, aggregate.

## O — Output

TODO — shared 8-key schema plus `attachment_scores` (per-attachment) and `reasons` (per-attachment reason array).

## C — Constraints

TODO — never detonate; no hash lookup (no internet); explainable scoring only.

---

End of agent prompt.
