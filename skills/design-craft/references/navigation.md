# Navigation

Practical navigation patterns for generating well-structured UI code. Navigation is the skeleton of any interface — get it wrong and nothing else matters.

---

## Navigation Principles

**Core idea:** Navigation exists to answer three questions: "Where am I?", "Where can I go?", "How do I get back?"

- The best navigation is invisible. Users find what they need without thinking about the nav system.
- Navigation structure should mirror user mental models, not org charts or database schemas.
- Limit top-level items to **5-7**. Miller's Law applies directly — more items means more cognitive load, slower decisions.
- Every navigation element must earn its place. If users don't need it frequently, it doesn't belong in primary nav.
- Consistency across pages is non-negotiable. Navigation that shifts layout or order destroys spatial memory.

**Review question:** If you removed all labels, could a returning user still navigate by position and icon alone?

---

## Primary Navigation Patterns

**Core idea:** Pick one primary pattern and commit. The choice depends on platform, section count, and content type.

**Top horizontal nav:**
- Best for **3-7 top-level items**.
- Standard for marketing sites, content sites, and simple apps.
- Breaks down when items exceed the horizontal space — don't wrap nav items to a second line.
- Logo left, nav center or right, actions (login/CTA) far right.

**Side navigation (sidebar):**
- Best for **5-15+ sections** in apps and dashboards.
- Collapsible on smaller screens. Icon-only collapsed state works if icons are unambiguous.
- Standard for B2B/SaaS admin panels, developer tools, data platforms.
- Group related items with section headers. Use dividers sparingly.

**Bottom tab bar:**
- **Mobile only.** 3-5 items. No exceptions.
- Thumb-reachable. The most important destinations only.
- Active tab needs both color and label differentiation.
- Don't hide labels to save space — icon-only bottom bars hurt discoverability.

**Hamburger menu:**
- Hides navigation behind an icon. **Last resort** — reduces discoverability by up to 50%.
- Acceptable on mobile when you have too many items for a bottom bar.
- Never use as sole navigation on desktop. Users expect visible primary nav.

**When to use which:**

| Condition | Pattern |
|---|---|
| < 5 sections, content site | Top nav |
| 5-15 sections, app/dashboard | Sidebar |
| Mobile primary nav | Bottom tabs (3-5 items) |
| Mobile secondary/overflow | Hamburger |
| Complex app on desktop | Sidebar + top bar (hybrid) |

**Review question:** Count your top-level nav items. If it's more than 7, your information architecture needs work before your navigation does.

---

## Breadcrumbs

**Core idea:** Breadcrumbs show where you are in a hierarchy. They answer "How did I get here?" and "How do I go up?"

- Use for **hierarchical content**: e-commerce categories, nested settings, file systems, documentation trees.
- **Don't use** for flat structures or linear flows. Breadcrumbs on a wizard are confusing — use a step indicator instead.
- Show the full path. Make each level clickable except the current page.
- Current page is the last item: not clickable, visually distinct (no link styling, muted color or bold weight).
- Separator: `>` or `/` — either works, stay consistent. Chevron (`>`) is more common.
- Keep labels short. Truncate with ellipsis if a level name is too long.

```
Home > Products > Laptops > MacBook Pro 16"
^clickable  ^clickable  ^clickable   ^current (not linked)
```

**Review question:** Are your breadcrumbs showing the content hierarchy or the user's browsing history? Only the former is correct.

---

## Tabs

**Core idea:** Tabs switch between views of the **same content**. They are not navigation — they are local state toggles.

- Use for related views of a single subject: Overview / Activity / Settings for a user profile, for example.
- **Active tab must be visually obvious.** Underline, background color change, or both. No ambiguity.
- **Max 7 tabs.** More than that means you need a different pattern (sidebar, dropdown, or IA restructure).
- Tabs should **not change the URL** in most cases. They represent local state, not distinct pages.
- Content below tabs should update **without full page reload**. If the page reloads, they're not tabs — they're nav links styled as tabs.
- On mobile: **scrollable tabs** (horizontal swipe) or a dropdown selector. Don't stack tabs vertically.

**Implementation rules:**
- Use `role="tablist"`, `role="tab"`, `role="tabpanel"` for accessibility.
- `aria-selected="true"` on active tab, `aria-selected="false"` on others.
- Keyboard nav: arrow keys move between tabs, Enter/Space activates.

**Review question:** If you deep-linked to a tab, would the URL make sense? If yes, they might actually be navigation, not tabs.

---

## Wayfinding

**Core idea:** Users must always know where they are. If they have to think about it, wayfinding has failed.

- **Current location must always be visible**: active nav item highlighted, breadcrumb present, page title matches nav label.
- Use consistent page titles that match nav labels **exactly**. If the nav says "Settings" the page title must say "Settings" — not "Preferences" or "Configuration."
- Avoid "mystery meat" navigation. Links should describe their destination, not tease it. "Mission Control" tells users nothing. "Dashboard" tells them everything.
- URL structure should mirror navigation structure. Humans read URLs: `/settings/billing` is self-documenting.
- Active state styling: use **two signals minimum** (color + weight, color + underline, background + text change). A subtle color shift alone is not enough.

