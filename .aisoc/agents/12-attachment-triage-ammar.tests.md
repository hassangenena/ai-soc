# Agent 12 — Attachment Triage — Test Cases

**Student:** Ammar  
**Agent:** 12-attachment-triage  
**Required pass rate:** 7/10 (70%)

---

## Test Case 01 — Clean PDF

**Input:**
```json
[
  {
    "filename": "report_annual.pdf",
    "hash": "a3f1c2d4e5b6a7c8d9e0f1a2b3c4d5e6f7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2",
    "mime_type": "application/pdf",
    "has_macro": false
  }
]
```

**Expected severity:** `CLEAN`  
**Expected score:** `{"report_annual.pdf": {"score": 0, "label": "CLEAN"}}`  
**Reasoning:** No risk signals triggered.

---

## Test Case 02 — Macro-Enabled Word Document

**Input:**
```json
[
  {
    "filename": "invoice_Q2.docm",
    "hash": "e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855",
    "mime_type": "application/vnd.ms-word.document.macroEnabled.12",
    "has_macro": true
  }
]
```

**Expected severity:** `HIGH`  
**Expected score:** `{"invoice_Q2.docm": {"score": 9, "label": "HIGH"}}`  
**Reasoning:** Macro-enabled Office format (+2), high-risk macro extension (+3), MIME confirms macro type — confidence HIGH.

---

## Test Case 03 — Executable Attachment

**Input:**
```json
[
  {
    "filename": "setup.exe",
    "hash": "b94f6f125c79e3a5ffaa826f584c10d52ada669e6762051b826b55776d05a8ac",
    "mime_type": "application/x-msdownload",
    "has_macro": false
  }
]
```

**Expected severity:** `HIGH`  
**Expected score:** `{"setup.exe": {"score": 9, "label": "HIGH"}}`  
**Reasoning:** High-risk extension `.exe` (+3), MIME type `application/x-msdownload` is consistent with executable — no mismatch, but high-risk extension and MIME both confirm threat.

---

## Test Case 04 — Double Extension Spoofing

**Input:**
```json
[
  {
    "filename": "invoice.pdf.exe",
    "hash": null,
    "mime_type": "application/x-msdownload",
    "has_macro": false
  }
]
```

**Expected severity:** `HIGH`  
**Expected score:** `{"invoice.pdf.exe": {"score": 8, "label": "MEDIUM"}}`  
**Reasoning:** Double extension (+2), high-risk extension `.exe` (+3), MIME mismatch with apparent `.pdf` (+3). Hash is null so confidence is MEDIUM.

> **Note:** Score of 8 maps to MEDIUM per scale — agent should reflect this correctly.

---

## Test Case 05 — Archive Container

**Input:**
```json
[
  {
    "filename": "documents.zip",
    "hash": "c0535e4be2b79ffd93291305436bf889314e4a3faec05ecffcbb7df31ad9e51a",
    "mime_type": "application/zip",
    "has_macro": false
  }
]
```

**Expected severity:** `LOW`  
**Expected score:** `{"documents.zip": {"score": 1, "label": "CLEAN"}}`  
**Reasoning:** Archive container (+1), no other signals. Score 1 = CLEAN label.

---

## Test Case 06 — MIME Mismatch

**Input:**
```json
[
  {
    "filename": "report.pdf",
    "hash": "d7a8fbb307d7809469ca9abcb0082e4f8d5651e46d3cdb762d02d0bf37c9e592",
    "mime_type": "application/x-msdownload",
    "has_macro": false
  }
]
```

**Expected severity:** `MEDIUM`  
**Expected score:** `{"report.pdf": {"score": 6, "label": "MEDIUM"}}`  
**Reasoning:** Extension/MIME mismatch — `.pdf` extension but executable MIME type (+3), high-risk MIME (+3). Score 6 = MEDIUM.

---

## Test Case 07 — Known-Bad Hash (CRITICAL)

**Input:**
```json
[
  {
    "filename": "payslip.docx",
    "hash": "KNOWN_BAD_44d88612fea8a8f36de82e1278abb02f",
    "mime_type": "application/vnd.openxmlformats-officedocument.wordprocessingml.document",
    "has_macro": false
  }
]
```

**Scenario context:** Hash `KNOWN_BAD_44d88612fea8a8f36de82e1278abb02f` is flagged as a known malware indicator.

**Expected severity:** `CRITICAL`  
**Expected score:** `{"payslip.docx": {"score": 11, "label": "CRITICAL"}}`  
**Reasoning:** Known-bad hash match immediately escalates to CRITICAL (score 11), all other checks skipped.

---

## Test Case 08 — Multiple Attachments Mixed

**Input:**
```json
[
  {
    "filename": "summary.pdf",
    "hash": "a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1b2",
    "mime_type": "application/pdf",
    "has_macro": false
  },
  {
    "filename": "macro_tool.xlsm",
    "hash": "f1e2d3c4b5a6978869504132231045667788990aabbccddeeff00112233445566",
    "mime_type": "application/vnd.ms-excel.sheet.macroEnabled.12",
    "has_macro": true
  }
]
```

**Expected severity:** `HIGH`  
**Expected scores:**
```json
{
  "summary.pdf": {"score": 0, "label": "CLEAN"},
  "macro_tool.xlsm": {"score": 9, "label": "HIGH"}
}
```
**Reasoning:** Top-level severity is the highest across all attachments — HIGH from `macro_tool.xlsm`.

---

## Test Case 09 — Macro Excel Sheet

**Input:**
```json
[
  {
    "filename": "budget_2025.xlsm",
    "hash": "9f86d081884c7d659a2feaa0c55ad015a3bf4f1b2b0b822cd15d6c15b0f00a08",
    "mime_type": "application/vnd.ms-excel.sheet.macroEnabled.12",
    "has_macro": true
  }
]
```

**Expected severity:** `HIGH`  
**Expected score:** `{"budget_2025.xlsm": {"score": 9, "label": "HIGH"}}`  
**Reasoning:** Macro-enabled Office format (+2), high-risk extension `.xlsm` (+3), MIME confirms macro-enabled Excel (+3 MIME match — no mismatch penalty, but macro signals dominate). Score 9 = HIGH.

---

## Test Case 10 — Empty Input

**Input:**
```json
[]
```

**Expected severity:** `CLEAN`  
**Expected summary:** `"No attachments submitted for analysis."`  
**Reasoning:** Empty array — agent must handle gracefully per constraints.

---

## Summary Table

| # | Scenario | Expected Severity |
|---|---|---|
| 01 | Clean PDF | CLEAN |
| 02 | Macro-enabled `.docm` | HIGH |
| 03 | Executable `.exe` | HIGH |
| 04 | Double extension `.pdf.exe` | HIGH |
| 05 | Archive `.zip` | CLEAN |
| 06 | MIME mismatch | MEDIUM |
| 07 | Known-bad hash | CRITICAL |
| 08 | Multiple attachments mixed | HIGH |
| 09 | Macro Excel `.xlsm` | HIGH |
| 10 | Empty input | CLEAN |