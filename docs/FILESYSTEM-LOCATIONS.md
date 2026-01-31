# Clawdbot Filesystem Locations

This document lists all filesystem locations affected by a Clawdbot installation.

## 1. State Directory (primary data)

| Path | Contents |
|------|----------|
| `~/.clawdbot/` | Main state directory (override: `CLAWDBOT_STATE_DIR`) |
| `~/.clawdbot/clawdbot.json` | Configuration file |
| `~/.clawdbot/credentials/` | OAuth tokens, WhatsApp auth, API keys |
| `~/.clawdbot/sessions/` | Pi session logs (`.jsonl` files) |
| `~/.clawdbot/agents/` | Per-agent directories (e.g., `agents/main/`) |
| `~/.clawdbot/logs/` | Log files (`clawdbot.log`, `commands.log`, `cache-trace.jsonl`) |
| `~/.clawdbot/hooks/` | Managed hooks |
| `~/.clawdbot/extensions/` | Installed plugins |
| `~/.clawdbot/media/` | Media staging for remote attachments |
| `~/.clawdbot/memory/` | SQLite indexes for memory/embeddings |
| `~/.clawdbot/sandboxes/` | Docker sandbox workspace roots |
| `~/.clawdbot/.env` | Environment variables file |
| `~/.clawdbot/exec-approvals.sock` | Exec approval Unix socket |
| `~/.clawdbot/exec-approvals.json` | Exec approval rules |

## 2. Workspace Directory (agent workspace)

| Path | Contents |
|------|----------|
| `~/clawd/` | Default workspace (configurable via `agents.defaults.workspace`) |
| `~/clawd/AGENTS.md` | Agent prompt file |
| `~/clawd/SOUL.md` | Personality/soul file |
| `~/clawd/TOOLS.md` | Tools description |
| `~/clawd/IDENTITY.md` | Identity file |
| `~/clawd/skills/` | Workspace skills |
| `~/clawd/canvas/` | Canvas files (for visual workspace) |
| `~/clawd/memory/` | Session memory markdown files |

## 3. macOS App (optional)

| Path | Contents |
|------|----------|
| `/Applications/Clawdbot.app` | macOS menu bar application |

## 4. System Service (daemon)

| Platform | Location |
|----------|----------|
| macOS | `~/Library/LaunchAgents/` (launchd plist) |
| Linux | `~/.config/systemd/user/` (systemd service) |
| Windows | Scheduled task (schtasks) |

## 5. Temporary/Ephemeral

| Path | Contents |
|------|----------|
| `/tmp/clawdbot-<uid>/` | Gateway lock files |
| System temp dir | Temporary media files |

## 6. Global Install (npm/pnpm)

| Path | Contents |
|------|----------|
| `/usr/local/lib/node_modules/clawdbot/` | npm global install |
| `~/.local/share/pnpm/global/` | pnpm global install |
| `~/.pnpm-global/` | Alternative pnpm location |

## 7. Browser Control (if enabled)

| Path | Contents |
|------|----------|
| Browser-specific profile directory | Dedicated Chrome/Chromium profile for Clawdbot |

## Environment Variables That Override Paths

| Variable | Overrides |
|----------|-----------|
| `CLAWDBOT_STATE_DIR` | `~/.clawdbot` base directory |
| `CLAWDBOT_CONFIG_PATH` | Config file location |
| `CLAWDBOT_OAUTH_DIR` | Credentials directory |

## Cleanup Command

To remove everything, Clawdbot provides:

```bash
clawdbot uninstall --all
```

This removes: service, state (`~/.clawdbot`), workspace (`~/clawd`), and macOS app.

## Related Code

- Path resolution: [`src/config/paths.ts`](../src/config/paths.ts)
- Uninstall logic: [`src/commands/uninstall.ts`](../src/commands/uninstall.ts)
