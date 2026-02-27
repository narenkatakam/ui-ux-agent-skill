# Interaction Psychology

Psychological principles that govern how users perceive, decide, and act in interfaces. Organized into four sections: HCI laws, cognitive biases, interaction flow, and attention economy.

---

## A. HCI Laws

### Fitts's Law

**Principle:** Time to reach a target is a function of the distance to the target and the size of the target. Closer + bigger = faster.

**Practical rules:**

- Primary CTAs must be large and close to where the user's pointer or thumb already is.
- On mobile, place primary actions in the thumb zone (bottom half of screen). Top corners are the hardest targets to reach one-handed.
- Infinite-edge targets (elements at screen edges and corners) have effectively infinite size — the cursor stops at the edge. Use this: place nav at edges, not floating in the middle.
- Small, distant, densely packed targets slow users down and increase errors. If users need to click something frequently, make it big and near.
- Minimum interactive target: 44x44 CSS px (web), 48x48 dp (mobile). Padding counts toward hit area.

**Review question:** Is the most frequent action also the easiest to reach and tap?

---

### Hick's Law

**Principle:** Decision time increases logarithmically with the number of choices. More options = slower decisions.

**Practical rules:**

- Keep primary choices to 3-5 options. Beyond 7, use progressive disclosure, categorization, or search.
- Sensible defaults eliminate choice: pre-select the most common option.
- Group related options to reduce perceived count. 12 items in 3 groups of 4 feels faster than 12 flat items.
- For complex selections, offer recommended/popular choices prominently and tuck the full list behind "Show all."
- Applies to navigation, settings, filter panels, pricing tiers, and any selection interface.

**Review question:** How many distinct choices does this screen present simultaneously? Can any be deferred, defaulted, or grouped?

---

### Miller's Law

**Principle:** Working memory holds roughly 7 (plus or minus 2) chunks of information. Beyond that, recall degrades rapidly.

**Practical rules:**

- Chunk information visually: group form fields, cluster related nav items, break long lists with section headers.
- Navigation levels: 5-7 top-level items maximum. More than that and users can't hold the structure in memory.
- If users must compare items, show them side-by-side. Don't force them to remember one screen while looking at another.
- Phone numbers, credit card numbers, and codes: display in groups (555-867-5309, not 5558675309).
- Multi-step flows: show a progress indicator. Users need to know how many chunks remain.

**Review question:** Does the user need to remember anything from a previous screen to complete the current task? If yes, carry that information forward visually.

---

## B. Cognitive Biases

### Anchoring

**Principle:** The first piece of information shapes how users evaluate everything that follows.

**Practical rules:**

- The first number shown in a pricing page anchors perception. Show the premium plan first to make the mid-tier look reasonable.
- Default values anchor expectation. A pre-filled quantity of "1" makes 2-3 feel normal. A pre-filled quantity of "10" shifts the range.
- Empty states anchor first impressions. A thoughtful empty state (example content, quick-start) anchors positively. "No data" anchors negatively.
- Form defaults: anchor toward the safe/common choice, not the choice that benefits the business. Users notice manipulation.

**Review question:** What is the first data point or value the user sees? Does it set a fair and useful anchor?

---

### Default Effect

**Principle:** Users disproportionately stick with whatever is pre-selected. Defaults are the most powerful design lever.

**Practical rules:**

- Defaults should serve the user's most common need. Don't default to the option that maximizes revenue or data collection.
- Opt-in is ethical. Opt-out is manipulative. Don't pre-check marketing consent, data sharing, or subscription auto-renewal.
- Settings pages: set every toggle to the value most users would choose. Let power users customize.
- Form defaults: pre-fill with the most common answer (country, currency, date format) based on context.

**Review question:** If a user accepted every default without changing anything, would the outcome serve their interest?

---

### Peak-End Rule

**Principle:** Users judge an experience primarily by its most intense moment (peak) and its final moment (end), not by the average.

**Practical rules:**

- Invest heavily in the onboarding first-success moment (peak) and the post-task confirmation (end).
- A tedious 10-step form is forgiven if it ends with a satisfying confirmation ("You're all set" + clear next step).
- Error recovery is a potential peak — a well-handled error can create a positive memory. A poorly handled one poisons the entire flow.
- Don't let the experience end on a dead end. Every completion screen should offer a meaningful next action.
- Common violation: a checkout flow that ends with a plain "Order confirmed" page and no clear path forward.

**Review question:** What is the peak moment in this flow? What is the last thing the user sees? Are both handled with care?

---

### Loss Aversion

**Principle:** People feel losses roughly twice as intensely as equivalent gains. Losing $10 hurts more than gaining $10 feels good.

**Practical rules:**

- Frame destructive actions in terms of what will be lost: "You'll lose 3 unsaved changes" is more effective than "Discard changes?"
- Unsaved work warnings: show what will be lost, not just a generic warning.
- Free trial endings: emphasize what features the user will lose, not what the upgrade costs.
- Subscription cancellation: show usage data ("You've created 47 projects here") to make the loss concrete.
- Don't abuse this: loss framing to prevent legitimate cancellation is a dark pattern. Use it for genuine protection, not retention tricks.

