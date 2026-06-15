# Quality Standards

Purpose:
This file defines the minimum quality standard for code, documentation, plans, reviews, and implementation work inside Karim AI Lab.

When to read:
- Read this file before coding, refactoring, reviewing, documenting, or proposing implementation.
- Apply it lightly for small tasks and strictly for production-facing work.

## Code Quality

1. Keep files focused.
   Each file should have one clear purpose. Avoid large mixed-responsibility files.

2. Keep functions small.
   Prefer small functions with clear inputs, outputs, and names.

3. Use clear naming.
   Names should explain purpose. Avoid vague names like `data`, `stuff`, `temp`, `final2`, or `newLogic` unless the scope is tiny and obvious.

4. Separate responsibilities.
   UI, business logic, API calls, validation, database access, and configuration should not be mixed together without reason.

5. Prefer typed contracts.
   Use TypeScript types, interfaces, schemas, or documented data shapes where practical.

6. Validate inputs.
   Validate user input, webhook input, API payloads, file uploads, LLM output, and database writes before trusting them.

7. Handle errors intentionally.
   Risky async operations should have error handling. Do not hide failures silently.

8. Avoid unrelated refactors.
   Do not rewrite unrelated code just because it can be improved. Stay inside the task scope.

9. Preserve existing behavior.
   Unless the task asks for behavior changes, avoid breaking existing flows.

10. Make changes reviewable.
    Prefer small commits, clear diffs, and simple explanations.

## Security Quality

Before considering work complete, check:
- No secrets were exposed.
- No private keys were committed.
- No service-role or admin logic moved to frontend.
- Protected data is filtered by the correct owner key.
- External input is validated.
- Error messages do not leak sensitive internals.
- Approval-gated actions were not performed without approval.

## Documentation Quality

Documentation should be:
- Short enough to be useful.
- Specific enough to guide action.
- Written with exact file paths when needed.
- Clear about what is confirmed and what is assumed.
- Updated when behavior, architecture, or workflow changes.

Avoid:
- Long generic advice.
- Duplicated rules across many files.
- Outdated instructions.
- Hidden assumptions.

## Planning Quality

A good plan should include:
- Goal
- Scope
- Files likely to change
- Required context files
- Risks or unknowns
- Test or verification path
- Approval gates, if any

For complex work, do not start implementation before the plan is accepted.

## Testing Quality

Testing expectations depend on task size.

For small documentation or prompt changes:
- Manual review is enough.

For code changes:
- Run existing relevant tests when practical.
- If tests cannot be run, explain why.
- Provide manual verification steps.

For production-sensitive changes:
- Require explicit approval.
- Test locally or in staging first when possible.
- Confirm rollback or recovery path if risk exists.

## Review Checklist

Before reporting completion, check:

- Did I stay inside the requested scope?
- Did I avoid touching unrelated files?
- Did I follow `executive_laws.md`?
- Did I respect `approval_gates.md`?
- Did I load only relevant context?
- Did I preserve security boundaries?
- Did I explain assumptions?
- Did I provide a clear next step?

## Quality Failure Rule

If the agent cannot meet these standards because context, access, tooling, or requirements are missing, it must say so clearly and ask Karim for the missing input.
