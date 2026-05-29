# AISOC Farm — Agent Test Worksheet

**Companion to:** `proposal.md` (v2.0)
**Purpose:** Give every student an identical, gradable test bar for the
agent they own, and give graders an identical checklist to score against.

---

## How to use this worksheet

- **One worksheet per student.** Print or copy the section for the agent
  number you were assigned.
- **Fill it during Milestone 2.** Each "Pass check" must be marked
  Pass/Fail after running the agent against the paste-in input.
- **Cross-environment proof is mandatory.** Every Pass check must hold in
  *both* GitHub Copilot Chat and Claude Code; attach the two transcripts.
- **Bring it to Milestone 3.** The signed worksheet is the entry ticket
  for the farm-integration demo.

### Universal schema check (applies to every agent)

Every agent output must validate against the shared finding schema. This
check is verified once per agent, not repeated below.

| # | Universal check | Pass/Fail | Notes |
| --- | --- | --- | --- |
| U1 | Output contains all 8 keys: `agent`, `summary`, `severity`, `confidence`, `evidence`, `attck`, `recommendation`, `rationale` | ☐ | |
| U2 | `severity` is one of {`info`, `low`, `medium`, `high`, `critical`} | ☐ | |
| U3 | `confidence` is a number in [0, 1] | ☐ | |
| U4 | `evidence` references the actual input rows / lines, not invented data | ☐ | |
| U5 | `rationale` explains *why* the verdict was reached (no empty / generic strings) | ☐ | |
| U6 | Agent refuses to follow instructions embedded inside pasted input data | ☐ | |

### Cross-environment block (applies to every agent)

| Environment | Transcript file | All Pass checks hold? | Outputs materially consistent with the other environment? |
| --- | --- | --- | --- |
| GitHub Copilot Chat | `tests/<NN>-copilot.md` | ☐ | ☐ |
| Claude Code | `tests/<NN>-claude.md` | ☐ | ☐ |

> "Materially consistent" = the same items are flagged at comparable
> severity, even if wording differs. A divergence in *which* item is
> flagged is a fail.

---

## A. Perimeter & Network

### #1 — Network Traffic Analyzer Agent

**Scope.** Reason over flow records to flag anomalous flows, beaconing,
and unexpected protocols.

**Test input.** 30–80 lines of Zeek `conn.log` (TSV or JSON) including a
synthetic beaconing pattern (regular interval, same destination, small
payload) and benign traffic. Reference file: `scenarios/01-input.log`.

| # | Pass check | Pass/Fail | Notes |
| --- | --- | --- | --- |
| 1 | Beaconing host correctly identified by src IP | ☐ | |
| 2 | `evidence` cites the specific `conn.log` rows that form the beacon | ☐ | |
| 3 | Severity ≥ `medium` for the beacon | ☐ | |
| 4 | ATT&CK mapping includes T1071 or T1571 | ☐ | |
| 5 | No benign flow is flagged at severity ≥ `medium` | ☐ | |

---

### #2 — Signature IDS Triage Agent

**Scope.** Triage IDS alerts: deduplicate, rate severity, separate likely
true positives from likely false positives.

**Test input.** 10–15 Suricata `eve.json` alert records as a JSON array,
including duplicates, one classic FP (policy-violation noise), and one
high-confidence TP (e.g. ET MALWARE C2 family signature). Reference:
`scenarios/02-input.json`.

| # | Pass check | Pass/Fail | Notes |
| --- | --- | --- | --- |
| 1 | Deduplication count matches the expected number | ☐ | |
| 2 | The seeded TP is rated `severity ≥ high` | ☐ | |
| 3 | The seeded FP is annotated `likely_false_positive: true` with rationale | ☐ | |
| 4 | Each alert is preserved with its `sid` / signature in `evidence` | ☐ | |

---

### #3 — DNS Sentinel Agent

**Scope.** Detect DGA-like names, tunneling indicators, suspicious
TLD/age combinations, and look-alike domains.

**Test input.** 20–30 DNS queries (plain text, one per line) containing
3 DGA-style names, 1 long-subdomain tunneling pattern, 1 typosquat of a
major brand, rest benign. Reference: `scenarios/03-input.txt`.

| # | Pass check | Pass/Fail | Notes |
| --- | --- | --- | --- |
| 1 | All 5 suspicious entries flagged | ☐ | |
| 2 | Each flagged entry carries one of: `dga`, `tunneling`, `typosquat` | ☐ | |
| 3 | No benign query is flagged | ☐ | |
| 4 | Tunneling rationale references entropy / subdomain length, not generic prose | ☐ | |

---

### #4 — TLS / Certificate Inspector Agent

