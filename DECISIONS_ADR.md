# Decision Log (ADR)

Every meaningful technical decision goes here. The goal is to stop re-litigating
architecture choices - if a decision's already made and the reasoning still
holds, move on.

Format:
```
## Decision: <short title>
**Date:**
**Why:**
**Alternatives considered:**
**Revisit when:**
```

---

## Decision: n8n over Make.com for orchestration
**Date:** 2026-08-25
**Why:** n8n has a native MCP Server Trigger node (can expose a workflow as an MCP server directly), supports free self-hosting with no usage ceiling, and gives real code nodes (JS/Python with npm access). Also a stated learning objective for this project.
**Alternatives considered:** Make.com - better free-tier op allowance and a more polished visual builder, but no self-hosting option and no MCP primitive; would require custom webhook bridges to approximate MCP.
**Revisit when:** Op volume or team-collaboration needs outgrow a self-hosted single instance.

## Decision: Self-hosted via Docker over n8n Cloud
**Date:** 2026-08-25
**Why:** n8n Cloud's free trial expired. Self-hosting is free indefinitely and required for a reliable unattended 5am scheduled run (n8n Cloud's paid tiers cost money for something Docker gives for free).
**Alternatives considered:** n8n Cloud paid tier; Oracle Cloud free-tier VPS (deferred for now - starting local on laptop via Docker, will migrate to an always-on VPS once workflows are proven, since a laptop must be awake and running for scheduled triggers to fire).
**Revisit when:** Ready to move off laptop-dependent execution and need guaranteed uptime for the 5am trigger.

## Decision: SQLite over Postgres for state / change-detection
**Date:** 2026-08-25
**Why:** Keeps the V1 Docker stack to a single container (just n8n). SQLite is sufficient for tracking "what have I already seen" per repository at this scale.
**Alternatives considered:** Postgres - better suited if querying historical data across time or dashboarding becomes a goal, and would double as SQL/schema-design practice. Explicitly deferred rather than rejected.
**Revisit when:** Want multi-table historical queries, dashboarding, or deliberate SQL practice.

## Decision: OpenAI API over Claude API for LLM analysis
**Date:** 2026-08-25
**Why:** An OpenAI API key is already set up and ready to use, avoiding the setup friction of creating a new API key/provider account just for V1.
**Alternatives considered:** Claude API (Haiku for cost-efficient summarization, or Sonnet for stronger "why it matters" reasoning) - viable fallback if OpenAI's output quality on importance-ranking/breaking-change judgment proves insufficient.
**Revisit when:** Existing OpenAI setup no longer meets needs, or output quality on structured analysis is inadequate.

## Decision: n8n Gmail node over SMTP / transactional email service
**Date:** 2026-08-25
**Why:** Lowest-friction delivery path for V1 - OAuth configured directly in the n8n UI, no separate SMTP credentials or third-party service account to manage.
**Alternatives considered:** SMTP; Resend/SendGrid - more control over deliverability and templating, but unnecessary complexity for a single-recipient V1.
**Revisit when:** Need better deliverability guarantees, richer templating, or send volume outgrows Gmail's sending limits.

## Decision: n8n built-in GitHub node over HTTP Request node
**Date:** 2026-08-27
**Why:** Faster to configure - native resource/operation dropdowns (release, issue, pullRequest, etc.) instead of hand-building API calls.
**Alternatives considered:** HTTP Request node - full control over the raw REST/GraphQL API and better visibility into request/response shape for learning; required for GitHub Security Advisories, which the built-in node doesn't expose as a resource.
**Revisit when:** Building the security-advisory pull (part of V1's detect list) - will likely need the HTTP Request node for that piece specifically, so this may end up a hybrid rather than either/or.

## Decision: Postgres over SQLite for state / change-detection
**Date:** 2026-08-30
**Why:** n8n has no native SQLite node - confirmed via n8n's own docs and its integrations catalog, which lists 205 data/storage integrations including Postgres, MySQL, and Microsoft SQL Server but no SQLite.
SQLite has only ever been an option for n8n's own internal backend (workflows/credentials), never a data source a workflow can query.
Postgres is a native n8n node with a proper hand-written SQL interface, matches how real n8n business-automation workflows store state, and keeps the SQL-writing practice this project originally chose SQLite for.
**Alternatives considered:** n8n's native Data Table node - easiest to wire up, but n8n-proprietary and doesn't teach a transferable skill.
Shelling out to the sqlite3 CLI via an Execute Command node - keeps a real SQLite file but isn't an idiomatic n8n pattern.
A community SQLite node - would need community packages enabled and an unverified third-party dependency.
**Revisit when:** Moving to a public-facing product (Milestone 8+), at which point Supabase (self-hosted, the same open-source Postgres/Auth/Storage/Realtime stack that powers supabase.com) becomes worth adopting for its bundled auth and auto-generated API - noted as a V2+ direction, not needed for the current n8n-only workflow.
**Supersedes:** "SQLite over Postgres for state / change-detection" (2026-08-25).

## Decision: Execute Query (raw SQL) over built-in Select/Insert operations for the Postgres nodes
**Date:** 2026-08-31
**Why:** Keeps every database step - table creation, the seen-items lookup, and the write - in hand-written SQL, consistent across all three Postgres nodes, and makes the ON CONFLICT (item_type, source_id, repo) DO NOTHING clause explicit rather than expressed through the "Insert or Update" operation's resource-mapper UI.
**Alternatives considered:** The Postgres node's built-in Select and Insert/Upsert operations - faster to configure via dropdowns, but hides the actual SQL and makes an explicit ON CONFLICT clause less direct to express.
**Revisit when:** The query set grows complex enough that hand-written SQL becomes harder to maintain than the structured operations would be.
