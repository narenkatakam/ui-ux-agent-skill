# Design Craft

A UI/UX guidance and review skill for AI coding assistants. Turns vague design feedback into actionable, implementable recommendations.

Built for [Codex](https://openai.com/codex), [Claude Code](https://docs.anthropic.com/en/docs/claude-code), [Cursor](https://cursor.com), and [Windsurf](https://windsurf.com).

## What It Does

Two modes:

- **Guide mode** — compact principles and concrete do/don't rules for modern, clean interfaces. Tell it what you're building, get back implementable design guidance.
- **Review mode** — structured audit of existing UI (screenshot, mock, HTML, or PR). Returns prioritized fixes (P0/P1/P2) with specific diagnoses and acceptance checks.

## What It Covers

| Domain | Reference |
|---|---|
| Core UX principles | Task-first UX, information architecture, feedback, consistency, affordance, error prevention, cognitive load, CRAP visual hierarchy |
| System-level principles | Concept constancy, copy discipline, state perceptibility, help text layering (L0-L3), feedback loops, progressive complexity |
| Accessibility | WCAG 2.1 AA baseline, keyboard navigation, screen readers, color contrast, forms, touch targets |
| Responsive design | Mobile-first approach, breakpoint strategy, fluid layouts, touch vs pointer, content adaptation |
| Typography | Type scale, font pairing, line height, measure, letter spacing, hierarchy, responsive typography |
| Color systems | Palette structure, semantic tokens, contrast, dark mode, data visualization color, color psychology |
| Navigation | Nav patterns, breadcrumbs, tabs, wayfinding, search, mobile navigation |
| Data visualization | Chart type selection, data-ink ratio, dashboard design, axes/labels, interaction, formatting |
| Design psychology | Affordances, signifiers, mapping, constraints, gulfs, slips vs mistakes (Norman) |
| Interaction psychology | Fitts's/Hick's/Miller's Law, cognitive biases, interaction flow, attention economy |
| Icons | No-emoji rule, icon set selection, when to use text instead |
| Motion | Animation purpose, motion vocabulary, canvas stability, anti-patterns |

## Installation

### Multi-agent install (recommended)

```bash
npx skills add narenkatakam/design-craft-guide -a codex -a claude-code -a cursor -a windsurf
```

Add `-g` for global (user-level) install:

```bash
npx skills add narenkatakam/design-craft-guide -g -a codex -a claude-code -a cursor -a windsurf
```

### Single-agent install

```bash
# Codex
npx skills add narenkatakam/design-craft-guide -a codex

# Claude Code
npx skills add narenkatakam/design-craft-guide -a claude-code

# Cursor
npx skills add narenkatakam/design-craft-guide -a cursor

# Windsurf
npx skills add narenkatakam/design-craft-guide -a windsurf
```

### Manual install

Copy the relevant files into your project or user-level config:

- **Codex**: place `skills/design-craft/` in `~/.codex/skills/` and `agents/openai.yaml` in project root
- **Claude Code**: place `CLAUDE.md` in project root (it loads `AGENTS.md` and `SKILL.md`)
- **Cursor**: place `.cursor/rules/design-craft.mdc` in project root
- **Windsurf**: place `AGENTS.md` in project root

## How to Use

### Guide mode

Ask your AI assistant to apply design guidance:

```
Use the design-craft skill in guide mode. I'm building a settings page
for a SaaS dashboard. The user needs to manage notification preferences,
API keys, and team member roles.
```

### Review mode

Ask your AI assistant to review existing UI:

```
Use the design-craft skill in review mode. Review this screenshot of our
onboarding flow. The primary user task is creating their first project.
```

### Expected output

**Guide mode** returns:
- Surface identification and primary task/CTA analysis
- Applicable principles with specific do/don't rules
- Component and layout recommendations
- Accessibility requirements

**Review mode** returns:
- Context and assumptions
- Prioritized findings (P0 blocker / P1 important / P2 polish)
- Diagnoses (execution gulf / evaluation gulf; slip / mistake)
- Specific, implementable fixes with acceptance checks
- Verification checklist

## Repository Structure

```
.
├── AGENTS.md                          # Shared instructions for all AI tools
├── CLAUDE.md                          # Claude Code bridge
├── README.md                          # This file
├── LICENSE.txt                        # Apache 2.0
├── .cursor/rules/design-craft.mdc     # Cursor integration
├── agents/openai.yaml                 # Codex/OpenAI integration
└── skills/design-craft/
    ├── SKILL.md                       # Core skill definition
    └── references/
        ├── system-principles.md       # 11 system-level guiding principles
        ├── interaction-psychology.md   # HCI laws, cognitive biases, flow
        ├── design-psych.md            # Norman's design concepts
        ├── accessibility.md           # WCAG, keyboard, screen readers
        ├── responsive-design.md       # Mobile-first, breakpoints, fluid
        ├── typography.md              # Type scale, pairing, hierarchy
        ├── color-systems.md           # Palettes, tokens, contrast, dark mode
        ├── navigation.md              # Nav patterns, wayfinding, mobile
        ├── data-visualization.md      # Charts, dashboards, data-ink ratio
        ├── icons.md                   # Icon rules, sets, mappings
        ├── review-template.md         # Structured review output format
        └── checklists.md              # Expanded checklists by surface type
```

## Multi-Tool Architecture

This skill uses a hub-and-spoke model:

- **`AGENTS.md`** is the single source of truth for repository-level conventions.
- **`SKILL.md`** contains the actual skill behavior, principles, and workflows.
- **Bridge files** (`CLAUDE.md`, `.cursor/rules/design-craft.mdc`, `agents/openai.yaml`) are thin pointers that load the shared content. No duplication of rules across tools.

## Attribution

This is a derivative work of [oil-oil/oiloil-ui-ux-guide](https://github.com/oil-oil/oiloil-ui-ux-guide) by [oil-oil](https://github.com/oil-oil), licensed under Apache 2.0. The original provided the foundational structure, core UX principles, design psychology references, and multi-tool integration architecture.

This fork adds: full English translation, accessibility guidance, responsive design, typography, color systems, navigation patterns, data visualization, expanded checklists, and a product-thinking perspective.

## License

[Apache License 2.0](./LICENSE.txt)
