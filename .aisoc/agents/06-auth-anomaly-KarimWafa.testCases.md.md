# Agent 06 — Authentication Anomaly — Test Cases

**Student:** Karim Wafa
**Agent:** 06-auth-anomaly
**Required pass rate:** 7/10 (70%)

---

## Test Case 01 — Clean Logins (baseline, no anomalies)

**Input:**
```csv
timestamp,user,src_ip,geo,result,mfa_status
2024-03-15T09:00:00Z,alice@corp.com,1.2.3.4,Germany,success,passed
2024-03-15T09:30:00Z,bob@corp.com,5.6.7.8,Germany,success,passed
2024-03-15T10:00:00Z,carol@corp.com,9.10.11.12,Germany,success,passed
```

**Expected severity:** `info`
**Expected `anomaly_types`:** `[]`
**Expected `attck`:** `[]`
**Expected `user_risk`:** `{"alice@corp.com": "clean", "bob@corp.com": "clean", "carol@corp.com": "clean"}`
**Reasoning:** No risk signals triggered — normal hours, MFA passed, no repetition.

---

## Test Case 02 — Brute Force Burst (seeded pattern)

**Input:**
```csv
timestamp,user,src_ip,geo,result,mfa_status
2024-03-15T14:02:00Z,grace@corp.com,77.21.9.4,Germany,failure,none
2024-03-15T14:05:00Z,grace@corp.com,77.21.9.4,Germany,failure,none
2024-03-15T14:08:00Z,grace@corp.com,77.21.9.4,Germany,failure,none
2024-03-15T14:11:00Z,grace@corp.com,77.21.9.4,Germany,failure,none
2024-03-15T14:14:00Z,grace@corp.com,77.21.9.4,Germany,failure,none
2024-03-15T14:16:00Z,grace@corp.com,77.21.9.4,Germany,locked,none
```

**Expected severity:** `high`
**Expected `anomaly_types`:** includes `brute_force`
**Expected `attck`:** includes `T1110` (and/or `T1110.001`)
**Expected `user_risk`:** `{"grace@corp.com": "high"}`
**Reasoning:** 5 failures for the same user inside a 14-minute window, followed by an account lockout — textbook brute-force confirmation.

---

## Test Case 03 — Password Spraying

**Input:**
```csv
timestamp,user,src_ip,geo,result,mfa_status
2024-03-15T03:00:00Z,alice@corp.com,1.2.3.4,Germany,failure,none
2024-03-15T03:05:00Z,bob@corp.com,1.2.3.4,Germany,failure,none
2024-03-15T03:10:00Z,carol@corp.com,1.2.3.4,Germany,failure,none
```

**Expected severity:** `medium`
**Expected `anomaly_types`:** includes `password_spray` (and likely `off_hours`, since 03:00 UTC)
**Expected `attck`:** includes `T1110.003`
**Expected `user_risk`:** all three users at least `medium`
**Reasoning:** Three distinct users each received exactly one failure from the same source IP within 30 minutes — classic spray pattern designed to dodge per-account lockout thresholds.

---

## Test Case 04 — Impossible Travel (seeded pattern)

**Input:**
```csv
timestamp,user,src_ip,geo,result,mfa_status
2024-03-15T11:00:00Z,alice@corp.com,1.2.3.4,Germany,success,passed
2024-03-15T11:10:00Z,alice@corp.com,5.6.7.8,Brazil,success,passed
```

**Expected severity:** `critical`
**Expected `anomaly_types`:** includes `impossible_travel`
**Expected `attck`:** includes `T1078` and/or `T1563`
**Expected `user_risk`:** `{"alice@corp.com": "critical"}`
**Reasoning:** Two successful logins for the same user, 10 minutes apart, from Germany and Brazil — physically impossible travel time, indicating a compromised credential or hijacked session.

---

## Test Case 05 — Off-Hours Admin Logon (seeded pattern)

**Input:**
```csv
timestamp,user,src_ip,geo,result,mfa_status
2024-03-15T03:00:00Z,admin@corp.com,10.0.0.2,Germany,success,passed
```

**Expected severity:** `low`
**Expected `anomaly_types`:** includes `off_hours`
**Expected `attck`:** `[]` (behavioural signal, no specific technique on its own)
**Expected `user_risk`:** `{"admin@corp.com": "low"}`
**Reasoning:** A successful login at 03:00 UTC falls inside the agent's 00:00–05:59 UTC off-hours window. Standalone off-hours activity is low severity, but worth flagging — especially for a privileged account.

---

## Test Case 06 — MFA Bypassed

