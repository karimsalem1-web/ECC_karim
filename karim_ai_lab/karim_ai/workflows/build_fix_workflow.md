# Build Fix Workflow

## Purpose

Guide agents through fixing build, type, lint, test, CI, install, dependency, and verification errors safely.

This workflow favors exact error capture, root-cause grouping, minimal fixes, and clear verification evidence over broad rewrites.

## When to Activate

Use this workflow when the user asks to fix:

- a build error
- a TypeScript error
- a lint error
- a test failure
- a CI failure
- an install or dependency error
- a broken deployment check
- a failed verification command
- a repeated runtime error caused by a recent code change

## Required Read Order

Read in this order when this workflow is activated:

```text
START_HERE.md
INDEX.md
workflows/INDEX.md
workflows/build_fix_workflow.md
rules/INDEX.md
relevant rule files based on error area
skills/core_development/testing_verification.md
skills/core_development/deployment_ops.md if deployment/CI related
memory/INDEX.md if project-specific build/deploy/debug behavior matters
affected project files
```

Read only the files needed to understand the failure. Do not load unrelated rules, memory, skills, or project files.

## Build Fix Flow

```text
capture exact failing command and output
-> identify project/tooling system
-> classify error type
-> group errors by likely root cause
-> inspect relevant files only
-> choose smallest safe fix
-> edit one root cause
-> rerun relevant check
-> repeat only if needed
-> run broader verification when stable
-> report evidence
-> update memory if stable project build/debug behavior changed
```

Do not randomly edit many files. Capture the exact failure first, then fix one root cause at a time.

## Error Classification

Classify the failure before editing:

| Error Type | Common Signals |
| --- | --- |
| Dependency/install error | Missing package, lockfile conflict, registry failure, incompatible dependency |
| TypeScript/type error | Type mismatch, missing property, invalid import type, generic constraint failure |
| Lint/format error | Style rule failure, unused import, formatting mismatch, restricted pattern |
| Test failure | Assertion failure, snapshot mismatch, fixture issue, mock mismatch |
| Build/compile failure | Bundler failure, syntax error, unresolved module, compiler error |
| Runtime error | Stack trace, thrown exception, undefined value, failed request at runtime |
| Environment/config error | Missing env var, invalid config, path mismatch, local setup issue |
| CI/deployment error | Pipeline-only failure, deployment check failure, platform config issue |
| Framework/tooling mismatch | Version mismatch, changed API, incompatible plugin or adapter |

If the error type is unclear, inspect the command, config files, and nearest affected files before editing.

## Root Cause Grouping

When multiple errors appear:

1. Group errors by likely root cause.
2. Fix imports, generated types, or dependency issues before downstream logic errors.
3. Fix shared configuration before individual file errors when the evidence points to config.
4. Track total errors for progress, but do not chase every symptom independently.
5. Re-run the failing command after each root-cause fix.

Prefer dependency-order fixes:

```text
install/config/imports
-> types/interfaces
-> implementation logic
-> tests/fixtures
-> formatting/lint cleanup
```

## Safe Fix Rules

- Fix one root cause at a time.
- Do not suppress errors just to pass checks.
- Do not remove tests unless Karim explicitly approves.
- Do not weaken types unless there is a justified reason.
- Do not hide lint errors with broad disable comments.
- Do not delete code blindly.
- Do not change architecture to fix a small build error unless approved.
- Do not change environment variables or secrets without approval.
- Do not upgrade major dependencies unless approved.
- Prefer targeted fixes over broad rewrites.

If a fix introduces more errors than it resolves, stop and reassess before continuing.

## Verification Loop

1. Rerun the exact failing command first.
2. If fixed, run the next relevant broader check.
3. If new errors appear, classify whether they are related or separate.
4. If the same error appears after two reasonable fixes, stop and reassess.
5. If the fix touches security, database, API, deployment, billing, credits, or customer-facing behavior, run relevant extra checks or ask for approval.

Use the narrowest meaningful command first, then broaden:

```text
exact failing command
-> focused package/app check
-> lint/type/test/build check as relevant
-> broader verification when stable
```

If a command cannot be run because dependencies, credentials, network access, or local services are missing, report that clearly and explain the residual risk.

## Memory Update Rule

Update memory only when the fix reveals stable project build, deploy, or debugging behavior future agents should know.

Examples:

- a recurring build command that differs from the default
- a required environment setup step
- a known framework/tooling constraint
- a stable CI or deployment quirk
- a verified debugging procedure for this project

Do not add memory for one-off errors, temporary local failures, or information already documented in the right project file.

## Stop Conditions

Stop and ask Karim if:

- the exact error cannot be reproduced or understood
- the fix requires architectural change
- the fix requires destructive change
- the fix requires a dependency major upgrade
- the fix requires an env or secret change
- the fix requires production deployment
- the fix touches billing, credits, payments, auth, RLS, tenant ownership, or customer data
- the same root error remains after two reasonable fix attempts
- multiple unrelated errors suggest a larger broken state

## Output Expectations

When build fix work is complete, report:

- failing command
- main error or root cause
- files changed
- fix applied
- verification commands run
- pass/fail result
- remaining issues if any
- memory updates if stable project build/debug behavior changed
