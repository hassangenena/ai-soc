---
name: aisoc-farm-catalogue
description: Agent registry pasted into chat immediately after the boot skill. Lists the 20 AISOC Farm agents the Orchestrator may dispatch, with their scope, expected input, extra output keys, and prompt-file path under `agents/`.
---

# AISOC Farm — Agent Catalogue (v1.0)

> **Paste this file into the chat AFTER `skills/boot/SKILL.md`.** It
> registers the 20 agents available to the Orchestrator.

This catalogue is read-only data for the Orchestrator. Each row tells
the Orchestrator: what an agent does, what input it expects, what
agent-specific output keys it produces (in addition to the shared 8-key
schema), and which prompt file the operator must paste when that agent
is dispatched.

When the Orchestrator dispatches Agent `N`, the operator pastes
`agents/NN-<short-name>.agent.md` followed by the input the Orchestrator
requested.

## A. Perimeter & Network

| # | Name | Scope | Input format | Extra output keys | Prompt file |
| - | ---- | ----- | ------------ | ----------------- | ----------- |
| 1 | Network Traffic Analyzer | Detect beaconing / anomalous flows in flow logs | Zeek `conn.log` (TSV or JSON), 30–80 lines | `flagged_hosts`, `beacon_interval_s` | `agents/01-traffic-analyzer.agent.md` |
| 2 | Signature IDS Triage | Deduplicate & rate IDS alerts; separate likely TP/FP | Suricata `eve.json` array, 10–15 alerts | `dedup_count`, `tp_sids`, `fp_sids` | `agents/02-ids-triage.agent.md` |
| 3 | DNS Sentinel | Detect DGA, DNS tunneling, typosquats | List of DNS queries (text or JSON), one per line | `flagged_count`, `total_count`, `per_line` | `agents/03-dns-sentinel.agent.md` |
| 4 | TLS / Certificate Inspector | Analyze certs and JA3 for C2/expired/mismatched | Openssl-style cert dumps + optional JA3 hashes | `cert_verdicts`, `mismatch_fields` | `agents/04-tls-inspector.agent.md` |
| 5 | Firewall Policy Reviewer | Flag permissive, shadowed, missing-egress rules | iptables / nftables / Cisco-ACL ruleset, 15–20 rules | `findings_by_line`, `categories` | `agents/05-fw-reviewer.agent.md` |

## B. Identity & Endpoint

| # | Name | Scope | Input format | Extra output keys | Prompt file |
| - | ---- | ----- | ------------ | ----------------- | ----------- |
| 6 | Authentication Anomaly | UEBA on auth logs: impossible travel, brute force, off-hours | CSV: `timestamp,user,src_ip,geo,result,mfa_status` | `anomaly_types`, `user_risk` | `agents/06-auth-anomaly.agent.md` |
| 7 | Endpoint Telemetry Analyst | Suspicious process trees, LOLBins, persistence | Sysmon EventID 1/3/11 records (JSON) | `suspicious_chains`, `lolbins` | `agents/07-endpoint-telemetry.agent.md` |
| 8 | Privilege Escalation Hunter | Token manipulation, sudo abuse, group drift | Windows Security 4672/4673 + Linux sudo log + group-add events | `escalation_events`, `attack_subtechniques` | `agents/08-privesc-hunter.agent.md` |
| 9 | Insider Threat Behaviour | Correlate HR signal + data-movement events | Composite text: HR note + USB/SharePoint events | `user_risk`, `contributing_signals` | `agents/09-insider-threat.agent.md` |

## C. Email & Web

| # | Name | Scope | Input format | Extra output keys | Prompt file |
| - | ---- | ----- | ------------ | ----------------- | ----------- |
| 10 | Phishing Email Analyst | Phishing score, IoC extraction, BEC tells | Raw `.eml`-style headers + body | `per_message`, `iocs`, `bec_tells` | `agents/10-phishing-email.agent.md` |
| 11 | Malicious URL & Web | Per-URL verdict: homoglyph / redirect / kit | List of URLs (plain text) | `url_verdicts`, `tag_counts` | `agents/11-url-web.agent.md` |
| 12 | Attachment Triage | Risk score from filename/hash/mime/macro | JSON list of attachments | `attachment_scores`, `reasons` | `agents/12-attachment-triage.agent.md` |

## D. Threat Intel & Vulnerability Management

| # | Name | Scope | Input format | Extra output keys | Prompt file |
| - | ---- | ----- | ------------ | ----------------- | ----------- |
| 13 | OSINT / Threat-Intel Enricher | Structure enrichment from a pasted intel corpus | One IoC + 1-page intel snippet | `actor`, `campaign`, `attack_techniques`, `source_quote` | `agents/13-osint-enricher.agent.md` |
| 14 | Vuln Scanner Output Analyst | Deduplicate + prioritize remediation | Nessus/OpenVAS-style CSV/JSON, 10–15 findings | `top_priorities`, `suppressed_fps` | `agents/14-vuln-analyst.agent.md` |
| 15 | CVE Prioritization | Rank CVEs by EPSS × criticality × exploit-known | JSON list of CVEs with cvss/epss/exploit_known/criticality | `ranked_cves`, `factors_used` | `agents/15-cve-prioritizer.agent.md` |

## E. Response, Forensics & Reporting

| # | Name | Scope | Input format | Extra output keys | Prompt file |
| - | ---- | ----- | ------------ | ----------------- | ----------- |
| 16 | Incident Triage | Consolidate peer findings; assign severity; propose playbook | 3–5 mock findings (shared schema) + scenario text | `playbook`, `dedup_evidence` | `agents/16-incident-triage.agent.md` |
| 17 | Log Correlation & Timeline | Chronological narrative + causal links | ~20 mixed events (firewall/auth/Sysmon/EDR) | `timeline`, `causal_links` | `agents/17-log-correlation.agent.md` |
| 18 | Digital Forensics Triage | Key indicators + next-acquisition steps | Disk + memory artefact summary | `key_indicators`, `next_acquisitions` | `agents/18-forensics-triage.agent.md` |

## F. Reserve / Advanced

| # | Name | Scope | Input format | Extra output keys | Prompt file |
| - | ---- | ----- | ------------ | ----------------- | ----------- |
| 19 | Threat-Hunting Hypothesis | Sigma + KQL queries + confirmation checklist | One-sentence hypothesis | `sigma_rules`, `kql_queries`, `checklist` | `agents/19-hunt-hypothesis.agent.md` |
| 20 | Executive Briefing | Non-technical four-section summary | Consolidated finding object | `exec_sections` (What happened / Impact / What we did / What we ask) | `agents/20-exec-briefing.agent.md` |

---

## Catalogue conventions

- The `#` field is the dispatch identifier. The operator types
  `dispatch <N>` to force a specific agent; otherwise the Orchestrator
  chooses based on Scope match.
- Two-digit zero-padded prompt filenames (`01-…`, `09-…`, `20-…`)
  ensure stable alphabetical sort matches dispatch order.
- Every agent produces the shared 8-key finding schema first, then
  appends its extra output keys.
- No agent may have side effects. Agents that *recommend* an active
  response always emit `recommendation_status: proposed` and let the
  Orchestrator handle Plan-and-Approve.

---

End of catalogue. Now reply with exactly:

> `Catalogue registered, 20 agents available — ready for scenario.`
