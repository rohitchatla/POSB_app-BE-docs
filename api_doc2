# Official API Documentation: MIS CBS Account Status Retrieval

## Overview
The MIS CBS Transactions page provides users with access to opened and closed account information for a specified business date (or date range). This document describes the integration reference for internal automation that programmatically retrieves "opened" and "closed" account details from the MIS portal page:

Endpoint (Portal Page)
```
https://mis.cept.gov.in/CBS/CBSTrans3
```

## Purpose
To obtain authoritative lists of:
- Newly Opened Accounts (for previous working day or defined date range)
- Closed Accounts (for previous working day or defined date range)

The internal automation consumes the rendered response (HTML and/or exported file) and normalises it into structured data used by downstream processes and the `/run-download` service in this repository.

## Request Characteristics
Because CBSTrans3.aspx is a WebForms page, requests include dynamic hidden fields (e.g., `__VIEWSTATE`, `__EVENTVALIDATION`) plus user form inputs. Automation must:
1. Perform initial GET to capture hidden state fields.
2. Submit POST with required form fields and preserved state values.
3. Optionally trigger an export action (if the portal supports Excel/CSV export) by posting the corresponding event target.

### Core Form Inputs (Conceptual)
| Functional Input | Description |
|------------------|-------------|
| Business Date / From Date | Date for which report is required. |
| To Date (optional) | End of range if multi-day export allowed. |
| Report Type / Transaction Category | Opened Accounts or Closed Accounts (automation runs both sequentially). |
| Circle / Division / Office (optional filters) | Geographic or organisational scoping filters. |

> Exact hidden and form field names are subject to change by the portal; automation should parse them dynamically rather than hard-coding names.

## Response Format
The portal returns an HTML page containing tabular data. An optional export (Excel/CSV) may be initiated. Automation parses either:
- HTML table rows, or
- Binary Excel stream (Content-Type: application/vnd.ms-excel) if export invoked.

### Normalised Data Model (Internal Representation)
```json
[
  {
    "account_number": "1234567890",
    "product": "SB", 
    "customer_name_masked": "RAO, A****", 
    "office_code": "XXXXXX", 
    "opened_date": "2025-08-07", 
    "closed_date": null, 
    "status": "OPENED"
  },
  {
    "account_number": "9876543210",
    "product": "RD", 
    "customer_name_masked": "KUMAR, B****", 
    "office_code": "XXXXXX", 
    "opened_date": "2024-11-12", 
    "closed_date": "2025-08-07", 
    "status": "CLOSED"
  }
]
```
> Personally Identifiable Information (PII) must be masked or minimised in logs and derivative datasets in compliance with internal data handling policies.

## Automation Flow Summary
1. Initiate GET to endpoint to retrieve session + hidden fields.
2. Submit POST for Opened Accounts (previous working day) -> parse / export.
3. Submit POST for Closed Accounts (previous working day) -> parse / export.
4. Store outputs into designated working directory (e.g., `web_results/` or configured download path).
5. Update audit ledger and trigger internal `/run-download` status endpoint to signal completion.

## Error Handling
| Scenario | Mitigation |
|----------|------------|
| Session timeout / redirect to login | Detect login page markers; re-authenticate and retry once. |
| Unexpected HTML layout change | Raise structured alert; capture snapshot for analysis. |
| Empty result set | Treat as valid; record zero-count entry. |
| Partial export / network error | Retry idempotently (max 3 attempts with exponential backoff). |


## Operational Recommendations
- Schedule execution once after official day-close settlement cycle completes.
- Implement checksum or row-count validation against historical averages to detect anomalies.
- Store raw exports (immutable) plus normalised JSON/CSV for downstream analytics in versioned directories (e.g., `Net_Additions_Reports/` by date).

## Change Management
Any portal UI / field changes must trigger:
1. Parser regression test run.
2. Update of field mapping documentation.
3. Version bump in internal extraction module.

## Contact
For authorised access requests or incident escalation:
- POSB Automation Team (CVRROCKET)
- Email: cvrrocket@gmail.com

(Internal Use Only – Do not distribute externally without approval.)
