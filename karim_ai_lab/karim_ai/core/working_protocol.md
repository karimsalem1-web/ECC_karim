# Working Protocol

Purpose:
This file defines the default working process every agent should follow from task intake to completion.

When to read:
- Read this file when starting a new task, feature, bugfix, refactor, research task, or review.
- If the task is small, apply the protocol briefly.
- If the task is complex, follow every step explicitly.

## Default Process

1. Understand
   - Restate the goal in simple terms.
   - Identify the expected output.
   - Identify the affected project area.
   - Do not write code yet.

2. Inspect
   - Read `karim_ai/START_HERE.md`.
   - Use `karim_ai/INDEX.md` to find relevant files.
   - Inspect only task-relevant project files.
   - Avoid loading unrelated folders.

3. Clarify
   - Ask Karim when requirements are missing or ambiguous.
   - Ask before inventing schema, routes, API behavior, business rules, or architecture.
   - For small low-risk assumptions, state the assumption before proceeding.

4. Plan
   - Explain the proposed approach before implementation.
   - List files likely to change.
   - Mention risks, dependencies, and approval gates.
   - For larger tasks, wait for approval before editing.

5. Execute
   - Make the smallest useful change.
   - Keep changes scoped to the task.
   - Do not refactor unrelated areas.
   - Preserve existing behavior unless the task says otherwise.

6. Test
   - Run available tests when practical.
   - If tests are not available, explain how the change was checked.
   - For UI, API, database, automation, or webhook changes, define a manual verification path.

7. Review
   - Check for security issues, broken imports, naming problems, missing validation, and risky side effects.
   - Confirm no secrets or sensitive data were exposed.
   - Confirm no approval-gated action was performed without approval.

8. Report
   - Summarize what changed.
   - Summarize why it changed.
   - List files changed.
   - List assumptions, risks, or follow-up tasks.

9. Update memory
   - When the memory system exists, update the relevant memory file after meaningful work.
   - If memory does not exist yet, mention what should be recorded later.

## Task Size Rules

For tiny tasks:
- Understand → Execute → Report is acceptable.
- Still respect approval gates and security laws.

For normal tasks:
- Understand → Inspect → Plan → Execute → Test → Report.

For complex tasks:
- Understand → Inspect → Clarify → Plan → wait for approval → Execute → Test → Review → Update memory.

## Stop Conditions

Stop and ask Karim before continuing if:
- The task requires schema changes.
- The task affects production.
- The task may cost money.
- The task touches credentials or secrets.
- The task may delete or overwrite data.
- The task conflicts with `executive_laws.md` or `approval_gates.md`.
