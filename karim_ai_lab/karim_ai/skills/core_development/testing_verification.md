# Testing and Verification

Purpose:
Help agents verify work before saying it is done, using the smallest reliable set of build, type, lint, test, manual, security, diff, and eval-style checks for the task.

Testing and verification skills do not override rules.
Before using this skill, read `rules/architecture.md` and the relevant technical rule files for the task.
For sensitive work, also read `rules/security.md`.

## When to Activate

Use this skill for:

- final verification before completion
- bug fixes
- high-risk logic
- backend/API/database/frontend changes
- refactors
- checkout, billing, auth, upload, order, AI-generation, customer, or admin flows
- AI output, prompt, retrieval, or agent behavior quality checks
- PR or release readiness checks

Do not run every possible test blindly when a smaller relevant check is enough.

## Required Read Order

Read:

1. `START_HERE.md`
2. `INDEX.md`
3. `rules/INDEX.md`
4. `rules/architecture.md`
5. The technical rule files relevant to the task
6. `rules/security.md` for sensitive work
7. `memory/INDEX.md` and relevant memory files if project behavior matters
8. Existing test, build, lint, and CI configuration

## Execution Pattern

Use this verification flow:

```text
define success criteria
-> run relevant build/type/lint checks
-> run relevant tests
-> perform manual flow check if needed
-> perform security smoke check if sensitive
-> review diff for unintended changes
-> report pass/fail with evidence
-> fix or stop if verification fails
-> update memory if stable project behavior changed
```

## Practical Checklist

Success criteria:

- Define what must be true for the task to count as complete.
- Include user-visible behavior, API contract, data behavior, security boundary, and regression expectations when relevant.
- For AI/prompt/agent behavior, define eval-style pass/fail criteria before judging quality.

Relevant checks:

- Run the project build when build output could be affected.
- Run type checks for typed code changes.
- Run lint/format checks when style, imports, or conventions could be affected.
- Run targeted unit or integration tests for touched logic.
- Run broader tests when shared behavior or cross-module contracts changed.
- Use manual flow checks when automated tests do not cover the behavior.

TDD guidance:

- Use TDD/RED-GREEN for bugs, high-risk logic, or where tests are expected.
- Confirm the RED failure is caused by the intended missing or broken behavior.
- Implement the smallest fix that turns the relevant test GREEN.
- Refactor only after the relevant check remains GREEN.
- Do not force full TDD ceremony for tiny documentation or low-risk navigation edits.

E2E guidance:

- Use E2E for checkout, billing, auth, upload, order, AI-generation, or critical customer/admin flows.
- Prefer stable selectors and user-visible assertions.
- Avoid arbitrary sleeps when the test can wait for a real UI, network, or state condition.
- Capture screenshots, traces, or video when failure evidence matters.

Eval-style checks:

- Use eval-style checks for AI outputs, prompts, retrieval quality, agent behavior, or other quality that normal tests cannot fully cover.
- Prefer deterministic graders when possible.
- Use a short rubric for model or human judgment.
- Record pass/fail evidence and known limits.

Security smoke check:

- Check that no secrets were added.
- Check frontend code does not expose private env vars.
- Check auth and authorization are still backend-enforced.
- Check errors do not expose stack traces, SQL errors, provider raw errors, or secret-bearing data.
- Check paid, destructive, webhook, upload, AI, and tenant-sensitive paths for duplicate or bypass risk.

Diff review:

- Review changed file list.
- Look for unrelated edits.
- Look for accidental generated files.
- Look for removed validation, loading states, error states, auth checks, or tests.
- Confirm the change stayed inside approved scope.

Failure handling:

- Do not claim work is done without verification.
- If checks fail, do not hide failure.
- Fix failures that are in scope.
- Stop and report if failure is outside scope, risky, or needs Karim approval.
- If verification cannot be run, clearly say what was not run and why.

## Stop Conditions

Stop and ask Karim if:

- success criteria are unclear
- a verification failure may require a product or architecture decision
- production data, billing, credits, auth, customer data, RLS, uploads, or AI cost behavior may be affected
- E2E or manual verification would need live production access
- eval quality needs human judgment
- a failing check is unrelated to the task but blocks completion
- verification requires installing new tools or dependencies without approval

## Output Expectations

When reporting verification:

- State which checks were run.
- State pass/fail results with useful evidence.
- State what was not run and why.
- Mention manual flow checks if performed.
- Mention security smoke checks for sensitive work.
- Mention diff review scope.
- Mention memory updates if stable project behavior changed.