**Review question:** Cover up all navigation elements. Can you still tell what page you're on from the content alone?

---

## Search

**Core idea:** When navigation fails, search catches. It's the fallback for every user who doesn't think in your categories.

- **Global search must be accessible from every page.** No exceptions.
- Common shortcut: **Cmd/Ctrl+K** for command palette style search. Users increasingly expect this in apps.
- Search results should indicate **which section/category** each result belongs to. Context prevents wrong clicks.
- **Empty state matters:** show recent searches, suggested queries, or popular items. A blank input with a blinking cursor is a dead end.
- For large datasets, provide **filter/faceted search**. Let users narrow by type, date, status, category.
- Search input: always visible or one click away. Don't hide it behind an icon on desktop.
- Debounce search input (**300ms** is standard). Show loading state for queries over 200ms.

**Implementation rules:**
- `role="search"` on the form/container.
- `aria-label="Search"` on the input if there's no visible label.
- Results list uses `role="listbox"` with keyboard navigation (arrow keys, Enter to select).
- Escape key closes search overlay/palette.

**Review question:** Search for the product's most common use case. Does the right result appear in the top 3?

---

## Mobile Navigation

**Core idea:** Mobile navigation must respect the thumb zone and accept that screen space is scarce. Flatten, simplify, prioritize.

- **Bottom tab bar** for primary navigation. 3-5 items. This is the most important nav decision on mobile.
- **Hamburger menu** for secondary/settings navigation that doesn't need constant access.
- **Back button / swipe-back must be consistent.** If back sometimes goes "up" in hierarchy and sometimes goes "back" in history, users lose trust.
- **Sheet/drawer pattern** for contextual actions (share, edit, delete) — not for primary navigation.
- **Avoid deep nesting on mobile.** More than 3 levels means the IA needs flattening. Every extra tap is a drop-off.
- Sticky headers with nav context (page title + back button) help orientation during scroll.
- Touch targets: **minimum 44x44px** (Apple HIG) / **48x48dp** (Material Design). No exceptions.

**Navigation priority on mobile:**

| Priority | Location | Contains |
|---|---|---|
| Primary | Bottom tab bar | 3-5 core destinations |
| Secondary | Hamburger / drawer | Settings, help, account, less-used sections |
| Contextual | Sheet / action menu | Actions related to current content |
| Utility | Top bar | Back button, page title, search, notifications |

**Review question:** Can a user reach the 3 most important features with one tap from any screen?

---

## Navigation Anti-Patterns

**Core idea:** Bad navigation isn't just annoying — it's the number one reason users abandon products. These are the patterns to reject in code review.

- **Hidden navigation with no visible cues.** Relying on gestures alone (swipe to reveal sidebar) with no affordance. If users don't know it exists, it doesn't exist.
- **Clever labels instead of clear labels.** "Mission Control" instead of "Dashboard." "The Lab" instead of "Experiments." Creativity in nav labels costs users time.
- **Inconsistent back behavior.** Sometimes goes up a level, sometimes goes back in browser history, sometimes does nothing. Pick a model and enforce it.
- **Context-shifting navigation.** Nav items that appear, disappear, or reorder based on state without visible indication. Users build spatial memory — don't break it.
- **Deep nesting.** More than 3 levels of navigation hierarchy means the information architecture needs redesign, not deeper menus.
- **Competing navigation systems.** A sidebar, a top nav, a bottom bar, and breadcrumbs all visible at once. Pick a primary system. Support it. Don't stack them.
- **Surprise new tabs.** Links that open in new tabs without indication (`target="_blank"` without warning). External links are the only acceptable case, and even then, indicate it visually.
- **Nav items that are actually actions.** A "Delete Account" button sitting in the nav bar alongside "Home" and "Settings." Navigation takes you places. Actions do things. Don't mix them.

**Review question:** Have a new user try to complete the top 3 tasks. Where do they hesitate? That's where your navigation is failing.

---

## Testing Checklist

Use this to audit any navigation implementation before shipping.

- [ ] Can a new user find the top 3 features within 10 seconds?
- [ ] Is the current location always visible (active state, breadcrumb, page title)?
- [ ] Can users always get back to where they came from?
- [ ] Does the nav structure match how users describe the product's features (not how the database is organized)?
- [ ] On mobile, are primary destinations reachable with one tap from the bottom bar?
- [ ] Does search work and return useful results from every page?
- [ ] Are all touch targets at least 44x44px on mobile?
- [ ] Do nav labels describe destinations clearly (no cleverness, no jargon)?
- [ ] Is keyboard navigation functional for all nav elements?
- [ ] Does the URL structure mirror the navigation hierarchy?
- [ ] Are there fewer than 7 items at any single navigation level?
- [ ] On resize from desktop to mobile, does nav degrade gracefully (not break or disappear)?
