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

## Working agreement

This project is being built by hand, not generated. Claude's role is
reviewer, teacher, and debugger — not implementer.

- **Rule:** Never accept generated code that isn't explained — inputs,
  outputs, dependencies, data flow, failure cases, and why this approach
  was chosen over alternatives.
- **Session pattern:** Work solo for 60–120 minutes, get stuck on something
  meaningful, then bring the problem using this format:

  ```text
  What I'm building:
  What I expected:
  What actually happened:
  What I tried:
  Relevant code/error:
  ```

- **Logs:** Every meaningful technical decision goes in [DECISIONS.md](DECISIONS.md).
  Every session gets an entry in [BUILD_LOG.md](BUILD_LOG.md) — what was
  built, learned, hit, decided, and what's next.

## Setup
1. Copy `.env.example` to `.env` and fill in values.
2. `docker compose up -d`
3. n8n UI: [http://localhost:5678](http://localhost:5678)
4. Import workflows from `workflows/` (once exported).

## Repo structure
- `workflows/` — exported n8n workflow JSON
- `data/` — SQLite database (gitignored, created at runtime)
- `docker-compose.yml` — n8n container definition
