# MCP Server Patterns

Purpose:
Help agents design, build, or review MCP servers and MCP tool integrations safely.

MCP is mainly for coding, diagnosis, and controlled tool access.
MCP should help agents inspect, implement, and diagnose systems like Supabase, Shopify, Google Cloud, logs, files, and project services.

MCP should not make the SaaS product itself unnecessarily dependent on MCP.
The SaaS must remain stable and understandable even without MCP.

## When to Activate

Use this skill when:

- building or reviewing an MCP server
- adding MCP tools, resources, or prompts
- giving agents structured access to external systems
- diagnosing Supabase, Shopify, Google Cloud, logs, files, or project services
- choosing transport for local or remote MCP use
- reviewing MCP permissions, secrets, or tool safety
- replacing ad-hoc scripts or manual copy/paste with safer controlled access

Do not add MCP only because it is interesting.
Use MCP when it gives agents safer, structured access than manual copy/paste or ad-hoc scripts.

## Required Read Order

Read:

1. `START_HERE.md`
2. `INDEX.md`
3. `rules/INDEX.md`
4. `rules/architecture.md` when MCP affects system architecture
5. `rules/backend.md` when building MCP server/backend logic
6. `rules/api_routes.md` when exposing endpoints or callbacks
7. `rules/security.md` for permissions, secrets, sensitive data, auth, and production access
8. `rules/environment.md` for env vars and deployment config
9. `memory/INDEX.md`
10. `memory/by_area/integrations.md` when MCP connects to external services
11. `memory/by_area/ai.md` when MCP is used by AI agents or workflows
12. `memory/by_area/security.md` when permissions or production data access are involved
13. Current MCP SDK or official docs when API signatures matter

MCP SDK APIs can change.
Verify current SDK method names and transport patterns before implementation.

## MCP Design Flow

Use this flow:

```text
identify real need for MCP
-> confirm existing APIs/tools are not enough
-> define allowed tool actions
-> separate read-only tools from write/destructive tools
-> define schemas for every input/output
-> protect secrets and permissions
-> add approval gates for destructive or paid actions
-> make errors structured and safe
-> add logging/rate/cost controls
-> test with harmless read-only calls first
-> document usage and limits
-> update memory if MCP behavior becomes project standard
```

## Core Concepts

Tools:
Actions the model can invoke. Tools should be narrow, explicit, validated, and safe to call.

Resources:
Read-only data the model can fetch, such as schema metadata, docs, logs, or status snapshots.

Prompts:
Reusable prompt templates the client can surface for consistent workflows.

Transport:
Use stdio for local agent/client use when appropriate.
Use HTTP transport when remote clients or cloud access are required.
Keep tool logic separate from transport where possible.

## Tool Design Rules

- One tool should do one clear action.
- Prefer read-only first.
- Use explicit names.
- Validate all inputs.
- Return typed or structured output.
- Avoid broad tools like `run_any_sql` or `execute_any_command`.
- Avoid tools that accept arbitrary shell commands.
- Add dry-run mode for risky actions where possible.
- Add confirmation or approval step for risky writes.
- Include pagination and limits for list/search tools.
- Include timeouts.
- Handle provider errors safely.
- Keep tool outputs useful but not excessive.
- Do not leak secrets in errors or logs.
- Do not include sensitive raw data in logs.

MCP tools should be narrow and explicit.
Read-only tools should be preferred first.
Write or destructive tools need explicit approval gates.
Paid actions need approval gates.
Production-data access needs approval gates.

## Good MCP Tool Examples

Good tools are narrow, inspectable, and safe by default:

- `list_shopify_orders`
- `get_shopify_order`
- `search_supabase_schema`
- `get_supabase_table_columns`
- `search_logs`
- `get_gcs_object_metadata`
- `check_vertex_job_status`

These tools can help agents diagnose and inspect systems without broad unsafe access.

## Dangerous MCP Tool Examples

Dangerous tools include:

- `run_any_sql`
- `delete_customer`
- `refund_payment`
- `charge_customer`
- `execute_shell_command`
- `update_production_env`
- `bulk_delete_records`

