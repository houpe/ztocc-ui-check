---
name: ztocc-ui-check
description: Inspect an existing system through real user tasks and/or formal UI acceptance. Use for human-centered walkthroughs, UI audits, defect triage, reacceptance, or evidence-backed HTML reports. Do not use to implement UI, conduct general code review, or create visual designs.
---

# Ztocc UI Check

One skill provides two distinct lenses: **persona playtest** asks whether a person can complete meaningful work and how the journey feels; **formal acceptance** checks a declared UI scope against an agreed baseline and explicit criteria. Choose by the user's request. Run both in sequence only when asked for both, or when a complete human-centered acceptance is clearly requested. Keep their findings and conclusions separate.

## Mode Routing

| Mode | Choose when | Result |
| --- | --- | --- |
| `playtest` | The user asks to experience the system as a person, complete a task, or find hesitation/friction. | First-person journey, `PT-###` friction records, task outcome and metrics. No formal verdict. |
| `formal` | The user asks for complete UI acceptance or a final pass/fail decision. | Full matrix coverage, `UI-###` defects and verdict only if all gates and authority requirements are satisfied. |
| `combined` | The user explicitly wants both real-task experience and formal acceptance. | Run playtest first, then independently apply formal acceptance. Link relevant PT evidence to UI issues without treating PT severity as acceptance severity. |
| `reaccept` | The user asks to verify fixes from an earlier acceptance round. | Recheck unresolved UI defects and required regression sample; preserve IDs. |
| `triage` | The user asks to interpret supplied screenshots, recordings, or reported UI problems. | Evidence-grounded preliminary issue list; no acceptance verdict. |
| `report-only` | The user asks to turn existing findings into a report without new testing. | Consolidated report that preserves source facts and marks gaps. |

If intent is ambiguous, choose the least invasive mode. A request for a final verdict invokes `formal`; a request for first-person experience invokes `playtest`. Do not silently expand one mode into the other.

## Playtest Mode

Before interacting, establish one persona, one concrete task and finish condition, a reachable entry point and authorized session method, plus environment/data/write boundaries. Prefer project-local `.playtest/personas/` and `.playtest/project.md` when present. Do not invent domain rules, credentials, emotions, or task outcomes. If required inputs or safe authorization are missing, ask for the minimum blocker and do not operate.

Read [playtest-playbook.md](references/playtest-playbook.md) before running. Use a real browser when available; when the user names ego lite or ego-browser, follow that skill's API. Otherwise, label the result `static evidence only` and mark interaction conclusions speculative. Capture screenshots/recordings for visual friction and preserve reproducible paths. Do not use OCR alone as visual evidence.

The playtest report must include a chronological first-person account grounded in observed actions; `PT-###` friction records with location, expected/actual, impact, preliminary severity, evidence and confidence; task outcome; observed steps/clicks/hesitations/misoperations; uncovered scope; and the persona's three highest-priority changes. Keep emotions evidence-based. Never issue P0-P3 classifications or a formal pass/fail verdict in this mode. Read [playtest-reporting.md](references/playtest-reporting.md) for the full record contract.

## Formal Acceptance Modes

For `formal`, read the full [acceptance matrix](references/acceptance-matrix.md) and [reporting contract](references/reporting.md). For `reaccept`, read the relevant severity, pass-criteria and regression sections plus reacceptance reporting. For `triage` or `report-only`, load only the relevant issue/evidence and mode-specific reporting sections.

Formal acceptance is blocked until all preconditions in the matrix are satisfied, the UI can be rendered and visually inspected, and a named acceptor owns the final decision. List missing inputs and the exact decision needed; do not issue a pass, fail, or partial verdict while blocked. Only the named acceptor has final severity and pass/fail authority. The agent supplies evidence and recommendations, not self-approval.

Evaluate only the declared scope and already-rendered UI or authorized test environment. Do not implement or redesign. Do not mutate production data, perform destructive actions, or cause external side effects without explicit authorization and recoverable test data. Keep secrets out of reports and evidence.

### Login and access handoff (all browser modes)

When the current page is a login screen, the session has expired, or the next step requires the user's authentication, stop browser actions immediately and tell the user plainly that manual login is required. Hand control to the user in the same browser session; do not request or accept a password in chat, guess credentials, use another profile/session, or continue with a different route. Say: **“当前页面需要登录。请在浏览器中手动登录；完成后回复‘继续’，我会从当前页面接着检查。请不要把密码发给我。”** Resume only after the user says “继续”, using the same session. Until then, report access as blocked and do not imply that any UI workflow was tested. In `formal`/`reaccept`, list login as a blocker; in `playtest`, mark the task not completed/blocked and count no post-login steps as observed. This handoff rule also applies to CAPTCHA, 2FA, and browser permission prompts; ask the user to complete the prompt in-browser, then wait for an explicit continuation.

## Combined Mode

1. Complete the playtest first, with its own persona/task prerequisites, safety checks, evidence, narrative and `PT-###` records.
2. Independently check the formal preconditions. A playtest does not satisfy a formal precondition unless it directly supplies the required evidence.
3. Run formal checks only when all formal gates are met. Map relevant PT observations into the matrix, gather any missing acceptance evidence, and create separate `UI-###` records where a formal criterion is violated.
4. Report two distinct outcomes: the persona's task result and the formal acceptance conclusion, or the precise reasons formal acceptance is blocked. PT high/medium/low is preliminary experience severity, not P0/P1/P2/P3. A positive or negative playtest alone never determines acceptance.

## Reports

When a file is requested, produce a single self-contained HTML report unless another format is explicitly requested. Include only the modes actually run, link evidence with paths relative to the report, and identify round, date, scope, environment and uncovered areas. Do not imply that untested pages, states, roles or viewports passed. Follow the relevant reporting contract; if a report path is not supplied, ask for it rather than choosing a project location silently.

For formal findings, `ui-ux-pro-max` and `impeccable` may inform separate diagnosis or remediation suggestions. They are not acceptance authorities and cannot replace the agreed design baseline, observed evidence, matrix, or named acceptor.
