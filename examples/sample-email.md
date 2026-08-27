# Sample email output

Target shape for **Milestone 5: Generate the email**. This isn't literal HTML
markup - it's the structure, tone, and grouping the real HTML template should
reproduce: four severity tiers, one line of context per item, and an explicit
"why you care" or "recommended action" wherever it adds signal.

The repos below are illustrative - they're meant to show the kind of breadth
a developer, engineer, or architect would realistically track (AI tooling,
cloud/infra, data engineering, core frameworks), not a fixed list. The real
list is whatever repos the user configures.

```
☀️ Your Developer Brief — Monday
12 updates across 18 repositories

🔴 Important
n8n — v1.XX released
New AI Agent functionality + changes to credential handling.
Why you care: You use n8n in your automation stack.
→ Read release notes

LangChain — v0.X breaking change
Deprecated legacy chain interface removed.
Why you care: Any code using the old `Chain` classes will break on upgrade.
→ Read migration guide

🟡 Worth knowing
LangGraph — 3 merged PRs
New persistence functionality and changes to streaming.

Terraform — 5 merged PRs
Provider plugin protocol updates ahead of next major.

🟢 Minor
FastAPI
14 commits since Friday. No breaking changes detected.

dbt-core
6 commits since Friday. Docs and test coverage updates only.

🛡️ Security
Supabase
New security advisory affecting versions X–Y.
Recommended action: Upgrade.

vLLM
New security advisory (moderate severity) — potential DoS via malformed request.
Recommended action: Review advisory, patch available in latest release.
```
