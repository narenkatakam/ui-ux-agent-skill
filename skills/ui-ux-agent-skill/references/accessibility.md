# Accessibility Reference

Practical accessibility rules for generating accessible UI code. Not a spec document — a build checklist.

---

## WCAG 2.1 AA Baseline

The standard you must meet. Four principles, each with the rules that matter most in day-to-day UI work.

### Perceivable — users can sense the content

- Text contrast: **4.5:1** minimum for normal text, **3:1** for large text (18px+ regular, 14px+ bold).
- UI component contrast: **3:1** for borders, icons, focus indicators, and other non-text elements against their background.
- Don't convey information through color alone. Pair color with text, icons, or patterns.
- Provide text alternatives for all non-decorative images.
- Captions for video. Transcripts for audio.

### Operable — users can interact with it

- All functionality available via keyboard. No mouse-only interactions.
- No keyboard traps — users can always Tab or Escape out.
- Provide visible focus indicators on every interactive element.
- Give users enough time. Avoid or allow users to extend time limits.
- No content that flashes more than 3 times per second.

### Understandable — users can comprehend it

- Page language declared (`lang` attribute on `<html>`).
- Form inputs have visible labels. Error messages are specific and suggest fixes.
- Navigation is consistent across pages.
- Inputs don't trigger unexpected context changes on focus or input.

### Robust — it works with assistive tech

- Valid, semantic HTML.
- ARIA attributes are correct and complete — wrong ARIA is worse than no ARIA.
- Custom components expose name, role, and state to the accessibility tree.

**Review question:** Can a keyboard-only user with a screen reader complete every task on this page?

---

## Color Accessibility

Color vision deficiency affects ~8% of men and ~0.5% of women. Design for it.

### Rules

- Never use color as the sole indicator of state, error, success, or meaning.
- Always pair color with a secondary signal: icon, text label, pattern, underline, or shape change.
- Error states: red color + error icon + descriptive text. Not just a red border.
- Links in body text: underline them. Don't rely on color difference alone.
- Charts and data viz: use patterns, labels, or distinct luminance values alongside hue.

### Common types to test

- **Deuteranopia** — reduced green sensitivity. Most common. Red/green look similar.
- **Protanopia** — reduced red sensitivity. Red appears dark/muddy.
- **Tritanopia** — reduced blue sensitivity. Rare. Blue/yellow confusion.

### Tools

- Chrome DevTools: Rendering panel > Emulate vision deficiencies.
- Firefox: Accessibility panel > Simulate.
- Figma: plugins like Color Blind or Stark.

**Review question:** If this UI were rendered in grayscale, could the user still distinguish every state and meaning?

---

## Keyboard Navigation

If it doesn't work with a keyboard, it doesn't work.

### Rules

- Every interactive element (links, buttons, inputs, selects, custom controls) must be reachable via `Tab`.
- Focus order must match the visual/logical reading order. Don't use positive `tabindex` values — they break natural order. Use `tabindex="0"` to make custom elements focusable, `tabindex="-1"` for programmatic focus only.
- **Visible focus indicators are mandatory.** Never write `outline: none` without providing a replacement. Use `outline`, `box-shadow`, or a visible border change. Minimum 3:1 contrast for the focus indicator itself.
- Skip navigation: add a "Skip to main content" link as the first focusable element on content-heavy pages. Hidden until focused.
- `Escape` must close modals, popovers, dropdowns, and overlays.
- Arrow keys for composite widgets:
  - **Tabs:** Left/Right arrows move between tabs. Tab key moves focus out of the tab list.
  - **Radio groups:** Arrow keys cycle through options. Tab moves past the group.
  - **Menus:** Arrow keys navigate items. Enter/Space activates. Escape closes.
- **Modal focus trapping:** When a modal opens, move focus to the first focusable element inside it. Tab and Shift+Tab must cycle within the modal only. On close, return focus to the trigger element.

### Implementation pattern — skip link

```html
<a href="#main-content" class="skip-link">Skip to main content</a>
<!-- ... header/nav ... -->
<main id="main-content" tabindex="-1">
```

```css
.skip-link {
  position: absolute;
  top: -100%;
  left: 0;
  z-index: 100;
}
.skip-link:focus {
  top: 0;
}
```

**Review question:** Unplug the mouse. Can you complete every flow using only Tab, Shift+Tab, Enter, Space, Escape, and arrow keys?

