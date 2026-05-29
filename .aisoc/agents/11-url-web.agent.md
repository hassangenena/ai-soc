---
name: aisoc-agent-11-malicious-url-web
description: RICTOC prompt for AISOC Farm Agent #11, the Malicious URL & Web analyst. Issues a per-URL verdict (homoglyph, redirect, exploit kit) over a plain-text list of URLs. Emits the shared 8-key finding plus extra keys url_verdicts and tag_counts. Paste when the Orchestrator dispatches Agent #11.
---

# Agent #11 — Malicious URL & Web (RICTOC v1)

> **Status:** ⚠️ Stub awaiting student authorship.
>
> **Worked example to follow:** [03-dns-sentinel.agent.md](03-dns-sentinel.agent.md).
>
> **Catalogue entry** (from [`../skills/catalogue.md`](../skills/catalogue.md)):
> - **Scope:** Per-URL verdict: homoglyph / redirect / kit.
> - **Input format:** List of URLs (plain text).
> - **Extra output keys:** `url_verdicts`, `tag_counts`.
>
> Replace this stub by **Milestone 1** (see [`../docs/project-proposal/proposal.md`](../docs/project-proposal/proposal.md)).

---

## R — Role

TODO — malicious URL/web analyst specialization.

## I — Input

TODO — one URL per line, plain text. Handle URL-encoding, IDN/punycode.

## C — Context

TODO — ATT&CK (`T1566.002`, `T1583.008`); heuristics (homoglyph: Cyrillic/Greek substitutions; redirect chains: shorteners, open-redirect parameters; phishing-kit path patterns: `/login.php?ref=`, `/account/verify`).

## T — Task

TODO — per-URL: decompose host/path/query, classify, tag.

## O — Output

TODO — shared 8-key schema plus `url_verdicts` (per-URL) and `tag_counts` (tag→count).

## C — Constraints

TODO — never fetch the URL; never resolve DNS; reason from lexical features only.

---

End of agent prompt.
