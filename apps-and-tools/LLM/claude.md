# claude

## basic info

### How Claude Was Built 

Pre-training: base model trained to predict next token on massive text corpus. Learns language, facts, reasoning

RLHF: human raters rank responses → train a reward model → RL nudges base model toward higher-scored outputs. Fragile — reward models can be gamed, raters are inconsistent, fluency can score higher than accuracy.

Constitutional AI: model critiques and revises its own outputs against a set of principles. More scalable than pure human rating. Failure mode: critique uses the same weights as generation — systematic blind spots survive self-review.

Key implications:

- Claude declines based on internalized values, not keyword filters — it can reason about why
- Values create tendencies, not deterministic rules — same question can get different answers across sessions
- Training on human text inherits human errors and biases

## Setting (config)

### user-level settings

- storage - `~/.claude/settings.json`

### project-level settings

`.claude/`

- settings.json
- ﻿﻿settings.local.json

TBD

### settings setup via shell

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

- `notes.md` — Personal notes file.

### Global Config

- `CLAUDE.md` — Global instructions. Loaded into the system prompt at the start of every session, in every project. Rules like "never auto-commit", "respond in English", etc.
- `settings.json` — Global preferences: preferred model, update channel, minimum version. Loaded every session.
- `remote-settings.json` — Telemetry/OTEL endpoint configuration with auth token. Set by organization or Anthropic. Contains sensitive credentials.
- `policy-limits.json` — Feature restrictions pushed server-side (e.g., `allow_remote_control: false`). Not user-editable.
- `history.jsonl` — Index of all past sessions (IDs, timestamps, working dirs). Powers `claude --resume` to list and pick a session to continue.
- `stats-cache.json` — Aggregated usage stats: messages/day, tokens by model, session counts. Powers the `/stats` command.

### Per-Project State

`projects/<project-name>/` — Per-project directory, named by path (e.g., `-Users-xsolla-user-dotfiles`).

- `memory/MEMORY.md` — Auto memory. Claude writes observations here (patterns, preferences, mistakes). Auto-loaded when working in that project. Persists across sessions.
- `settings.local.json` — Project-specific tool permissions (which tools are auto-allowed).
- `*.jsonl` — Session transcripts. Full conversation logs. Used by `claude --resume` / `claude -c` to continue a session.
- `subagents/` — Conversation logs from sub-agents (Task tool). Tied to sessions, not reused.

### Session Working Files

- `plans/<random-name>.md` — Plan mode files. Created when entering plan mode, referenced during that session. Not auto-loaded in new sessions. Session-scoped working artifact.
- `todos/*.json` — Task lists as JSON. One file per session/task list. Shown via `Ctrl+T`. Persist on disk but not auto-loaded in new sessions unless `CLAUDE_CODE_TASK_LIST_ID` env var points at a specific list.

### Internal Backups & Cache

- `backups/` — Auto-backups of `.claude.json` (auth/account config). Timestamped, rotated automatically. Internal safety net — not related to the dotfiles backup system.
- `cache/` — Cached data like the changelog. Regenerable.
- `plugins/` — Downloaded marketplace plugins. ~6.5MB. Fully re-downloadable, no unique state.

### Edit & Shell History

- `file-history/<session-uuid>/<hash>@v1,v2,...` — Version history of every file edited during a session. Each file gets a content hash and incrementing versions. Powers undo/rollback within a session. Not reused later.
- `shell-snapshots/snapshot-zsh-*.sh` — Capture of shell state (functions, aliases, env vars) at session start. Claude reads this to understand the shell environment.
- `paste-cache/<hash>.txt` — Content pasted into Claude Code. Stored by content hash so large pastes don't bloat conversation context. Referenced during the session, not reused.
- `session-env/<session-uuid>/` — Per-session environment capture directory. Meant to store env vars and working directory at session start. Often empty — created as a placeholder.
- `downloads/` — Landing spot for files downloaded during a session (e.g., via WebFetch). Empty if unused.

