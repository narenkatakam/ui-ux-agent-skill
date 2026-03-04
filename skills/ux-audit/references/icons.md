# Icons

Rules for icon selection, usage, and quality in UI design. Icons supplement text — they rarely replace it.

---

## Hard Rules

These are binary. No exceptions.

- **No emoji as icons.** Emoji renders inconsistently across platforms, lacks semantic precision, and signals amateur design. Use a proper icon from a consistent set.
- **One icon family per product.** No mixing outlined, filled, 3D, and hand-drawn styles. Pick a set, use it everywhere.
- **Icon-only buttons require universal recognition.** Only these icons work without a label: close (X), search (magnifying glass), menu (hamburger), back (arrow), settings (gear), home. Everything else needs a text label.
- **Consistent sizing.** Icons within the same context use the same pixel size. Typical set: 16px (inline), 20px (buttons/nav), 24px (primary actions), 32px+ (feature highlights).
- **Accessible icon buttons.** Every icon-only button must have `aria-label`. Decorative icons adjacent to text must have `aria-hidden="true"`.

---

## Sizing Strategy

Concrete numbers. Stop guessing.

- **Standard size scale:** 12px (micro: badges, status dots) → 16px (inline with text, labels) → 20px (buttons, navigation items) → 24px (primary actions, prominent UI) → 32px (feature highlights, empty states) → 48px+ (hero illustrations, onboarding).
- **Sizing rule:** icon size should match the text size it appears alongside. 16px icon with 14–16px text. 20px icon with 16–18px text. Mismatched sizes create visual tension — the eye catches it even if the brain can't name it.
- **Touch targets vs icon size:** the icon can be 20px but the tap target must be 44px minimum. Add padding to the button, not the icon. Inflating the icon to meet touch target requirements makes it look cartoonish.
- **Consistent context sizing:** all icons in a nav bar = same size. All icons in a toolbar = same size. All icons in a table row = same size. Mixing sizes within a context creates visual noise.

**Quick reference:**

| Context | Icon size | Touch target |
|---|---|---|
| Inline with body text | 16px | n/a |
| Buttons and nav items | 20px | 44×44px min |
| Primary actions | 24px | 48×48px min |
| Feature highlights | 32px | n/a |

---

## SVG vs Font Icons

This is a settled debate. SVG won. But you'll encounter both.

**SVG icons (recommended for modern projects):**

- Tree-shakeable — only ship icons you use.
- Style with CSS (`fill`, `stroke`, `currentColor`).
- Pixel-perfect at every size.
- Each icon is a separate file or component.
- Best with: React (inline SVG or icon component library), Tailwind projects.

**Font icons (legacy, avoid for new projects):**

- Load entire icon set regardless of usage (bundle bloat).
- Can cause FOUT (flash of unstyled text) while font loads.
- Harder to style individual icons.
- Still fine if: you're working with an existing codebase that uses them. Don't rewrite for the sake of it.

**Implementation pattern (React + SVG):**

```jsx
// Using a library like lucide-react
import { Search, Plus, Trash2 } from 'lucide-react'

<Search className="w-5 h-5 text-gray-500" />         // 20px, muted
<Plus className="w-5 h-5" />                           // 20px, inherits color
<Trash2 className="w-4 h-4 text-red-500" />           // 16px, danger
```

**Optical alignment note:** some icons (triangles, arrows, play buttons) appear off-center even when technically centered. Nudge by 1–2px when needed. This is expected, not a bug — it's called optical compensation.

---

## The "Intuitive + Refined" Checklist

Run every icon choice through these checks before committing:

**Intuitive — can users understand it?**

- [ ] Does the icon convey the concept without a label? Test: show it to someone with no context. If they can't guess within 3 seconds, add a label.
- [ ] Is the metaphor culturally stable? A floppy disk for "save" is fading. A mailbox for "inbox" is stable. A phone handset for "call" is stable.
- [ ] Does the icon match the action, not the object? A pencil icon for "edit" (action) is better than a document icon for "edit document" (object).
- [ ] Is it free from ambiguity? A heart could mean "like," "favorite," "health," or "love." If it could mean multiple things in your context, add a label.

**Refined — does it look right?**

- [ ] Consistent stroke width across all icons in the set.
- [ ] Consistent optical size — icons should feel the same weight even if pixel dimensions vary slightly.
- [ ] Clean at target size — no blurry edges, broken strokes, or lost detail at 16-20px.
- [ ] Aligned to the pixel grid. Sub-pixel rendering makes icons look fuzzy.
- [ ] Tested in context: does the icon hold up against the background, adjacent text, and surrounding spacing?

