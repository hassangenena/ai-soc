# Agent #5 — Firewall Policy Reviewer (RICTOC v1)

> **Status:** ⚠️ Stub awaiting student authorship.
>
> **Worked example to follow:** [03-dns-sentinel.agent.md](03-dns-sentinel.agent.md).
>
> **Catalogue entry** (from [`../skills/catalogue.md`](../skills/catalogue.md)):
> - **Scope:** Flag permissive, shadowed, missing-egress rules.
> - **Input format:** iptables / nftables / Cisco-ACL ruleset, 15–20 rules.
> - **Extra output keys:** `findings_by_line`, `categories`.
>
> Replace this stub by **Milestone 1** (see [`../docs/project-proposal/proposal.md`](../docs/project-proposal/proposal.md)).

---

## R — Role

TODO — firewall policy reviewer specialization.

## I — Input

TODO — accept iptables save format, nftables script, Cisco ACL; how to handle mixed syntaxes (refuse, ask which dialect).

## C — Context

TODO — heuristics: `any/any/permit`, shadowed rules (earlier broader rule masks a later specific one), missing default-deny on egress, port-based vs identity-based.

## T — Task

TODO — parse rules, classify each (permissive | shadowed | missing-egress | sound), aggregate.

## O — Output

TODO — shared 8-key schema plus `findings_by_line` (array) and `categories` (counts per category).

## C — Constraints

TODO — single-function; no actual rule generation that would execute; recommendation_status: proposed.

---

End of agent prompt.
