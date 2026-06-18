# Agent 01 — Network Traffic Analyzer — Test Cases

**Student:** yourname
**Agent:** 01-network-traffic-analyzer
**Required pass rate:** 7/10 (70%)

---

## Test Case 01 — Positive: Regular HTTP Beaconing

**Input:**
```
#fields ts uid id.orig_h id.orig_p id.resp_h id.resp_p proto service duration orig_bytes resp_bytes conn_state history orig_pkts resp_pkts
1716950400.0 C1 10.0.0.5 52345 203.0.113.10 80 tcp http 1.2 120 110 SF Nh 10 12
1716950405.0 C2 10.0.0.5 52345 203.0.113.10 80 tcp http 1.1 118 112 SF Nh 10 12
1716950410.0 C3 10.0.0.5 52345 203.0.113.10 80 tcp http 1.3 119 111 SF Nh 10 12
1716950415.0 C4 10.0.0.5 52345 203.0.113.10 80 tcp http 1.2 121 109 SF Nh 10 12
1716950420.0 C5 10.0.0.5 52345 203.0.113.10 80 tcp http 1.1 118 113 SF Nh 10 12
1716950425.0 C6 10.0.0.5 52345 203.0.113.10 80 tcp http 1.2 120 110 SF Nh 10 12
1716950430.0 C7 10.0.0.5 52345 203.0.113.10 80 tcp http 1.3 119 111 SF Nh 10 12
```

**Expected output:**
- `severity`: `high`
- `attck`: includes `T1071`
- `flagged_hosts`: one entry for `10.0.0.5 → 203.0.113.10:80/tcp` with `reason: beaconing`
- `beacon_interval_s`: `5`
- `confidence`: high (≥ 0.7)

---

## Test Case 02 — Positive: Encrypted TLS Beaconing

**Input:**
```
#fields ts uid id.orig_h id.orig_p id.resp_h id.resp_p proto service duration orig_bytes resp_bytes conn_state history orig_pkts resp_pkts
1716950500.0 D1 10.0.0.6 53421 198.51.100.22 443 tcp ssl 0.9 65 70 SF ShADadf 8 10
1716950504.8 D2 10.0.0.6 53421 198.51.100.22 443 tcp ssl 0.8 66 69 SF ShADadf 8 10
1716950509.6 D3 10.0.0.6 53421 198.51.100.22 443 tcp ssl 0.9 65 70 SF ShADADf 8 10
1716950514.4 D4 10.0.0.6 53421 198.51.100.22 443 tcp ssl 0.8 66 69 SF ShADadf 8 10
1716950519.2 D5 10.0.0.6 53421 198.51.100.22 443 tcp ssl 0.9 65 70 SF ShADadf 8 10
1716950524.0 D6 10.0.0.6 53421 198.51.100.22 443 tcp ssl 0.8 66 69 SF ShADadf 8 10
```

**Expected output:**
- `severity`: `high`
- `attck`: includes `T1573` and `T1071`
- `flagged_hosts`: encrypted regular channel to `198.51.100.22:443/tcp`
- `beacon_interval_s`: `5`
- `confidence`: high (≥ 0.7)

---

## Test Case 03 — Positive: Non-Standard Port Anomaly

**Input:**
```
#fields ts uid id.orig_h id.orig_p id.resp_h id.resp_p proto service duration orig_bytes resp_bytes conn_state history orig_pkts resp_pkts
1716950600.0 E1 10.0.0.7 54000 203.0.113.20 8088 tcp http 2.5 1024 980 SF ShADadf 12 14
1716950620.0 E2 10.0.0.7 54001 203.0.113.20 8088 tcp http 1.8 1100 1050 SF ShADadf 11 13
1716950640.0 E3 10.0.0.7 54002 203.0.113.20 8088 tcp http 2.1 1030 990 SF ShADadf 11 12
1716950660.0 E4 10.0.0.7 54003 203.0.113.20 8088 tcp http 2.2 1040 995 SF ShADadf 11 12
```

**Expected output:**
- `severity`: `medium`
- `attck`: includes `T1571`
- `flagged_hosts`: one entry for `10.0.0.7 → 203.0.113.20:8088/tcp` with `reason: non_standard_port`
- `confidence`: moderate (around 0.6)

---

## Test Case 04 — Positive: Non-App-Layer ICMP Flow