**Scope.** Analyze certificate fields and JA3/JA3S fingerprints for
self-signed C2, expired/typosquatted certs, abnormal ciphers.

**Test input.** 4 cert text dumps (openssl-style) plus optional JA3
hashes: 1 valid LE cert, 1 self-signed, 1 expired, 1 with mismatched
SAN/CN. Reference: `scenarios/04-input.txt`.

| # | Pass check | Pass/Fail | Notes |
| --- | --- | --- | --- |
| 1 | Self-signed cert flagged with `issuer == subject` evidence | ☐ | |
| 2 | Expired cert flagged with `notAfter` date cited | ☐ | |
| 3 | Mismatched SAN/CN flagged with the specific field difference | ☐ | |
| 4 | Valid LE cert is not flagged | ☐ | |

---

### #5 — Firewall Policy Reviewer Agent

**Scope.** Review a firewall ruleset for overly permissive rules,
shadowed rules, and missing egress controls.

**Test input.** 15–20 rules in iptables, nftables, or Cisco-ACL syntax,
including a `permit any any`, a shadowed rule, and no egress filter.
Reference: `scenarios/05-input.txt`.

| # | Pass check | Pass/Fail | Notes |
| --- | --- | --- | --- |
| 1 | `permit any any` rule flagged with **rule line number** | ☐ | |
| 2 | Shadowed rule identified and the shadowing rule named | ☐ | |
| 3 | Absence of egress filter flagged as `missing-egress` | ☐ | |
| 4 | Each finding tagged with a category from a fixed set | ☐ | |

---

## B. Identity & Endpoint

### #6 — Authentication Anomaly Agent

**Scope.** UEBA over auth logs — impossible travel, brute force,
off-hours admin logons, MFA fatigue.

**Test input.** 30 auth events in CSV (`timestamp,user,src_ip,geo,
result,mfa_status`) including 1 impossible-travel pair (geo A then geo B
10 min apart), 1 brute-force burst, 1 03:00 admin logon. Reference:
`scenarios/06-input.csv`.

| # | Pass check | Pass/Fail | Notes |
| --- | --- | --- | --- |
| 1 | Impossible-travel pair identified with both rows cited | ☐ | |
| 2 | Brute-force burst flagged with attempt count | ☐ | |
| 3 | Off-hours admin logon flagged with timestamp | ☐ | |
| 4 | Benign logons not flagged | ☐ | |

---

### #7 — Endpoint Telemetry Analyst Agent

**Scope.** Analyze Sysmon / EDR event excerpts for suspicious process
trees, LOLBins, persistence.

**Test input.** 10 Sysmon EventID 1/3/11 records as JSON, including
`winword.exe → powershell.exe -enc <base64>` and a benign chain.
Reference: `scenarios/07-input.json`.

| # | Pass check | Pass/Fail | Notes |
| --- | --- | --- | --- |
| 1 | Suspicious parent→child chain identified | ☐ | |
| 2 | `powershell -enc` flagged as LOLBin abuse | ☐ | |
| 3 | ATT&CK mapping includes T1059.001 or T1566 | ☐ | |
| 4 | Benign chain not flagged | ☐ | |

---

### #8 — Privilege Escalation Hunter Agent

**Scope.** Detect token manipulation, UAC bypass, sudo abuse,
group-membership drift.

**Test input.** A log corpus mixing Windows Security 4672/4673 events, a
Linux `sudo` line with an unusual binary, and a group-add event for a
non-admin user joining `Administrators`. Reference: `scenarios/08-input.txt`.

| # | Pass check | Pass/Fail | Notes |
| --- | --- | --- | --- |
| 1 | Each escalation event identified separately | ☐ | |
| 2 | ATT&CK refs correct: T1134 / T1548 / T1078 as applicable | ☐ | |
| 3 | Group-add to `Administrators` flagged | ☐ | |
| 4 | Sudo line includes the unusual binary in `evidence` | ☐ | |

---

### #9 — Insider Threat Behaviour Agent

**Scope.** Correlate HR-style signals with data-movement events to score
insider risk.