Dangerous tools are not automatically forbidden forever.
They require explicit approval gates, narrow scope, dry-run where possible, audit logging, strong validation, and clear ownership of risk.

## MCP vs Normal Product Code

MCP is often for agents and operators.
Normal SaaS features should usually use normal backend services and provider APIs.

Do not move core business logic into MCP just to make the agent happy.

Product runtime must remain maintainable without hidden MCP dependency unless explicitly designed and approved.

When possible, project code should use normal APIs and services.
MCP is mainly for agent tooling, diagnosis, and controlled operations.

If the SaaS runtime depends on MCP, that must be explicitly approved and documented.

MCP should not hide business logic that belongs in the app.
MCP should not become an undocumented production dependency.

## Secrets and Permissions

Protect secrets and permissions carefully:

- Store secrets in env vars or provider secret storage.
- Never hardcode API keys, service-role keys, tokens, or private credentials.
- Separate read-only credentials from write-capable credentials when possible.
- Use least-privilege provider scopes.
- Do not expose secrets in tool responses.
- Do not log secrets or raw sensitive payloads.
- Validate which user or agent is allowed to call the tool.
- Add approval gates for production data, paid actions, and destructive writes.

## Inputs, Outputs, and Errors

Tool inputs must be validated with Zod, JSON Schema, or an equivalent schema system.

Tool outputs should be structured:

- status
- data
- safe message
- error code when failed
- source/provider metadata when useful
- pagination cursor or limit metadata for list tools

Errors should:

- avoid raw stack traces
- avoid provider secret leaks
- avoid raw customer data
- include safe codes or categories
- preserve enough context for debugging

## Rate, Cost, and Idempotency

Add rate and cost controls when tools call providers or expensive services.

For write-capable tools:

- require explicit approval gates
- use idempotency keys where repeat calls can cause duplicate effects
- add dry-run when possible
- log audit metadata
- make retry behavior explicit

For read tools:

- add limits and pagination
- avoid unbounded scans
- protect production systems from heavy queries

## Provider and Diagnostic Access

MCP can be useful for controlled diagnostic access to:

- Supabase schema, table metadata, RLS policy metadata, and safe query inspection
- Shopify shops, products, orders, webhooks, and app install state
- Google Cloud objects, jobs, logs, service status, and metadata
- n8n workflows or execution status when safe
- application logs and health/status endpoints
- project files and generated artifacts

Use provider/API clients behind the tools.
Normalize provider responses into safe structured output.

## Testing MCP Tools

Test with harmless read-only calls first.

Check:

- input validation rejects bad input
- output shape is stable
- permissions are enforced
- missing env vars fail safely
- provider errors are structured
- pagination and limits work
- logs do not leak sensitive data
- dangerous tools require approval and dry-run where possible

## Memory Updates

Update memory if MCP behavior becomes project standard.

Useful memory notes may include:

- approved MCP tool scope
- provider connections used for diagnostics
- read-only versus write-capable tools
- required env vars without secret values
- production access limits
- approved approval gates

Use the relevant memory area:

- `memory/by_area/integrations.md`
- `memory/by_area/ai.md`
- `memory/by_area/security.md`
- `memory/by_area/backend.md`
- `memory/by_area/environment.md`

## Stop Conditions

Stop and ask Karim if:

- MCP would access production data
- MCP would perform write, destructive, or paid actions
- permissions or secret handling are unclear
- the tool scope is broad or dangerous
- MCP would become required for production runtime
- existing APIs or admin dashboards may be safer
- the MCP server could expose customer data to an AI provider
- provider terms, rate limits, or cost behavior are unclear

## Output Expectations

When reporting MCP work, state:

- why MCP was needed
- which tools, resources, or prompts were created or changed
- whether tools are read-only or write-capable
- what approval gates exist
- what secrets or env vars are required without exposing values
- how inputs are validated
- how errors and logs avoid leaking sensitive data
- how it was tested
- what memory was updated if behavior became stable
