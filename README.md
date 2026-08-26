# Developer Morning Brief

## Problem
I follow too many repos/tools and can't reliably keep up with important changes.

## User
Developers / AI engineers / data engineers

## V1 outcome
Every morning, receive one clean email containing the important changes
across my selected GitHub repositories.

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

## Setup
1. Copy `.env.example` to `.env` and fill in values.
2. `docker compose up -d`
3. n8n UI: [http://localhost:5678](http://localhost:5678)
4. Import workflows from `workflows/` (once exported).

## Repo structure
- `workflows/` — exported n8n workflow JSON
- `data/` — SQLite database (gitignored, created at runtime)
- `docker-compose.yml` — n8n container definition
