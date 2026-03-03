# Color Systems

Practical reference for building and reviewing color systems in UI. Use this when generating, auditing, or theming interfaces.

---

## Color Palette Structure

**Core idea:** A good color palette is small, intentional, and dominated by neutrals. If it feels like a lot of colors, it's too many.

- **Minimal palette:**
  - Background (page-level canvas)
  - Surface (cards, panels, elevated containers)
  - Text (primary, secondary, disabled)
  - Primary accent (one color for primary actions and key states)
  - Semantic colors: success, warning, error, info
- Each color needs light and dark variants. Use a 50-900 scale (or equivalent 5-6 step range) so you have options for backgrounds, borders, text, and hover states within each hue.
- One accent color for primary actions and key states. This is your brand's handshake — the single color users associate with "do the thing." If you add a second accent, you've split attention.
- **Neutrals (grays) do 80% of the work.** Backgrounds, borders, dividers, secondary text, disabled states, shadows — all gray. A well-built neutral scale (6-10 steps) matters more than a fancy accent palette.
- **Rule:** If you need more than 5-6 intentional colors (excluding neutrals), your design is fighting itself. Step back and ask what's competing for attention.

**Review question:** Can you name every non-neutral color in the palette and state its purpose in one sentence? If not, cut colors until you can.

---

## Semantic Color Tokens

**Core idea:** Never use raw hex/rgb values in production code. Use semantic tokens that describe purpose, not appearance.

- **Token naming:** Describe purpose, not color. `--color-error` not `--color-red`. `--color-text-secondary` not `--color-gray-600`. The moment you rename a token after its appearance, you've locked yourself into a specific palette.
- **Core semantic token set:**

  **Text:**
  - `--color-text-primary` — main body text
  - `--color-text-secondary` — supporting text, labels, captions
  - `--color-text-disabled` — inactive/unavailable text
  - `--color-text-inverse` — text on dark/accent backgrounds

  **Backgrounds:**
  - `--color-bg-primary` — page canvas
  - `--color-bg-secondary` — subtle section differentiation
  - `--color-bg-elevated` — cards, modals, popovers

  **Borders:**
  - `--color-border-default` — standard borders and dividers
  - `--color-border-focus` — focus rings on interactive elements

  **Actions:**
  - `--color-action-primary` — primary buttons, links, key interactive elements
  - `--color-action-hover` — hover state for primary actions
  - `--color-action-disabled` — disabled interactive elements

  **Semantic status:**
  - `--color-success` — positive outcomes, confirmations
  - `--color-warning` — attention needed, review required
  - `--color-error` — errors, destructive states, validation failures
  - `--color-info` — informational, neutral callouts

- **Why this matters:** Semantic tokens are what make theming and dark mode possible without rewriting your UI. Change the token values, not the components. One set of tokens, multiple themes.
- **Implementation:** CSS custom properties at the `:root` level, overridden with `[data-theme="dark"]` or `.dark` class. Tailwind users: map tokens to your config's `colors` object.

**Review question:** If you swapped the entire color palette tomorrow, would you need to touch component code or just token definitions?

---

## Contrast and Accessibility

**Core idea:** If it doesn't pass contrast checks, it doesn't ship. No exceptions for "it looks fine to me."

- **WCAG AA minimums:**
  - Normal text (<18px or <14px bold): **4.5:1** contrast ratio
  - Large text (18px+ or 14px+ bold): **3:1**
  - UI components and graphical objects (icons, borders, form controls): **3:1**
- **Test every text/background combination.** Not just the hero section. Every card, every tooltip, every disabled state, every overlay. Contrast bugs hide in the combinations you didn't check.
- **Gray text on white:** `#767676` is the lightest gray that passes AA on white (`#FFFFFF`). Anything lighter fails. `#999999` on white is a 2.85:1 ratio — that fails. This catches people constantly.
- **Avoid pure white on pure black for body text.** `#FFFFFF` on `#000000` has a 21:1 ratio — maximum contrast, maximum eye strain. Use off-white (`#F5F5F5` or similar) on near-black (`#1A1A1A` or `#121212`) for comfortable reading.
- **Never use color as the sole indicator.** Red/green for pass/fail needs an accompanying icon, text label, or pattern. Approximately 8% of men have red-green color vision deficiency. Design for them.

**Practical rules:**
- Use a contrast checker on every text color + background color pair. Tools: WebAIM Contrast Checker, Chrome DevTools Accessibility panel, Figma plugins (Stark, A11y).
- When in doubt, increase contrast. Users never complain that text is too readable.
- For decorative or non-essential text (watermarks, placeholder-like hints), 3:1 is acceptable, but ask whether you actually need the text at all.

**Review question:** Have you tested the contrast ratio of every text/background combination in the interface — including hover states, disabled states, and overlays?

---

## Dark Mode

**Core idea:** Dark mode is a separate design problem, not a CSS filter. Inverting your light palette will produce something that technically renders but is painful to use.

**Key differences from light mode:**

- **Elevation = lighter surfaces, not shadows.** In light mode, shadows show depth. In dark mode, shadows disappear into the dark background. Instead, use progressively lighter surface colors to show layers: base (`#121212`) < card (`#1E1E1E`) < modal (`#2C2C2C`).
- **Reduce color saturation.** Vivid, fully saturated colors on dark backgrounds cause visual vibration and eye strain. Desaturate your accent and semantic colors by 10-20% for dark mode. A blue that looks great on white will scream on dark gray.
- **Use lighter font weights cautiously.** Thin text (300 weight and below) on dark backgrounds smears on many displays, especially non-retina. Bump body text to regular (400) minimum in dark mode.
- **Increase text-to-background contrast slightly.** Light mode body text might be `#333333` on `#FFFFFF` (12.6:1). Dark mode equivalent should aim for at least the same ratio — `#E0E0E0` on `#121212` or similar.

