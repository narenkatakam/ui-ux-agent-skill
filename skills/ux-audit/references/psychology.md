# Psychology

Psychological foundations for UI/UX design. Two lenses: **design foundations** (Norman's diagnostic framework — why interfaces fail) and **interaction psychology** (HCI laws, cognitive biases, flow, and attention — how users perceive, decide, and act).

---

## Part 1: Design Foundations

Core concepts from Norman's *The Design of Everyday Things*. These are the diagnostic lenses used during design review — they explain *why* an interface fails, not just *what* looks wrong.

### Affordances

**Definition:** Properties of an object that determine how it can be used. A flat surface affords pushing. A handle affords pulling. A button affords pressing.

**In UI:**

- A raised, shaded rectangle affords clicking. A flat, borderless label does not.
- A text field with a visible border and cursor affords typing. A bare div does not.
- A draggable handle (grip dots, drag icon) affords drag-and-drop. An element without one does not.
- Affordances are relational — they exist between the user and the object. A swipeable card affords swiping only if the user knows swiping is possible.

**Rules:**

- Real affordances must be visible. A hidden affordance (swipe-to-delete without any hint) might as well not exist.
- False affordances (decorative elements that look interactive but aren't) erode trust. If it looks clickable, it must be clickable.
- Anti-affordances are deliberate: a grayed-out, non-pointer button signals "you cannot do this."

**Review question:** For every interactive element, does its visual form clearly suggest how to interact with it?

---

### Signifiers

**Definition:** Visual cues that communicate *where* to act and *how* to act. Signifiers make affordances visible.

**In UI:**

- Underlined blue text signifies "this is a link."
- A chevron (>) on a list row signifies "tap to navigate deeper."
- A cursor change to `pointer` signifies interactivity. To `grab` signifies draggability.
- A text label on a button signifies the action: "Save," "Delete," "Continue."
- Placeholder text in an input signifies what to type.

**Rules:**

- Every affordance needs at least one signifier. An unlabeled icon button relies on the icon alone — that's weak signifying unless the icon is universal (X for close, magnifying glass for search).
- Signifiers must be perceivable by all users: visible (sighted users), announced (screen reader users), and discoverable (keyboard users through focus states).
- Redundant signifiers strengthen perception: a primary button uses color (blue) + label ("Save") + position (bottom-right) + cursor (pointer). Any one could be missed; together they're unmistakable.
- Weak signifiers: subtle color differences, hover-only reveals, thin underlines on light backgrounds. If users need to squint or guess, the signifier has failed.

**Quick check:**

- [ ] Every clickable element has `cursor: pointer` and a visible hover/focus state.
- [ ] Buttons look like buttons: distinct from surrounding content through color, border, or elevation.
- [ ] Links are visually distinct from non-linked text: underlined, colored, or both.
- [ ] Disabled elements look disabled: reduced opacity, no pointer events, and ideally a tooltip explaining why.
- [ ] Primary actions are visually dominant. Secondary actions are visually recessive. Destructive actions are cautious (not large and red).
- [ ] Icon-only buttons are reserved for universal icons (close, search, settings, menu, back, home). All others have a text label.
- [ ] Drag handles are visually distinct from decoration.
- [ ] Non-interactive elements do not look interactive: no underlines on plain text, no hover effects on static content, no pointer cursor on read-only fields.
- [ ] Form inputs have visible borders or backgrounds that distinguish them from labels and static text.
- [ ] Toggle states are unambiguous: "On"/"Off" labels, filled vs. outlined icons, or explicit text.

**Review question:** Remove all hover effects. Can users still identify every interactive element and predict what it does?

---

### Mapping

**Definition:** The relationship between controls and their effects. Good mapping makes the connection obvious. Bad mapping requires instructions.

**In UI:**

- A slider that moves left-to-right to increase a value: natural mapping.
- A toggle switch that flips to the right for "on": natural mapping.
- A dropdown at the top of the page that filters a table at the bottom: weak mapping (control is distant from effect).
- A "Settings" icon that opens a panel controlling a completely different section: broken mapping.

**Rules:**

- Controls should be spatially close to what they affect. A filter should sit above or beside the content it filters. An edit button should sit on or adjacent to the editable content.
- The direction and scale of control movement should match the direction and scale of the effect. Drag right = increase. Scroll down = more content. Pull down = refresh.
- When spatial mapping is impossible, use visual connection: highlight the affected area when a control is activated, draw a line between control and target, or animate the transition to show causality.
- Labels compensate for weak mapping but don't fix it. "This dropdown controls the chart below" is a confession, not a solution.

**Review question:** For every control, can the user predict what it affects without reading a label?

---

### Constraints

**Definition:** Limitations that guide users toward correct actions and prevent incorrect ones. The most elegant form of error prevention.

**Types:**

- **Physical constraints** (UI equivalent: input masks, disabled states, max-length). A date picker constrains input to valid dates. A character counter constrains length.
- **Semantic constraints** (meaningful restrictions). A "reply" button only appears on messages, not on user profiles. Context constrains available actions.
- **Cultural constraints** (conventions). Red means danger/stop. Green means success/go. X means close. These are learned, not inherent — and they vary by culture.
- **Logical constraints** (only one possibility remains). In a wizard, completing step 2 enables step 3 and disables step 1. The flow constrains sequence.

**Rules:**

- Prefer constraints over instructions. A dropdown prevents typos better than a text field with validation rules listed beside it.
- Make constraints visible: show why something is disabled (tooltip on disabled button), show remaining capacity (character count), show valid ranges.
- Don't over-constrain. If 90% of users need free text and only 10% make errors, a text field with validation is better than a restrictive dropdown.
- Invisible constraints frustrate: a form that silently truncates input, a button that doesn't respond without explanation, a text field that rejects input with no message.

**Review question:** Where can users make errors? For each case, is there a constraint that prevents the error, or are you relying on the user to read instructions?

---

### Conceptual Model

**Definition:** The user's mental picture of how the system works. It doesn't need to match the actual implementation — it needs to be consistent, predictable, and useful.

**In UI:**

- A "folder" metaphor for organizing files: users expect nesting, moving, renaming.
- A "cart" metaphor for e-commerce: users expect adding, removing, seeing totals, and checking out.
- A "canvas" metaphor for design tools: users expect panning, zooming, placing objects, and layers.

**Rules:**

- Choose a conceptual model early and commit to it. Mixed metaphors confuse: if items are "cards" in one view and "rows" in another, the model is fractured (this is different from concept constancy — this is about the system metaphor, not individual concept naming).
- The model must support user predictions. If the model says "drafts auto-save," then hitting the back button must not lose work.
- When the system's behavior deviates from the model, explain it. "Unlike regular folders, shared folders sync across all members" acknowledges the deviation.
- Simple models are better than accurate ones. Users don't need to understand the database schema — they need a mental picture that lets them predict what happens next.
- Onboarding should teach the conceptual model, not the feature list. "Think of it as a shared notebook" is more useful than a feature tour.

**Review question:** Can you describe this product's conceptual model in one sentence? Can a new user form that model within their first session?

---

### Feedback

**Definition:** Information the system provides about the result of an action and the current system state. Without feedback, users operate blind.

**In UI:**

- A button that changes state on click: immediate feedback.
- A loading spinner after form submission: process feedback.
- A success toast after saving: outcome feedback.
- A progress bar during upload: progress feedback.

**Types and timing:**

| Feedback type | Timing | Example |
|---|---|---|
| **Immediate** | < 100ms | Button press state, hover effect, input character appearing |
| **Process** | 100ms - 10s | Loading spinner, skeleton screen, progress bar |
| **Outcome** | After completion | Success/error message, state change, navigation |
| **Persistent** | Ongoing | Status badges, save state indicator, connection status |

**Rules:**

- Immediate feedback is non-negotiable. If a button doesn't visually respond to a click within 100ms, users will click again — causing double submissions.
- The form of feedback should match the importance of the action. A "like" button needs a subtle animation. A payment confirmation needs a prominent success state.
- Absence of feedback is negative feedback. If a user performs an action and nothing changes, they assume it failed.
- Over-feedback is noisy. Not every hover needs an animation. Not every save needs a toast. Match feedback volume to action significance.

**Review question:** Perform every action in the flow with your eyes closed for 1 second after clicking. When you open your eyes, can you tell what happened?

---

### Gulf of Execution

**Definition:** The gap between what a user wants to do and what the interface allows them to figure out. A wide gulf means users can't figure out how to accomplish their goal.

**Symptoms:**

- User stares at the screen, not knowing where to start.
- User tries multiple wrong approaches before finding the right one.
- User asks "How do I...?" — the action exists but isn't discoverable.

**Causes:**

- Missing signifiers: the button exists but doesn't look like a button.
- Poor labeling: the action is labeled with internal jargon the user doesn't know.
- Hidden functionality: the feature is behind a menu, a long-press, or a gesture with no visual hint.
- Wrong conceptual model: the user's mental model doesn't match the system's interaction model.

**Fixes:**

- Make all available actions visible (or at least discoverable through progressive disclosure).
- Use labels users understand — test with real users, not team consensus.
- Provide clear starting points: "Create your first project" rather than an empty screen.
- Match interaction patterns to what users already know from similar products.

**Review question:** If a user knows *what* they want to do, can they figure out *how* to do it within 10 seconds?

---

### Gulf of Evaluation

**Definition:** The gap between the system's actual state and the user's ability to perceive that state. A wide gulf means users can't tell what happened or what the current situation is.

**Symptoms:**

- User asks "Did it save?" — the system gave no confirmation.
- User can't tell if a toggle is on or off — the visual distinction is too subtle.
- User doesn't notice a status change — it happened outside their attention.
- User misinterprets system state — the indicators are ambiguous.

**Causes:**

- Missing feedback: actions produce no visible response.
- Ambiguous indicators: is gray the active state or the inactive state? Is the toggle on when it's left or right?
- Delayed feedback: changes happen after a lag that breaks the cause-effect connection.
- Displaced feedback: the effect appears far from the cause (clicking a button at the top changes data at the bottom, off-screen).

**Fixes:**

- Ensure every action produces immediate, visible feedback near the point of action.
- Use unambiguous state indicators: labels on toggles ("On"/"Off"), filled vs. outlined icons for selected vs. unselected, color + text for status.
- When feedback must be displaced (global notification, remote update), use animation to connect cause and effect — the item flies to the cart, the number badge pulses.

**Review question:** At any given moment, can the user accurately describe the current state of the system? Can they tell the difference between "loading," "empty," "error," and "success"?

---

### Slips vs. Mistakes

**Definition:** Two fundamentally different types of human error, requiring different design responses.

#### Slips

The user intends the right action but executes it wrong. They know what to do; their fingers betray them.

- **Examples:** tapping "Delete" instead of "Edit" (adjacent buttons), typing "teh" instead of "the," accidentally swiping away a notification.
- **Causes:** adjacent targets, similar-looking actions, motor imprecision, speed.
- **Fixes:** increase target size, separate destructive actions from common actions, add undo, use confirmation only for irreversible destructive slips.

#### Mistakes

The user intends the wrong action because they misunderstand the situation. The execution is fine; the plan is wrong.

- **Examples:** filling "billing address" in the "shipping address" field because labels were unclear, choosing the wrong pricing plan because the comparison was confusing.
- **Causes:** poor labeling, misleading layout, wrong conceptual model, insufficient information to make a decision.
- **Fixes:** clearer labels and descriptions, better information hierarchy, preview/review steps before commitment, inline explanations for complex choices.

**Diagnostic rule:** When reviewing an error state, ask: "Did the user know what to do?" If yes, it's a slip (fix the interface mechanics). If no, it's a mistake (fix the information and model).

**Review question:** For the most common errors in this flow, are they slips (fix with spacing, undo, constraints) or mistakes (fix with labels, previews, better information)?

---

### Knowledge in the World vs. Knowledge in the Head

**Definition:** Information needed to use an interface either lives in the interface itself (in the world) or in the user's memory (in the head). Good design minimizes what users must memorize.

**Knowledge in the world:**

- Labels on buttons, visible shortcuts, format hints on inputs, breadcrumbs showing location, progress indicators showing step count.
- Advantage: no memorization needed. Works for first-time users.
- Cost: takes up screen space. Can create visual clutter.

**Knowledge in the head:**

- Keyboard shortcuts, gesture commands, memorized navigation paths, remembered field formats.
- Advantage: fast for expert users. Clean interface.
- Cost: must be learned. Invisible to new users. Forgotten if not used regularly.

**Rules:**

- Default to knowledge in the world. Make everything discoverable through visible UI.
- Offer knowledge-in-the-head as acceleration: keyboard shortcuts visible in tooltips, gesture hints on first use, then fade away.
- Never require knowledge in the head for primary tasks. Shortcuts and gestures are power-user accelerators, not the primary path.
- Progressive transition: show the full label at first, reveal the shortcut with use, eventually let power users hide the training wheels (collapsible sidebar labels, minimal mode).

**Review question:** Could a user who has never seen this interface before complete the primary task without any external help?

---

### Modes

**Definition:** A state in which the same action produces different results. Modes are a frequent source of confusion and errors.

**In UI:**

- Edit mode vs. view mode: clicking text either navigates (view mode) or places a cursor (edit mode).
- Selection mode: tapping an item either opens it (normal mode) or selects it (selection mode) — common on mobile.
- Rich text editors: the same keyboard shortcut does different things depending on which toolbar mode is active.

**Problems with modes:**

- **Mode errors:** the user acts as if they're in one mode when they're actually in another. They type in a form field thinking they're editing content, but the modal has focus and they're triggering keyboard shortcuts.
- **Mode invisibility:** the user doesn't realize the mode changed. Caps Lock is the classic physical example.

**Rules:**

- Minimize modes. If the same interface can be mode-free, make it mode-free. Inline editing eliminates "edit mode."
- When modes are necessary, make the current mode unmistakably visible: distinct background color, visible mode label, different cursor, changed toolbar state.
- Mode transitions should be explicit and reversible: enter with a clear action (click "Edit"), exit with Escape or a visible "Done" button.
- Never auto-enter a mode without the user's intention. If the interface switches to selection mode after a long-press, provide a clear visual indicator and an easy way to exit.
- Keyboard focus is a form of mode. When a modal opens, the keyboard context changes. Make this explicit with focus trapping and visible focus indicators.

**Review question:** Are there any modes in this interface? For each one, is the current mode always visible, and can the user always exit without confusion?

---

## Part 2: HCI Laws

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

## Part 3: Cognitive Biases

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

## Part 4: Interaction Flow

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

## Part 5: Attention Economy

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