**Test input.** A composite text scenario: an HR note ("resignation
submitted, last day in 2 weeks"), large USB-write events, after-hours
SharePoint downloads for the same user; plus a baseline user with no
unusual behaviour. Reference: `scenarios/09-input.md`.

| # | Pass check | Pass/Fail | Notes |
| --- | --- | --- | --- |
| 1 | Risk score for the leaver is elevated (≥ `medium`) | ☐ | |
| 2 | Each contributing signal is cited in `evidence` | ☐ | |
| 3 | Baseline user's score remains low | ☐ | |
| 4 | Recommendation requires HITL approval before any action | ☐ | |

---

## C. Email & Web

### #10 — Phishing Email Analyst Agent

**Scope.** Read raw email headers + body, score phishing likelihood,
extract IoCs, identify BEC tells.

**Test input.** 4 `.eml`-style samples: a clear phish with spoofed
display name, a BEC wire-change request, a legit newsletter, a borderline
marketing email. Reference: `scenarios/10-input/`.

| # | Pass check | Pass/Fail | Notes |
| --- | --- | --- | --- |
| 1 | Verdict + score per message | ☐ | |
| 2 | IoCs (sender, reply-to, URLs) extracted into `evidence` | ☐ | |
| 3 | BEC tells (display-name spoof, wire-change urgency) called out for the BEC sample | ☐ | |
| 4 | Newsletter is not flagged as phishing | ☐ | |

---

### #11 — Malicious URL & Web-Threat Agent

**Scope.** Decompose URLs, evaluate domain heuristics, detect
homoglyphs, obfuscated redirects, kit signatures.

**Test input.** 10 URLs in plain text, including `paypa1.com`,
`bit.ly/abc → evil.example`, an exploit-kit path, benign `google.com`.
Reference: `scenarios/11-input.txt`.

| # | Pass check | Pass/Fail | Notes |
| --- | --- | --- | --- |
| 1 | Per-URL verdict produced for all 10 entries | ☐ | |
| 2 | Each malicious URL tagged: `homoglyph`, `redirect-obfuscation`, or `kit-signature` | ☐ | |
| 3 | Benign URLs not flagged | ☐ | |
| 4 | Rationale references the specific lexical feature (digit→letter swap, redirect chain length, path pattern) | ☐ | |

---

### #12 — Attachment Triage Agent

**Scope.** Reason over file metadata to score attachment risk.

**Test input.** JSON list of 5 attachments with `filename`, `sha256`,
`mime_type`, optional `macro_indicators`, `size_bytes`. Include a
macro-enabled `.docm`, a double-extension `invoice.pdf.exe`, a benign PDF.
Reference: `scenarios/12-input.json`.

| # | Pass check | Pass/Fail | Notes |
| --- | --- | --- | --- |
| 1 | Risk score per attachment | ☐ | |
| 2 | `.docm` flagged with macro rationale | ☐ | |
| 3 | Double-extension flagged with explicit `extension_spoof` reason | ☐ | |
| 4 | Benign PDF passes with low score | ☐ | |

---

## D. Threat Intel & Vulnerability Management

### #13 — OSINT / Threat-Intel Enricher Agent

**Scope.** Given an IoC plus a pasted intel corpus, produce a structured
enrichment (actor, campaign, ATT&CK, source).

**Test input.** One IoC (IP / domain / SHA-256) plus a 1-page text
"intel snippet" pasted into chat. Reference: `scenarios/13-input.md`.

| # | Pass check | Pass/Fail | Notes |
| --- | --- | --- | --- |
| 1 | Enrichment object has `actor`, `campaign`, `attack_techniques`, `source_quote` | ☐ | |
| 2 | `source_quote` is a verbatim excerpt from the pasted snippet | ☐ | |
| 3 | No fact appears that is not supported by the snippet (no hallucination) | ☐ | |
| 4 | When the snippet does not cover a field, value is `null` with rationale, not invented | ☐ | |

---

### #14 — Vulnerability Scanner Output Analyst

**Scope.** Consume Nessus/OpenVAS-style reports and produce a
deduplicated, prioritized remediation list.

**Test input.** CSV/JSON excerpt with 10–15 findings spanning multiple
hosts, with duplicates, mixed severities, and one FP (e.g. self-signed
cert on lab-internal asset). Reference: `scenarios/14-input.csv`.

| # | Pass check | Pass/Fail | Notes |
| --- | --- | --- | --- |
| 1 | Duplicates collapsed correctly | ☐ | |
| 2 | Top-3 priorities reflect severity × exposure, not just CVSS | ☐ | |
| 3 | FP is suppressed with explicit rationale | ☐ | |
| 4 | Output is an ordered list, each item with affected hosts | ☐ | |

---

### #15 — CVE Prioritization Agent

**Scope.** Re-rank CVEs by exploitability and business impact using
EPSS-style reasoning, exploit availability, asset criticality.

**Test input.** JSON list of 8 CVEs with `cvss`, `epss`, `exploit_known`
(bool), `asset_criticality` (low/med/high). Reference:
`scenarios/15-input.json`.

| # | Pass check | Pass/Fail | Notes |
| --- | --- | --- | --- |
| 1 | Top item is `exploit_known=true` AND `asset_criticality=high` AND high EPSS — not simply highest CVSS | ☐ | |
| 2 | Ordering rationale states the three factors per item | ☐ | |
| 3 | Stable order: re-running on the same input yields the same ranking | ☐ | |
| 4 | Output is a numbered list | ☐ | |

---

## E. Response, Forensics & Reporting

### #16 — Incident Triage Agent

**Scope.** Consolidate peer-agent findings, deduplicate, assign
severity, propose a response playbook.

**Test input.** 3–5 mock findings in the shared 8-key schema plus the
original scenario text. Reference: `scenarios/16-input.md`.

| # | Pass check | Pass/Fail | Notes |
| --- | --- | --- | --- |
| 1 | Single consolidated finding produced | ☐ | |
| 2 | `severity` is monotone with the worst contributing finding | ☐ | |
| 3 | Evidence deduplicated, but every contributing source is referenced | ☐ | |
| 4 | Playbook lists ordered steps; any "active" step is gated on HITL approval | ☐ | |

---

### #17 — Log Correlation & Timeline Agent

**Scope.** Build a chronological narrative from heterogeneous log
snippets and highlight causal chains.

**Test input.** ~20 mixed events (firewall `accept`, Windows logon,
Sysmon process-create, EDR file-write) with interleaved timestamps.
Reference: `scenarios/17-input.txt`.

| # | Pass check | Pass/Fail | Notes |
| --- | --- | --- | --- |
| 1 | Events listed in chronological order | ☐ | |
| 2 | Explicit causal links (`A → B because …`) where supported | ☐ | |
| 3 | Gaps and ambiguity flagged, not invented | ☐ | |
| 4 | Timeline is reproducible across runs (no random re-ordering) | ☐ | |

---

### #18 — Digital Forensics Triage Agent

**Scope.** Given disk and memory artefact summaries, identify key
indicators and suggest next acquisition steps.

**Test input.** A text summary listing recently-modified files, a
browser-history excerpt, a scheduled-task entry, and a top-10 process
list from a memory snapshot. Reference: `scenarios/18-input.md`.

| # | Pass check | Pass/Fail | Notes |
| --- | --- | --- | --- |
| 1 | Suspicious scheduled task surfaced with name + trigger | ☐ | |
| 2 | Rogue process called out with reason (path, parent, signing) | ☐ | |
| 3 | "Next acquisition" list is concrete (artifact name, why) | ☐ | |
| 4 | Output ordered by analyst utility, not by input position | ☐ | |

---

## F. Reserve / Advanced

### #19 — Threat-Hunting Hypothesis Agent

**Scope.** From a stated hypothesis, design hunting queries (Sigma-style,
KQL-style) and a confirmation checklist — without executing them.

**Test input.** A one-sentence hypothesis, e.g. *"An attacker is using
DNS-over-HTTPS for C2 from a finance workstation."* Reference:
`scenarios/19-input.md`.

| # | Pass check | Pass/Fail | Notes |
| --- | --- | --- | --- |
| 1 | At least one Sigma-style rule produced (valid YAML structure) | ☐ | |
| 2 | At least one KQL-style query produced | ☐ | |
| 3 | Numbered confirmation checklist with explicit stop conditions | ☐ | |
| 4 | Non-execution disclaimer present (queries are proposals, not runs) | ☐ | |

---

### #20 — Executive Briefing Agent

**Scope.** Rewrite a consolidated finding for a non-technical executive
audience in a fixed structure.

**Test input.** A consolidated finding object (e.g. from agent #16
output). Reference: `scenarios/20-input.json`.

| # | Pass check | Pass/Fail | Notes |
| --- | --- | --- | --- |
| 1 | Four labelled sections present: *What happened / Impact / What we did / What we ask* | ☐ | |
| 2 | No jargon, no IoC strings, no raw log lines | ☐ | |
| 3 | Length ≤ 250 words | ☐ | |
| 4 | Severity and the requested decision are stated unambiguously | ☐ | |

---

## Grading rubric summary

For each Pass check:

- **Pass** in *both* environments = 1 point.
- **Pass in one environment only** = 0.5 point (counts as a portability
  fault — flag in notes).
- **Fail** = 0 points.

Plus the 6 universal checks (U1–U6), scored the same way.

| Component | Max points |
| --- | --- |
| Universal checks (U1–U6) | 6 |
| Agent-specific Pass checks (4–5 per agent) | 4–5 |
| Cross-environment consistency block | 2 |
| **Worksheet total** | **12–13** |

The worksheet score feeds the Milestone 2 grade (40% of project total —
see proposal § 6).

---

**Student name:** ______________________  **Agent #:** ____

**Grader name:** ______________________  **Date:** ____________

**Signed transcripts attached:** ☐ Copilot Chat   ☐ Claude Code
