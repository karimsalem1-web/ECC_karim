# Environment Rules

Purpose:
Guide agents when working with local, staging, and production environment variables, config files, dashboards, and secret handling.

When to read:
- Read this file before editing environment variables, config files, deployment settings, dashboards, secret managers, or integration credentials.
- Always read before changing `.env`, `.env.local`, Vercel, Supabase, n8n, Shopify, GitHub Actions, or other platform settings.

## Environment Principles

1. Secrets never go to Git.
   Do not commit `.env`, `.env.local`, private keys, tokens, service-role keys, or credential dumps.

2. Public env vars must be intentionally public.
   Variables with public prefixes, such as `NEXT_PUBLIC_`, can be exposed to the browser. Do not place secrets there.

3. Keep environments separate.
   Local, staging, and production values must not be mixed unless explicitly intended.

4. Document required variables safely.
   Use `.env.example` or documentation with placeholder values only.

5. Do not print secrets.
   Never log or display actual secret values.

## Env File Rules

Allowed:
- Create or update `.env.example` with placeholder values.
- Document where a variable should be configured.
- Add comments explaining what a variable is for.

Not allowed without approval:
- Modify real `.env` files.
- Reveal secret values.
- Move secrets between platforms.
- Rotate keys.
- Add production secrets.
- Delete or overwrite secret configuration.

## Naming

Use uppercase snake case for environment variables.

Examples:
- `SUPABASE_URL`
- `SUPABASE_ANON_KEY`
- `SUPABASE_SERVICE_ROLE_KEY`
- `SHOPIFY_API_SECRET`
- `N8N_WEBHOOK_URL`
- `OPENAI_API_KEY`
- `CLAUDE_API_KEY`

Public variables must follow the framework convention and only contain safe public values.

## Local / Staging / Production

Local:
- Used for development.
- Can use test keys.
- Must not contain production customer data unless explicitly approved.

Staging:
- Used for realistic testing.
- Should use separate keys and safe test data when possible.

Production:
- Requires approval before changes.
- Do not modify production variables without explicit approval.
- Document what changed and why.

## Dashboard Config

For dashboard-based secrets such as Vercel, Supabase, n8n, Shopify, Trigger.dev, or GitHub Actions:
- State the exact variable name needed.
- State where Karim should add it.
- Do not ask Karim to paste secrets into chat unless unavoidable.
- Prefer giving step-by-step instructions for Karim to add secrets himself.

## Environment Checklist

Before reporting completion, check:
- No real secrets were committed.
- `.gitignore` protects env files.
- Public env vars are safe.
- Required variables are documented.
- Production changes were not made without approval.
- Secret values were not printed or logged.

## Stop Conditions

Stop and ask Karim before continuing if:
- A real secret is required.
- A production variable must be changed.
- A key must be rotated.
- A credential appears in code or logs.
- The correct environment is unclear.