### Telemetry & Analytics

- `debug/` — Debug/diagnostic logs. Large (~82MB). Regenerated each session.
- `telemetry/` — Analytics data sent to Anthropic.
- `statsig/` — Feature flag evaluation cache (Statsig is Anthropic's feature flag service).

### Project-Level Files (inside the repo, not ~/.claude/)

- `<project>/.claude/settings.local.json` — Project-scoped tool permissions. Same role as the one in `~/.claude/projects/`.
- `<project>/CLAUDE.md` — Project-specific instructions. Loaded alongside the global `~/.claude/CLAUDE.md` when working in that project.

## Anthropic API

### The messages format

Every API call is stateless. You send the full conversation each time. 

Core structure:

```python
import anthropic

client = anthropic.Anthropic(api_key="your_key")

response = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=1024,
    system="You are a senior backend engineer.",
    messages=[
        {"role": "user", "content": "What is a race condition?"}
    ]
)

print(response.content[0].text)
```

### Key parameters

|Parameter|What it does|Note|
|---|---|---|
|`model`|Which Claude|Match to task complexity and cost|
|`max_tokens`|Hard output cap|Model stops mid-sentence if hit|
|`temperature`|Sampling randomness|Rule T-02|
|`system`|System prompt|Persona, constraints, format|
|`stop_sequences`|Halt generation on match|Useful for structured outputs|
|`stream`|Stream tokens as generated|Better UX for long responses|

### Multi-turn conversations

Model is stateless — you rebuild history every call:

```python
messages = []

# Turn 1
messages.append({"role": "user", "content": "What is a race condition?"})
response = client.messages.create(
    model="claude-sonnet-4-6", max_tokens=1024, messages=messages
)
messages.append({"role": "assistant", "content": response.content[0].text})

# Turn 2
messages.append({"role": "user", "content": "Give me a Python example."})
response = client.messages.create(
    model="claude-sonnet-4-6", max_tokens=1024, messages=messages
)
```

Every turn sends the full history. This is where T-03 becomes a real cost concern — history grows linearly, compute cost grows with it.

### Streaming

Without streaming the API waits until the full response is ready. With streaming tokens arrive as generated:

```python
with client.messages.stream(
    model="claude-sonnet-4-6",
    max_tokens=1024,
    messages=[{"role": "user", "content": "Explain transformers."}]
) as stream:
    for text in stream.text_stream:
        print(text, end="", flush=True)
```

### Token counting and cost

Before an expensive call you can count tokens:

```python
response = client.messages.count_tokens(
    model="claude-sonnet-4-6",
    messages=[{"role": "user", "content": "Your prompt here"}]
)
print(response.input_tokens)
```

Pricing: per million input tokens + per million output tokens. Output costs more than input. A long system prompt repeated every call adds up fast.

### Prompt Caching

Anthropic supports prompt caching — if you send the same prefix (system prompt + static context) repeatedly, the KV cache for those tokens gets stored server-side for up to 5 minutes. Subsequent calls that hit the cache are charged at ~10% of normal input token cost.

How to enable it — mark the cacheable prefix with a cache control breakpoint:

python

```python
response = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=1024,
    system=[{
        "type": "text",
        "text": "You are a senior backend engineer...",
        "cache_control": {"type": "ephemeral"}
    }],
    messages=[
        {"role": "user", "content": "What is a race condition?"}
    ]
)
```

Everything up to the breakpoint gets cached. Works best for: long system prompts, large static documents injected as context, few-shot example blocks that never change.

**Key constraint:** cache TTL is 5 minutes. High-frequency apps benefit most — if calls are spaced more than 5 minutes apart the cache misses and you pay full price.

### Claude Web vs API — key differences

||Claude Web|API|
|---|---|---|
|Model|Set by Anthropic|You choose|
|Temperature|Set by Anthropic|You control|
|System prompt|Not available|Full control|
|History|Managed by UI|You manage|
|Cost|Subscription|Per token|
|Use case|Human interaction|Building products|