**Implementation:**

```css
:root {
  --color-bg-primary: #FFFFFF;
  --color-text-primary: #1A1A1A;
  --color-action-primary: #2563EB;
}

[data-theme="dark"] {
  --color-bg-primary: #121212;
  --color-text-primary: #E0E0E0;
  --color-action-primary: #60A5FA; /* desaturated, lighter variant */
}
```

- Toggle with a `data-theme` attribute on `<html>` or `<body>`. Prefer `data-theme` over a class for cleaner specificity.
- **Respect `prefers-color-scheme`** for automatic switching:

```css
@media (prefers-color-scheme: dark) {
  :root {
    --color-bg-primary: #121212;
    --color-text-primary: #E0E0E0;
  }
}
```

- **Test dark mode separately.** Contrast issues, invisible borders, unreadable labels, and clashing saturated colors all hide in dark mode. Run a full contrast audit on the dark palette independently.

**Review question:** Have you tested dark mode as its own design — not just toggled a switch and assumed it works?

---

## Color in Data Visualization

**Core idea:** Data visualization color has stricter rules than UI color. A palette that looks good in a button can be useless in a chart.

- **Sequential palettes** — for ordered/continuous data (revenue over time, temperature). Single hue, light to dark. Users read light = low, dark = high.
- **Diverging palettes** — for data with a meaningful midpoint (profit/loss, above/below average). Two hues diverging from a neutral center. Classic: blue-white-red.
- **Categorical palettes** — for distinct, unordered groups (product categories, user segments). Use maximally distinguishable hues. **Cap at 6-8 colors.** Beyond that, colors become indistinguishable — use labels, patterns, or interactive filtering instead.
- **Color blindness:** Test every data viz palette with a color blindness simulator. Red/green is the most common deficiency (~8% of men). Avoid red-green as your only distinguishing pair. Use blue-orange as a safer default categorical pair.
- **Always add a second channel.** Labels, patterns (hatching, dots), or tooltips alongside color. A chart that only works in color is an incomplete chart.

**Review question:** If you print this chart in grayscale, can you still distinguish the data series?

---

## Color Psychology (Use Sparingly)

**Core idea:** These associations are real but not universal. Use them as sensible defaults, not design doctrine.

- **Red** — error, danger, destructive actions, urgency. Reserve for states that need immediate attention. Don't use red decoratively — it triggers alarm.
- **Green** — success, positive confirmation, completion, "go." Reserve for confirmations and positive states. Don't use green and red as your only status pair (color blindness).
- **Yellow / Amber** — warning, caution, "needs review." Good for states that aren't errors but need attention. Amber backgrounds need dark text (yellow text on white is invisible).
- **Blue** — trust, information, primary actions. The safest accent color. Universally positive associations across cultures. Default to blue when you have no brand color.
- **Gray** — neutral, disabled, secondary, de-emphasized. The workhorse of any UI. A good gray scale handles 80% of the interface's visual weight.

**Rule:** Don't design by color psychology alone. Cultural context varies wildly (white = purity in Western cultures, mourning in some East Asian cultures). When in doubt, test with your actual users.

**Review question:** Is every color choice defensible by function, or are some chosen "because it felt right"?

---

## Common Anti-Patterns

Flag these during review. Each one is a sign of a color system that needs work.

- **Raw hex values scattered through code** instead of semantic tokens. Maintenance nightmare. Theming impossible.
- **Multiple accent colors competing for attention.** If the primary button is blue and the secondary CTA is teal and there's an orange badge and a purple link — nothing is primary.
- **Gray text that fails contrast.** `#999999` on white (`#FFFFFF`) is a 2.85:1 ratio. Fails AA. This is the single most common color accessibility failure.
- **Color as the sole state indicator.** A red border on an error field with no icon, no error message, no `aria-invalid` — invisible to color-blind users and screen readers.
- **Indistinguishable shades of the same hue.** If your palette has `#2563EB` and `#3B82F6` used for different purposes, users won't tell them apart. Especially on cheaper monitors.
- **Neon or highly saturated colors on large surfaces.** A saturated blue link is fine. A saturated blue page background is an assault. Reserve high saturation for small, intentional accents.
- **No dark mode consideration from the start.** Retrofitting dark mode into a codebase built with hardcoded colors is painful and error-prone. Build with tokens from day one.

---

## Testing Checklist

Run this before shipping any interface or approving any color system.

- [ ] **Grayscale test:** Convert the entire screen to grayscale. Can you still understand the hierarchy and identify interactive elements?
- [ ] **Contrast audit:** Every text/background combination meets WCAG AA (4.5:1 for normal text, 3:1 for large text and UI components)?
- [ ] **Color-only indicators:** Is color ever the sole way to convey meaning, status, or state? If yes, add an icon, label, or pattern.
- [ ] **Palette coherence:** Does the palette look intentional — like one designer chose it — or like colors were added ad hoc over time?
- [ ] **Dark mode audit:** Has dark mode been tested independently for contrast, readability, and saturation issues?
- [ ] **Color blindness simulation:** Has the interface been tested with deuteranopia simulation at minimum? (Use Chrome DevTools > Rendering > Emulate vision deficiencies, or Stark plugin in Figma.)
- [ ] **Token coverage:** Are all colors referenced via semantic tokens, or are there raw hex/rgb values in component code?
- [ ] **Accent discipline:** Is there exactly one primary accent color, or are multiple colors competing for the "primary action" role?
