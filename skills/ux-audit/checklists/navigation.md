# Navigation Checklist

Building navigation? Check these before shipping.

---

1. **Current page is clearly indicated.** Active nav item has a distinct visual state — not just a color change, but weight, background, or indicator bar. The user must always know where they are. (Ref: `navigation.md`)

2. **Navigation position is stable across pages.** Same placement, same items, same order on every page. Navigation that moves or changes between pages breaks spatial memory. (Principle D: Consistency)

3. **Max 7±2 top-level items.** More than that and users can't scan. Group related items under dropdowns or secondary nav. Audit ruthlessly — most apps have too many nav items, not too few. (Principle G: Cognitive Load, Ref: `navigation.md`)

4. **Labels are nouns, not actions.** "Projects," "Settings," "Inbox" — not "View projects," "Go to settings." Navigation names things; buttons do things. (Ref: `review-template.md` — Copy gate)

5. **Mobile navigation pattern: pick one.** Bottom tab bar (up to 5 items) or hamburger menu (more items). Not both. Bottom tabs for frequent destinations; hamburger for infrequent. (Ref: `navigation.md`, `responsive-design.md`)

6. **Breadcrumbs for 3+ levels deep.** If the user can drill from Dashboard → Project → Task → Subtask, breadcrumbs show the path and let them jump back. Not needed for flat structures. (Ref: `navigation.md`)

7. **Skip link as first focusable element.** "Skip to main content" link, visible on focus, jumps past the nav. Required for keyboard and screen reader users. (Ref: `accessibility.md`)

8. **Dropdown menus close on outside click and Escape.** Open on click (not hover — hover is unreliable on touch). Close when focus leaves or Escape is pressed. (Principle F: Error Prevention)

9. **Touch targets: 44px minimum height.** Nav items on mobile must be easy to tap. No cramped text links. Adequate spacing between items to prevent mis-taps. (Ref: `responsive-design.md`)

10. **Logo links to home.** Users expect it. Don't break this convention. (Principle D: Consistency)

11. **Search available for content-heavy apps.** If the user might need to find something that isn't in the nav, provide search. Prominent placement — not buried in a submenu. (Principle B: Information Architecture)

12. **Responsive transition is seamless.** Desktop sidebar → mobile hamburger (or vice versa) must preserve the same items in the same logical order. No items disappearing at breakpoints. (Ref: `responsive-design.md`)
