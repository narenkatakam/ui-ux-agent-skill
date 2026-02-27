---
name: design-craft
description: >
  Production-grade UI/UX guidance and review skill. Transforms vague design feedback into
  actionable, implementable recommendations. Two modes: `guide` (principles + do/don't rules
  for modern interfaces) and `review` (structured audit with prioritized fixes). Covers
  task-first UX, information architecture, CRAP visual hierarchy, accessibility, responsive
  design, typography, color systems, cognitive psychology, and interaction patterns.
  Enforces a modern minimal aesthetic — clean, spacious, typography-led — with zero tolerance
  for emoji-as-icons, decoration-first design, or AI-generated visual excess.
---

# Design Craft — UI/UX Guide (Modern Minimal)

A skill for AI coding assistants that turns abstract design feedback into code you can ship.

Two modes:

- `guide` — compact principles and concrete do/don't rules for building modern, clean interfaces.
- `review` — structured audit of existing UI (screenshot / mock / HTML / PR) with prioritized, implementable fixes.

Keep outputs concise. Bullets over paragraphs. Specific over generic.

---

## Workflow

### 1) Guide workflow

1. Identify the surface: marketing page / dashboard / settings / creation flow / list-detail / form / mobile screen.
2. Identify the primary user task and primary CTA.
3. Apply system-level guiding principles first — these are the mental model and interaction logic layer (`references/system-principles.md`).
4. Apply core principles below — start from UX, then refine with CRAP visual hierarchy.
5. Apply domain-specific guidance as needed:
   - Icons: `references/icons.md`
   - Accessibility: `references/accessibility.md`
   - Responsive: `references/responsive-design.md`
   - Typography: `references/typography.md`
   - Color: `references/color-systems.md`
   - Navigation: `references/navigation.md`
   - Data visualization: `references/data-visualization.md`

### 2) Review workflow

1. State assumptions (platform, target user, primary task).
2. List findings as `P0 / P1 / P2` (blocker / important / polish) with short evidence.
3. For each major issue, label the diagnosis: execution vs evaluation gulf; slip vs mistake (see `references/design-psych.md`).
4. Propose fixes that are implementable (layout, hierarchy, components, copy, states).
5. Check accessibility compliance against `references/accessibility.md`.
6. End with a short checklist to verify changes.

Use `references/review-template.md` for a stable output format.

---

## Non-Negotiables (hard rules)

These are binary. No exceptions.

- **No emoji as icons** — no emoji as UI decoration, status indicators, section markers, or button labels. Replace with a proper icon from a consistent set.
- **One icon family** — use a single consistent icon set across the product. No mixing outlined/filled/3D/emoji styles.
- **Minimize copy by default** — add explanatory text only when it prevents errors, reduces ambiguity, or builds trust. Text is the last resort, not the default.
- **Accessible by default** — WCAG 2.1 AA is the floor, not an aspiration. Color contrast, keyboard navigation, screen reader support, and focus management are non-negotiable.
- **No decoration without purpose** — every visual effect (gradient, shadow, blur, glow, animation) must answer: "what does this help the user understand?" No answer = remove it.

---

## System-Level Guiding Principles

Apply these as first-order constraints before choosing components or page patterns.
Full definitions and review questions: `references/system-principles.md`.

Key principles: concept constancy / primary task focus / UI copy source discipline / state perceptibility / help text layering (L0-L3) / feedback loop closure / prevention + recoverability / progressive complexity / action perceptibility / cognitive load budget / evolution with semantic continuity.

---

## Core Principles

### A) Task-First UX

The thing about any interface is that it exists to serve a task. Not to look good. Not to impress stakeholders. To help someone do something.

- Make the primary task obvious in <3 seconds.
- Allow exactly one primary CTA per screen/section.
- Optimize the happy path; hide advanced controls behind progressive disclosure.
- Remove anything that doesn't serve the user's current goal.

Review question: If someone lands here cold, can they figure out what to do in 3 seconds?

### B) Information Architecture (grouping + findability)

- Group by user mental model (goal / object / time / status), not by backend data structure.
- Use clear section titles; keep navigation patterns stable across similar screens.
- When item count grows: add search/filter/sort early, not late.
- Card sort your IA against real user language, not your internal nomenclature.

Review question: Does the grouping match how users think about this, or how the database stores it?

### C) Feedback and System Status