**Input:**
```csv
timestamp,user,src_ip,geo,result,mfa_status
2024-03-15T10:00:00Z,alice@corp.com,1.2.3.4,Germany,success,bypassed
```

**Expected severity:** `critical`
**Expected `anomaly_types`:** includes `mfa_bypassed`
**Expected `attck`:** includes `T1621`
**Expected `user_risk`:** `{"alice@corp.com": "critical"}`
**Reasoning:** Any successful login with `mfa_status: bypassed` is treated as an automatic critical finding — MFA was circumvented entirely.

---

## Test Case 07 — MFA Absent on Success

**Input:**
```csv
timestamp,user,src_ip,geo,result,mfa_status
2024-03-15T10:00:00Z,alice@corp.com,1.2.3.4,Germany,success,none
```

**Expected severity:** `medium` or `high`
**Expected `anomaly_types`:** includes `mfa_absent`
**Expected `attck`:** includes `T1078`
**Expected `user_risk`:** `{"alice@corp.com": "medium"}` or `"high"`
**Reasoning:** A successful login with no MFA at all is a policy-level risk signal — the account has no second factor protecting it. (Agent may reasonably score this `high` depending on its internal confidence threshold; both `medium` and `high` are acceptable per the agent's own severity table.)

---

## Test Case 08 — Benign Baseline Users Not Flagged (negative test)

**Input:**
```csv
timestamp,user,src_ip,geo,result,mfa_status
2024-03-15T08:00:00Z,bob@corp.com,10.0.0.12,Germany,success,passed
2024-03-15T09:00:00Z,bob@corp.com,10.0.0.12,Germany,success,passed
2024-03-15T12:00:00Z,bob@corp.com,10.0.0.12,Germany,success,passed
2024-03-15T16:00:00Z,bob@corp.com,10.0.0.12,Germany,success,passed
2024-03-15T20:00:00Z,bob@corp.com,10.0.0.12,Germany,success,passed
```

**Expected severity:** `info`
**Expected `anomaly_types`:** `[]`
**Expected `user_risk`:** `{"bob@corp.com": "clean"}`
**Reasoning:** Repeated successful logins for the same user from the same IP, same geo, normal hours, MFA passed — this is exactly how a real employee behaves. A correct agent must NOT raise a false positive here, even though there are multiple events for one user across the day.

---

## Test Case 09 — Mixed Anomalies, Same User (severity escalation)

**Input:**
```csv
timestamp,user,src_ip,geo,result,mfa_status
2024-03-15T02:00:00Z,heidi@corp.com,44.55.66.77,Germany,failure,none
2024-03-15T02:05:00Z,heidi@corp.com,44.55.66.77,Germany,failure,none
2024-03-15T02:10:00Z,heidi@corp.com,44.55.66.77,Germany,failure,none
2024-03-15T02:15:00Z,heidi@corp.com,44.55.66.77,Germany,failure,none
2024-03-15T02:20:00Z,heidi@corp.com,44.55.66.77,Germany,failure,none
2024-03-15T02:25:00Z,heidi@corp.com,44.55.66.77,Germany,success,bypassed
```

**Expected severity:** `critical`
**Expected `anomaly_types`:** includes `brute_force`, `off_hours`, and `mfa_bypassed`
**Expected `attck`:** includes `T1110`, `T1621`
**Expected `user_risk`:** `{"heidi@corp.com": "critical"}`
**Reasoning:** Brute-force burst (5 failures in 25 min) immediately followed by a successful login with MFA bypassed, all during off-hours. Multiple co-occurring anomalies for the same user must escalate severity to the highest level — this is the most realistic "account takeover in progress" pattern in the test set.

---

## Test Case 10 — Empty Input

**Input:**
```csv
timestamp,user,src_ip,geo,result,mfa_status
```

**Expected severity:** `info`
**Expected summary:** Something indicating no authentication events were provided to analyze.
**Expected `user_risk`:** `{}`
**Reasoning:** Header row only, zero data rows — the agent must handle this gracefully without inventing users or events, per the "no invented data" constraint.

---

## Summary Table

| # | Scenario | Expected Severity |
|---|---|---|
| 01 | Clean logins (baseline) | info |
| 02 | Brute force burst | high |
| 03 | Password spraying | medium |
| 04 | Impossible travel | critical |
| 05 | Off-hours admin logon | low |
| 06 | MFA bypassed | critical |
| 07 | MFA absent on success | medium/high |
| 08 | Benign repeated user (negative test) | info |
| 09 | Mixed anomalies, same user | critical |
| 10 | Empty input | info |
