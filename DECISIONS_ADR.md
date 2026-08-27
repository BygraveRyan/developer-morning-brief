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
