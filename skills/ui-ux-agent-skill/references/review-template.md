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
