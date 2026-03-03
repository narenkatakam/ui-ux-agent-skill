# Review Output Template

Standard format for design review output. Use this template to ensure reviews are consistent, prioritized, and actionable.

---

## Header

```
## Design Review: [Screen / Flow Name]

**Platform:** [Web / iOS / Android / Cross-platform]
**Target user:** [Brief description]
**Primary task:** [What the user is trying to accomplish]
**Reviewed against:** [Screenshot / Figma link / live URL / PR]
```

---

## Assumptions

State assumptions made during the review. This sets context and prevents misinterpretation.

```
### Assumptions
- [Platform and viewport assumed]
- [User type assumed (new / returning / admin / etc.)]
- [Primary task assumed]
- [Content state assumed (populated / empty / error / etc.)]
```

---

## Findings

Prioritized list of issues. Each finding includes severity, diagnosis, evidence, and a fix.

### Severity levels

| Level | Meaning | Action |
|---|---|---|
| **P0 — Blocker** | Prevents task completion or causes data loss. Accessibility failures that block entire user groups. | Fix before ship. |
| **P1 — Important** | Significant usability friction, confusion, or inconsistency. Degrades experience but doesn't block. | Fix in current cycle. |
| **P2 — Polish** | Minor visual issues, copy improvements, micro-interaction refinements. | Fix when convenient. |

### Diagnosis labels

Tag each finding with the root cause. This guides the type of fix.

| Label | Meaning | Typical fix |
|---|---|---|
| **Gulf of Execution** | User can't figure out how to act. Action is hidden, unlabeled, or non-obvious. | Add signifiers, labels, visibility. |
| **Gulf of Evaluation** | User can't tell what happened or what the current state is. | Add feedback, state indicators, confirmation. |
| **Slip** | User intended the right action but executed it wrong (adjacent targets, motor error). | Increase spacing, add undo, separate destructive actions. |
| **Mistake** | User intended the wrong action due to misunderstanding. | Improve labels, add previews, fix conceptual model. |
| **Cognitive overload** | Too many choices, too much information, or too much to remember. | Simplify, group, default, progressively disclose. |
| **Consistency break** | Same concept treated differently across screens. | Align to the established pattern. |
| **Accessibility failure** | Does not meet WCAG 2.1 AA. Excludes users with disabilities. | Fix per WCAG criterion. See `accessibility.md`. |
| **Feedback gap** | Action produces no visible response or ambiguous response. | Add immediate, inline feedback. |

### Finding format

```
### [P0/P1/P2] — [Short title]

**Diagnosis:** [Gulf of Execution / Gulf of Evaluation / Slip / Mistake / Cognitive overload / Consistency break / Accessibility failure / Feedback gap]

**Evidence:** [What was observed — specific, factual. Reference the screenshot region, element, or interaction.]

**Impact:** [Who is affected and how. What task is impaired.]

**Fix:** [Specific, implementable recommendation. Component, copy, layout, or code-level.]

**Principle:** [Which principle from system-principles.md or SKILL.md this violates — optional but useful for pattern tracking.]
```

---

## Summary

After all findings, provide a brief summary.

```
### Summary

**Total findings:** [count] ([P0 count] blocker / [P1 count] important / [P2 count] polish)

**Primary issue pattern:** [One sentence describing the dominant problem type — e.g., "Most issues stem from weak feedback after user actions" or "State changes are not perceptible."]

**Strongest aspect:** [One thing the design does well — acknowledge good work when it exists.]
```

---

## Verification Checklist

Run after fixes are applied. Check every item.

### Task completion
- [ ] Primary task can be completed without confusion
- [ ] All P0 issues resolved
- [ ] Error paths handled (error messages are specific and actionable)
- [ ] Empty states are designed (not blank screens)
- [ ] Loading states prevent layout shifts

### Visual hierarchy
- [ ] Primary CTA is the most visually prominent element
- [ ] Squint test passes: most important element stands out first
- [ ] Visual weight budget respected (one focal point, not many)
- [ ] Spacing follows a consistent scale (no off-grid values)

### Consistency
- [ ] Same concept = same name, icon, component, and behavior across screens
- [ ] Interaction patterns match established conventions in the product
- [ ] Component variants are from the design system (no one-offs)