- Cover all states: loading, empty, error, success, permission. Full checklist in `references/checklists.md`.
- After any action, answer three questions: "Did it work?" + "What changed?" + "What can I do next?"
- Prefer inline, contextual feedback over global toasts (except for cross-page actions).
- Loading states must prevent layout jumps — use skeletons, not spinners that collapse content.

Review question: At any moment, can the user tell what the system is doing and what they should do next?

### D) Consistency and Predictability

- Same interaction = same component + same wording + same placement.
- Use a small, stable set of component variants; avoid one-off styles.
- Consistency across screens matters more than pixel-perfection on any single screen.

Review question: If someone learns this pattern on page A, will it transfer to page B without relearning?

### E) Affordance + Signifiers (make actions obvious)

- Clickable things must look clickable: button/link styling + hover/focus + cursor change. On web, custom clickable elements need `cursor: pointer` and visible focus styles.
- Primary actions need a label; icon-only is reserved for universally-known actions (search, close, settings).
- Show constraints before submit (format, units, required), not only after errors.
- For deeper theory: `references/design-psych.md`.

Review question: Without any help text or tooltips, can users predict what is actionable and what will happen?

### F) Error Prevention and Recovery

- Prevent errors with constraints, defaults, and inline validation.
- Make destructive actions reversible when possible; otherwise require deliberate confirmation.
- Error messages must be actionable: what happened + how to fix it. Never just "Something went wrong."
- Frame destructive confirmations around what will be lost, not the action itself.

Review question: Is the path designed to be easy to do right and safe to recover when wrong?

### G) Cognitive Load Control

- Reduce choices: sensible defaults, presets, and progressive disclosure.
- Break long tasks into steps only when it genuinely reduces thinking (not just to look "enterprise").
- Keep visual noise low: fewer borders, fewer colors, fewer competing highlights.
- Don't force users to remember information across screens — carry context forward.

Review question: As information grows, does comprehension cost stay stable?

### H) CRAP (visual hierarchy + layout)

- **Contrast** — emphasize the few things that matter (CTA, current state, key numbers). If everything is bold, nothing is.
- **Repetition** — tokens, components, and spacing follow a scale. Avoid "almost the same" styles — they create cognitive noise.
- **Alignment** — align to a clear grid. Fix 2px drift. Align baselines where text matters.
- **Proximity** — tight within a group, loose between groups. Spacing is the primary grouping tool, not borders.

Review question: Close your eyes, open them — is the first thing you see the most important thing on the page?

### I) Accessibility

Accessibility is not a feature. It's a quality standard. Like structural integrity in a building — you don't "add" it later.

- WCAG 2.1 AA is the minimum. Test against it.
- Color must never be the only way to convey information.
- All interactive elements must be keyboard accessible with visible focus indicators.
- Screen reader users must get equivalent information and functionality.
- Full guidance: `references/accessibility.md`.

Review question: Can a user who can't see color, can't use a mouse, or can't see at all still complete the primary task?

### J) Responsive Design

- Design for the smallest screen first, then enhance for larger ones.
- Content hierarchy should be preserved across breakpoints, not just reflowed.
- Touch targets: minimum 44x44 CSS px (web) / 48x48 dp (mobile).
- Full guidance: `references/responsive-design.md`.

Review question: Does the interface work on a phone held in one hand, a tablet in landscape, and a wide desktop monitor?

### K) Typography

Typography is the backbone of visual hierarchy. Get it right and everything else falls into place.

- Use a type scale — not arbitrary font sizes.
- Limit to 2 typefaces maximum (one for headings, one for body — or one for everything).
- Line length: 45-75 characters for body text. Wider than that and reading comprehension drops.
- Full guidance: `references/typography.md`.

Review question: Can you identify the hierarchy (title > subtitle > body > caption) just from the typography?

### L) Color Systems

- Keep the palette small and intentional. One accent color for primary actions and key states.
- Use semantic color tokens, not raw hex values — this makes theming and dark mode possible.
- Test all color combinations for WCAG AA contrast (4.5:1 for text, 3:1 for large text and UI components).
- Full guidance: `references/color-systems.md`.

Review question: If you printed this screen in grayscale, would the hierarchy still be clear?

---

## Spacing and Layout Discipline

Use this when implementing or reviewing layouts. Short set of rules, strictly enforced.

- **Rule 1 — One spacing scale:**
  - Base unit: 4px.
  - Allowed set: 4 / 8 / 12 / 16 / 24 / 32 / 40 / 48 / 64 / 80.
  - Off-scale values need a clear, documented reason.

