# Responsive Design

Reference for building interfaces that work across screen sizes. Rules here apply to any UI code generation — CSS, Tailwind, component libraries.

---

## Mobile-First Approach

**Core idea:** Start with the smallest screen. Enhance upward. Never shrink downward.

- Write base styles for mobile. Add complexity at wider breakpoints.
- Mobile constraints force content prioritization. If something doesn't fit on a 375px screen, question whether it belongs at all.
- Information architecture problems surface immediately on small screens — unclear hierarchy, too many competing elements, bloated navigation.
- Mobile-first CSS is smaller and faster. You're adding rules, not overriding them.

```css
/* Right: mobile-first */
.card { padding: 1rem; }
@media (min-width: 768px) { .card { padding: 2rem; } }

/* Wrong: desktop-first */
.card { padding: 2rem; }
@media (max-width: 767px) { .card { padding: 1rem; } }
```

**Review question:** Does the mobile layout contain only what users actually need? If you're hiding half the desktop content on mobile, the desktop version probably has too much.

---

## Breakpoint Strategy

**Core idea:** Breakpoints serve content, not devices. Add a breakpoint where the layout breaks, not where a device spec says to.

- Common reference points:
  - `320px` — small phones (iPhone SE)
  - `375px` — standard phones
  - `768px` — tablets, portrait
  - `1024px` — tablets landscape, small laptops
  - `1280px` — desktop
  - `1440px+` — wide desktop
- Use `min-width` media queries. This is the mobile-first direction.
- Three layout tiers handle most cases: phone (< 768px), tablet (768px-1023px), desktop (1024px+). Don't over-segment.
- Name breakpoints semantically (`--bp-tablet`, `--bp-desktop`), not by device (`--bp-ipad`).
- Tailwind defaults (`sm: 640px`, `md: 768px`, `lg: 1024px`, `xl: 1280px`, `2xl: 1536px`) are reasonable starting points.

```css
/* Three-tier system */
:root {
  --bp-tablet: 768px;
  --bp-desktop: 1024px;
}
```

**Review question:** Can you justify every breakpoint? If removing one doesn't break the layout, remove it.

---

## Fluid Layouts

**Core idea:** Layouts should stretch and compress smoothly between breakpoints, not snap rigidly at fixed widths.

- Use relative units for layout: `%`, `rem`, `em`, `vw`, `vh`. Reserve `px` for borders, shadows, and fine details.
- CSS Grid for page-level and two-dimensional layouts. Flexbox for single-axis alignment and distribution.
- Do not use floats or absolute positioning for layout. Those are legacy patterns.
- Container queries (`@container`) for component-level responsiveness. A card component should adapt to its container, not the viewport.
- Cap content width with `max-width` to prevent unreadable line lengths. Prose tops out around `65ch` or `720px`.
- `clamp()` for fluid typography and spacing that scales without breakpoints:

```css
/* Fluid type: 16px at 320px viewport, 20px at 1280px, no breakpoints needed */
font-size: clamp(1rem, 0.875rem + 0.5vw, 1.25rem);

/* Fluid spacing */
padding: clamp(1rem, 2vw, 3rem);
```

- Use `fr` units in Grid for proportional columns:

```css
/* Collapses gracefully with auto-fit */
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1.5rem;
}
```

- Set `box-sizing: border-box` globally. Always.

**Review question:** Resize the browser slowly from 320px to 1440px. Does the layout ever look broken between breakpoints, or does it flow smoothly?

---

## Touch vs Pointer

**Core idea:** Touch and mouse are fundamentally different input modes. Design for both without making either worse.

- Minimum touch target: **44x44 CSS pixels** (Apple HIG) / **48x48 dp** (Material). This is non-negotiable.
- Minimum gap between interactive elements: **8px**. Cramped buttons cause mis-taps.
- Hover is a **progressive enhancement**. Never gate functionality behind hover states. Tooltips, dropdown menus on hover, hover-to-reveal actions — all need tap/click alternatives on touch.
- Swipe gestures must have visible button alternatives. Not everyone discovers gestures.
- Place primary actions in the **bottom half** of mobile screens — thumb reach zone. Top-of-screen actions are hard to reach one-handed.
- Use `touch-action` CSS to control browser touch behavior (prevent accidental zoom, scroll interference):

```css
.carousel { touch-action: pan-x; }
.map { touch-action: none; }
```

- `pointer` and `hover` media queries detect input capability:

```css
/* Only apply hover effects on devices that support hover */
@media (hover: hover) and (pointer: fine) {
  .button:hover { background: var(--hover-bg); }
}
```

**Review question:** Can every interaction be completed with a thumb on a phone held in one hand? If not, is the tradeoff justified?

---

## Content Adaptation

**Core idea:** Content hierarchy must be preserved across screen sizes. Responsive design is content choreography, not just reflowing boxes.

### Images
- Use `srcset` and `sizes` for resolution switching. Serve appropriately sized images — don't send 2000px images to a 375px phone.
- Use `<picture>` with `<source>` for art direction — different crops per breakpoint.
- Set `max-width: 100%; height: auto;` on images as a baseline.

