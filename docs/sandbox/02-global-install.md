# Part 2: Global Installation - Risks and Basic Mitigations

> **Duration:** 10 minutes  
> **Tone:** Practical, honest about trade-offs  
> **Goal:** Explain what happens with a standard install and first-level protections

---

## 🎬 Opening

### [VISUAL: Terminal showing npm install command]

**VOICEOVER:**

> "When you run `npm install -g clawdbot`, you're doing a global installation. The tool runs with your user permissions - everything you can do, it can do.
>
> This is the simplest setup, and honestly, it's fine for many personal use cases. But let's understand exactly what it means and how to add basic protections."

---

## 📦 What Global Installation Means

### [VISUAL: Diagram showing Clawdbot process and user account access]

**VOICEOVER:**

> "A global npm install puts Clawdbot in your system path. When it runs, it runs as your user account."

### Access Level:

```text
YOUR USER ACCOUNT CAN:
- Read/write all your files
- Run any command
- Access network
- Install software (maybe with sudo)

CLAWDBOT INHERITS ALL OF THIS
```

### [VISUAL: File tree showing ~/.clawdbot, workspace, and user home with sensitive areas highlighted]

**VOICEOVER:**

> "Clawdbot stores its state in ~/.clawdbot - configuration, credentials, session logs. It also has a workspace, typically ~/clawd, where it does its work.
>
> But here's the thing: the AI isn't confined to these directories. If you ask it to read ~/.ssh/id_rsa, it will happily do so. Because YOU can read that file, and the AI runs as you."

---

## 🔴 Risk Level: High (Without Mitigations)

### [VISUAL: Red warning indicator]

**VOICEOVER:**

> "With zero configuration beyond the defaults, your risk level is HIGH. Not because Clawdbot is malicious - it's not. But because:
>
> - Anyone who messages you can potentially interact with the AI
> - The AI can run any command without confirmation
> - A mistake or manipulation could affect your entire home directory
>
> Let's add some basic protections."

---

## 🛡️ Mitigation 1: DM Pairing (Default)

### [VISUAL: Pairing code appearing in chat]

**VOICEOVER:**

> "The good news: DM pairing is ON by default. When someone new tries to message your bot, they get a pairing code. The message is ignored until you approve them."

### Technical Details:

```json
{
  "channels": {
    "whatsapp": {
      "dmPolicy": "pairing"
    },
    "telegram": {
      "dmPolicy": "pairing"
    }
  }
}
```

### [VISUAL: Terminal showing pairing commands]

```bash
# View pending pairing requests
clawdbot pairing list

# Approve a specific request
clawdbot pairing approve telegram ABC123

# Reject a request
clawdbot pairing reject telegram ABC123
```

**VOICEOVER:**

> "Check pending requests with `clawdbot pairing list`. Approve ones you recognize, reject ones you don't. Simple but effective."

### Why This Matters:

**VOICEOVER:**

> "Without pairing, a random stranger finding your Telegram bot could start giving it commands. With pairing, they'll just get a code and be ignored. This blocks the most obvious attack vector."

---

## 🛡️ Mitigation 2: Require Mentions in Groups

### [VISUAL: Group chat with @mention highlighted]

**VOICEOVER:**

> "In group chats, you probably don't want the bot responding to every message. Set it to require mentions."

### Configuration:

```json
{
  "channels": {
    "whatsapp": {
      "groups": {
        "*": { "requireMention": true }
      }
    },
    "telegram": {
      "groups": {
        "*": { "requireMention": true }
      }
    }
  }
}
```

**VOICEOVER:**

> "Now in groups, the bot only responds when someone explicitly mentions it - @clawd or whatever name you've configured. Without this, every message in the group could potentially trigger tool execution."

---

## 🛡️ Mitigation 3: Basic File Permissions

### [VISUAL: Terminal showing chmod commands]

**VOICEOVER:**

> "Lock down your Clawdbot config and state directories. These contain credentials and session logs."

### Commands:

```bash
# State directory - user only
chmod 700 ~/.clawdbot

# Config file - user read/write only
chmod 600 ~/.clawdbot/clawdbot.json

# Credentials directory
chmod 700 ~/.clawdbot/credentials

# Check what you have
ls -la ~/.clawdbot
```

**VOICEOVER:**