---

## Screen Readers

Semantic HTML does 80% of the work. ARIA handles the rest. Using ARIA to fix what semantic HTML would have handled for free is a code smell.

### Semantic HTML first

- Use `<button>` for actions, not `<div onclick>`.
- Use `<a href>` for navigation, not `<span onclick>`.
- Use `<nav>`, `<main>`, `<header>`, `<footer>`, `<aside>`, `<section>` for landmarks.
- Use `<ul>`/`<ol>` for lists. Screen readers announce "list, 5 items" — that context matters.
- Use `<table>` with `<th>` for tabular data. Not CSS grid pretending to be a table.

### ARIA — when and how

Use ARIA only when native HTML semantics are insufficient (custom components, dynamic UI).

- **`aria-label`** — labels an element when no visible text exists. Use on icon buttons: `<button aria-label="Close">`.
- **`aria-labelledby`** — points to the ID of another element that serves as the label. Preferred over `aria-label` when visible label text exists.
- **`aria-describedby`** — provides supplementary description. Use for error messages, help text, format hints. Not a replacement for labels.
- **`aria-hidden="true"`** — removes an element from the accessibility tree. Use on decorative icons, redundant text. Never use on interactive elements.
- **`aria-expanded`** — indicates if a collapsible section, dropdown, or accordion is open (`true`) or closed (`false`).
- **`aria-live`** — announces dynamic content changes. `polite` waits for the user to finish. `assertive` interrupts immediately. Use `polite` for most cases (status messages, search results count). Use `assertive` for critical alerts only.
- **`role`** — defines what an element is. `role="alert"`, `role="dialog"`, `role="tablist"`, `role="tab"`, `role="tabpanel"`. Don't override native roles (don't put `role="button"` on a `<button>`).

### Heading hierarchy

- One `<h1>` per page.
- Never skip levels: `h1 > h2 > h3`. Don't jump from `h2` to `h4`.
- Use headings to create a scannable document outline, not for visual sizing. Style with CSS.

### Alt text

- **Informational images:** Describe what the image conveys. "Bar chart showing revenue growth from $2M to $5M over Q1-Q4." Not "chart" or "image of chart."
- **Decorative images:** `alt=""` (empty alt). The image is skipped by screen readers. Never omit the `alt` attribute entirely — that causes screen readers to read the filename.
- **Images of text:** Avoid. If unavoidable, the alt text must contain the full text in the image.
- **Icons with adjacent text:** `aria-hidden="true"` on the icon. The text is the label.
- **Icon-only buttons:** `aria-label` on the button. `aria-hidden="true"` on the icon.

**Review question:** With the screen turned off, does the screen reader convey the structure, state, and purpose of every element?

---

## Forms

Forms are where most accessibility failures happen. Every field needs a label, every error needs context.

### Rules

- Every `<input>`, `<select>`, and `<textarea>` must have a visible `<label>` with a matching `for`/`id` pair.
- **Placeholder is not a label.** Placeholder text disappears on focus, fails contrast requirements, and isn't reliably announced by all screen readers. Use it for format hints only, alongside a real label.
- Required fields: indicate both visually (asterisk, "required" text) and programmatically (`aria-required="true"` or `required` attribute).
- Error messages: display adjacent to the field, associated via `aria-describedby`. Be specific — "Email must include @" not "Invalid input."
- Group related fields (e.g., address, payment details) with `<fieldset>` and `<legend>`.
- Don't auto-advance focus between fields (e.g., splitting phone number into 3 inputs that auto-tab). It disorients keyboard and screen reader users.
- Submit buttons must be clearly labeled. "Submit" is acceptable. "Go" or an icon alone is not.

### Implementation pattern — accessible form field with error

```html
<div>
  <label for="email">Email address <span aria-hidden="true">*</span></label>
  <input
    id="email"
    type="email"
    aria-required="true"
    aria-invalid="true"
    aria-describedby="email-error"
  />
  <p id="email-error" role="alert">Enter a valid email address.</p>
</div>
```

**Review question:** Can a screen reader user identify every field's purpose, required status, and any errors without seeing the screen?

---

## Touch and Pointer

Mobile is not an afterthought. Half your users are on touch devices.

### Rules

