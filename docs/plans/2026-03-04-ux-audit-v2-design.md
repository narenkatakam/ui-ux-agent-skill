# UX Audit v2 — Design Doc

**Date:** 2026-03-04
**Goal:** Sharpen ux-audit from B+ guidance skill to production-ready design intelligence tool
**Timeline:** 2-3 weeks (4 phases)
**Approach:** Depth + Bridge Layer (A+C hybrid)

## Problem

ux-audit has strong foundations — psychology, system-level principles, motion vocabulary, opinionated non-negotiables — but gaps in three areas:

1. **Actionability** — tells developers how to think, not what to code
2. **Coverage** — missing critical UI patterns (states, forms, overlays)
3. **Proof** — README lacks before/after examples and platform guidance

## Phase 1: Core Sharpening (Week 1)

### 1.1 Replace "Modern Minimal" vibes with rules
- File: `skills/SKILL.md` (lines 224-234)
- Add: max shadow blur values, max colors per component, whitespace:content ratios, concrete border-radius rules
- Target: as specific as the spacing rules (4px base unit, allowed set)

### 1.2 Add code examples to all 12 core principles
- File: `skills/SKILL.md` (lines 79-192)
- Each principle gets a "Do / Don't" in Tailwind + React
- Developers copy-paste patterns, not interpret paragraphs

### 1.3 Deepen dark mode
- File: `skills/references/color-systems.md`
- Add: elevation strategy (surface levels 0-5), saturation reduction approach, image/content handling, separate contrast audit checklist

### 1.4 Strengthen data-viz.md
- File: `skills/references/data-visualization.md`
- Add: table design patterns (sort, filter, bulk actions, row expansion), dashboard layout rules, progressive disclosure, legends/annotation placement

### 1.5 Expand icons.md
- File: `skills/references/icons.md`
- Add: sizing strategy (16/20/24/32 with use cases), optical alignment rules, SVG vs font-icon trade-offs

## Phase 2: Missing References (Week 1-2)

### 2.1 ui-states.md (NEW)
- Empty states: patterns, copy guidance, illustration vs icon, CTA placement
- Loading states: skeleton vs spinner decision tree, timing, preventing layout shift, partial load handling
- Error states: toast vs inline vs modal decision tree, copy patterns ("what happened + how to fix"), recovery UI
- Success states: confirmation patterns, next-action suggestions

### 2.2 forms.md (NEW)
- Validation: inline vs on-submit, timing, error message placement
- Multi-step: progress indication, back/forward, draft preservation
- Field patterns: grouping, label placement, placeholder guidance, auto-save
- Accessibility: form-specific a11y beyond existing coverage

### 2.3 overlays.md (NEW)
- Modal: when to use, sizing, focus trapping, stacking rules, dismissal
- Drawer: side vs bottom, content width, persistent vs temporary
- Popover: positioning, arrow placement, hover vs click trigger
- Bottom sheet: mobile patterns, snap points, handle affordance
- Decision tree: "which overlay for which use case?"

## Phase 3: Bridge Layer (Week 2)

### 3.1 Component checklists
- New directory: `skills/checklists/`
- One file per surface type: buttons, cards, tables, forms, modals, navigation, dashboards
- Format: numbered checklist, 8-15 items each, referencing relevant principles and references
- "Building a [X]? Check these things before shipping."

### 3.2 Quick-reference index
- Add to top of `skills/SKILL.md`
- "I need help with [X] → read [Y]" decision tree
- Covers all references + checklists

### 3.3 Tighten review-template.md
- Split verification checklist: Essential (15 items) vs Polish (25 items)
- Add 3-4 more example findings at P0, P1, P2 severity
- Add examples for different surface types (dashboard, form, settings page)

## Phase 4: README + Distribution (Week 3)

### 4.1 README rewrite
- Before/after code examples (pulled from Phase 1 work)
- Real usage examples showing input → output for both modes
- Platform-specific install + usage (Claude Code, Cursor, Windsurf, Codex)
- Expected time investment per audit

### 4.2 Distribution
- Submit to awesome-claude-code
- Submit as PR to anthropics/skills
- LinkedIn/X launch post
- Origin story paragraph in README

## Success Criteria

- A developer can pick up any component checklist and know exactly what to build
- Every core principle has a copy-pasteable code example
- The 3 new references cover the most common "what do I do here?" moments
- README converts visitors to users within 60 seconds of reading
- 16+ stars within 4 weeks of distribution (Starstruck achievement)
