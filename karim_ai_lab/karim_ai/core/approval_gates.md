# Approval Gates

Purpose:
This file defines actions that require Karim's explicit approval before an agent performs them.

When to read:
- Read this file before any action that changes external systems, production systems, billing, credentials, deployment, Git history, or irreversible project state.

## Approval Required Before These Actions

The agent must ask Karim and receive clear approval before doing any of the following:

1. Git and repository actions
   - Push to a remote branch
   - Merge a branch or pull request
   - Rebase shared history
   - Force push
   - Delete a branch
   - Delete or overwrite important files
   - Change repository settings

2. Deployment and production actions
   - Deploy to production
   - Change hosting settings
   - Change environment variables in production
   - Restart production services
   - Run production migrations
   - Modify production webhooks
   - Publish apps, plugins, packages, or extensions

3. Database and data actions
   - Create, alter, or drop database tables
   - Change RLS policies
   - Run migrations
   - Delete records
   - Update large batches of records
   - Export customer, order, payment, or private user data
   - Change authentication or authorization logic

4. Billing and paid service actions
   - Trigger paid API calls at scale
   - Start paid jobs
   - Upgrade plans
   - Add payment methods
   - Change billing settings
   - Use expensive AI, image, video, or automation APIs without cost confirmation

5. Credential and secret actions
   - Create, rotate, reveal, move, or delete API keys
   - Modify `.env`, secret managers, or dashboard secrets
   - Add secrets to GitHub, Vercel, Supabase, n8n, Shopify, or other platforms
   - Print or log secret values

6. External communication actions
   - Send emails
   - Send WhatsApp, Instagram, SMS, or customer support messages
   - Post to social media
   - Publish blog posts or pages
   - Contact customers, clients, reviewers, or support teams

7. Architecture-changing actions
   - Change the main folder structure
   - Replace the technology stack
   - Introduce a new framework
   - Replace database design
   - Change multi-tenant strategy
   - Change authentication model
   - Remove existing safety rules

## Allowed Without Approval

The agent may usually do these without extra approval, if they stay inside the requested scope:

- Read files
- Search the repo
- Summarize findings
- Propose plans
- Create draft documentation inside the lab folder
- Make local-only changes requested by Karim
- Suggest code changes
- Prepare prompts for Codex, Claude, Antigravity, or Kilo Code

## Approval Format

Acceptable approval examples:
- "yes, push"
- "deploy it"
- "run the migration"
- "create the table"
- "send it"
- "publish"
- "approved"

Unclear phrases are not approval:
- "looks good"
- "continue"
- "ok"
- "nice"
- "what next?"

If approval is unclear, ask again before acting.

## Emergency Stop Rule

If an action may cause data loss, public exposure, customer impact, billing cost, or production downtime, stop and ask Karim even if the task sounds urgent.
