# Typography

Practical typography rules for generating well-crafted UI code. Every decision here affects readability, hierarchy, and trust.

---

## Type Scale

**Core idea:** Use a mathematical ratio to generate font sizes. Never pick sizes arbitrarily.

- Base size: **16px** (browser default, accessibility baseline).
- Use **rem** for all font sizes. Respects the user's browser settings.
- Limit to **5-6 sizes** in your scale. More than that creates visual noise.

**Recommended scales:**

| Scale | Ratio | Sizes (px) | Best for |
|---|---|---|---|
| Minor Third | 1.2 | 12 / 14 / 16 / 19 / 23 / 28 | Dense UIs, dashboards, data-heavy apps |
| Major Third | 1.25 | 12 / 14 / 16 / 20 / 25 / 31 | Most apps (balanced default) |
| Perfect Fourth | 1.333 | 12 / 14 / 16 / 21 / 28 / 37 | Content sites, marketing pages |

**Review question:** Can you list every font size in the UI? If the answer is "not sure," the scale is broken.

---

## Font Pairing

**Core idea:** Fewer typefaces, better typography. Constraint breeds coherence.

- **2 typefaces maximum.** One is often enough.
- Classic pairing: sans-serif headings + serif body (or vice versa).
- If using one family, differentiate with **weight**, not size alone.
- System font stacks are fast and native-feeling:
  ```
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
  ```

**Good default pairs:**

| Headings | Body | Notes |
|---|---|---|
| Inter | Source Serif Pro | Modern sans + readable serif |
| Manrope | Newsreader | Geometric sans + editorial serif |
| System UI | System UI | Zero load time, native feel |

- Avoid decorative or display fonts for body text. They exist for signage, not paragraphs.

**Review question:** Remove all color and imagery. Does the typography alone communicate hierarchy and brand?

---

## Line Height (Leading)

**Core idea:** As font size increases, line-height ratio decreases.

| Context | Line-height | Example |
|---|---|---|
| Body text | 1.4 - 1.6 | 1.5 is the safe default |
| Headings | 1.1 - 1.3 | Large text needs less leading |
| Small text / captions | 1.4 - 1.5 | Keeps small text legible |

- Fixed line heights (px or rem) are more predictable than unitless values for component design.
- Unitless values are better for inherited styles across a page.

**Review question:** Does body text feel spacious enough to read comfortably, and do headings feel tight and cohesive?

---

## Line Length (Measure)

**Core idea:** If lines are too long, readers lose their place. Too short, rhythm breaks.

- Optimal: **45-75 characters** per line for body text.
- Set `max-width` on text containers: roughly `65ch` or `600-700px` for body.
- Headings can run wider. Captions should run narrower.
- Never let body text fill the full viewport width on desktop.

```css
.prose {
  max-width: 65ch;
}
```

**Review question:** On a wide monitor, does body text stretch beyond comfortable reading width?

---

## Letter Spacing

**Core idea:** Leave it alone unless you have a specific reason to change it.

| Context | Spacing | Why |
|---|---|---|
| Body text | `0` (default) | Typefaces are designed for their native spacing |
| ALL CAPS text | `0.05 - 0.1em` | Compensates for reduced readability of uppercase |
| Large headings (40px+) | `-0.01 to -0.02em` | Tightens visual gaps at large sizes |

- Do not manually letter-space body text. It is almost always wrong.

**Review question:** Is letter-spacing being used to fix a problem that a better font choice would solve?

---

## Font Weight

**Core idea:** Weight creates hierarchy. If everything is bold, nothing is bold.

- Use **2-3 weights** from the same family: Regular (400), Medium (500), Bold (700).
- Semi-bold (600) is useful as a middle step when 400-to-700 feels too abrupt.
- Do not use Light (300) for body text. It fails contrast checks on many screens.
- Loading more than 3 weights adds page weight for diminishing returns.

**Review question:** Remove all size differences. Can you still distinguish hierarchy by weight alone?

---

## Hierarchy Rules

**Core idea:** Title > Subtitle > Body > Caption must be distinguishable by typography alone.

- Size difference between adjacent levels: **at least 20-25%** (one step on the type scale).
- Use **size changes** to separate levels.
- Use **weight changes** for emphasis within a level.
- Color and opacity can supplement hierarchy but must not be the primary differentiator.
- If squinting at the page doesn't reveal a clear visual order, the hierarchy is insufficient.

**Review question:** Can a new user identify what to read first, second, and third within 2 seconds?

---

## Responsive Typography

**Core idea:** Typography must adapt to the viewport without breaking readability.

- Use `clamp()` for fluid sizing:
  ```css
  font-size: clamp(1rem, 2.5vw, 1.5rem);
  ```
- Headings should **scale down** on mobile, not just wrap to more lines.
- Body text stays at **16px minimum** on all screens. Non-negotiable for accessibility.
- Line length must adjust with viewport. Use `max-width`, not just `width`.
- Test at 320px wide (small phones) and 1440px+ (large monitors).

**Review question:** Does body text remain readable and headings remain impactful from 320px to 1440px?

---

## Common Anti-Patterns

Things that look wrong immediately to a trained eye:

- **Body text below 14px.** Fails accessibility for many users.
- **More than 3 font weights** on a page. Signals indecision, not design.
- **Mixing more than 2 typefaces.** Every additional font is a liability.
- **Using px for font sizes.** Doesn't respect user preferences. Use rem.
- **Centered body text.** Fine for short headings. Terrible for paragraphs.
- **Justified text on the web.** Creates rivers of whitespace without proper hyphenation support.
- **Using placeholder text styling for actual content decisions.** Lorem ipsum won't reveal real-world line-break problems.
- **Decorative fonts for UI text.** Display faces belong in hero sections, not navigation.

---

## Testing Checklist

Run through these before shipping:

- [ ] Can you identify the hierarchy (title > subtitle > body > caption) at a glance?
- [ ] Is body text readable without zooming on a phone?
- [ ] Are line lengths comfortable (not too wide on desktop, not too narrow on mobile)?
- [ ] Does the type scale feel consistent, or are there arbitrary sizes?
- [ ] Does the typography work with the user's system font size doubled?
- [ ] Are all font sizes in rem, not px?
- [ ] Are you loading 3 or fewer font weights?
- [ ] Does ALL CAPS text have added letter-spacing?
- [ ] Is body text left-aligned (not centered or justified)?
