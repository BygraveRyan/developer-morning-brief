# Developer Morning Brief

## Problem
I need to stay current with the tools and services I actually rely on, so I catch new releases and news early, and get critically notified as soon as possible about any downstream-breaking changes.

## User
Developers, engineers, and architects who depend on fast-moving open
source tools and cloud services. Anyone whose systems break when an
upstream dependency changes underneath them already knows the drill:
too many repos to watch manually, and no reliable way to know something
important happened until it's already caused a problem.

## V1 outcome
Every morning, receive one clean email containing the important changes
across my selected GitHub repositories. Instead of checking a dozen tabs
or hoping a GitHub notification didn't get buried, one email tells you
what changed and whether it's worth reacting to today - turning silence
into a signal, not a blind spot.

## V1 inputs
- GitHub repositories
- Your stack config (optional) - the languages, frameworks, and tools you actually depend on, so severity gets judged against what could really affect you, not just generic importance

## V1 detects
- Releases
- Security advisories
- Significant PR/issue activity
- Breaking changes

## V1 output
- Daily HTML email
- Severity (red / amber / green)
- Summary
- Why it matters, assessed against your configured stack
- Recommended action
- Link to source

See [examples/sample-email.md](examples/sample-email.md) for a sample of the target output.

## How it works
1. Pulls the latest activity from the GitHub API for each repo you're tracking.
2. Stores and tracks it in Postgres, so nothing gets flagged twice once change detection lands.
3. The LLM checks each change against your stack config, grades severity red to green, and writes the summary and recommended action.
4. n8n pushes the finished email straight to your inbox.

## V1 stack
| Layer | Technology |
| --- | --- |
| Orchestration | n8n (self-hosted via Docker) |
| Source data | GitHub API |
| State / change detection | Postgres (self-hosted via Docker) |
| Analysis | OpenAI API (structured outputs) |
| Delivery | n8n Gmail node |

**Development tooling:** built with Claude Code as reviewer and debugger (see `CLAUDE.md` for the working agreement), and `n8n-mcp` for inspecting and troubleshooting workflows directly during development.

```
                    GitHub API
                        ↓
                 n8n orchestration
                        ↓
                    Postgres
                        ↓
                Change detection
                        ↓
                  LLM analysis
                        ↓
              Structured briefing
                        ↓
                   HTML email
                        ↓
                    Your inbox
```

## Milestones

Small, sequential slices - not "build the app."

- [x] 1. Get GitHub data
- [x] 2. Detect & store what's new (releases)
- [ ] 3. Expand to issues, PRs, and security advisories
- [ ] 4. Have an LLM classify/summarise it
- [ ] 5. Generate the email
- [ ] 6. Schedule it
- [ ] 7. Expand from one repo (n8n-io/n8n) to a real tracked list (10+ repos)
- [ ] 8. Run it for real - use daily until confident it's reliable and the output is actually useful

## Future additions (V2+)

Ideas raised during design that are deliberately deferred past V1 rather
than dropped - keeps scope honest without losing the idea.

- **OSV.dev as a security data source.** [OSV.dev](https://osv.dev) is a
  Google-backed open source vulnerability database with a public API,
  purpose-built around package ecosystems (npm, PyPI, Go, Maven, crates.io,
  etc.) and source repos/commit ranges. It aggregates GitHub Security
  Advisories plus 20+ other sources into one consistent schema, making it a
  broader signal than GHSA alone. V1 sticks to GitHub Security Advisories
  since that's already native to the GitHub API being pulled from; OSV.dev
  is worth adding once V1 is proven.

## Setup
1. Copy `.env.example` to `.env` and fill in values.
2. `docker compose up -d`
3. n8n UI: [http://localhost:5678](http://localhost:5678)
4. Import workflows from `workflows/` (once exported).

## Repo structure
- `workflows/` - exported n8n workflow JSON
- `docker-compose.yml` - n8n and Postgres container definitions
