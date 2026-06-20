# Planning Workflow

## Purpose

Guide agents through planning non-trivial work before implementation.

This workflow exists to restate requirements, inspect the relevant project context, identify risks, and create a practical step-by-step implementation plan before editing files.

## When to Activate

Use this workflow when the user asks for:

- a new feature
- a non-trivial code change
- a multi-file change
- unclear or ambiguous implementation scope
- an architecture-sensitive change
- a database, API, backend, or frontend feature
- an AI feature
- a billing or credit feature
- a Shopify or e-commerce feature
- an MCP or tooling change
- anything where the agent should plan before editing

Do not use this workflow for tiny safe edits, explanation-only tasks, or simple file reads where a workflow would add noise without reducing risk.

## Required Read Order

Read in this order when this workflow is activated:

```text
START_HERE.md
INDEX.md
workflows/INDEX.md
workflows/planning_workflow.md
core/approval_gates.md
rules/INDEX.md
relevant rule files
memory/INDEX.md when project context matters
relevant memory/by_area files
skills/INDEX.md when execution playbook is useful
relevant skill section index and skill file
affected project files
```

Do not load every rule, memory file, or skill by default. Read only what the task needs.

## Planning Flow

```text
understand request
-> classify task type
-> check workflow need
-> read relevant rules
-> read relevant memory if project context matters
-> read relevant skills if execution guidance is useful
-> inspect affected files
-> identify unknowns and risks
-> define implementation phases
-> define validation plan
-> define memory updates needed after work
-> present plan
-> wait for Karim approval when required
```

## Context Inspection

Before writing the plan:

1. Restate the request in clear terms.
2. Identify the task category and why this workflow is active.
3. Search for existing project patterns in the affected area.
4. Inspect files that the implementation is likely to touch.
5. Capture relevant conventions instead of inventing new ones.

When available, ground the plan in existing patterns for:

| Category | What to Capture |
| --- | --- |
| Naming | File, function, type, route, command, or script naming in the affected area |
| Error handling | How failures are raised, returned, logged, or surfaced |
| Logging | Levels, format, and what gets logged |
| Data access | Repository, service, query, ORM, or filesystem patterns |
| Tests | Test file location, framework, fixtures, and assertion style |
| UI | Component structure, state patterns, accessibility, and styling conventions |

If no similar pattern exists, say that clearly. Do not invent a pattern and present it as project precedent.

## Risk and Approval Gates

Identify risks before implementation. Approval may be required when work touches:

- architecture or shared infrastructure
- database schema, migrations, RLS, or tenant ownership
- authentication, authorization, sessions, or customer data
- security-sensitive inputs or provider callbacks
- billing, credits, payments, or paid provider actions
- production deployment, environment variables, or secrets
- destructive data or filesystem changes
- large refactors or behavior changes across multiple areas
- files outside the allowed scope

If an approval gate is triggered, present the plan and wait for Karim approval before editing.

## Implementation Plan Format

Use this format for the plan:

```markdown
## Plan

### Task Restatement
...

### Scope
In scope:
- ...

Out of scope:
- ...

### Files/Areas Likely Affected
- ...

### Rules Read
- ...

### Memory Read
- ...

### Skills Read
- ...

### Risks / Approval Gates
- ...

### Implementation Phases
1. ...
2. ...
3. ...

### Validation Plan
- ...

### Memory Updates After Work
- ...

### Questions / Needed Approval
- ...
```

Keep plans practical. Avoid over-engineering, speculative architecture, or large design documents when the task needs a focused implementation path.

## Validation Plan

Every plan should include validation appropriate to the risk and change type.

Prefer validation that proves the changed behavior directly:

- unit tests for isolated logic
- integration tests for APIs, database behavior, providers, or workflows
- type checks for TypeScript or typed code paths
- lint/format checks when available
- build commands for application or package-level changes
- E2E checks for critical user flows
- manual verification steps only when automation is not available

If validation cannot be run, state why and list the residual risk.

## Memory Update Plan

During planning, identify whether the work may create stable project knowledge that should be captured after implementation.

Memory updates may be needed when the work changes:

- architecture decisions
- API behavior
- database or migration expectations
- provider integration behavior
- build, deploy, or debugging procedures
- product behavior that future agents must know

Do not create duplicate memory if the same information already lives in project docs, code comments, or an existing memory file. If there is no obvious place for a needed memory update, ask Karim before creating a new top-level file.

## Stop Conditions

Stop and ask Karim if:

- scope is unclear
- approval gate is triggered
- database schema change is needed
- destructive change is possible
- paid provider action may be triggered
- production deployment is involved
- security, auth, RLS, or tenant isolation is involved and requirements are unclear
- the task requires editing outside allowed scope
- project memory conflicts with current code
- affected files or architecture are unclear

## Output Expectations

When planning is complete, report:

- task restatement
- scope
- files or areas likely affected
- rules, memory, and skills read
- risks and approval gates
- phased plan
- validation plan
- whether Karim approval is needed before implementation

After presenting the plan, wait for Karim approval when required. Do not jump into coding when an approval gate applies.
