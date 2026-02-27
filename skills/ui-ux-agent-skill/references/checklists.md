# Checklists

Expanded checklists for reviewing and building UI. Organized by domain. Use these as verification gates — not creative guidelines.

---

## Universal States

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

**Review question:** Pick any component. Force each state. Does every state have a designed response?

---

## Affordance and Signifiers

- [ ] Every clickable element has `cursor: pointer` and a visible hover/focus state.
- [ ] Buttons look like buttons: distinct from surrounding content through color, border, or elevation.
- [ ] Links are visually distinct from non-linked text: underlined, colored, or both.
- [ ] Disabled elements look disabled: reduced opacity, no pointer events, and ideally a tooltip explaining why.
- [ ] Primary actions are visually dominant. Secondary actions are visually recessive. Destructive actions are cautious (not large and red).
- [ ] Icon-only buttons are reserved for universal icons (close, search, settings, menu, back, home). All others have a text label.
- [ ] Drag handles are visually distinct from decoration.
- [ ] Non-interactive elements do not look interactive: no underlines on plain text, no hover effects on static content, no pointer cursor on read-only fields.
- [ ] Form inputs have visible borders or backgrounds that distinguish them from labels and static text.
- [ ] Toggle states are unambiguous: "On"/"Off" labels, filled vs. outlined icons, or explicit text.

**Review question:** Without hovering or clicking anything, can you identify every actionable element on the screen?

---

## Lists

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

**Review question:** Load 200 items into this list. Is it still usable? Now filter to 0 results. Is that state handled?

---

## Detail Pages

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

**Review question:** Navigate from a list to this detail page and back. Did you land in the same position? Did the detail page answer the question you had from the list?

---

## Forms

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

**Review question:** Fill out this form with every possible error. Are all errors caught, explained, and recoverable without re-entering correct data?

---

## Settings

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

**Review question:** Would a new user understand what each setting does without changing it to see what happens?

---

## Motion and Animation

- [ ] Every animation serves a purpose: hierarchy (layers), state change (feedback), or spatial orientation (where something came from / went to).
- [ ] No decorative motion: no breathing backgrounds, floating elements, or bouncing icons.
- [ ] Duration: micro-interactions 100-200ms. Panel/page transitions 200-350ms. Nothing longer than 500ms without a reason.
- [ ] Easing: use ease-out for entrances, ease-in for exits, ease-in-out for position changes. No linear motion for UI elements.
- [ ] `prefers-reduced-motion` is respected. All motion reduced to instant or near-instant transitions.
- [ ] No animation blocks interaction. Users must be able to click/tap during or immediately after an animation.
- [ ] Loading animations are subtle and located near the content they represent.
- [ ] Same component type uses the same motion pattern throughout the product.
- [ ] No more than one animation playing at a time in the user's focal area.
- [ ] Page transitions are minimal for routine navigation. Reserve dramatic transitions for significant context changes.

**Review question:** Disable all animations. Does the interface still communicate hierarchy and state changes clearly? If not, the animation is doing work that the static design should handle.

---

## Dashboards

- [ ] Primary metric / KPI is the most visually prominent element on the page.
- [ ] Metrics show context: current value + trend (up/down/flat) + comparison period.
- [ ] Charts have clear titles, labeled axes, and legible legends. No chartjunk (3D effects, excessive gridlines, decorative fills).
- [ ] Data density is appropriate: dashboards should be scannable, not require close reading.
- [ ] Time range selector is visible and defaults to the most useful period.
- [ ] Empty data states are handled: "No data for this period" not a blank chart.
- [ ] Loading states for each widget independently — don't hold the entire dashboard for one slow query.
- [ ] Filters apply globally and their state is visible. Active filters shown as chips or in a filter bar.
- [ ] Drill-down is available: clicking a metric or chart segment leads to detail data.
- [ ] Responsive: widgets reflow from multi-column to single-column. Charts remain legible at narrow widths.
- [ ] Color is used consistently: green = positive, red = negative, gray = neutral. Never reversed.
- [ ] Data refreshes are indicated: last-updated timestamp, auto-refresh indicator, or manual refresh button.

**Review question:** Glance at this dashboard for 3 seconds. Can you answer the most important question it's meant to answer?

---

## Copy Rules

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

**Review question:** Read every piece of text on the screen aloud. Does each one help the user act or understand? If not, cut it.

---

## Accessibility

Brief checklist. Full guidance in `references/accessibility.md`.

- [ ] Color contrast meets WCAG AA: 4.5:1 for normal text, 3:1 for large text and UI components.
- [ ] Color is never the sole indicator of meaning (error, status, selection).
- [ ] All interactive elements reachable and operable via keyboard.
- [ ] Visible focus indicators on every interactive element (minimum 3:1 contrast).
- [ ] Semantic HTML used (button, a, nav, main, h1-h6, label, fieldset).
- [ ] ARIA used only when native HTML is insufficient. Correct roles, states, and properties.
- [ ] Images have appropriate alt text. Decorative images have `alt=""`.
- [ ] Form fields have visible labels (not just placeholders) with `for`/`id` pairing.
- [ ] Error messages associated with fields via `aria-describedby`.
- [ ] `prefers-reduced-motion` respected.
- [ ] Heading hierarchy is logical (h1 > h2 > h3, no skipped levels).
- [ ] One `<h1>` per page.
- [ ] Dynamic content changes announced via `aria-live` regions.
- [ ] Touch targets meet 44x44px minimum.
- [ ] Zoom to 200% does not break layout or clip content.

**Review question:** Can a keyboard-only user with a screen reader complete the primary task? Full audit: `references/accessibility.md`.

---

## Responsive Design

Brief checklist. Full guidance in `references/responsive-design.md`.

- [ ] Mobile-first CSS: base styles for mobile, enhancements at wider breakpoints.
- [ ] Content hierarchy preserved across screen sizes (not just reflowed).
- [ ] Touch targets meet 44x44px minimum on mobile.
- [ ] No horizontal scroll at any viewport width (320px to 1440px+).
- [ ] Images are appropriately sized per breakpoint (`srcset`/`sizes`).
- [ ] Body text stays at 16px minimum on all screens.
- [ ] Line length capped at ~65ch for body text on desktop.
- [ ] Navigation adapts: visible on desktop, collapsed/tabbed on mobile.
- [ ] Forms are single-column on mobile.
- [ ] Tables have a mobile strategy (scroll, stack, or priority columns).
- [ ] Tested on at least one real iOS and one real Android device.
- [ ] Layout survives browser zoom at 200%.

**Review question:** Does the interface work on a phone held in one hand? Full audit: `references/responsive-design.md`.

---

## Navigation

Brief checklist. Full guidance in `references/navigation.md`.

- [ ] User can identify their current location at all times (active nav state, breadcrumb, page title).
- [ ] Top-level nav items limited to 5-7.
- [ ] Nav labels describe destinations clearly (no jargon, no cleverness).
- [ ] Nav label matches page title exactly.
- [ ] Back/return behavior is consistent and predictable.
- [ ] On mobile, primary destinations reachable with one tap from bottom tab bar.
- [ ] Search is accessible from every page.
- [ ] URL structure mirrors navigation hierarchy.
- [ ] Keyboard navigation works for all nav elements.
- [ ] Hamburger menu is used only on mobile, never as sole desktop navigation.
- [ ] Deep nesting limited to 3 levels maximum.
- [ ] Nav does not shift layout or reorder on different screens.

**Review question:** Can a new user find the top 3 features within 10 seconds? Full audit: `references/navigation.md`.
