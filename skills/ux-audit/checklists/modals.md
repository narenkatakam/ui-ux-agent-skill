# Modals & Overlays Checklist

Building modals, drawers, popovers, or bottom sheets? Check these before shipping.

---

1. **Use the right overlay type.** Modal for focused tasks/confirmations. Drawer for secondary content or filters. Popover for contextual info. Bottom sheet for mobile actions. Decision tree in `references/overlays.md`. (Ref: `overlays.md`)

2. **Clear title describing the purpose.** "Delete Q4 Report?" not "Confirm action." "Filter transactions" not "Options." The title is the user's first signal of what this overlay is for. (Principle A: Task-First UX)

3. **One primary action + one dismiss action.** "Delete report" + "Cancel." "Apply filters" + close button. Don't pack three CTAs into a modal. (Principle A: Task-First UX)

4. **Focus trapped inside the modal.** Tab cycles through modal elements only — never escapes to the page behind. First focusable element receives focus on open. (Ref: `overlays.md`, `accessibility.md`)

5. **Return focus to trigger on close.** When the modal closes, keyboard focus goes back to the button that opened it. Without this, keyboard users are lost. (Ref: `accessibility.md`)

6. **Close on Escape key.** Always. Non-negotiable. (Ref: `overlays.md`)

7. **Close on overlay/backdrop click — unless destructive.** Clicking outside dismisses for informational overlays. For destructive confirmations or unsaved forms, require explicit action to prevent accidental dismissal. (Principle F: Error Prevention)

8. **Max width constraints.** Simple confirmations: 480px max. Complex content (forms, settings): 640px max. Full-screen only on mobile or for truly immersive flows. (Ref: `overlays.md`)

9. **Scroll inside the modal body, not the page.** Modal content scrolls within its container. The backdrop stays fixed. Page scroll is locked while the modal is open. (Ref: `overlays.md`)

10. **No nested modals.** A modal opening another modal is a UX failure. Rethink the flow — use step progression within the modal, or navigate to a page. (Ref: `overlays.md`)

11. **Mobile: prefer bottom sheet over centered modal.** Bottom sheets are thumb-friendly and feel native on mobile. Centered modals on small screens often have cramped content and awkward dismiss targets. (Ref: `overlays.md`, `responsive-design.md`)

12. **Semantic markup.** `role="dialog"`, `aria-modal="true"`, `aria-labelledby` pointing to the title. These are not optional. (Ref: `accessibility.md`)

13. **Animation: subtle entrance, immediate exit.** Fade + slight scale-up on open (200ms, ease-out). Close should feel instant. Never make users wait for an animation to finish before they can proceed. (Motion Guidance)
