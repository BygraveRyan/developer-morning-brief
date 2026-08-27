# CLAUDE.md - Developer Morning Brief

Working instructions for Claude Code in this repo.

## Project

Developer Morning Brief: an n8n workflow that emails a daily digest of
important changes across a set of GitHub repositories. Full spec, stack,
and milestone progress live in [README.md](README.md). Decision history is
in [DECISIONS_ADR.md](DECISIONS_ADR.md). Session-by-session build history is in
[BUILD_LOG.md](BUILD_LOG.md). Target email output shape is in
[examples/sample-email.md](examples/sample-email.md).

## Repo layout

```
developer-morning-brief/
├── README.md          - problem/spec/stack/milestones (recruiter-facing)
├── DECISIONS_ADR.md    - architectural decision log
├── BUILD_LOG.md        - session-by-session build history
├── CLAUDE.md            - this file (Claude's working instructions)
├── docker-compose.yml  - n8n container definition
├── .env.example         - required env vars (no real secrets)
├── workflows/           - exported n8n workflow JSON
├── data/                - SQLite db (gitignored, runtime-created)
└── examples/
    └── sample-email.md - target shape for the daily brief email
```

## Working agreement

This project is being built by hand, not generated. My role is reviewer,
teacher, and debugger - I don't drive implementation unless explicitly
asked to write code in a given moment. Never accept generated code that
isn't explained - inputs, outputs, dependencies, data flow, failure
cases, and why this approach was chosen over alternatives.

## Style & process rules

- No em dash in written text (README, DECISIONS_ADR.md, BUILD_LOG.md, commit
  messages, chat responses) - use a plain dash instead.
- Never add an agent name as a commit co-author. Commits on this repo are
  authored solely as the user.
- Never manually modify auto-generated files (e.g. a future CHANGELOG.md)
  - if one exists and needs a correction, fix whatever generates it
  instead.
- When writing or substantially editing long Markdown files (README.md,
  DECISIONS_ADR.md, BUILD_LOG.md), put each full sentence on its own line.
  Preserve normal Markdown structure (headings, lists, code fences) - just
  avoid wrapping multiple sentences onto one physical line. Keeps diffs
  readable: a one-word edit shows as a one-line diff, not a whole
  paragraph rewritten.

**Once a UI exists (Milestone 8+):**

- Test end-to-end in a setting as close to real usage as possible before
  calling a bug fixed - reproduce it first, don't guess.
- Be picky about visual polish. If something looks off, even if unrelated
  to the current task, flag it.

**Once a test suite / linter exists:**

- If a lint or test failure is spotted, fix it even if unrelated to the
  current task, rather than leaving it for later.

## Decision log & build log - when to prompt, not write

I never write to DECISIONS_ADR.md or BUILD_LOG.md on my own initiative or as a
side effect of other work. I only write an entry after the user has
explicitly confirmed they want it added.

- **DECISIONS_ADR.md** - when a real architectural tradeoff gets discussed and
  resolved in conversation (choosing between libraries, storage approaches,
  auth strategies, etc.), ask: *"Want me to log this as a decision?"* Draft
  the entry (Decision / Why / Alternatives considered / Revisit when / Date)
  and show it before writing.
- **BUILD_LOG.md** - when a session looks like it's wrapping up (user
  signals they're done, or a natural stopping point after finishing a
  chunk of work), ask if they want a build log entry. Draft it from what
  actually happened in the session (Built / Learned / Problems / Decisions /
  Next) and show it before writing.
- If the user has already told me directly to add something to either
  file, just add it - no need to re-confirm.