**Input:**
```
#fields ts uid id.orig_h id.orig_p id.resp_h id.resp_p proto service duration orig_bytes resp_bytes conn_state history orig_pkts resp_pkts
1716950700.0 F1 10.0.0.8 0 198.51.100.50 0 icmp - 0.0 64 64 SF - 1 1
1716950705.0 F2 10.0.0.8 0 198.51.100.50 0 icmp - 0.0 64 64 SF - 1 1
1716950710.0 F3 10.0.0.8 0 198.51.100.50 0 icmp - 0.0 64 64 SF - 1 1
1716950715.0 F4 10.0.0.8 0 198.51.100.50 0 icmp - 0.0 64 64 SF - 1 1
1716950720.0 F5 10.0.0.8 0 198.51.100.50 0 icmp - 0.0 64 64 SF - 1 1
1716950725.0 F6 10.0.0.8 0 198.51.100.50 0 icmp - 0.0 64 64 SF - 1 1
```

**Expected output:**
- `severity`: `medium`
- `attck`: includes `T1095`
- `flagged_hosts`: one entry for `10.0.0.8 → 198.51.100.50:0/icmp` with `reason: non_app_layer`
- `confidence`: medium

---

## Test Case 05 — Positive: Volume Anomaly Against Background Traffic

**Input:**
```
#fields ts uid id.orig_h id.orig_p id.resp_h id.resp_p proto service duration orig_bytes resp_bytes conn_state history orig_pkts resp_pkts
1716950800.0 G1 10.0.0.9 52000 203.0.113.30 443 tcp ssl 0.9 64 70 SF Sh 8 10
1716950810.0 G2 10.0.0.9 52001 203.0.113.31 443 tcp ssl 0.8 60 68 SF Sh 8 10
1716950820.0 G3 10.0.0.10 53000 203.0.113.40 8443 tcp http 1.5 1200 1180 SF Sh 15 17
1716950830.0 G4 10.0.0.10 53001 203.0.113.40 8443 tcp http 1.4 1180 1160 SF Sh 14 16
1716950840.0 G5 10.0.0.10 53002 203.0.113.40 8443 tcp http 1.6 1190 1170 SF Sh 15 16
1716950850.0 G6 10.0.0.10 53003 203.0.113.40 8443 tcp http 1.5 1205 1175 SF Sh 15 16
1716950860.0 G7 10.0.0.10 53004 203.0.113.40 8443 tcp http 1.4 1185 1165 SF Sh 15 16
1716950870.0 G8 10.0.0.10 53005 203.0.113.40 8443 tcp http 1.5 1195 1175 SF Sh 15 16
```

**Expected output:**
- `severity`: `medium`
- `attck`: includes `T1571`
- `flagged_hosts`: one entry for `10.0.0.10 → 203.0.113.40:8443/tcp` with `reason: non_standard_port` or `volume_anomaly`
- `confidence`: medium

---

## Test Case 06 — Negative: No Suspicious Flows

**Input:**
```
#fields ts uid id.orig_h id.orig_p id.resp_h id.resp_p proto service duration orig_bytes resp_bytes conn_state history orig_pkts resp_pkts
1716950900.0 H1 10.0.0.11 52200 198.51.100.60 80 tcp http 1.3 4500 4800 SF ShADadf 22 24
1716950915.0 H2 10.0.0.12 52300 198.51.100.61 443 tcp ssl 1.2 5600 5300 SF ShADadf 20 22
1716950930.0 H3 10.0.0.13 52400 203.0.113.50 53 udp dns 0.0 80 90 SF - 2 2
1716950945.0 H4 10.0.0.14 52500 203.0.113.51 22 tcp ssh 1.4 1500 1450 SF ShADadf 12 13
```

**Expected output:**
- `severity`: `info`
- `attck`: `[]`
- `flagged_hosts`: `[]`
- `confidence`: low/near 0.0

---

## Test Case 07 — Low-Confidence Beaconing Pattern

**Input:**
```
#fields ts uid id.orig_h id.orig_p id.resp_h id.resp_p proto service duration orig_bytes resp_bytes conn_state history orig_pkts resp_pkts
1716951000.0 I1 10.0.0.15 52600 203.0.113.60 80 tcp http 1.1 200 195 SF Nh 10 10
1716951005.4 I2 10.0.0.15 52600 203.0.113.60 80 tcp http 1.0 210 190 SF Nh 10 10
1716951011.0 I3 10.0.0.15 52600 203.0.113.60 80 tcp http 1.2 205 192 SF Nh 10 10
1716951016.8 I4 10.0.0.15 52600 203.0.113.60 80 tcp http 1.0 215 185 SF Nh 10 10
1716951022.5 I5 10.0.0.15 52600 203.0.113.60 80 tcp http 1.1 202 198 SF Nh 10 10
1716951028.2 I6 10.0.0.15 52600 203.0.113.60 80 tcp http 1.2 208 193 SF Nh 10 10
```

**Expected output:**
- `severity`: `low`
- `attck`: `[