**Review question:** When a user is about to lose something (data, access, progress), does the interface make the loss concrete and specific?

---

### Inattentional Blindness

**Principle:** Users focused on a task will miss information that isn't in their attentional spotlight, even if it's visually present.

**Practical rules:**

- Critical warnings placed outside the user's task focus area will be missed. Put errors and warnings inline, near the element that caused them.
- Banner blindness: users ignore top-of-page banners because they've been trained to. For important notices, use inline callouts within the content area.
- Terms, disclaimers, and permission prompts placed below the fold or in small text will not be read.
- System status changes (background save, sync, connection loss) need proactive notification — the user won't check a status bar on their own.

**Review question:** While the user is focused on the primary task, would they notice a critical state change elsewhere on the screen?

---

## C. Interaction Flow

### Interruption Cost

**Principle:** Every interruption breaks the user's flow state. Resuming costs time and cognitive effort. The more complex the task, the higher the cost.

**Practical rules:**

- Modals and alerts during active workflows are expensive interruptions. Use them only for critical decisions or destructive actions.
- Non-critical notifications (success confirmations, tips, promotions) should never interrupt the current task. Use passive indicators: toast in a corner, subtle inline update.
- Multi-step forms: auto-save progress. If the user is interrupted (notification, phone call, accidental back-navigation), their progress must survive.
- Context switches (navigating away and back) must restore state. If a user leaves a half-filled form and returns, it should be exactly as they left it.
- Modals on top of modals: never. If you need a second confirmation inside a modal, the first modal's content is probably too complex.

**Review question:** If the user's phone rings mid-task and they return 5 minutes later, can they resume without starting over?

---

### Action Momentum

**Principle:** Users build momentum during repeated or sequential actions. The interface should support continuous operation without forcing stops.

**Practical rules:**

- Batch operations: let users select multiple items and act on all at once. Don't force item-by-item processing.
- After completing an action, offer the next logical action immediately. "Item saved" should include "Create another" or navigate to the next step.
- Keyboard shortcuts for power users: let them chain actions without reaching for the mouse.
- Confirmation dialogs break momentum. Reserve them for destructive actions only. For reversible actions, execute immediately and offer undo.
- Bulk editing: inline editing is faster than opening a detail view for each item.

**Review question:** For the most frequent workflow, count the clicks/taps. Can any step be eliminated, combined, or made keyboard-accessible?

---

### Reversibility

**Principle:** Actions that can be undone are faster to learn, less stressful to perform, and more efficient than asking for confirmation before every operation.

**Practical rules:**

- Undo is always better than "Are you sure?" Undo lets users act confidently. Confirmation dialogs train users to click "OK" without reading.
- Implement undo for: delete, move, archive, send, status change, bulk operations. Show an undo option for 5-10 seconds after the action.
- Soft-delete by default. Hard-delete only after a grace period (30 days trash) or explicit user request.
- Version history for documents and content: let users browse and restore previous versions.
- When undo is impossible (sending an email, publishing a post, financial transactions), make this clear before the action — and require deliberate confirmation.

**Review question:** For every destructive or significant action, can the user undo it? If not, is the confirmation dialog specific enough to prevent mistakes?

---

## D. Attention Economy

### Visual Weight Budget

**Principle:** Every screen has a limited budget of visual emphasis. Overspending (too many bold/colored/large elements) devalues everything.

**Practical rules:**

- Assign visual emphasis in tiers: one primary focal point, 2-3 secondary elements, everything else is tertiary (muted).
- If everything is bold, nothing is bold. If three things are red, none of them feel urgent.
- Budget check: squint at the screen. The thing that stands out should be the most important thing. If something secondary stands out first, rebalance.
- Adding visual weight to element A means removing it from element B. Emphasis is zero-sum.
- Common violations: multiple colored badges competing with a colored CTA; bold text throughout a list; multiple icons with different colors in a single row.

**Review question:** Squint at the screen. Does the most important element dominate? Are there competing focal points?

---

### Scanning Patterns

**Principle:** Users scan before they read. Scanning patterns determine which content gets seen first.

**Common patterns:**

- **F-pattern:** for text-heavy pages (articles, search results). Users scan the first line fully, then progressively shorter horizontal sweeps. Put key information on the left and in the first two lines.
- **Z-pattern:** for minimal, CTA-driven pages (landing pages, login). Eye moves: top-left logo, top-right nav, diagonal to bottom-left content, bottom-right CTA.
- **Gutenberg diagram:** for evenly distributed content. Strong focus top-left (primary optical area) and bottom-right (terminal area). Weak focus top-right and bottom-left.

**Practical rules:**

- Place the most important content where the scanning pattern puts the user's eye first: top-left for content pages, center for focused actions.
- Left-align labels and content. Centered text breaks scanning rhythm for anything longer than a heading.
- Bold the first few words of paragraphs or list items — scanners read only those.
- Break walls of text with headings, bullet points, and whitespace. Scanning patterns fail on undifferentiated blocks.
- On mobile, scanning is primarily vertical: top-to-bottom with brief horizontal scans. Important content must be near the top.

**Review question:** If a user only spends 3 seconds scanning this screen, what will they take away? Is that the right message?
