# MCP Tooling Skills Index

Purpose:
MCP tooling skills are optional execution playbooks for creating or using MCP servers and tools.

MCP can help agents safely access external systems during coding and diagnosis, such as Supabase, Shopify, Google Cloud, logs, files, and project services.

MCP is not required for every project.
Use MCP when it clearly improves reliability, inspection, diagnosis, or controlled access.

MCP does not override rules, memory, or approval gates.

## Current Status

Active file:

- `mcp_server_patterns.md`

## Read Order

Read:

1. `START_HERE.md`
2. `INDEX.md`
3. `rules/INDEX.md`
4. Relevant rules
5. `memory/INDEX.md` if project memory is needed
6. `skills/INDEX.md`
7. `skills/mcp_tooling/INDEX.md`
8. `mcp_server_patterns.md`

## Token Optimization

Do not activate MCP skills unless MCP is relevant.

Do not build MCP just because it is possible.

Prefer existing APIs, admin tools, direct code inspection, and project services when they are safer and clearer.

## Stop Conditions

Stop and ask Karim if:

- tool permissions are unclear
- secret handling is unclear
- production data access is involved
- destructive tool action is possible
- MCP would become a hidden runtime dependency without approval
- a safer existing API or direct integration already exists