> "700 means only you can access the directory. 600 means only you can read/write the file. This prevents other users on a shared system from reading your configuration."

---

## 🛡️ Mitigation 4: Separate Bot Account

### [VISUAL: Personal phone vs bot phone illustration]

**VOICEOVER:**

> "If you're serious about separation, run Clawdbot on a SEPARATE phone number or social account. Your personal WhatsApp stays personal. A secondary number handles the AI.
>
> This isn't technical isolation - the AI still runs as your user. But it massively reduces the blast radius of social engineering. Someone manipulating the bot can't see your personal conversations."

---

## 🛡️ Mitigation 5: Gateway Authentication

### [VISUAL: Lock icon on gateway connection]

**VOICEOVER:**

> "The Gateway accepts WebSocket connections. By default, it requires authentication - good! Make sure you have a token set."

### Configuration:

```bash
# Generate a gateway token
clawdbot doctor --generate-gateway-token

# Or set manually in config
clawdbot config set gateway.auth.mode "token"
clawdbot config set gateway.auth.token "your-long-random-token"
```

### [VISUAL: Config file snippet]

```json
{
  "gateway": {
    "auth": {
      "mode": "token",
      "token": "your-32-character-random-string"
    }
  }
}
```

**VOICEOVER:**

> "This prevents unauthorized local connections to your Gateway. Without it, any process on your machine could theoretically connect and send commands."

---

## 📊 Where We Are Now

### [VISUAL: Risk matrix showing progress]

**VOICEOVER:**

> "With these basic mitigations, you've moved from HIGH risk to MODERATE risk."

### What's Protected:
```text
✅ Strangers can't message your bot (pairing)
✅ Groups require explicit mentions
✅ Config files are permission-protected
✅ Gateway requires authentication
```

### What's Still Risky:
```text
❌ Approved users can still trigger ANY command
❌ Content-based attacks still possible
❌ AI mistakes can still affect your filesystem
❌ No isolation between AI and your data
```

---

## 🤔 Who Should Stop Here?

### [VISUAL: Use case comparison]

**VOICEOVER:**

> "This level of protection is reasonable for:
>
> - Personal use where only you message the bot
> - Trusted family members on an allowlist
> - Low-risk use cases (chat, simple queries)
>
> It's NOT sufficient for:
>
> - Letting untrusted people use your bot
> - Running in shared environments
> - High-security use cases
> - Production deployments
>
> If you need more protection, Part 3 introduces Docker sandboxing - where we actually isolate what the AI can touch."

---

## 🔧 Quick Setup Checklist

### [VISUAL: Checklist graphic]

**VOICEOVER:**

> "Let's recap the basic hardening checklist:"

```bash
# 1. Verify pairing is enabled (it should be by default)
clawdbot config get channels.whatsapp.dmPolicy
# Should output: "pairing"

# 2. Set require mention for groups
clawdbot config set channels.whatsapp.groups '{"*": {"requireMention": true}}'

# 3. Lock down file permissions
chmod 700 ~/.clawdbot
chmod 600 ~/.clawdbot/clawdbot.json

# 4. Ensure gateway auth is set
clawdbot doctor --generate-gateway-token

# 5. Run security audit
clawdbot security audit
```

**VOICEOVER:**

> "Run these commands and you've got a reasonably protected basic setup. The `security audit` command will flag anything obvious you might have missed."

---

## ➡️ Next: Sandboxing

### [VISUAL: Docker container preview]

**VOICEOVER:**

> "But we're still running without isolation. In Part 3, we'll add Docker sandboxing. The AI will run commands inside containers, unable to touch your actual filesystem.
>
> That's where things get really secure. See you there."

---

## 📝 Production Notes

### Commands to Demo
- `clawdbot pairing list`
- `clawdbot security audit`
- `chmod` commands on ~/.clawdbot
- Config file inspection

### Diagrams
- User account access scope
- Pairing flow diagram
- Risk matrix (before/after basic mitigations)

### Key Timestamps
- 0:00 - What global install means
- 2:00 - Risk level explained
- 3:00 - DM pairing
- 4:30 - Require mentions
- 5:30 - File permissions
- 6:30 - Separate accounts
- 7:30 - Gateway auth
- 8:30 - Where we are now
- 9:30 - Setup checklist
