# claude

## Setting (config)

### user-level settings

- storage - `~/.claude/settings.json`
### shell

```sh
claude config set --global preferReducedMotion true
```

## instructions

#### user-level instructions

Global instructions for every project, every session

- Storage - `~/.claude/CLAUDE.md`
- Shown in `/context` under "Memory files"
- Project-level CLAUDE.md files add on top of this — they don't override it

```sh
# check file
cat ~/.claude/CLAUDE.md 2>/dev/null || echo "FILE NOT FOUND"
ok

```


## Memory

- storage `~/.claude/projects/*/memory/MEMORY.md` — loaded per-project


## Built-in Commands

### Session Management

#### shell

```sh
claude -c # (or claude --continue ) to resume the last conversation

claude --resume # to pick from recent conversations
```

#### internal

- `/clear` - Clear conversation history
- `/reset` - Reset the current session
- `/tasks` - View background tasks and running agents

### Skills (Slash Commands)

These are specialized workflows you can invoke:

- `/commit` - Create a git commit with AI-generated message
- `/review-pr` - Review a pull request
- Use `/help` to see all available skills

## .claude

### Custom

- **`notes.md`** — Personal notes file.

### Global Config

- **`CLAUDE.md`** — Global instructions. Loaded into the system prompt at the start of every session, in every project. Rules like "never auto-commit", "respond in English", etc.
- **`settings.json`** — Global preferences: preferred model, update channel, minimum version. Loaded every session.
- **`remote-settings.json`** — Telemetry/OTEL endpoint configuration with auth token. Set by organization or Anthropic. Contains sensitive credentials.
- **`policy-limits.json`** — Feature restrictions pushed server-side (e.g., `allow_remote_control: false`). Not user-editable.
- **`history.jsonl`** — Index of all past sessions (IDs, timestamps, working dirs). Powers `claude --resume` to list and pick a session to continue.
- **`stats-cache.json`** — Aggregated usage stats: messages/day, tokens by model, session counts. Powers the `/stats` command.

### Per-Project State

**`projects/<project-name>/`** — Per-project directory, named by path (e.g., `-Users-xsolla-user-dotfiles`).

- `memory/MEMORY.md` — Auto memory. Claude writes observations here (patterns, preferences, mistakes). **Auto-loaded** when working in that project. Persists across sessions.
- `settings.local.json` — Project-specific tool permissions (which tools are auto-allowed).
- `*.jsonl` — Session transcripts. Full conversation logs. Used by `claude --resume` / `claude -c` to continue a session.
- `subagents/` — Conversation logs from sub-agents (Task tool). Tied to sessions, not reused.

### Session Working Files

- **`plans/<random-name>.md`** — Plan mode files. Created when entering plan mode, referenced during that session. **Not auto-loaded** in new sessions. Session-scoped working artifact.
- **`todos/*.json`** — Task lists as JSON. One file per session/task list. Shown via `Ctrl+T`. Persist on disk but **not auto-loaded** in new sessions unless `CLAUDE_CODE_TASK_LIST_ID` env var points at a specific list.

### Internal Backups & Cache

- **`backups/`** — Auto-backups of `.claude.json` (auth/account config). Timestamped, rotated automatically. Internal safety net — not related to the dotfiles backup system.
- **`cache/`** — Cached data like the changelog. Regenerable.
- **`plugins/`** — Downloaded marketplace plugins. ~6.5MB. Fully re-downloadable, no unique state.

### Edit & Shell History

- **`file-history/<session-uuid>/<hash>@v1,v2,...`** — Version history of every file edited during a session. Each file gets a content hash and incrementing versions. Powers undo/rollback within a session. Not reused later.
- **`shell-snapshots/snapshot-zsh-*.sh`** — Capture of shell state (functions, aliases, env vars) at session start. Claude reads this to understand the shell environment.
- **`paste-cache/<hash>.txt`** — Content pasted into Claude Code. Stored by content hash so large pastes don't bloat conversation context. Referenced during the session, not reused.
- **`session-env/<session-uuid>/`** — Per-session environment capture directory. Meant to store env vars and working directory at session start. Often empty — created as a placeholder.
- **`downloads/`** — Landing spot for files downloaded during a session (e.g., via WebFetch). Empty if unused.

### Telemetry & Analytics

- **`debug/`** — Debug/diagnostic logs. Large (~82MB). Regenerated each session.
- **`telemetry/`** — Analytics data sent to Anthropic.
- **`statsig/`** — Feature flag evaluation cache (Statsig is Anthropic's feature flag service).

### Project-Level Files (inside the repo, not ~/.claude/)

- **`<project>/.claude/settings.local.json`** — Project-scoped tool permissions. Same role as the one in `~/.claude/projects/`.
- **`<project>/CLAUDE.md`** — Project-specific instructions. Loaded alongside the global `~/.claude/CLAUDE.md` when working in that project.