- **Rule 2 — Repetition first:**
  - Same component type keeps the same internal spacing (cards, list rows, form groups, section blocks).
  - Components with the same visual role must not have different spacing patterns.

- **Rule 3 — Alignment + grouping:**
  - Align to one grid and fix 1-2px drift.
  - Tight spacing within a group, looser spacing between groups.
  - Spacing is the primary grouping tool. If you need a border to show grouping, your spacing is wrong.

- **Rule 4 — No decorative nesting:**
  - Extra wrappers must add real function (grouping, state, scroll, affordance).
  - If a wrapper only adds border/background, remove it and group with spacing instead.

- **Quick review pass:**
  - Any off-scale spacing values?
  - Any baseline/edge misalignment?
  - Any wrapper layer removable without losing meaning?

---

## Modern Minimal Style Guidance

This is about taste, not trends. Modern minimal is not "flat and boring." It's using restraint as a design tool.

- Use whitespace + typography to create hierarchy. Avoid decoration-first design.
- Prefer subtle surfaces (light elevation, low-contrast borders). Avoid heavy shadows.
- Keep color palette small; use one accent color for primary actions and key states.
- Copy: short, direct labels. Add helper text only when it reduces mistakes or increases trust.
- Let content breathe. Dense information is not the same as useful information.

---

## Motion and Animation Guidance

- Motion explains **hierarchy** (what is a layer/panel) and **state change** (what just happened). Not decoration.
- Default motion vocabulary: fade > small translate+fade > tiny scale+fade for overlays. Avoid bouncy, elastic, or "showy" motion.
- Keep the canvas/content area stable. Panels and overlays can move; the work surface must not float.
- Same component type uses the same motion pattern. Consistency over creativity.
- Avoid layout jumps. Use placeholders/skeletons to keep layout stable while loading.
- Interaction feedback (hover/pressed) must feel immediate. Never make users wait for an animation to complete before they can proceed.

Red flags:
- Continuous decorative motion (breathing backgrounds, floating cards)
- Large bouncy/elastic overshoot that steals attention
- Big page-level transitions for routine navigation
- Multiple simultaneous animations competing for attention

---

## Anti-AI Self-Check (run after generating any UI)

Before finalizing generated UI, verify these items. Violating any one is a mandatory fix.

- **Gradient restraint** — gradients must convey meaning (progress, depth, state distinction). Purely decorative gradients: at most one per page. If background, buttons, and borders all use gradients simultaneously, that is overuse. Pick one and flatten the rest.
- **No emoji as UI** — re-check: no emoji slipped in as section icons, status indicators, or button labels. This is already a non-negotiable. Double-check.
- **Copy necessity** — for every piece of text, ask: if I remove this, can the user still understand through layout, icons, and position alone? If yes, remove it.
- **Decoration justification** — every purely visual effect (blur, glow, animated entrance, layered shadows) must answer: "what does this help the user understand?" No answer = remove.
- **Contrast check** — run a quick contrast check on all text/background combinations. WCAG AA minimum.
- **Touch target check** — all interactive elements meet minimum 44x44 CSS px.
- **Keyboard test** — tab through the entire interface. Can you reach and activate every interactive element? Is focus visible?

---

## References

- System-level guiding principles: `references/system-principles.md`
- Interaction psychology (Fitts/Hick/Miller, cognitive biases, flow, attention): `references/interaction-psychology.md`
- Design psychology (affordances, signifiers, mapping, constraints, gulfs, slips vs mistakes): `references/design-psych.md`
- Accessibility (WCAG, keyboard, screen readers, color, forms, media): `references/accessibility.md`
- Responsive design (breakpoints, mobile-first, touch, fluid layouts): `references/responsive-design.md`
- Typography (type scale, pairing, line height, measure, hierarchy): `references/typography.md`
- Color systems (palettes, semantic tokens, contrast, dark mode): `references/color-systems.md`
- Navigation patterns (nav structures, wayfinding, mobile nav): `references/navigation.md`
- Data visualization (chart selection, data-ink ratio, dashboard design): `references/data-visualization.md`
- Icon rules and "intuitive refined" guidance: `references/icons.md`
- Review output template and scoring: `references/review-template.md`
- Expanded checklists (states, affordance, lists, forms, settings, motion, dashboards, copy): `references/checklists.md`
