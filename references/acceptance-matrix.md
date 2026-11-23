# UI Acceptance Matrix

This file is the sole owner of formal acceptance dimensions, checks, severity definitions, pass criteria, and regression rules.

## 1. Formal Preconditions

Enter formal acceptance only when every requirement below is satisfied. A missing item is rejectable and is not itself a development defect.

| Item | Requirement |
| --- | --- |
| Design baseline | The visual baseline is final and has a version identifier. Acceptance uses only that version. |
| Functional state | Development self-test passed, primary flows run, and no blocking error exists. |
| Environment | The target environment, access method, prepared data, and any required roles are explicit. |
| Browsers/devices | The viewport and device range is agreed. Declare every resolution separately; do not infer coverage. |
| Data | Data volume and states are realistic for the declared scope. |
| Scope declaration | Included and excluded pages are explicit, including the boundary of any excluded page. |

## 2. Acceptance Dimensions

### 2.1 Visual Consistency Against Design

Compare against the finalized design version, preferably design tokens rather than eyeball estimates.

- Color, font size, weight, and line height match the baseline.
- Spacing, alignment, radius, shadow, and border match the specification.
- Icons, illustrations, images, and media are not distorted, stretched, jagged, missing, or replaced by unintended placeholders.
- Status semantics are globally consistent: the same status uses the same color and wording everywhere.
- Text does not overflow, truncate unexpectedly, wrap incorrectly, or misalign in mixed-script content.

Judgment: overlay same-resolution screenshots, or inspect the rendered image with a visual-capable model/tool. OCR-only text extraction is not valid visual evidence.

### 2.2 Layout And Responsive Behavior

At every agreed viewport:

- No unexpected horizontal page scrolling, overlapping elements, or collapsed whitespace.
- Layout survives browser zoom from 80% to 125%.
- Wide tables provide horizontal scrolling or fixed columns; headers remain aligned with content.
- Popovers, dropdowns, tooltips, drawers, and modals stay inside the viewport and are not incorrectly obscured.

### 2.3 Interaction And Feedback

- Every clickable element has appropriate hover, active, and disabled states, with correct cursor semantics.
- Click, submit, delete, and other actions give clear feedback; no silent no-op behavior.
- Asynchronous operations show loading, skeleton, spinner, or progress feedback and prevent duplicate submission.
- Modals, drawers, dropdowns, and popovers open and close correctly; mask behavior follows the agreed convention.
- Form validation triggers at the agreed blur/submit moment and errors identify the specific field.
- Refresh and browser back/forward do not create exceptions, stuck state, or dirty data.

### 2.4 State Coverage

Check these states item by item where the declared scope makes them applicable.

| State | Required result |
| --- | --- |
| Loading | First screen, partial loading, and pagination all have visible feedback. |
| Empty | An empty state explains why it is empty and what the user can do next. |
| Error | API failure and network interruption provide a fallback message and retry entry. |
| No permission | Hidden versus disabled behavior is consistent, with an explanation rather than a blank screen. |
| Disabled/read-only | Visual and interaction behavior clearly differ from actionable controls. |
| Partial success | Batch results separately identify successful and failed items. |

### 2.5 Copy

- No typos, punctuation misuse, or inconsistent spacing between scripts.
- Notices explain what happened, why, and the next action.
- Errors are actionable; a generic failure or system-error message alone is a failure.
- Buttons use concrete verbs; avoid making generic confirmation words carry all meaning.
- Dangerous-action confirmation states the consequence.
- Terminology is globally consistent; one concept must not have multiple competing names.

### 2.6 Feedback And Notification Style

- Toast, modal, inline validation, and popover are used in the appropriate scenario.
- Dangerous notices are visually distinct from ordinary notices.
- Error notices do not auto-dismiss; success notices use a reasonable duration.
- Notification layering is correct and not obscured by other elements.

### 2.7 Data Display And Boundaries

- Extremely long text, very large numbers, zero, negative numbers, and null render sensibly.
- Units, precision, and rounding rules are consistent.
- Time format and timezone are consistent, with timezone labels for cross-region systems.
- Currency symbols and thousands separators are consistent.
- Sorting, filtering, pagination, totals, page data, and export results agree.
- Export content matches page data and large exports do not time out.

### 2.8 Roles And Permissions

- Verify each declared role for menus, buttons, fields, and data scope.
- URL and parameter tampering is blocked with a friendly response rather than exposing data.
- Data isolation follows the declared business boundary.

### 2.9 Accessibility And Field Usability

Apply these as mandatory when the product baseline requires them; otherwise mark them `不适用 + 原因`.

- Key operations are keyboard reachable with a sensible focus order.
- Status is not conveyed by color alone; text or icon is also present.
- Images have alternative text and form controls have associated labels.
- For field or touch scenarios, verify touch-target size, readability in low light, and one-hand operation paths.

## 3. Severity Definitions

| Level | Definition | Handling |
| --- | --- | --- |
| P0 Blocker | Task cannot be completed, data is wrong, dangerous misoperation is possible, or the page whites out or errors. | Must be fixed; acceptance cannot pass while unclosed. |
| P1 Critical | Efficiency or comprehension is materially affected, layout is broken, or permission behavior is abnormal. | Must be fixed; acceptance cannot pass while unclosed. |
| P2 General | Visual deviation, copy flaw, or non-primary-flow experience problem. | May be scheduled by agreement, but must have an owner and explicit due date. |
| P3 Suggestion | Optimization idea that does not violate this specification. | Record in the optimization pool. |

Classification disputes are settled by whether the issue affects task completion or data accuracy. The named acceptor makes the final severity decision.

## 4. Pass Criteria

Formal acceptance passes only when every condition is true:

- Unclosed P0 count is zero.
- Unclosed P1 count is zero.
- Every P2 has an owner and explicit due date.
- Every applicable matrix item has exactly one conclusion: `通过`, `不通过`, or `不适用 + 原因`; blank or omitted items are failures.
- Conclusions are independent of implementer self-assessment and are supported by reproducible evidence.

## 5. Regression Rules For Reacceptance

Reacceptance verifies only:

1. Every unclosed defect from the prior acceptance round.
2. A change-based regression sample covering other functions on changed pages and other pages that reuse affected components.

Record the sampled pages, components, cases, result, and every new defect found by the sample. A sample failure is handled under the severity and pass rules above. Do not expand to full-form checks unless the requester explicitly upgrades the mode.
