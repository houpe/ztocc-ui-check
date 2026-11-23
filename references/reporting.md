# UI Acceptance Reporting

This file is the sole owner of issue records, evidence naming, report paths, and HTML/report contracts.

## Evidence And Report Naming

- Give every issue a stable ID in the form `UI-###`. Never renumber or reuse an ID in later rounds.
- Name evidence by issue, round, and state, for example `UI-023-round-02-before.png`; use `-after` for repaired state and `-evidence` for a non-image artifact.
- Store evidence at a stable path relative to its report and link it with a relative URL. Do not move or rename previously published evidence; add a new evidence file and link when state changes.
- Name each report with a stable scope slug, mode, and round, for example `billing-ui-reaccept-round-02.html`. Keep the same scope slug across rounds.
- A report path is an explicit requester input. If a file is requested but no path is supplied, ask for the path; never infer a fixed project path. If no file is requested, deliver the report content directly in the requested format.
- Reports and evidence must not contain credentials, tokens, cookies, private keys, environment secrets, or unredacted sensitive values. Redact reproduction access details before preserving text or images.

## Issue Records

The fields required for triage define the minimum defect record.

Do not merge separate defects into one row.

| Field | Triage | Formal and reaccept |
| --- | --- | --- |
| ID | Required; preserve any supplied ID. | Required; preserve the prior-round ID. |
| Page/location | Required. Concrete page, component, dialog, row, or state. | Required. |
| Reproduction steps | Required to the extent supplied; identify missing steps. | Required, including data prerequisite and exact operation path. |
| Expected result | Required. State the violated baseline or requester expectation. | Required. Cite the design version, rule, or agreed behavior. |
| Actual result | Required. Objective facts only. | Required. |
| Evidence | Required, or an explicit reason it is unavailable. | Required and reproducible. Use an image or recording for UI defects; use a command, response, or artifact for data, export, or access defects. |
| Dimension | Optional. | Required for a formal finding; use one dimension from the acceptance matrix. |
| Severity | Preliminary only; explicitly state that it is not the acceptor's final classification. | Final severity requires the named acceptor. |
| Owner/status | Optional. | Required. Status is `待修`, `已修待验`, `已闭环`, or `挂起`; suspension requires a reason and follow-up owner. |
| Due date | Optional. | Required for every open P2. |

## Mode-Specific Reports

### Formal Acceptance Report

Generate a single self-contained HTML file unless the requester explicitly asks for another format. It must cover every included page and every applicable matrix item, including unchanged conclusions.

Minimum sections:

1. Header: scope, round, date, acceptor, baseline version, environment identity, build/version, browser/device matrix, and conclusion.
2. Status summary: issue counts by severity, remaining P0/P1, P2 ownership and due dates, and pass/fail conclusion.
3. Scope list: every included page and every excluded page with the exclusion reason.
4. Coverage matrix: page x dimension x applicable state, each with `通过`, `不通过`, or `不适用 + 原因`, plus evidence links where applicable.
5. Issue list implementing the full issue-record contract.
6. Remaining P2/P3 items with owner, due or tracking status, and rationale.

The top-level conclusion may be `通过` only when the acceptance-matrix pass criteria are satisfied.

### Reacceptance Report

For every original defect, show same-state before/after evidence: same or explicitly equivalent viewport, data state, role, and operation path. State whether the original defect closed and preserve the original ID. Include the sampled pages, reused components, cases, results, and new defects. Do not restate full-form coverage conclusions unless the source formal report is attached.

### Triage Report

Use the requested format. Include the minimum issue-record fields, preliminary severity, unanswered questions, and missing evidence. Clearly state that preliminary severity is not final classification and emit no acceptance verdict.

### Report-Only Output

Use the requested format and source mode. Consolidate existing records without changing their IDs, facts, classifications, or evidence; mark missing or broken source data as a gap. Do not add an acceptance verdict, reclassify issues, or imply new testing unless the requester explicitly requests a mode upgrade.

## Integrity Rules

- Preserve stable IDs across rounds.
- Keep evidence and links relative to the report so the report directory remains portable.
- Do not hide failed, missing, or not-applicable conclusions behind summary-only text.
- Include enough environment, baseline, build, role, data-state, and viewport identity for an independent reviewer to reproduce the comparison, without including secrets.