### Feedback
- [ ] Every action produces visible feedback within 100ms
- [ ] Success, error, and loading states are distinct and unambiguous
- [ ] Destructive actions confirm what will be lost

### Accessibility
- [ ] Color contrast meets WCAG AA (4.5:1 text, 3:1 UI components)
- [ ] Color is not the sole indicator of state or meaning
- [ ] All interactive elements are keyboard-accessible with visible focus
- [ ] Screen reader announces all interactive elements with correct labels and roles
- [ ] Touch targets meet 44x44px minimum
- [ ] Focus order matches visual reading order
- [ ] Images have appropriate alt text (descriptive or empty for decorative)
- [ ] `prefers-reduced-motion` is respected

### Copy
- [ ] Labels are clear and action-oriented (verbs on buttons, nouns on nav)
- [ ] Error messages explain what happened and how to fix it
- [ ] No jargon or internal terminology in user-facing text
- [ ] Help text justifies its presence (L1-L3 layering respected)

### Responsive
- [ ] Layout works at 320px, 768px, and 1280px+
- [ ] Content hierarchy preserved across breakpoints
- [ ] No horizontal scroll at any viewport width
- [ ] Touch targets adequate on mobile

---

## Surface-Specific Verification Gates

Apply the relevant gate based on what surface you're reviewing. Not all gates apply to every review — pick the ones that match.

### Universal States

Every data-driven component must handle all of these states. Missing any one is a bug.

- [ ] **Loading** — skeleton or placeholder that preserves layout. No spinners that collapse content. No blank screens.
- [ ] **Empty** — meaningful empty state with guidance. "No projects yet. Create your first project." Not just blank space.
- [ ] **Populated (few)** — layout works with 1-3 items. Cards don't stretch awkwardly. Lists don't look broken with a single row.
- [ ] **Populated (many)** — layout works with 50+ items. Pagination, virtual scrolling, or lazy loading in place. Performance doesn't degrade.
- [ ] **Error** — specific, actionable error message. Retry action available. Not "Something went wrong."
- [ ] **Partial error** — some data loaded, some failed. Show what loaded. Indicate what failed. Don't blow up the whole page.
- [ ] **Permission denied** — explain what access is needed and how to get it. Not a dead end.
- [ ] **Offline** — indicate connection loss. Show cached data where possible. Queue actions for when connection returns.
- [ ] **Stale data** — if data may be outdated, indicate when it was last refreshed. Offer a refresh action.

### Lists

- [ ] Row height is consistent across all items.
- [ ] Rows have clear tap/click targets (the entire row is interactive, not just a small link within it).
- [ ] Active/selected row is visually distinct from others.
- [ ] Sort order is visible and changeable. Default sort is the most useful for the primary task.
- [ ] Filter state is visible. Active filters are shown as chips or labels. A "clear all" option is available.
- [ ] Empty state is handled: "No results match your filters" with a clear-filters action.
- [ ] Bulk actions: select all, select range (shift-click), select individual. Bulk action bar appears on selection.
- [ ] Pagination or infinite scroll is implemented for large lists. Virtual scrolling for lists > 100 items.
- [ ] Search is available for lists with 15+ items.
- [ ] Loading state: skeleton rows preserve layout. Not a centered spinner.
- [ ] Truncation: long titles/names truncate with ellipsis and show full text on hover/tap.
- [ ] Responsive: list columns drop gracefully on narrow screens. Key columns remain visible.

### Detail Pages

- [ ] Page title matches the nav label or list item the user came from.
- [ ] Back/breadcrumb navigation returns the user to their previous context (list position preserved, not reset to top).
- [ ] Primary action (edit, submit, approve) is visually prominent and in a consistent position.
- [ ] Metadata (created date, author, status) is visible but secondary to the primary content.
- [ ] Content sections are clearly grouped with headings and spacing.
- [ ] Related items (comments, attachments, history) are accessible without leaving the page.
- [ ] Editable fields are distinguishable from read-only fields.
- [ ] Status changes (approve, reject, archive) produce immediate visible feedback.
- [ ] Long content scrolls naturally. Sticky headers or TOC for pages longer than 2 screen heights.
- [ ] Share/copy link is available for pages that might be referenced.

