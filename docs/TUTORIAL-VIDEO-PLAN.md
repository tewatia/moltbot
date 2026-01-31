# Clawdbot Tutorial Video Plan

## Goal

Create a comprehensive tutorial video covering Clawdbot setup, integrations, and advanced features. Target audience: developers and power users who want a personal AI assistant across multiple platforms.

## Video Structure

### Part 1: Introduction (3-5 min)
- What is Clawdbot?
- Key value proposition: personal AI assistant on YOUR devices
- High-level architecture overview (Gateway → Channels → Agent)

### Part 2: Basic Setup (10-15 min)
- Prerequisites (Node ≥22)
- Installation (`npm install -g clawdbot@latest`)
- Onboarding wizard (`clawdbot onboard`)
- Model authentication (Anthropic OAuth, OpenAI Codex, API keys)
- First interaction via CLI

### Part 3: Channel Integrations (15-20 min)
- WhatsApp setup (QR code linking)
- Telegram bot configuration
- Discord integration
- Slack workspace connection
- Signal (requires signal-cli)
- iMessage (macOS only)
- Mention other channels (Teams, Matrix, Google Chat, etc.)

### Part 4: Platform Apps (10 min)
- macOS menu bar app
- iOS/Android nodes
- Voice Wake setup
- Talk Mode demonstration

### Part 5: Tools & Capabilities (15 min)
- Browser control (dedicated Clawdbot browser)
- Canvas (visual workspace + A2UI)
- Bash/exec tool
- File operations
- Session management (multi-agent)

### Part 6: Automation (10 min)
- Cron jobs
- Webhooks
- Gmail Pub/Sub integration

### Part 7: Skills (10 min)
- Built-in skills overview
- ClawdHub (skill registry)
- Creating custom skills

### Part 8: Advanced Features (10 min)
- Multi-agent routing
- Security (DM pairing, sandbox mode)
- Tailscale integration
- Remote gateway (Linux VPS)

### Part 9: Wrap-up (3-5 min)
- Recap of capabilities
- Resources (docs, Discord, GitHub)
- Call to action

---

## Detailed Section Breakdown

### Part 1: Introduction

**Script points:**
- "Clawdbot is a personal AI assistant you run on your own devices"
- Works across 20+ messaging channels
- Single Gateway control plane
- Local-first, privacy-focused
- Show the architecture diagram from README

**Visuals:**
- Clawdbot logo/mascot
- Architecture diagram
- Quick demo reel of different channels

---

### Part 2: Basic Setup

**Demo steps:**
1. Install: `npm install -g clawdbot@latest`
2. Run wizard: `clawdbot onboard --install-daemon`
3. Auth options:
   - Anthropic OAuth (Claude Pro/Max)
   - OpenAI Codex OAuth
   - API keys
4. First message: `clawdbot agent --message "Hello, what can you do?"`
5. Show Gateway running: `clawdbot gateway --port 18789 --verbose`

**Key files to show:**
- `~/.clawdbot/clawdbot.json` (config)
- `~/clawd/` (workspace)

---

### Part 3: Channel Integrations

#### WhatsApp
- Run: `clawdbot channels login`
- Scan QR code with phone
- Configure allowlist
- Send a test message
- **Note:** Uses Baileys (unofficial)

#### Telegram
- Create bot via @BotFather
- Set token: `clawdbot config set channels.telegram.botToken <token>`
- Configure groups/allowlist

#### Discord
- Create Discord application
- Set token and enable intents
- Native slash commands

#### Slack
- Create Slack app with socket mode
- Bot token + App token
- Workspace installation

#### Others (overview)
- Signal (signal-cli required)
- iMessage (macOS only)
- Microsoft Teams (enterprise setup)
- Matrix, Google Chat, Zalo, etc.

---

### Part 4: Platform Apps

#### macOS App
- Installation location: `/Applications/Clawdbot.app`
- Menu bar presence
- Voice Wake trigger
- Talk Mode overlay
- Debug tools

#### iOS/Android Nodes
- Canvas surface
- Camera snap/clip
- Screen recording
- Location access
- Bonjour pairing

**Demo:**
- Show Voice Wake activation
- Talk Mode conversation
- Canvas pushing content

---

### Part 5: Tools & Capabilities

#### Browser Control
- Dedicated Chrome profile
- CDP (Chrome DevTools Protocol) control
- Snapshots and actions
- Login assistance

