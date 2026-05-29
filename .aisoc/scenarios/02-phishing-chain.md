# Scenario 02 — Reported Phishing Email with Embedded URL and Attachment

> **How to use this file.** See `scenarios/01-beaconing.md` for the
> paste workflow. Same pattern applies here.

---

## Operator narrative (paste this into chat)

A finance team member (`alice@corp.example`) forwarded a suspicious
email to the SOC inbox. The email claims to be from a known supplier
and asks her to "update banking details" using a link, with a
`.docm` attachment titled `Invoice-Update-052026.docm`. The display
name matches the legitimate supplier but the `Reply-To` is a freemail
address. We need a verdict, IoC extraction, BEC-tell analysis, and a
consolidated incident severity.

---

## Reference inputs

### Input for Agent #10 — Phishing Email Analyst

```text
From: "Acme Supplies Billing" <billing@acmessupplies-secure.example>
Reply-To: acme-billing-7723@gmail.com
To: alice@corp.example
Date: Tue, 12 May 2026 08:14:22 +0000
Subject: URGENT: Updated banking details for May invoice
Authentication-Results: corp.example;
  spf=fail (sender IP is 198.51.100.205)
  dkim=none
  dmarc=fail
Return-Path: <bounce@acmessupplies-secure.example>

Dear Alice,

Our banking details have changed effective immediately. Please update
the wire instructions before processing this month's invoice. The
attached document contains the new account information. The change
must be confirmed within 24 hours.

Open the secure link below if the attachment fails to load:
https://acme-billing-portal.example/login?ref=invoice-052026

Best regards,
J. Patel
Acme Supplies Billing Team
```

### Input for Agent #11 — Malicious URL & Web

```text
https://acme-billing-portal.example/login?ref=invoice-052026
https://acmessupplies-secure.example/track
https://bit.ly/acme-pay-052026
https://acmesupplies.example/contact
```

### Input for Agent #12 — Attachment Triage

```json
[
  {
    "filename": "Invoice-Update-052026.docm",
    "sha256": "9f1b3d4e5a6c7b8e0f2a4b6c8d0e2f4a6b8c0d2e4f6a8b0c2d4e6f8a0b2c4d6e",
    "mime_type": "application/vnd.ms-word.document.macroEnabled.12",
    "macro_present": true,
    "size_bytes": 184230
  }
]
```

### Input for Agent #16 — Incident Triage  *(after the three above return)*

The Orchestrator passes the three returned finding objects plus this
narrative back to the operator, who then paste them as the input for
Agent #16.

---

## Expected behaviour (for graders — not pasted)

- **Likely PLAN:** #10 → #11 → #12 → #16. (#11 and #12 may run in
  any order after #10.)
- **Expected findings:**
  - #10: `severity: high`, BEC tells include *urgency*, *banking-change*,
    *freemail-Reply-To*, *SPF/DKIM/DMARC failures*.
  - #11: the freemail Reply-To and the look-alike `acmessupplies-secure`
    flagged with `homoglyph`-ish tag; legitimate `acmesupplies.example`
    not flagged.
  - #12: `.docm` with macro + external sender → `severity: high`.
  - #16: consolidated `severity: high`; playbook includes (proposed)
    quarantine of the message in `alice@corp.example`'s mailbox,
    block of the sender domain, and a HR/awareness follow-up.
- **Failure modes to grade against:** flagging the legitimate
  `acmesupplies.example` as malicious; severity below `high`; missing
  the BEC banking-change tell.
