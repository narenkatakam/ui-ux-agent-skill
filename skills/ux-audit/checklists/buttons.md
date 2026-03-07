# Buttons Checklist

Building buttons? Check these before shipping.

---

1. **One primary CTA per section/screen.** If two buttons compete for attention, the user hesitates. Pick one. (Principle A: Task-First UX)

2. **Label with the action verb.** "Save changes," "Create project," "Delete account" — not "Submit," "OK," "Yes." The label should tell the user what happens next. (Principle E: Affordance)

3. **Visual hierarchy: primary → secondary → tertiary.**
   - Primary: filled background + white text. One per section.
   - Secondary: outline border. Supporting actions.
   - Tertiary: text-only or ghost. Low-priority actions (cancel, skip, learn more).

4. **Cover all states.** Default, hover, active/pressed, focus, disabled, loading. Missing any one is a bug. (Ref: `ui-states.md`)

5. **Loading state keeps width stable.** Disable the button + show an inline spinner. Never let the button resize or disappear — it causes layout shift. (Principle C: Feedback)

6. **Min touch target: 44x44 CSS px.** Even if the visible button is smaller, the tap area must be at least 44px. (Ref: `responsive-design.md`)

7. **Visible focus ring for keyboard users.** `focus:ring-2 focus:ring-offset-2` or equivalent. Never `outline: none` without a replacement. (Ref: `accessibility.md`)

8. **Destructive actions use a distinct style.** Red or outline-red, separated from primary actions. Irreversible actions require a confirmation that names what will be lost. (Principle F: Error Prevention)

9. **Icon + text pairing: icon reinforces, never replaces.** Icon-only buttons are reserved for universally understood actions (close, search, settings). Everything else gets a label. (Ref: `icons.md`)

10. **Consistent sizing across the product.** Pick a size set (e.g., sm/md/lg) and use it everywhere. No one-off padding or font-size tweaks.  (Principle D: Consistency)

11. **Disabled buttons need context.** A greyed-out button with no explanation is a dead end. Show why it's disabled — tooltip, adjacent text, or keep it enabled and validate on click. (Principle E: Affordance)

12. **Semantic HTML: use `<button>`.** Not `<div onclick>`, not `<a>` for actions. If you must use a non-button element, add `role="button"`, `tabindex="0"`, and keyboard event handlers. (Ref: `accessibility.md`)
