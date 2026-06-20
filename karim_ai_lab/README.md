# Karim AI Lab

Karim AI Lab is a clean, portable AI guidance system built inside the `ECC_karim` repo.

Its purpose is to give Codex, Claude, Antigravity, Kilo Code, and future coding agents one shared source of truth for:

- mission
- entry instructions
- core operating rules
- technical rules
- project memory
- skills
- workflows
- future templates

## Current Status

Asset 1 - Cross-agent entry strategy: complete.  
Asset 2 - Core rules layer: complete.  
Asset 3 - Project memory system skeleton: complete.  
Asset 4 - Core Development Skills: complete.  
Asset 5 - Initial Specialized Skills: complete.  
Asset 6 - Workflows: complete.

Current working folder:

```text
karim_ai_lab/
```

Original ECC files outside this folder are not modified yet.

## Current Strategy

Build and test the clean lab first.

Then later:

- Add templates.
- Audit original ECC.
- Keep useful ideas.
- Rewrite or ignore noisy parts.
- Promote the final clean system.
- Archive old ECC files before deleting anything.
- Add optional future specialized skills only when needed.
- Add optional future workflows only when needed.

## Agent Entry Files

Different agents may start from different files:

- `AGENTS.md`
- `CLAUDE.md`
- `ANTIGRAVITY.md`
- `KILO.md`
- `.codex/AGENTS.md`

All entry files should point back to:

- `karim_ai/START_HERE.md`
- `karim_ai/INDEX.md`

## Active Guidance Layers

Current active layers:

- `karim_ai/core/`
- `karim_ai/rules/`
- `karim_ai/memory/`
- `karim_ai/skills/`
- `karim_ai/workflows/`

Use indexes to avoid loading unnecessary files:

- `karim_ai/core/INDEX.md`
- `karim_ai/rules/INDEX.md`
- `karim_ai/memory/INDEX.md`
- `karim_ai/skills/INDEX.md`
- `karim_ai/workflows/INDEX.md`

Skills are optional execution playbooks, not mandatory rules.
Rules override skills when they conflict.
Memory is project truth; skills are execution guidance.
Workflows are step-by-step task processes.
Workflows are not loaded for every task; use them only when the task needs a process.
Rules override workflows when they conflict.

Active skill sections:

```text
karim_ai/skills/core_development/
karim_ai/skills/ai_llm/
karim_ai/skills/product_docs/
karim_ai/skills/mcp_tooling/
```

Future specialized skill sections should only be added when needed.

Active workflow files:

```text
karim_ai/workflows/INDEX.md
karim_ai/workflows/planning_workflow.md
karim_ai/workflows/build_fix_workflow.md
```

## Important Rule

Do not modify original ECC files outside `karim_ai_lab` unless Karim explicitly approves it.
