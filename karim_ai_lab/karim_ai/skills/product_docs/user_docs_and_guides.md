# User Docs and Guides

Purpose:
Help agents write useful product documentation, help center content, setup guides, onboarding guides, FAQs, tutorials, release notes, and SEO blog posts for SaaS products.

Docs must reflect actual product behavior.
Do not invent features.
Do not invent UI labels.
Do not invent pricing, refund, privacy, or security claims.

## When to Activate

Use this skill for:

- docs pages
- help center articles
- setup guides
- onboarding guides
- FAQs
- tutorials
- feature explanations
- troubleshooting docs
- release notes
- SEO blog posts when relevant
- turning notes or product behavior into user-facing documentation

## Required Read Order

Read:

1. `START_HERE.md`
2. `INDEX.md`
3. `memory/INDEX.md`
4. `memory/summary.md` when broad project context is needed
5. Relevant `memory/by_area/` file
6. `rules/security.md` if docs mention security, privacy, customer data, auth, payments, or uploads
7. `rules/api_routes.md`, `rules/backend.md`, or `rules/frontend.md` if docs explain technical product behavior
8. Existing product docs if they exist
9. Actual code, UI, config, or product source of truth when exact behavior matters

## Core Principles

1. Lead with the concrete user outcome.
2. Explain what the user should do next.
3. Use proof over hype.
4. Use direct wording.
5. Keep docs scannable.
6. Never invent product behavior, credibility, or customer evidence.
7. Prefer practical steps over abstract explanation.

## Voice and Style

Default to a clear operator voice:

- practical
- plain-language
- specific
- calm
- user-focused
- free of hype

Avoid:

- generic AI introductions
- "In today's rapidly evolving landscape"
- "game-changer"
- "cutting-edge"
- "revolutionary"
- unsupported superlatives
- vague promises
- closing questions added only for engagement

Use examples where they help the user act.

## Writing Process

1. Clarify the audience and job of the doc.
2. Inspect memory, existing docs, and product behavior.
3. Choose the right doc type.
4. Build a short outline with one job per section.
5. Write concrete steps and expected outcomes.
6. Add troubleshooting or FAQ only when useful.
7. Remove unsupported claims and generic filler.
8. Update memory if stable product terminology or user-facing explanation becomes important.

## Documentation Structures

### Help Center Article

Use for a focused support question.

```text
# Clear task title

Short answer or outcome.

## Before You Start
- requirement
- permission
- needed account/setup

## Steps
1. Do the first action.
2. Do the next action.
3. Confirm the result.

## Troubleshooting
- Problem: what the user sees
- Fix: what to try
```

### Setup Guide

Use when the user must connect, configure, or install something.

```text
# Set Up [Feature/Integration]

What this setup enables.

## Requirements
- account/access
- permissions
- required configuration

## Setup Steps
1. Open the correct area.
2. Add or connect the required item.
3. Save or verify.

## Verify Setup
Explain the safe way to confirm it works.

## Troubleshooting
Common issues and recovery steps.
```

### Onboarding Guide

Use when a new user needs the first successful path.

```text
# Get Started with [Product/Feature]

What the user will accomplish.

## Step 1: Prepare
## Step 2: Configure
## Step 3: Use the Feature
## Step 4: Verify the Result
## Next Steps
```

### FAQ

Use when users ask repeated short questions.

```text
## Frequently Asked Questions

### Question?
Direct answer.

### Question?
Direct answer with limitation or next step.
```

FAQ rules:

- Keep answers short.
- Say when behavior depends on plan, role, integration, or setup.
- Do not hide limitations.
- Do not invent policy answers.

### Tutorial

Use when teaching a workflow end to end.

```text
# Build / Create / Configure [Outcome]

What the user will make.

## What You Need
## Step-by-Step
## Check Your Result
## Common Issues
## What to Try Next
```

### SEO Blog Post

Use only when relevant.

```text
# Search-focused title

Lead with the concrete problem or outcome.

## Practical explanation
## Step-by-step guidance
## Examples
## Common mistakes
## Product tie-in if supported
```

SEO rules:

- Help the reader first.
- Do not stuff keywords.
- Use product claims only when supported.
- Include examples, not filler.

### Release Notes

Use when announcing shipped changes.

```text
# Release Notes: [Date or Version]

## New
- What changed and who benefits.

## Improved
- What is better.

## Fixed
- What was fixed.

## Notes
- Any migration, setup, or limitation users should know.
```

## Screenshots and UI Labels

Use screenshots when they reduce confusion.

If screenshots are needed but unavailable, mark them clearly:

```text
[Screenshot needed: describe exact screen or state]
```

Do not invent exact UI labels.
If labels are unknown, inspect the UI/code or ask Karim.

## Troubleshooting Guidance

Good troubleshooting sections include:

- what the user sees
- likely cause
- what to try
- when to contact support or escalate

Avoid:

- blaming the user
- vague "try again later" without context
- exposing internal errors
- promising fixes that do not exist

## Accuracy Gate

Before delivering docs:

- Check claims against memory, code, product behavior, or supplied source material.
- Remove unsupported pricing, refund, privacy, security, or performance claims.
- Confirm terminology is consistent.
- Confirm each section adds something useful.
- Confirm the doc tells the user what to do next.
- Confirm formatting matches the intended medium.

## Stop Conditions

Stop and ask Karim if:

- product behavior is unclear
- the doc would make unsupported promises
- exact UI labels are needed but unknown
- pricing, refund, privacy, security, legal, or compliance claims are unclear
- screenshots are required but unavailable
- the requested doc depends on an unbuilt feature
- project terminology is inconsistent

## Output Expectations

When reporting docs work:

- State the doc type.
- State source files, memory, or product behavior inspected.
- Mention any assumptions.
- Flag unsupported claims removed or avoided.
- Mention screenshots/placeholders if used.
- Mention memory updates if stable terminology or user-facing explanation became important.