### Tables
- Wide tables on mobile: choose one strategy per table.
  - **Horizontal scroll** with `-webkit-overflow-scrolling: touch` for data-dense tables.
  - **Stacked layout** — each row becomes a card with label-value pairs.
  - **Priority columns** — show key columns, hide secondary ones behind a toggle.

### Navigation
- **Mobile:** hamburger menu, bottom tab bar, or slide-out drawer.
- **Desktop:** full horizontal nav, sidebar nav, or mega menu.
- Don't show a hamburger menu on desktop. Don't show a 12-item horizontal nav on mobile.

### Forms
- Single column on mobile. Always.
- Multi-column on desktop only when fields are logically grouped (e.g., first name / last name side by side).
- Input fields: full width on mobile, constrained width on desktop.
- Use appropriate input types (`type="email"`, `type="tel"`) to trigger correct mobile keyboards.

### Content Visibility
- Hide secondary content on mobile using `display: none` or conditional rendering — don't just shrink it until it's unreadable.
- Progressive disclosure: show summary on mobile, expand on interaction or on desktop.
- Sidebars become modals or accordions on mobile.

**Review question:** Read the mobile layout as a user. Does the content hierarchy make sense without seeing the desktop version?

---

## Layout Patterns

**Core idea:** There are a handful of proven responsive layout patterns. Pick the one that fits your content structure.

### Mostly Fluid
Multi-column on desktop, single column on mobile. Columns compress then stack.
```
Desktop:  [col] [col] [col]
Tablet:   [col] [col]
          [col]
Mobile:   [col]
          [col]
          [col]
```
Best for: content-heavy pages, blogs, dashboards.

### Column Drop
Columns drop below each other as the viewport narrows. Order matters — most important column stays on top.
```css
.container { display: flex; flex-wrap: wrap; }
.primary { flex: 1 1 60%; }
.secondary { flex: 1 1 40%; }
```

### Layout Shifter
Different layout structures per breakpoint. Not just reflow — genuinely different arrangements.
Best for: marketing pages, complex app layouts where mobile and desktop serve different interaction models.

### Off Canvas
Content panels live off-screen on mobile, slide in on interaction. Sidebar navigation is the classic case.
```css
.sidebar {
  transform: translateX(-100%);
  transition: transform 0.3s ease;
}
.sidebar.open { transform: translateX(0); }
```

**Review question:** Which pattern does your layout use? If you can't name it, the layout probably lacks a coherent responsive strategy.

---

## Performance on Mobile

**Core idea:** Mobile means slower connections, less memory, weaker CPUs. Optimize aggressively.

- Lazy load images below the fold with `loading="lazy"`.
- Prioritize above-the-fold content: critical CSS inline, defer non-critical resources.
- Compress images. Use modern formats (WebP, AVIF) with `<picture>` fallbacks.
- Avoid layout shifts — set explicit `width` and `height` on images and embeds, or use `aspect-ratio`.
- The 300ms tap delay on older mobile browsers: mitigated by `<meta name="viewport" content="width=device-width">` and `touch-action: manipulation`.
- Reduce JavaScript on mobile. Every KB of JS costs more on mobile than desktop — parsing and execution are slower.
- Use `preload` for critical fonts and above-the-fold images.
- Test performance on throttled connections (Chrome DevTools: Slow 3G, Fast 3G).

```html
<!-- Responsive image with lazy loading and modern format -->
<picture>
  <source srcset="hero.avif" type="image/avif">
  <source srcset="hero.webp" type="image/webp">
  <img src="hero.jpg" alt="Hero" loading="lazy" width="1200" height="600">
</picture>
```

**Review question:** Load the page on a throttled 3G connection. Is the core content visible within 3 seconds?

---

## Testing Checklist

**Core idea:** Browser resize is not device testing. Validate on real hardware and real conditions.

### Functional checks
- [ ] Test on at least one real iOS device and one real Android device
- [ ] Verify all breakpoints with actual content, not placeholder text
- [ ] Test both landscape and portrait orientations
- [ ] Confirm no unintentional horizontal scrollbar at any viewport width
- [ ] Touch targets meet 44x44px minimum on actual phones
- [ ] All interactive elements reachable without hover

### Accessibility checks
- [ ] Increase system font size to largest setting — layout must not break
- [ ] Test with screen reader on mobile (VoiceOver on iOS, TalkBack on Android)
- [ ] Verify focus order makes sense at every breakpoint
- [ ] Check color contrast at all sizes (small text needs higher ratio)

### Performance checks
- [ ] Page loads usably within 3s on a 3G connection
- [ ] No layout shifts during load (CLS < 0.1)
- [ ] Images are appropriately sized for each breakpoint (no 2x oversized images on mobile)
- [ ] No render-blocking resources that aren't critical

### Edge cases
- [ ] Test with very long content (names, titles, descriptions)
- [ ] Test with missing content (empty states, no image)
- [ ] Test with browser zoom at 200%
- [ ] Check notch/safe-area handling on modern phones (`env(safe-area-inset-*)`)

**Review question:** Have you tested this on a device you'd actually use, under conditions a real user would face?
