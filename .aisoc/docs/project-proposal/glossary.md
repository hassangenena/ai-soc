# AISOC Farm — Glossary

**Companion to:** [`proposal.md`](./proposal.md),
[`proposal-3wk.md`](./proposal-3wk.md),
[`test-worksheet.md`](./test-worksheet.md),
[`test-worksheet-3wk.md`](./test-worksheet-3wk.md)

This file defines every abbreviation, acronym, and project-specific
term used in the proposals and test worksheets. Terms are grouped by
topic; within each group entries are alphabetical.

---

## A. Project-specific terms

| Term | Expansion / definition |
| --- | --- |
| **AISOC** | AI Security Operations Center — the in-chat multi-agent farm defined by this project. |
| **Agent (in this project)** | A single prompt-defined specialist, authored using RICTOC, that performs one well-scoped security function. |
| **Finding** | The shared 8-key JSON object every agent returns (`agent`, `summary`, `severity`, `confidence`, `evidence`, `attck`, `recommendation`, `rationale`). |
| **Finding schema** | The JSON Schema at `.aisoc/schema/finding.json` that all findings must validate against. |
| **Farm** | The full set of agents plus the Orchestrator, operating together in one chat session. |
| **Operator** | The human SOC user (student, instructor, or grader) driving the chat session. |
| **Orchestrator** | The control agent that runs PLAN → APPROVE → EXECUTE and consolidates findings. |
| **Plan-and-Approve** | The orchestration protocol: Orchestrator proposes a plan, waits for operator approval, only then dispatches agents. |
| **RICTOC** | The prompt-design template used for every agent: **R**ole, **I**nput, **C**ontext, **T**ask, **O**utput, **C**onstraints. |
| **Self-check** | The silent step where an agent re-reads its own output against its Constraints section before returning. |
| **Stub** | The pre-filled but incomplete agent file the instructor ships; the student replaces it with a working v1 in Week 1 / Milestone 1. |
| **Worked example** | The single complete reference agent (`03-dns-sentinel.agent.md`) used as the canonical RICTOC implementation. |

---

## B. Security & SOC abbreviations

