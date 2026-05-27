> ## ⚠️ Archived — merged into Dograh
>
> This project has been merged into [**stefandsl/dograh**](https://github.com/stefandsl/dograh) as the
> Telegram IM channel. The Telegram bot now lives at
> [`telegram-bot/`](https://github.com/stefandsl/dograh/tree/main/telegram-bot)
> there, with a UI for managing bot tokens at `/channels/im` and a 9-button
> Syntx-style menu wired into Dograh's workflow engine.
>
> See [`docs/channels/telegram.md`](https://github.com/stefandsl/dograh/blob/main/docs/channels/telegram.md)
> for setup, and [`docs/adr/ADR-100..103`](https://github.com/stefandsl/dograh/tree/main/docs/adr)
> for the design notes from the merge.
>
> This repository is read-only; open issues and PRs against Dograh instead.

# CliClaw

Universal AI assistant in Telegram. Choose your backend: **Claude Code**, **Gemini CLI** (free), or **Codex CLI**.

## Features

- **Multi-backend**: Claude Code, Codex CLI, Qwen Code — choose at install
- **Voice messages**: Groq Whisper API (free)
- **Session management**: create, switch, close — inline buttons
- **Memory vault**: auto-saves facts from conversations, injects context from past sessions
- **Scheduled tasks**: natural language cron ("remind me tomorrow at 14:00")
- **Self-update**: /update command from Telegram
- **Image support**: send photos for vision analysis
- **One-command install** on Ubuntu/Debian VPS

## Install

```bash
curl -fsSL https://raw.githubusercontent.com/a-prs/CliClaw/main/install.sh -o /tmp/install.sh && sudo bash /tmp/install.sh
```

Or with backend pre-selected:
```bash
sudo bash /tmp/install.sh --backend=claude
```

## Backends

| Backend | Auth | Cost |
|---|---|---|
| Claude Code | API key | Paid (Anthropic API) |
| Gemini CLI | API key | **Free** tier (10-50 RPM, no credit card) |
| Codex CLI | Phone login or API key | Paid (OpenAI API / ChatGPT subscription) |

## Commands

- `/menu` — control panel
- `/new` — new session
- `/sessions` — session list
- `/status` — system status (backend, memory, voice)
- `/setup` — configure voice, API keys
- `/update` — update bot from GitHub

## Memory

The bot automatically:
- Saves session logs to `workspace/memory/sessions/`
- Extracts explicit "remember" requests to `workspace/memory/facts.md`
- Searches memory before each prompt and injects relevant context

FTS5 full-text search — finds facts in <1ms.

## Server Requirements

- 1 vCPU, 512MB+ RAM (swap auto-created if <1.5GB)
- Ubuntu 22.04+ / Debian 11+
- No GPU needed

## Architecture

```
bot/
  main.py          — Telegram bot (aiogram 3.x)
  runner.py        — Generic CLI runner with queue
  backends/        — Claude, Codex, Qwen strategies
  memory/          — Vault (markdown files) + FTS5 search + hooks
  voice.py         — Groq Whisper API
  formatting.py    — Markdown → Telegram HTML
  scheduler.py     — Cron-like task scheduler
```

## License

MIT