### Forms

- [ ] Every field has a visible `<label>` (not just placeholder text).
- [ ] Required fields are marked visually (asterisk, "required" text) and programmatically (`required` or `aria-required`).
- [ ] Field types match the data: date picker for dates, number input for numbers, email input for emails.
- [ ] Inline validation fires on blur, not on every keystroke and not only on submit.
- [ ] Error messages are adjacent to the field and specific: "Email must include @" not "Invalid input."
- [ ] Error summary at the top links to each errored field (for forms with 5+ fields).
- [ ] Sensible defaults are pre-filled where possible (country, currency, today's date).
- [ ] Format hints appear before the user types, not after they fail: "MM/DD/YYYY" below the date field.
- [ ] Multi-step forms show progress (step indicator) and allow back-navigation without data loss.
- [ ] Submit button is labeled with the action: "Create project" not "Submit." Disabled until required fields are filled.
- [ ] Success confirmation is clear: what was saved, what happens next.
- [ ] Long forms are grouped with fieldsets and visual section dividers.
- [ ] Tab order matches visual order. No tabindex hacks.
- [ ] Autofill works: fields have correct `name` and `autocomplete` attributes.

### Settings

- [ ] Settings are grouped by user mental model (Account, Notifications, Privacy, Appearance), not by backend structure.
- [ ] Every setting has a clear label and, where needed, a brief description of what it controls.
- [ ] Toggle changes are either auto-saved (with confirmation) or require an explicit "Save" action. Never ambiguous.
- [ ] Dangerous settings (delete account, reset data, revoke access) are separated from routine settings — different section, additional confirmation.
- [ ] Default values are sensible. A user who accepts all defaults should have a good experience.
- [ ] Search is available for settings pages with 15+ options.
- [ ] Changes are reflected immediately where possible (dark mode toggle takes effect instantly).
- [ ] Reset to defaults is available for user-customizable settings.
- [ ] Mobile: settings are full-width list items with clear tap targets. No cramped toggle rows.
- [ ] Permission-gated settings show the required access level and how to get it.

### Copy

- [ ] Button labels are verbs: "Save," "Create," "Delete," "Send." Not "OK," "Go," "Yes," "Continue" (unless the preceding question makes it unambiguous).
- [ ] Navigation labels are nouns: "Settings," "Projects," "Inbox." Not "Go to settings" or "View projects."
- [ ] Sentence case everywhere (except proper nouns and acronyms). Not Title Case On Every Button.
- [ ] Error messages: what happened + how to fix it. "Email must include an @ symbol." Not "Invalid input."
- [ ] Empty states: what this area is for + how to populate it. "No notifications yet. You'll see updates here when someone comments on your work."
- [ ] Confirmation dialogs name the consequence: "Delete 'Q4 Report'? This removes the file and all 12 comments. This cannot be undone."
- [ ] Tooltip text is one sentence maximum. If it needs more, it should be inline help (L1) or a help link (L3).
- [ ] No marketing copy in product UI. Promotional language in functional interfaces erodes trust.
- [ ] Consistent terminology: if it's "project" in the sidebar, it's "project" in the empty state, error messages, and settings. Never "workspace" in one place and "project" in another.
- [ ] Microcopy is tested: format hints match the actual validation ("MM/DD/YYYY" should accept exactly that format).

---

## Example Finding

```
### P0 — No feedback after form submission

**Diagnosis:** Feedback gap / Gulf of Evaluation

**Evidence:** Clicking "Save" on the settings form produces no visible response. The button doesn't change state, no toast appears, and the page doesn't scroll or change. Tested on Chrome 120 at 1280px.

**Impact:** All users. Users don't know if their settings saved. They may click repeatedly (causing duplicate submissions) or leave assuming it failed.

**Fix:**
1. Add a loading state to the "Save" button (disabled + spinner) on click.
2. On success, show an inline success message below the form: "Settings saved."
3. On failure, show an inline error message with retry guidance.
4. Button returns to default state after feedback is shown.

**Principle:** Feedback loop closure (system-principles.md #6).
```
