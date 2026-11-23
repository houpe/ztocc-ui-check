# Persona Playtest Playbook

## 1. Prepare

### Persona Card

A usable persona card contains:

| Field | Requirement |
| --- | --- |
| Identity and role | Named role, not a generic "user". |
| Job context | Device, location, workload, time pressure, colleagues, and interruptions. |
| Primary goal | Why this task matters in the person's work. |
| Success criterion | How they know the job is finished and correct. |
| Concerns | What they fear getting wrong, such as irreversible loss, wrong recipient, delay, or blame. |
| Constraints | Optional physical, environmental, language, connectivity, or permission constraints. |
| High-frequency tasks | Concrete tasks available for reuse. |

Do not turn a role title into a personality stereotype. If motivation or emotion is not stated, infer only from observed hesitation, repeated correction, task failure, or the persona's stated success criterion, and label the inference.

### Project Configuration

Use `.playtest/project.md` when present. Keep it non-secret and version-friendly:

```markdown
entry: http://localhost:5173
auth: use the existing browser session; ask the user for manual login
environment: disposable local test environment
excluded: /admin/settings, all production customer data
tasks:
  - persona: warehouse-operator
    task: receive today's chilled shipment
    finish: shipment is recorded and the page confirms the warehouse and quantity
```

Credentials, tokens, cookies, and private keys belong in the runtime secret mechanism or manual user login, never in this file or a generated report.

## 2. Safety Gates

- Identify the environment before operating. If it may be production and the task can write, stop unless the user explicitly authorizes that exact action and provides a recovery plan.
- Use only declared disposable data for create, update, upload, send, delete, payment, dispatch, or other side effects.
- Prefer read-only exploration until the safety boundary is known.
- Do not force an error state by shutting down infrastructure. Use a supplied fixture, mock, disposable environment, or skip the state and record it as not covered.
- If the current page is a login screen, the session has expired, or authentication is required next, stop browser actions and immediately tell the user: “当前页面需要登录。请在浏览器中手动登录；完成后回复‘继续’，我会从当前页面接着检查。请不要把密码发给我。” Hand control to the user in that same browser session and wait; do not ask for credentials in chat, guess them, switch profiles/sessions, or route around the login. Resume only after the user explicitly replies “继续”. Until then, mark access blocked, task not completed, and all post-login steps uncovered.
- Apply the same in-browser handoff and explicit-continuation rule to CAPTCHA, 2FA, browser permission choices, and physical device interactions; never work around the prompt.

## 3. Run The Task

Operate from the persona's starting point and record the actual path. For each meaningful step, capture enough information to answer:

- Where did I start, what did I first notice, and did I know what to do next?
- Which control did I choose and why?
- Did I hesitate, reread text, backtrack, scroll hunt, or ask for help?
- What did I expect before the action? What actually happened?
- Did I make a correction or nearly make an irreversible mistake?
- Did system wording match the persona's vocabulary?
- Did feedback confirm the business result, or only that a request ended?
- What remained invisible, such as queued work, partial success, or another role's next step?

Count:

- Steps and distinct clicks or keyboard commands.
- Hesitation moments.
- Backtracking or wrong-branch events.
- Corrections and near misses.
- Requests or operations not covered.

Emotion labels must be tied to evidence. For example, "tense: the confirmation did not name the recipient and the persona's stated fear is sending goods to the wrong warehouse" is valid; "probably unhappy" is not.

## 4. Evidence

- Record the actual operation path with page names and controls.
- Save a screenshot for visual friction, confusion, missing feedback, overflow, and dangerous confirmation.
- Save a recording or sequential screenshots for multi-step expectations and recovery behavior.
- Use a command/response/export artifact for data or access conclusions.
- Redact credentials, personal data, tokens, and unrelated business data.
- Evidence must be sufficient for another agent to reproduce the same task state; if not, record the missing prerequisite rather than inventing it.

## 5. Static Evidence Mode

If no browser is available, only supplied screenshots, recordings, or rendered pages may be evaluated. State at the top:

> Static evidence only: interaction, recovery, navigation, and timing conclusions are speculative. The following operations were not performed.

Do not click mentally and report the imagined result as a system behavior. A static run may still evaluate visible hierarchy, wording comprehension, state clarity, and what the persona would need next, provided each conclusion is labeled as visual inference.

## 6. Stop Conditions

Stop and report rather than continuing when:

- The task cannot proceed because access, data, or authorization is missing.
- A required action violates the safety gate.
- The system enters a state the requester did not authorize.
- The persona's task outcome is determined: completed, completed with effort, or blocked.
- Evidence cannot be preserved and the requester requires evidence-backed findings.

The final report must say what was not covered; it must not convert a partial journey into a whole-system conclusion.
