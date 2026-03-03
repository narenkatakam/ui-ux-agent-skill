# Architecture

Hub-and-spoke model — one source of truth, zero duplication across tools.

```mermaid
flowchart TB
    SKILL["SKILL.md — 12 principles + workflows"]
    REF["10 reference docs"]
    AGENTS["AGENTS.md — shared conventions"]

    SKILL --> REF
    AGENTS --> SKILL

    AGENTS --> CL["CLAUDE.md"]
    AGENTS --> CU[".cursor/rules/"]
    AGENTS --> OA["openai.yaml"]
    AGENTS --> WS["Windsurf"]
```

---

## File Tree

```
.
├── AGENTS.md                              # Shared conventions
├── CLAUDE.md                              # Claude Code bridge
├── .cursor/rules/ux-audit.mdc    # Cursor bridge
├── agents/openai.yaml                     # Codex bridge
├── docs/                                  # Detailed documentation
│   ├── README.md
│   ├── how-it-works.md
│   ├── coverage.md
│   ├── architecture.md
│   └── origin-story.md
└── skills/ux-audit/
    ├── SKILL.md                           # Core: 12 principles + 2 workflows
    └── references/                        # 10 deep-dive reference docs
        ├── accessibility.md
        ├── color-systems.md
        ├── data-visualization.md
        ├── icons.md
        ├── navigation.md
        ├── psychology.md
        ├── responsive-design.md
        ├── review-template.md
        ├── system-principles.md
        └── typography.md
```

---

## Manual Installation

Copy files into your project or user-level config:

| Agent | What to copy | Where |
|---|---|---|
| **Codex** | `skills/ux-audit/` + `agents/openai.yaml` | `~/.codex/skills/` + project root |
| **Claude Code** | `CLAUDE.md` | Project root (it loads `AGENTS.md` and `SKILL.md`) |
| **Cursor** | `.cursor/rules/ux-audit.mdc` | Project root |
| **Windsurf** | `AGENTS.md` | Project root |