#### Canvas & A2UI
- Visual workspace
- Agent pushes UI updates
- Screenshot sharing
- Interactive elements

#### Bash/Exec
- Command execution
- Elevated mode toggle
- Sandbox restrictions

#### Session Tools
- `sessions_list` - discover agents
- `sessions_history` - fetch transcripts
- `sessions_send` - cross-session messaging

---

### Part 6: Automation

#### Cron Jobs
- Schedule recurring tasks
- Config example
- Use cases: daily summaries, reminders

#### Webhooks
- HTTP triggers
- External service integration
- Token authentication

#### Gmail Pub/Sub
- Real-time email notifications
- Google Cloud setup
- Email-triggered actions

---

### Part 7: Skills

#### Built-in Skills (52 available!)
- **Productivity:** Apple Notes, Reminders, Things, Notion, Obsidian
- **Smart Home:** Philips Hue, Sonos
- **Media:** Spotify, video frames
- **Development:** GitHub, coding-agent, tmux
- **Messaging:** Discord, Slack, iMessage, WhatsApp
- **AI:** Whisper transcription, image generation
- **Utilities:** Weather, 1Password, food ordering

#### ClawdHub
- Managed skill registry
- Auto-search and install
- clawdhub.com

#### Custom Skills
- Create `SKILL.md`
- Define tool schemas
- Install from workspace

---

### Part 8: Advanced Features

#### Multi-Agent Routing
- Multiple agent configurations
- Route channels to specific agents
- Isolated workspaces

#### Security
- DM pairing (default protection)
- Allowlists per channel
- Docker sandbox for non-main sessions
- `clawdbot doctor` for security audit

#### Tailscale Integration
- Serve (tailnet-only)
- Funnel (public HTTPS)
- Password auth for funnel

#### Remote Gateway
- Run on Linux VPS
- Connect macOS/mobile nodes
- SSH tunnels
- Device-local actions via node.invoke

---

### Part 9: Wrap-up

**Key takeaways:**
- Personal AI across all your messaging apps
- Local-first, privacy-respecting
- Highly extensible via skills
- Multi-platform support

**Resources:**
- Docs: https://docs.clawd.bot
- GitHub: https://github.com/clawdbot/clawdbot
- Discord: https://discord.gg/clawd
- DeepWiki: https://deepwiki.com/clawdbot/clawdbot

---

## Production Notes

### Screen Recording Checklist
- [ ] Clean desktop
- [ ] Large font sizes
- [ ] Dark theme for terminal
- [ ] Notifications disabled
- [ ] Test data/accounts (not real credentials)

### B-Roll Ideas
- Gateway logs scrolling
- Messages flowing between channels
- Voice Wake activation
- Canvas content updating
- Mobile app screens

### Graphics Needed
- Architecture diagram
- Channel logos grid
- Skills category icons
- Feature comparison table

### Timestamps (for YouTube)
```
0:00 Introduction
3:00 Prerequisites & Installation
8:00 Onboarding Wizard
15:00 WhatsApp Integration
22:00 Telegram Setup
28:00 Discord & Slack
35:00 macOS App
42:00 iOS/Android Nodes
50:00 Browser Control
57:00 Canvas & A2UI
65:00 Automation (Cron/Webhooks)
72:00 Skills Overview
80:00 Security & Multi-Agent
88:00 Remote Gateway
95:00 Wrap-up & Resources
```

---

## Appendix: All Available Channels

### Core Channels
1. WhatsApp (Baileys)
2. Telegram (grammY)
3. Slack (Bolt)
4. Discord (discord.js)
5. Signal (signal-cli)
6. iMessage (imsg)

### Extension Channels
7. Microsoft Teams
8. Google Chat
9. Matrix
10. BlueBubbles
11. Zalo
12. Zalo Personal
13. LINE
14. Mattermost
15. Nextcloud Talk
16. Nostr
17. Tlon
18. WebChat (built-in)
19. Voice Call
20. Copilot Proxy

### Deployment Platforms
1. macOS
2. Linux
3. Windows (WSL2)
4. Docker
5. Raspberry Pi
6. DigitalOcean
7. Fly.io
8. GCP
9. Hetzner
10. Railway
11. Render
12. exe.dev
13. macOS VM
14. iOS
15. Android
