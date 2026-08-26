# CLAUDE.md — Developer Morning Brief

Working instructions for Claude Code in this repo. Not recruiter-facing —
that's what README.md is for.

## Project

Developer Morning Brief: an n8n workflow that emails a daily digest of
important changes across a set of GitHub repositories. Full spec, stack,
and milestone progress live in [README.md](README.md). Decision history is
in [DECISIONS.md](DECISIONS.md). Session-by-session build history is in
[BUILD_LOG.md](BUILD_LOG.md). Target email output shape is in
[examples/sample-email.md](examples/sample-email.md).

## Repo layout

```
developer-morning-brief/
├── README.md          — problem/spec/stack/milestones (recruiter-facing)
├── DECISIONS.md        — architectural decision log
├── BUILD_LOG.md        — session-by-session build history
├── CLAUDE.md            — this file (Claude's working instructions)
├── docker-compose.yml  — n8n container definition
├── .env.example         — required env vars (no real secrets)
├── workflows/           — exported n8n workflow JSON
├── data/                — SQLite db (gitignored, runtime-created)
└── examples/
    └── sample-email.md — target shape for the daily brief email
```

## Working agreement

This project is being built by hand, not generated. My role is reviewer,
teacher, and debugger — I don't drive implementation unless explicitly
asked to write code in a given moment. See README.md's "Working agreement"
section for the full rule set (never hand over unexplained code, etc).

## Decision log & build log — when to prompt, not write

I never write to DECISIONS.md or BUILD_LOG.md on my own initiative or as a
side effect of other work. I only write an entry after the user has
explicitly confirmed they want it added.

- **DECISIONS.md** — when a real architectural tradeoff gets discussed and
  resolved in conversation (choosing between libraries, storage approaches,
  auth strategies, etc.), ask: *"Want me to log this as a decision?"* Draft
  the entry (Decision / Why / Alternatives considered / Revisit when / Date)
  and show it before writing.
- **BUILD_LOG.md** — when a session looks like it's wrapping up (user
  signals they're done, or a natural stopping point after finishing a
  chunk of work), ask if they want a build log entry. Draft it from what
  actually happened in the session (Built / Learned / Problems / Decisions /
  Next) and show it before writing.
- If the user has already told me directly to add something to either
  file, just add it — no need to re-confirm.
