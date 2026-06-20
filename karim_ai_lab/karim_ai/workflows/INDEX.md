# Workflows Index

Purpose:
Workflows tell agents which step-by-step process to follow for specific task types.

Workflows do not replace rules, memory, or skills.

Rules define boundaries.
Memory defines project truth.
Skills define execution expertise.
Workflows define the task process.

## Current Status

Asset 6 - Workflows is in progress.

Current planned workflow files:

```text
planning_workflow.md
build_fix_workflow.md
```

These files are not active until they exist.

## When to Read This Index

Read this index after START_HERE.md and INDEX.md when the task may need a process.

Use this index to decide whether a workflow is needed.

## Workflow Activation Rule

Do not use a workflow for every task.

Use a workflow when:

- the task has multiple steps
- the scope is unclear
- implementation needs planning
- debugging needs a disciplined process
- there is a build/type/lint/CI error
- the change may affect architecture, database, API, security, billing, credits, deployment, or customer-facing behavior

Do not use a workflow when:

- the task is a tiny safe text edit
- the change is clearly one-line and low risk
- the user explicitly asks only for explanation
- the task is only reading/summarizing a file
- using a workflow would add noise without reducing risk

## Active Workflow Routing

### Planning Workflow

Planned file:

```text
planning_workflow.md
```

Use this workflow when the user asks for:

- new feature
- non-trivial code change
- unclear implementation
- multi-file change
- architecture-sensitive change
- database/API/backend/frontend feature
- AI feature
- billing/credit feature
- Shopify/e-commerce feature
- MCP/tooling change
- anything where the agent should plan before editing

Expected behavior:
The agent should inspect context, create a plan, identify risks, and wait for Karim approval before implementation when required by approval gates.

### Build Fix Workflow

Planned file:

```text
build_fix_workflow.md
```

Use this workflow when the user asks to fix:

- build error
- TypeScript error
- lint error
- test failure
- CI failure
- install/dependency error
- broken deployment check
- failed verification command
- repeated runtime error caused by recent code change

Expected behavior:
The agent should capture the exact error, identify the build system, group errors, fix one root cause at a time, rerun the relevant check, and stop if the fix requires architectural change or approval.

## Workflow Selection Examples

### New Feature

User says:

```text
Build a new credits dashboard.
```

Use:

```text
workflows/planning_workflow.md
```

### API Change

User says:

```text
Add an endpoint to deduct credits after successful AI generation.
```

Use:

```text
workflows/planning_workflow.md
```

Also likely read:

```text
rules/api_routes.md
rules/backend.md
rules/database.md
rules/security.md
skills/core_development/api_design.md
skills/core_development/backend_service_patterns.md
skills/core_development/database_migrations.md
```

### Build Error

User says:

```text
Fix this build error.
```

Use:

```text
workflows/build_fix_workflow.md
```

Also likely read:

```text
skills/core_development/testing_verification.md
```

### Tiny Safe Edit

User says:

```text
Change the button text from Save to Continue.
```

A workflow may not be needed.

Use only the relevant rule file and affected project file.

### Explanation-Only Task

User says:

```text
Explain what this file does.
```

A workflow is usually not needed.

Read the relevant files and answer.

## Recommended Read Flow

When a workflow is needed:

```text
START_HERE.md
-> INDEX.md
-> workflows/INDEX.md
-> chosen workflow file
-> rules/INDEX.md
-> relevant rule files
-> memory/INDEX.md if project context is needed
-> relevant memory files
-> skills/INDEX.md if execution playbook is useful
-> relevant skill files
-> task files
```

When no workflow is needed:

```text
START_HERE.md
-> INDEX.md
-> relevant rules/memory/skills only if needed
-> task files
```

## Token Optimization

Do not load all workflows.

Read:

- this index
- only the workflow file that matches the task

Do not read both workflows unless the task clearly needs both.

Example:
A build error during feature implementation may use:

```text
planning_workflow.md
-> build_fix_workflow.md only after a build/type/lint failure appears
```

## Workflow vs Skill

A workflow may call skills when useful.

Example:
build_fix_workflow.md may call:

```text
skills/core_development/testing_verification.md
```

Example:
planning_workflow.md may call:

```text
skills/core_development/api_design.md
skills/core_development/database_migrations.md
skills/ai_llm/llm_cost_optimization.md
```

But workflows should not load every skill by default.

## Stop Conditions

Stop and ask Karim if:

- the correct workflow is unclear
- the task seems too broad
- the workflow would require editing files outside approved scope
- the task requires destructive change
- the task requires paid action
- the task requires production deployment
- the workflow conflicts with rules
- using a workflow would add noise instead of reducing risk

## New Workflow Rule

Do not create new workflow files unless Karim approves.

If a new workflow is needed, propose it first and explain:

- why existing workflows are not enough
- when it should activate
- what files/rules/skills it should read
- what stop conditions it needs
