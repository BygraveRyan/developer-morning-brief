# Build Log

Session-by-session record of what was built, learned, hit, decided, and what's
next. This is the honest paper trail of how the system actually got built -
useful for picking up where you left off, and later as raw material for
LinkedIn posts or interview stories.

Add one entry per session, oldest at bottom or top - pick one and stay consistent.

Template:
```
## <DD Mon YYYY>

### Built
-

### Learned
-

### Problems
-

### Decisions
- (cross-reference DECISIONS.md if a real architectural decision was made)

### Next
-
```

## 27 Aug 2026

### Built
- Milestone 1 workflow.
Manual Trigger -> GitHub node (release/getAll) pulling the 5 most recent releases for n8n-io/n8n, using the built-in GitHub node and existing credential.
- Exported and committed as workflows/developer-morning-brief.json.

### Learned
- n8n persists workflows to its own internal DB in the n8n_data Docker volume - that's "saved" but not version-controlled.
Exporting to JSON and committing is a separate step to get real git checkpoints.
- The resource locator pattern ({__rl, mode, value}) n8n uses on fields like owner/repository - can search live, take a URL, or a typed name.
- The built-in GitHub node has no "security advisory" resource - relevant later since V1's detect list includes security advisories.

### Problems
- None blocking.

### Decisions
- See DECISIONS.md: GitHub node over HTTP Request node.

### Next
- Milestone 1 PR (with a canvas screenshot).
- Milestone 2: detect what's new (diff against SQLite state).
