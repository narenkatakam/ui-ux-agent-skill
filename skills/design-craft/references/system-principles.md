# System-Level Guiding Principles

These are first-order design constraints. Apply them before choosing components, layouts, or patterns. They govern the interaction logic and mental model layer of any interface — the rules that hold whether you are building a dashboard, a settings page, or a creation flow.

Each principle includes a definition, practical details, and a review question.

---

## 1. Concept Constancy

**Definition:** The same concept must look the same, behave the same, and be named the same everywhere it appears.

**Details:**

- One concept = one name, one visual representation, one interaction pattern. If "project" appears as a card in one place and a row in another, the concepts must still share the same label, status model, and available actions.
- Renaming a concept mid-flow breaks the user's mental model. If the creation screen says "workspace" and the sidebar says "space," users don't know if they're the same thing.
- Visual consistency: same icon, same color token, same badge style for the same entity type. Different entity types must be visually distinguishable.
- Audit technique: search the UI for every instance of a concept. List every name, icon, and interaction used. Anything that doesn't match is a violation.

**Review question:** Pick any concept in the product (user, project, task, document). Is it named, displayed, and interacted with identically on every screen?

---

## 2. Primary Task Focus

**Definition:** Every screen has one primary task. The entire visual hierarchy must serve it.

**Details:**

- Identify the primary task and the primary CTA for each screen. Everything else is secondary or supporting.
- Secondary actions exist to support the primary task, not compete with it. They should be visually recessive: smaller, lower contrast, further from the focal point.
- If a screen has two equally prominent CTAs, neither is primary. Pick one. Demote the other.
- When adding a new feature, ask: does this serve the primary task of this screen? If not, it belongs elsewhere.
- Common violation: settings pages where the "Save" button is the same visual weight as "Delete Account."

**Review question:** Cover everything except the top 20% of the screen. Can you still identify the primary action?

---

## 3. UI Copy Source Discipline

**Definition:** Every string visible to the user must come from a controlled, reviewable source — never hardcoded in component logic.

**Details:**

- All user-facing text belongs in a dedicated copy layer: i18n files, CMS, constants file, or a copy module. Not scattered through JSX, templates, or component props as raw strings.
- This enables: copy review without code review, localization, A/B testing on copy, and consistency audits.
- Error messages, empty states, tooltips, button labels, confirmation dialogs, placeholder text — all of it. If a user can read it, it belongs in the copy source.
- Copy review cadence: every sprint, scan the copy source for inconsistencies — different terms for the same action, inconsistent sentence case vs. title case, conflicting tone.
- Naming conventions matter: `action.delete.confirm` is traceable; a string buried in `components/Modal/index.tsx` line 47 is not.

**Review question:** Can a content designer review and update every user-facing string without opening a single component file?

---

## 4. State Perceptibility

**Definition:** The user must perceive the system's current state at all times, through a clear signal hierarchy.

**Details:**

Signal hierarchy (strongest to weakest):

1. **Layout change** — something moves, appears, or disappears. Unmissable.
2. **Color change** — status badge turns red, background shifts. Strong but requires sufficient contrast.
3. **Icon/badge change** — a warning icon appears, a count badge updates. Moderate.
4. **Text change** — a label updates. Weakest — easy to miss in dense UIs.

Rules:
- Important state changes require signals at level 1 or 2. Don't rely on text alone for errors or status transitions.
- Combine signals for critical states: an error should produce a layout shift (inline message appears) + color change (red border) + icon (error icon) + text (what went wrong).
- Transient feedback (toast, snackbar) should be used for confirmations, not for state changes the user needs to track.
- If a state change happens silently, it didn't happen from the user's perspective.

**Review question:** Trigger every state change in the flow. For each one, can you identify the change within 1 second without reading any text?

---

## 5. Help Text Layering (L0-L3)

**Definition:** Help information must be layered by intrusiveness and need, from always-visible to on-demand deep guidance.

**Layers:**

| Level | Description | Example | When to use |
|---|---|---|---|
| **L0** | Self-evident UI | Clear labels, obvious affordances, sensible defaults | Always — this is the goal |
| **L1** | Inline help | Hint text under a field, format examples, microcopy | When L0 isn't enough to prevent errors |
| **L2** | On-demand help | Tooltip, popover, info icon (i) | When context would clutter the screen at L1 |
| **L3** | Deep guidance | Documentation link, walkthrough, onboarding tour | When the concept requires explanation beyond a sentence |

Rules:
- Every piece of help text must justify its level. If it can move to a lower level (less intrusive), move it.
- **Copy budget heuristic:** if a single screen has more than 40-60 words of L1 help text, the UI itself is probably unclear. Fix the UI before adding more copy.
- L2 (tooltips/popovers) must be accessible: keyboard-triggerable, not hover-only, announced by screen readers.
- L3 links should open in context (panel, modal) rather than navigating away. Don't break the user's flow for help.
- Anti-pattern: long paragraphs of instructional text above a form. If you need a paragraph to explain a form, the form is too complex.

**Review question:** Remove all help text (L1-L3). Is the screen still usable for the primary task? If not, L0 has failed.

---

## 6. Feedback Loop Closure

**Definition:** Every user action must produce visible feedback that confirms the outcome and suggests the next step.

**Details:**

A complete feedback loop answers three questions:
1. **Did it work?** — success/failure indication.
2. **What changed?** — the affected element reflects the new state.
3. **What next?** — the user knows their options.