- **Minimum touch target size:** 44x44 CSS pixels (WCAG 2.5.5). Material Design recommends 48x48dp. When in doubt, go bigger.
- **Spacing between targets:** At least 8px gap between adjacent touch targets to prevent mis-taps. More for critical actions (e.g., delete).
- **No hover-dependent information.** Tooltips, hover cards, and title attributes are invisible on touch devices. If the information matters, make it available through tap, a visible label, or always-visible text.
- Support both `click` and `touch` events. Modern browsers unify these well — use `click` handlers (they fire on tap). Avoid `mouseenter`/`mouseleave` for essential interactions.
- Ensure swipe gestures have a single-pointer alternative. Not everyone can swipe — provide buttons too.
- Pinch-to-zoom must not be disabled. Never use `user-scalable=no` or `maximum-scale=1` on the viewport meta tag.

**Review question:** Can someone with large fingers and no hover capability use every control without frustration?

---

## Media and Content

### Images

- Informational: descriptive `alt` text.
- Decorative: `alt=""`.
- Complex (charts, diagrams): `alt` with a summary, plus a longer description in adjacent text or `aria-describedby` linking to a detailed description.

### Video

- Captions: synchronized, accurate, include speaker identification and relevant sound effects.
- Transcripts: full text alternative for users who can't watch.
- Audio descriptions: describe important visual information not conveyed in dialogue (where feasible).

### Audio

- Provide a transcript.
- Don't autoplay audio. If unavoidable, provide an immediately accessible pause/stop control.

### Animations

- Respect `prefers-reduced-motion`. Check the media query and disable or reduce animations.

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

- No auto-playing carousels, animations, or videos without a pause control.
- Avoid parallax scrolling effects or provide a way to disable them.

**Review question:** If the user has disabled animations, muted audio, and can't see video — can they still access all the content?

---

## Testing Checklist

Run these checks before shipping. Automated tools catch ~30% of issues. The rest requires manual testing.

### Automated (run first, fix the obvious)

- [ ] **axe DevTools** browser extension — scan page, fix all critical and serious issues.
- [ ] **Lighthouse** accessibility audit — aim for 90+ score. Note: a perfect score does not mean accessible.
- [ ] **eslint-plugin-jsx-a11y** (React) or equivalent linting — catch issues at build time.

### Keyboard (5 minutes, catches the most impactful bugs)

- [ ] Tab through the entire page. Every interactive element reachable?
- [ ] Focus order logical? No jumps to unexpected locations?
- [ ] Focus indicators visible on every element?
- [ ] Modals trap focus? Escape closes them? Focus returns to trigger?
- [ ] Skip link works and targets main content?

### Screen reader (10 minutes, the real test)

- [ ] Turn on VoiceOver (Cmd+F5 on Mac) or NVDA (Windows). Navigate the page.
- [ ] Headings create a logical outline? (VoiceOver: Ctrl+Option+Cmd+H to cycle headings)
- [ ] Images announced with meaningful alt text or correctly skipped?
- [ ] Form fields announce their label, required status, and errors?
- [ ] Dynamic content changes announced via live regions?
- [ ] Buttons and links announce their purpose?

### Visual

- [ ] All text/background combinations meet contrast ratios (4.5:1 normal, 3:1 large).
- [ ] Color is never the only differentiator.
- [ ] Zoom page to 200% — layout still functional? No content clipped or overlapping?
- [ ] Test with browser color blindness simulation (Chrome DevTools > Rendering).

### Touch

- [ ] Touch targets meet 44x44px minimum.
- [ ] No essential hover-only interactions.
- [ ] Pinch-to-zoom not disabled.

---

## Quick Decision Rules

| Situation | Do this |
|---|---|
| Building a clickable element | Use `<button>` or `<a href>`. Not a `<div>`. |
| Icon with no text | Add `aria-label` to the parent interactive element. |
| Decorative image | `alt=""`. Not `alt="image"`. Not missing `alt`. |
| Dynamic content update | Add `aria-live="polite"` to the container before content changes. |
| Custom component | Expose `role`, `aria-expanded`, `aria-selected`, keyboard handlers. |
| Form validation error | Associate error text with `aria-describedby`. Use `aria-invalid="true"`. |
| Modal opening | Move focus in. Trap it. Return it on close. |
| Color indicating state | Add a non-color signal: icon, text, pattern, or shape. |
| Hiding something visually but keeping it for screen readers | Use a visually-hidden CSS class, not `display: none`. |
| Hiding something from screen readers but keeping it visual | Use `aria-hidden="true"`. |