| Term | Expansion / definition |
| --- | --- |
| **ACL** | Access Control List — e.g., Cisco firewall ruleset format (Agent #5). |
| **ATT&CK** | MITRE's *Adversarial Tactics, Techniques, and Common Knowledge* matrix — the canonical catalogue of adversary behaviours. Technique IDs (e.g. `T1071`) live in the `attck` field of every finding. |
| **BEC** | Business Email Compromise — phishing variant impersonating executives or suppliers, typically requesting wire transfers or banking changes (Agent #10). |
| **C2** | Command-and-Control — the infrastructure an attacker uses to instruct compromised endpoints. |
| **CVE** | Common Vulnerabilities and Exposures — public identifiers for known vulnerabilities (Agent #15). |
| **CVSS** | Common Vulnerability Scoring System — 0–10 severity score attached to a CVE. |
| **D3FEND** | MITRE's defensive counterpart to ATT&CK — catalogues defensive techniques. |
| **DGA** | Domain Generation Algorithm — malware-generated pseudo-random domain names, often used for C2 resilience (Agent #3). |
| **DKIM** | DomainKeys Identified Mail — email authentication signature; `dkim=fail` is a phishing signal (Agent #10). |
| **DMARC** | Domain-based Message Authentication, Reporting and Conformance — email-auth policy enforcement layer on top of SPF + DKIM. |
| **DNS** | Domain Name System. |
| **DoH** | DNS-over-HTTPS — encrypted DNS, occasionally abused for covert C2 (Agent #19 example hypothesis). |
| **EDR** | Endpoint Detection and Response — endpoint security platform producing telemetry similar to Sysmon (Agent #7). |
| **EPSS** | Exploit Prediction Scoring System — probability that a CVE will be exploited in the wild within 30 days (Agent #15). |
| **FP** | False Positive — an alert that fires on benign activity. |
| **HITL** | Human-In-The-Loop — control pattern requiring operator approval before an "active" action runs. |
| **HR** | Human Resources — used loosely for HR-style signals (resignation notes, role changes) feeding Agent #9. |
| **IoC** / **IoCs** | Indicator(s) of Compromise — observable artefacts (IPs, domains, hashes, etc.) tied to an attack. |
| **IDS** | Intrusion Detection System — e.g., Suricata, Snort (Agent #2). |
| **JA3** / **JA3S** | TLS client / server fingerprint hashes derived from handshake parameters (Agent #4). |
| **KQL** | Kusto Query Language — Microsoft's query language used in Sentinel, Defender, Azure Data Explorer (Agent #19). |
| **LE** | Let's Encrypt — free public CA; "valid LE cert" is the benign baseline in Agent #4 tests. |
| **LOLBin** | Living-Off-the-Land Binary — a legitimate signed OS binary abused for attacker purposes (e.g., `powershell.exe -enc`). |
| **MFA** | Multi-Factor Authentication. `mfa_status: bypassed` is a flagged anomaly in Agent #6. |
| **OSINT** | Open-Source Intelligence — publicly available intel used to enrich IoCs (Agent #13). |
| **SAN** / **CN** | Subject Alternative Name / Common Name — certificate fields (Agent #4). A SAN/CN mismatch is a flagged anomaly. |
| **Sid** | Signature ID — the numeric identifier of a Suricata / Snort rule (Agent #2 must preserve it in evidence). |
| **Sigma** | Generic open-source detection-rule format (YAML); Agent #19 emits Sigma-style rules. |
| **SOC** | Security Operations Center. |
| **SPF** | Sender Policy Framework — email-sender IP authorization record. `spf=fail` is a phishing signal. |
| **Sysmon** | System Monitor — Microsoft Sysinternals tool producing detailed endpoint telemetry (Agent #7). Event IDs in scope: 1 (process create), 3 (network connect), 11 (file create). |
| **T1071, T1547, T1059.001, T1134, T1548, T1078, T1566, T1568.002, T1571** | Specific MITRE ATT&CK technique IDs referenced in the scenarios and worksheet pass conditions. Verify each at <https://attack.mitre.org/>. |
| **TLS** | Transport Layer Security — the encryption protocol above TCP underlying HTTPS. |
| **TP** | True Positive — an alert that correctly identifies adversarial activity. |
| **Typosquat** | Look-alike domain (e.g., `paypa1.com` for `paypal.com`) used in phishing. |
| **UAC** | User Account Control — Windows privilege-elevation prompt; "UAC bypass" techniques are an Agent #8 target. |
| **UEBA** | User and Entity Behaviour Analytics — baseline-and-deviation analysis on auth and access logs (Agent #6). |
| **USB** | Universal Serial Bus — relevant in Agent #9 as a data-exfiltration vector (large USB-write events). |

---

## C. Tools, formats & file types

| Term | Expansion / definition |
| --- | --- |
| **CSV** | Comma-Separated Values — text tabular format used for auth-log inputs (Agent #6). |
| **`.docm`** | Word document with macros enabled — the macro-bearing attachment type tested in Agent #12. |
| **`.eml`** | Plain-text email format (RFC 5322) — input shape for Agent #10. |
| **`eve.json`** | Suricata's JSON event-log format (Agent #2). |
| **iptables / nftables** | Linux firewall rule formats (Agent #5). |
| **JSON** | JavaScript Object Notation — primary structured-data format for findings and many inputs. |
| **JSON Schema** | The metalanguage used by `.aisoc/schema/finding.json` (draft 2020-12). |
| **Nessus / OpenVAS** | Vulnerability-scanner products whose report formats feed Agent #14. |
| **TSV** | Tab-Separated Values — text tabular format used for Zeek `conn.log` (Agent #1). |
| **YAML** | YAML Ain't Markup Language — used for SKILL.md frontmatter and Sigma rules (Agent #19). |
| **Zeek `conn.log`** | Zeek (formerly Bro) flow-record log; input shape for Agent #1. |

---

## D. Tooling & runtime environment

| Term | Expansion / definition |
| --- | --- |
| **API** | Application Programming Interface — used in the Anthropic API context for Claude Code authentication. |
| **CLI** | Command-Line Interface — Claude Code is shipped as a CLI. |
| **Claude Code** | Anthropic's IDE-integrated coding assistant; one of the two mandatory target environments. |
| **GitHub Copilot Chat** | GitHub's IDE-integrated chat assistant (VS Code extension); the second mandatory target environment. |
| **IDE** | Integrated Development Environment — here, VS Code (for Copilot Chat) or the `claude` CLI (for Claude Code). |
| **LLM** | Large Language Model — the underlying model executing each agent prompt. |
| **MCP** | Model Context Protocol — Anthropic's tool/resource interop protocol. Explicitly forbidden inside the portable agent prompt (allowed only in optional wrappers). |
| **PR** | Pull Request — GitHub mechanism for submitting work to the classroom repo. |
| **SKILL.md** | The Claude Code skill convention: a markdown file with YAML frontmatter inside a skill subfolder (`boot/SKILL.md`, `catalogue/SKILL.md`). |
| **VS Code** | Microsoft Visual Studio Code — the editor hosting Copilot Chat. |

---

## E. External standards & references

| Term | Expansion / definition |
| --- | --- |
| **CICIDS2017** | Public network-traffic dataset published by the University of New Brunswick. |
| **MITRE** | Not-for-profit operator of federally funded R&D centres; publishes ATT&CK and D3FEND. |
| **NIST** | National Institute of Standards and Technology — publisher of SP 800-61 Rev. 2 (incident handling). |
| **OWASP** | Open Worldwide Application Security Project — publisher of the *Top 10 for LLM Applications* used as required reading. |
| **RFC 5322** | IETF standard defining the Internet Message Format (email). |
| **SP 800-61 Rev. 2** | NIST Special Publication: *Computer Security Incident Handling Guide*. |

---

## F. Severity vocabulary

Used in the `severity` field of every finding (worksheet check U2):

| Value | Meaning |
| --- | --- |
| `info` | No adversarial signal detected; reported for completeness. |
| `low` | Weak indicator(s); likely benign but worth noting. |
| `medium` | Material indicator(s); investigation warranted. |
| `high` | Strong, multi-signal indicator(s); operator action expected. |
| `critical` | Clear active-incident signal; immediate Plan-and-Approve cycle for containment. |

---

## G. Operator command vocabulary

Defined in `.aisoc/skills/boot/SKILL.md` Block 3 and used in the
Orchestrator chat loop:

| Command | Short | Effect |
| --- | --- | --- |
| `approve` | `a` | Approve the current plan; proceed to EXECUTE. |
| `revise <text>` | `r <text>` | Re-PLAN incorporating the feedback. |
| `abort` | `x` | Discard all in-flight state; wait for a new scenario. |
| `dispatch <N>` | — | Force agent `#N` as the next dispatch. |
| `status` | — | Show the current plan and which agents have returned. |
| `report` | — | Produce the consolidated REPORT now; mark pending agents `skipped`. |
| `schema` | — | Print the shared 8-key finding schema. |
| `catalogue` | — | Print the registered agent catalogue. |
