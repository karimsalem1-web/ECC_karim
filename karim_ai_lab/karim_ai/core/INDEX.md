# Core Index

Purpose:
This folder contains the non-negotiable operating layer for all agents working inside Karim AI Lab.

When to read:
- Read this index when a task involves planning, coding, review, refactoring, execution, or risk.
- Do not load every core file automatically unless the task is broad, risky, or unclear.
- Use this index to choose the minimum relevant core files.

## Files

- executive_laws.md — non-negotiable laws for all work.
- working_protocol.md — default task process from understanding to completion.
- approval_gates.md — actions that require Karim's explicit approval.
- quality_standards.md — coding, documentation, planning, testing, and review quality expectations.
- context_optimization.md — token and context-saving behavior.

## Loading Guide

For any coding or repo-changing task:
1. Read executive_laws.md.
2. Read working_protocol.md.
3. Read approval_gates.md if the task touches Git, deploy, production, data, billing, credentials, or external communication.
4. Read quality_standards.md before implementation or review.
5. Read context_optimization.md for broad, long, or multi-file tasks.

For planning-only tasks:
- Read working_protocol.md and context_optimization.md.
- Add executive_laws.md if the plan affects architecture, security, data, or production.

For review tasks:
- Read executive_laws.md, approval_gates.md, and quality_standards.md.

## Do Not

- Do not duplicate these rules in adapter files.
- Do not load unrelated future rules or skills unless the task requires them.
- Do not treat placeholders in future folders as final guidance.