---

## When to Prefer Text Over an Icon

Icons are not always better. Use text when:

- The action is specific and not universally represented. There is no universally understood icon for "Export as CSV" or "Merge duplicates."
- The icon would be ambiguous in context. A gear icon in a settings page is clear; a gear icon inside a card could mean settings, preferences, configuration, or admin.
- Multiple icons compete in a small space. A row of 5 icon-only buttons becomes a guessing game. Add labels or use a menu.
- The user is making an irreversible decision. "Delete" in text is clearer than a trash icon when the stakes are high.
- The target audience includes non-technical users. Icon literacy varies. Text is universal.

**Rule of thumb:** if you need a tooltip to explain the icon, you need a label instead.

---

## Suggested Icon Sets

| Set | Style | License | Notes |
|---|---|---|---|
| **Lucide** | Clean outlined | ISC (open) | Fork of Feather. Active community. Excellent for modern minimal UIs. |
| **Phosphor** | Outlined + filled variants | MIT | 6 weights. Good versatility. Works across styles. |
| **Heroicons** | Outlined + solid | MIT | By Tailwind team. Pairs naturally with Tailwind projects. |
| **Material Symbols** | Variable weight/fill/grade | Apache 2.0 | Google's latest. Flexible but can feel generic. |
| **SF Symbols** | Native Apple style | Apple only | iOS/macOS apps. Not for web. |
| **Remix Icon** | Outlined + filled | Apache 2.0 | Large set. Good coverage. |

**Recommendation:** for most web projects, start with **Lucide** or **Heroicons**. For projects needing extensive coverage (enterprise, dashboards), use **Phosphor** or **Material Symbols**.

---

## Common Icon Mappings

Standard meanings. Deviating from these creates confusion.

| Concept | Icon | Notes |
|---|---|---|
| Close / dismiss | X | Universal. No label needed. |
| Search | Magnifying glass | Universal. No label needed. |
| Menu / navigation | Hamburger (three horizontal lines) | Recognized on mobile. On desktop, prefer visible nav. |
| Settings / preferences | Gear / cog | Universal. |
| User / account | Person silhouette | Use circle avatar when user has a photo. |
| Home | House | Universal. |
| Back / return | Left arrow (or left chevron) | Platform convention: iOS uses chevron, Android uses arrow. |
| Edit | Pencil | Strong convention. Add label if editing a specific type of content. |
| Delete / remove | Trash can | Always add a label for destructive actions. |
| Add / create | Plus | Works icon-only in FABs. Add label in toolbars. |
| Favorite / like | Heart (filled = active) | Can be ambiguous — clarify with label if context is unclear. |
| Bookmark / save | Bookmark ribbon (filled = active) | Don't confuse with "favorite." |
| Share | Share icon (platform-specific) | iOS uses square+arrow-up. Android uses three-dot-connected. Use the platform convention. |
| Download | Down arrow + tray | Distinct from "save." |
| Upload | Up arrow + tray | Distinct from "share." |
| Notifications | Bell | Filled or with badge for unread. |
| Filter | Funnel | Add "Filter" label in dense UIs. |
| Sort | Vertical arrows or bars | Always label with current sort state. |
| Copy | Overlapping rectangles | Weak metaphor — always add a label. |
| Info / help | Circled "i" or question mark | (i) for contextual info, (?) for help. |
| Warning | Triangle with exclamation | Always pair with text. |
| Error | Circle with X or exclamation | Always pair with text. |
| Success | Checkmark (in circle) | Brief display is fine; persistent needs context. |
| Expand / collapse | Chevron (down = collapsed, up = expanded) | Animate the rotation for clarity. |
| Drag handle | Six dots (grip) or three horizontal lines with spacing | Must look distinct from hamburger menu. |
| Calendar / date | Calendar page | Universal. |
| Lock / security | Padlock | Closed = secure. Open = unsecured/unlocked. |

---

## Anti-Patterns

- **Mystery icons:** an icon that requires documentation to understand. If users can't guess it, it's failed.
- **Icon inflation:** adding icons to every list item, menu item, and label "for visual interest." Icons should clarify, not decorate.
- **Mixed metaphor depth:** some icons are detailed illustrations while others are minimal glyphs. This creates visual noise.
- **Platform-wrong icons:** using iOS-style share icon on Android, or Material icons in an Apple ecosystem app. Respect platform conventions.
- **Color-coded icons without text:** relying on icon color to convey state (green check vs. red X) without any text label. Fails for color-blind users.

**Review question:** For every icon in the interface, can you state its meaning in one word? If not, it needs a label or should be replaced with text.