Rules:
- Feedback must appear within 100ms for direct manipulation, 1s for system operations. Beyond 1s, show a loading state. Beyond 10s, show progress.
- Inline feedback (the changed element updates in place) is always preferred over displaced feedback (toast at a different location).
- Destructive action feedback must be especially clear: "3 items deleted" with an undo option, not just a flash of green.
- Failed actions must explain why and offer a path forward. "Save failed — check your connection and try again" not "Error."
- Common gap: form submission succeeds silently. The user doesn't know if the save worked because nothing visually changed.

**Review question:** Perform every action in the flow. After each one, do you know — without doubt — what happened and what to do next?

---

## 7. Prevention First + Recoverability

**Definition:** The best error handling prevents errors. The second best makes them recoverable.

**Details:**

Prevention hierarchy:
1. **Make the wrong action impossible** — disable invalid options, constrain inputs, remove destructive paths from common flows.
2. **Make the wrong action unlikely** — sensible defaults, inline validation, format hints, progressive disclosure of risky options.
3. **Make the wrong action reversible** — undo, draft saves, soft deletes, version history.
4. **Make the wrong action diagnosable** — when none of the above works, provide clear, specific error messages with recovery instructions.

Rules:
- Constraints are the strongest form of prevention: a date picker prevents invalid dates; a dropdown prevents free-text errors.
- Inline validation should fire on blur or after a short delay, not on every keystroke (which creates noise) and not only on submit (which is too late).
- Destructive confirmations must name what will be lost: "Delete 'Q4 Report' and all 12 attachments? This cannot be undone." Not "Are you sure?"
- Undo is always preferable to confirmation dialogs. "Deleted. Undo (10s)" is faster and less disruptive than "Are you sure? [Cancel] [Delete]."

**Review question:** What is the worst thing a user could accidentally do on this screen? How hard is it to do accidentally, and how easy is it to undo?

---

## 8. Progressive Complexity

**Definition:** Show the simple path first. Reveal complexity only when the user needs it.

**Details:**

- Default view: the 80% case. Advanced options, filters, bulk operations, and developer settings stay hidden until invoked.
- Reveal mechanisms: expandable sections ("Advanced options"), secondary panels, settings pages, progressive forms that show next fields based on previous answers.
- Complexity should reveal in the user's direction of motion. If they're filling a form, the next field appears below — not in a sidebar or modal.
- Never front-load configuration. Sensible defaults should let users start immediately and customize later.
- Expert users must be able to access all power features — but those features must not confuse beginners.
- Anti-pattern: a "simple mode" and "advanced mode" toggle. This creates two products. Instead, layer complexity within a single coherent flow.

**Review question:** Can a first-time user complete the primary task without encountering any advanced options? Can a power user access everything they need without switching modes?

---

## 9. Action Perceptibility

**Definition:** Every available action must be visually discoverable and distinguishable from content.

**Details:**

- Buttons look like buttons. Links look like links. Non-interactive text does not look clickable.
- Visual signals for interactivity: color contrast, underline (links), elevation/border (buttons), cursor change (pointer), hover/focus state.
- Primary actions must be visually dominant. Secondary actions visually recessive. Destructive actions visually cautious (not red and large — that invites accidental clicks).
- Hidden actions (revealed on hover, right-click, long-press) are acceptable only as shortcuts for expert users. There must always be a visible path to the same action.
- Ghost buttons (transparent background, just a border) are acceptable for secondary actions, but they must still read as interactive — never as decorative rectangles.
- Disabled states: reduce opacity and remove pointer events. Never leave a button looking active but silently failing on click.

**Review question:** Without hovering over anything, can you identify every actionable element on the screen? Can you tell which is primary?

---

## 10. Cognitive Load Budget

**Definition:** Every screen has a finite budget of user attention. Spend it on the primary task.

**Details:**

Budget categories:
- **Choices** — every option added costs attention. Hick's Law: decision time increases logarithmically with options. 3-5 choices is fast; 15+ is paralyzing.
- **Visual elements** — every color, icon, badge, border, and animation competes for attention. Budget: 3-4 visual emphasis levels per screen (primary, secondary, tertiary, muted).
- **Memory demands** — anything the user must remember across screens is expensive. Carry context forward. Show selections, show summaries, show breadcrumbs.
- **Novel patterns** — familiar patterns are cheap (users already know them). Novel patterns are expensive (users must learn them). Use novel patterns only when the interaction genuinely requires it.

Rules:
- When adding an element to a screen, identify what it displaces in the attention budget. If nothing is displaced, the screen is getting noisier.
- Sensible defaults reduce choice cost. Smart sorting reduces scanning cost. Search reduces finding cost.
- Periodic audit: for each screen, count distinct visual weights, interactive elements, and text blocks. If the count keeps growing, the screen needs editing.

**Review question:** As the product scales and content grows, does this screen's comprehension cost stay stable — or does it degrade linearly with data volume?

---

## 11. Evolution with Semantic Continuity

**Definition:** When the product evolves, users must recognize what changed and what stayed the same.

**Details:**

- UI changes must preserve semantic anchors: if users learned that "blue badge = active," changing blue to green without notice breaks their model.
- Introduce new features by extending existing patterns, not by creating parallel systems. A new filter type should appear in the existing filter bar, not in a separate panel.
- When a breaking change is unavoidable, provide a visible transition: migration notice, "what's new" callout, or a one-time guided walkthrough of the changed area.
- Feature deprecation must be gradual: notify, offer the alternative, sunset with a timeline. Never silently remove a feature users depend on.
- Relabeling must be systematic: if "Projects" becomes "Workspaces," update every instance simultaneously — nav, breadcrumbs, empty states, documentation, error messages.
- Version transitions: maintain visual and interaction continuity. A redesigned screen should still feel like the same product.

**Review question:** If a regular user returns after a 2-week absence, will they recognize the interface and find their workflows intact?
