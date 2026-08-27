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
what changed and whether it's worth reacting to today — turning silence
into a signal, not a blind spot.

## V1 inputs
- GitHub repositories

## V1 detects
- Releases
- Security advisories
- Significant PR/issue activity
- Breaking changes

## V1 output
- Daily HTML email
- Importance
- Summary
- Why it matters
- Recommended action
- Link to source

See [examples/sample-email.md](examples/sample-email.md) for a sample of the target output.

## V1 stack
- **Orchestration:** n8n (self-hosted via Docker)
- **Source data:** GitHub API
- **State / change detection:** SQLite
- **Analysis:** OpenAI API (structured outputs)
- **Delivery:** n8n Gmail node

```
                    GitHub API
                        ↓
                 n8n orchestration
                        ↓
                     SQLite
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

Small, sequential slices — not "build the app."

- [ ] 1. Get GitHub data
- [ ] 2. Detect what's new
- [ ] 3. Store it
- [ ] 4. Have an LLM classify/summarise it
- [ ] 5. Generate the email
- [ ] 6. Schedule it
- [ ] 7. Use it yourself for a week
- [ ] 8. Put a landing page in front of it

## Future additions (V2+)

Ideas raised during design that are deliberately deferred past V1 rather
than dropped — keeps scope honest without losing the idea.

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
- `workflows/` — exported n8n workflow JSON
- `data/` — SQLite database (gitignored, created at runtime)
- `docker-compose.yml` — n8n container definition
