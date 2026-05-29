# Scenario 01 — Suspected C2 Beaconing from a Finance Workstation

> **How to use this file.** After the operator has pasted the boot
> and catalogue skills into a fresh chat session, paste the
> **Operator narrative** below as the first scenario message. The
> Orchestrator will produce a PLAN; when it dispatches each agent,
> paste that agent's `agents/NN-…agent.md` file followed by the
> matching input block from **Reference inputs** below.

---

## Operator narrative (paste this into chat)

A finance workstation (`fin-ws-07`, internal IP `10.42.3.18`) has been
observed making periodic outbound connections to an external host over
the past 24 hours. The connections occur at strikingly regular
intervals, payloads are small, and the destination does not match any
business application on our allow-list. The same workstation also issued
a series of unusual DNS queries to a parent domain we do not recognise.

We need to determine whether `fin-ws-07` is beaconing to a C2, what the
likely technique is, and what immediate (proposed) containment actions
to recommend.

---

## Reference inputs (paste when the Orchestrator dispatches each agent)

### Input for Agent #1 — Network Traffic Analyzer

```text
# Zeek conn.log excerpt (TSV) — fin-ws-07 last 2 h
# ts                       uid        id.orig_h     id.orig_p  id.resp_h        id.resp_p  proto  service  duration  orig_bytes  resp_bytes  conn_state
1715600100.123  CxYz01  10.42.3.18  51022  198.51.100.77  443  tcp  -  0.42  187  214  SF
1715600400.118  CxYz02  10.42.3.18  51080  198.51.100.77  443  tcp  -  0.39  189  210  SF
1715600700.119  CxYz03  10.42.3.18  51133  198.51.100.77  443  tcp  -  0.41  186  217  SF
1715601000.125  CxYz04  10.42.3.18  51201  198.51.100.77  443  tcp  -  0.40  188  213  SF
1715601300.117  CxYz05  10.42.3.18  51270  198.51.100.77  443  tcp  -  0.42  187  215  SF
1715601600.124  CxYz06  10.42.3.18  51344  198.51.100.77  443  tcp  -  0.40  190  211  SF
# … (truncate / pad to 30–80 rows of mixed benign + beaconing traffic)
```

### Input for Agent #3 — DNS Sentinel

```text
fin-ws-07.local A update-svc-77a3b9.example
fin-ws-07.local A telemetry-pkg-cd91.example
fin-ws-07.local A health-check-2f8e1.example
fin-ws-07.local A asset-cdn-77b3.example
fin-ws-07.local A office365.com
fin-ws-07.local A windowsupdate.com
```

### Input for Agent #4 — TLS / Certificate Inspector  *(optional follow-up)*

```text
# openssl s_client -showcerts capture against 198.51.100.77:443
subject= CN=update-svc-77a3b9.example
issuer=  CN=update-svc-77a3b9.example
notBefore= 2026-04-30T00:00:00Z
notAfter=  2026-05-30T00:00:00Z
# self-signed; CN matches one of the DNS queries above
```

---

## Expected behaviour (for graders — not pasted)

- **Likely PLAN:** dispatch #1 (Traffic Analyzer) → #3 (DNS Sentinel) →
  optionally #4 (TLS Inspector) → consolidate.
- **Expected finding:** `severity: high`, `confidence ≥ 0.7`,
  `attck` includes `T1071.001` and/or `T1568.002`.
- **Expected recommendation:** propose isolating `fin-ws-07` from
  egress to `198.51.100.77` *and* blocking the DGA parent —
  `recommendation_status: proposed`.
- **Failure modes to grade against:** invented hosts; severity below
  `high` despite the regular interval + self-signed cert combo;
  recommendation marked `approved` without a second Plan-and-Approve.
