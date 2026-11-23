# Persona Playtest Reporting

## Friction Record Contract

Use stable IDs in the form `PT-###`. Do not merge separate experiences into one row.

| Field | Requirement |
| --- | --- |
| ID | Stable within the project, for example `PT-014`. |
| Persona/task | Named persona and concrete task. |
| Location | Page, dialog, row, field, state, or transition. |
| Type | One of `find`, `understand`, `hesitate`, `mistake`, `feedback`, `unclear consequence`, `environment strain`, `workflow gap`, or `other`. |
| My experience | First-person objective experience, without insults or generic wording. |
| Expected | What the persona expected before acting. |
| Actual | Observed system behavior. |
| Impact | Extra work, delay, risk of wrong data, need for help, or task blockage. |
| Preliminary severity | `high` blocks the task or creates material misoperation risk; `medium` causes obvious effort or confusion; `low` irritates but does not materially affect the task. |
| Evidence | Screenshot/recording path plus exact reproduction path; command/response/export artifact for non-visual conclusions. |
| Confidence | `observed` for performed interaction, `visual inference` for static evidence, or `speculative` when interaction was not performed. |
| Acceptor note | What formal acceptance should verify; never a final P0-P3 classification. |

Preliminary playtest severity and formal UI acceptance severity are different systems. Do not rename `high` to `P0` or infer an acceptance verdict.

## Report Sections

1. Header: project, round, date, environment, build/version if known, persona file or explicit persona, task, browser/tool mode, and evidence base.
2. Safety and scope: permitted writes, disposable data, excluded operations, login method, account strategy (designated existing test account or temporary synthetic account), temporary resource types/IDs created by this run, verified cleanup outcome, and anything not covered. Never include credentials or secret values.
3. First-person narrative in chronological order. Use plain language and concrete moments; do not turn the narrative into a design lecture.
4. Task result: `completed`, `completed with effort`, or `not completed`, with the blocking point.
5. Metrics: steps, clicks/keyboard commands, hesitations, backtracks, corrections, near misses, and uncovered operations.
6. Friction register implementing the full record contract.
7. Environment effects: constraints that actually changed the experience. Omit the section when no constraint was declared or observed.
8. Three changes this persona would prioritize. Explain each from task impact, not aesthetic preference.
9. Acceptance handoff: which `PT-###` records need independent triage and what evidence is already available.

## Integrity Rules

- Never claim an operation that was not performed.
- Distinguish observed behavior, persona-grounded inference, and speculation.
- Do not let implementation rationale erase the user's experience.
- Do not present design-system preference as a user fact.
- Do not emit a formal pass/fail conclusion.
- Preserve evidence links relative to the report and redact secrets and unrelated personal data.

## Handoff To Formal Acceptance

When passing findings to formal acceptance (including the formal mode of `ztocc-ui-check`):

1. Preserve every `PT-###` ID and evidence path.
2. Provide expected result, actual result, reproduction path, and confidence.
3. Identify the likely acceptance dimension, such as interaction, state coverage, copy, data display, permissions, or accessibility.
4. State that preliminary severity is not final classification.
5. Let the named independent acceptor decide whether a friction record becomes a UI defect, a design suggestion, or an accepted tradeoff.
