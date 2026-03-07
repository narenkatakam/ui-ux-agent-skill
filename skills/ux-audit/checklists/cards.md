# Cards Checklist

Building cards? Check these before shipping.

---

1. **Each card has one clear purpose.** A card represents one object or one decision. If a card tries to do two things, split it. (Principle A: Task-First UX)

2. **One primary action per card.** If the card is clickable, the whole card is the target — not a small link buried inside. Secondary actions (menu, bookmark) sit in a corner and don't compete. (Principle E: Affordance)

3. **Consistent internal spacing.** Same padding, same gap between title/body/footer across all cards of the same type. Use the spacing scale (4/8/12/16/24). (Spacing Rules)

4. **Visual grouping through spacing, not borders.** Tight spacing within a card, loose spacing between cards. If you need an inner border to separate sections, your spacing is wrong. (Principle H: CRAP — Proximity)

5. **Surface treatment: pick one style and repeat it.** Shadow or border, not both. Same border-radius across all cards. Max shadow: `0 1px 3px rgba(0,0,0,0.1)`. (Modern Minimal Style)

6. **Hover state signals interactivity.** If the card is clickable, it needs a visible hover change — subtle shadow lift, background tint, or border color shift. No hover = not clickable. (Principle E: Affordance)

7. **Image handling: locked aspect ratio + fallback.** Images inside cards must not stretch or collapse. Use `object-cover` with a fixed aspect ratio. Provide a placeholder for missing images. (Ref: `ui-states.md`)

8. **Content truncation with escape hatch.** Long titles truncate with ellipsis. Full text available on hover (tooltip) or click-through. Never let content overflow and break the card layout. (Principle G: Cognitive Load)

9. **Responsive: grid on desktop, stack on mobile.** Cards should use a responsive grid (`grid-cols-1 md:grid-cols-2 lg:grid-cols-3`). On mobile, full-width cards stacked vertically. (Ref: `responsive-design.md`)

10. **Handle varying content length.** Cards in a row should have equal height (CSS grid handles this). A card with one line of text shouldn't look broken next to one with three lines. (Principle H: CRAP — Alignment)

11. **Accessibility: semantic markup.** If the whole card is a link, use `<a>` wrapping the card content. Avoid nested interactive elements (link inside a link). Screen readers should announce the card's purpose. (Ref: `accessibility.md`)

12. **Empty collection state.** When there are zero cards to show, display a meaningful empty state — not a blank grid. "No projects yet. Create your first project." (Ref: `ui-states.md`)
