# Scenario 03 — Credential Theft Leading to Lateral Movement

> **How to use this file.** See `scenarios/01-beaconing.md` for the
> paste workflow. Same pattern applies here.

---

## Operator narrative (paste this into chat)

Over the last six hours we have observed three concurrent signals on
a single user account (`bob@corp.example`):

1. Successful logon from a country the user has never logged in from,
   ten minutes after a successful logon from their normal office IP.
2. A series of failed logons preceding the foreign success, suggesting
   a credential-spray that landed.
3. On the user's workstation (`fin-ws-12`), a Sysmon process tree
   shows `winword.exe → powershell.exe -enc <base64>` minutes after
   the suspicious successful logon, followed by a registry write to
   `HKCU\…\Run`.

We want a unified incident view: what happened, how, in what order,
and what to propose.

---

## Reference inputs

### Input for Agent #6 — Authentication Anomaly

```csv
timestamp,user,src_ip,geo,result,mfa_status
2026-05-12T11:02:14Z,bob,10.42.4.55,DE-Munich,success,passed
2026-05-12T11:04:01Z,bob,203.0.113.91,RO-Bucharest,failure,not_required
2026-05-12T11:04:09Z,bob,203.0.113.91,RO-Bucharest,failure,not_required
2026-05-12T11:04:18Z,bob,203.0.113.91,RO-Bucharest,failure,not_required
2026-05-12T11:04:32Z,bob,203.0.113.91,RO-Bucharest,success,bypassed
2026-05-12T11:08:55Z,alice,10.42.3.18,DE-Munich,success,passed
2026-05-12T11:12:11Z,bob,203.0.113.91,RO-Bucharest,success,bypassed
```

### Input for Agent #7 — Endpoint Telemetry Analyst

```json
[
  {"event_id": 1, "ts": "2026-05-12T11:14:02Z", "host": "fin-ws-12", "user": "bob", "ppid": 4012, "pid": 7204, "image": "C:\\Program Files\\Microsoft Office\\Office16\\WINWORD.EXE", "cmdline": "WINWORD.EXE /n \"C:\\Users\\bob\\Downloads\\Q2-Forecast.docx\""},
  {"event_id": 1, "ts": "2026-05-12T11:14:08Z", "host": "fin-ws-12", "user": "bob", "ppid": 7204, "pid": 7301, "image": "C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe", "cmdline": "powershell.exe -nop -w hidden -enc SQBFAFgAIAA..."},
  {"event_id": 11, "ts": "2026-05-12T11:14:14Z", "host": "fin-ws-12", "user": "bob", "image": "powershell.exe", "target_filename": "C:\\Users\\bob\\AppData\\Roaming\\svchosts.ps1"},
  {"event_id": 1, "ts": "2026-05-12T11:14:21Z", "host": "fin-ws-12", "user": "bob", "ppid": 7301, "pid": 7402, "image": "C:\\Windows\\System32\\reg.exe", "cmdline": "reg add HKCU\\Software\\Microsoft\\Windows\\CurrentVersion\\Run /v Updater /t REG_SZ /d \"powershell.exe -f C:\\Users\\bob\\AppData\\Roaming\\svchosts.ps1\""}
]
```

### Input for Agent #17 — Log Correlation & Timeline

The Orchestrator passes the #6 and #7 findings (plus the originating
auth/Sysmon rows above) back to the operator for #17's input.

---

## Expected behaviour (for graders — not pasted)

- **Likely PLAN:** #6 → #7 → #17 → optionally #8 (Privesc Hunter)
  if Sysmon hints at token manipulation → consolidated REPORT.
- **Expected findings:**
  - #6: `severity: high`, `anomaly_types` includes
    `impossible-travel`, `brute-force`, `mfa-bypass`; `user_risk[bob]`
    ≥ 0.8.
  - #7: `severity: high`, suspicious chain `winword → powershell -enc`
    + persistence write to `Run` key; ATT&CK `T1059.001`, `T1547.001`.
  - #17: chronological timeline with explicit causal link
    `MFA-bypass success at 11:04:32Z → DOC opened at 11:14:02Z →
    encoded PowerShell at 11:14:08Z → persistence at 11:14:21Z`.
- **Consolidated severity:** `critical`. Proposed playbook:
  isolate `fin-ws-12`, force password reset + revoke sessions for
  `bob`, hunt for the same `Run` value across the fleet —
  all `recommendation_status: proposed`.
- **Failure modes to grade against:** silently merging `alice`'s
  benign successful logon into the incident; missing the
  `mfa_status: bypassed` signal; inventing a causal link that the
  evidence does not support.
