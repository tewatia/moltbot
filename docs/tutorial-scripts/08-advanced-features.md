# Part 8: Advanced Features

> **Duration:** 10 minutes  
> **Tone:** Power-user, sophisticated, trust-building  
> **Prerequisites:** Completed basic setup, familiar with concepts from previous parts

---

## 🎬 Opening

### [VISUAL: Control panel with advanced settings]

**VOICEOVER:**

> "Alright, power users - this section is for you. We've covered the basics, connected channels, explored tools and skills. Now let's dive into the advanced features that really unlock Clawdbot's potential.
>
> Multi-agent routing, security deep-dive, Tailscale integration, and running a remote gateway. Let's level up."

---

## 🤖 Multi-Agent Routing

### [VISUAL: Multiple agent avatars for work, personal, family]

**VOICEOVER:**

> "What if you want different AI personalities or configurations for different contexts? A professional assistant for work, a casual helper for personal stuff, a kid-friendly version for family.
>
> That's multi-agent routing."

### Configuration:

```json
{
  "agents": {
    "main": {
      "model": "anthropic/claude-opus-4-5",
      "workspace": "~/clawd/main",
      "systemPrompt": "You are a professional AI assistant."
    },
    "work": {
      "model": "anthropic/claude-sonnet-4-20250514",
      "workspace": "~/clawd/work",
      "systemPrompt": "You are a work-focused assistant. Be concise and professional."
    },
    "family": {
      "model": "anthropic/claude-sonnet-4-20250514",
      "workspace": "~/clawd/family",
      "systemPrompt": "You are a friendly family assistant. Be warm and helpful with everyone, including kids."
    }
  },
  "routing": {
    "channels": {
      "telegram": {
        "default": "main",
        "groups": {
          "work-team": "work"
        }
      },
      "whatsapp": {
        "contacts": {
          "+1234567890": "family"
        }
      }
    }
  }
}
```

### [VISUAL: Config file showing agent routing]

**VOICEOVER:**

> "Each agent has its own model, workspace, and system prompt. Then in the routing section, you map channels and contacts to agents. Telegram's work-team group uses the work agent. WhatsApp contacts go to family. Everyone else hits main."

### Workspaces:

```bash
# Each agent gets its own workspace
~/clawd/main/      # Main agent files
~/clawd/work/      # Work agent files  
~/clawd/family/    # Family agent files

# Each can have different:
# - AGENTS.md (behavior)
# - SOUL.md (personality)
# - Skills installed
```

**VOICEOVER:**

> "Each workspace is isolated. Work agent doesn't see family conversations. Family agent doesn't have access to your work skills. Clean separation."

### Switching Agents:

```bash
# In chat, switch agents manually
/agent work

# Or specify when sending via CLI
clawdbot agent --agent work --message "What meetings do I have today?"
```

---

## 🔒 Security Deep-Dive

### [VISUAL: Shield icons and lock symbols]

**VOICEOVER:**

> "Security matters. You're giving an AI access to your systems. Let's make sure it's locked down properly."

### DM Pairing (Default Protection)

**VOICEOVER:**

> "We covered DM pairing earlier, but let's go deeper. By default, when someone new messages any of your channels, they get a pairing code. The bot doesn't respond until you approve."

### Technical Steps:

```bash
# View pending pairing requests
clawdbot pairing list

# Sample output:
# ID         Channel     From              Code     Expires
# abc123     telegram    @unknown_user     XK7M     2h left
# def456     whatsapp    +1555123456       QR9P     1h left

# Approve a request
clawdbot pairing approve abc123

# Reject a request
clawdbot pairing reject def456

# Configure pairing expiration
clawdbot config set pairing.codeExpiryMinutes 120
```

### Allowlists:

```json
{
  "channels": {
    "telegram": {
      "allowFrom": ["@trusted_username", "@another_friend"],
      "groups": {
        "@my_group": { "allowFrom": ["*"] }
      }
    },
    "whatsapp": {
      "allowFrom": ["+1234567890", "+0987654321"]
    }
  }
}
```

**VOICEOVER:**

> "For each channel, you can set an allowlist. Only these users get through without pairing. Use `*` to allow everyone - but be careful, that uses your API credits!"

### Sandbox Mode:

### [VISUAL: Docker container diagram]

**VOICEOVER:**

> "By default, the main session has full system access. But for other sessions - especially from channels - you might want sandboxing."

```json
{
  "sandbox": {
    "enabled": true,
    "docker": true,
    "networkPolicy": "restricted",
    "filesystemPolicy": "isolated"
  }
}
```

**VOICEOVER:**

> "Enable sandbox mode to run non-main sessions in Docker containers. They can't access your filesystem, network is restricted, and they can't run dangerous commands. Much safer for allowing wider access."

### Security Audit:

```bash
# Run security audit
clawdbot doctor --security

# Checks:
# - Credentials properly stored
# - Allowlists configured
# - Sandbox enabled for channels
# - No exposed endpoints
```

---

## 🌐 Tailscale Integration

### [VISUAL: Tailscale mesh network diagram]

**VOICEOVER:**

> "Want to access your Clawdbot from anywhere? Tailscale creates a secure mesh network between your devices. Combined with Clawdbot's serve and funnel features, it's incredibly powerful."

### Setup:

```bash
# Install Tailscale (if not already)
# https://tailscale.com/download/

# Enable Clawdbot serve over Tailscale
clawdbot config set tailscale.serve.enabled true

# Now accessible at:
# https://your-machine.your-tailnet.ts.net:18789
```

### [VISUAL: Tailscale admin console]

**VOICEOVER:**

> "Once enabled, your Gateway is accessible from any device on your Tailnet. Phone on cellular, laptop at a coffee shop - they all connect securely without exposing anything to the public internet."

### Public Funnel:

```bash
# For public access (use with caution!)
clawdbot config set tailscale.funnel.enabled true

# Funnel requires authentication
clawdbot config set tailscale.funnel.auth.type password
clawdbot config set tailscale.funnel.auth.password "your-secure-password"
```

**VOICEOVER:**

> "Funnel makes your Gateway publicly accessible via HTTPS through Tailscale. Great for webhooks from external services. But always use authentication - don't leave it wide open!"

---

## 🖥️ Remote Gateway (VPS)

### [VISUAL: Cloud server connecting to local devices]

**VOICEOVER:**

> "The ultimate setup: run your Gateway on a cloud VPS that's always on, then connect your local devices as nodes. This way, your AI is always available, even when your laptop is closed."

### Architecture:

```text
┌─────────────────┐
│   Linux VPS     │
│   (Gateway)     │
│   Always On     │
└────────┬────────┘
         │
    ┌────┴────┐
    │         │
┌───▼───┐ ┌───▼───┐
│ MacOS │ │ Phone │
│ Node  │ │ Node  │
└───────┘ └───────┘
```

**VOICEOVER:**

> "Gateway runs on a cheap VPS - DigitalOcean, Hetzner, whatever. Your Mac and phone connect as nodes. When you need device-local actions - like accessing files or controlling lights - the Gateway sends commands to your nodes."

### VPS Setup:

```bash
# On your VPS
# Install Node.js 22+
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt install -y nodejs

# Install Clawdbot
npm install -g clawdbot@latest

# Onboard (headless mode for servers)
clawdbot onboard --headless

# Start Gateway
clawdbot gateway --port 18789 --bind 0.0.0.0
```

### Connect Nodes:

```bash
# On your Mac
clawdbot node connect --gateway "wss://your-vps:18789" --name "macbook"

# Node capabilities are now available remotely
# Gateway can invoke: camera, filesystem, apps, etc.
```

### [VISUAL: Node connecting to remote gateway]

**VOICEOVER:**

> "Your Mac connects as a node. Whenever the AI needs something from your local machine - take a screenshot, run a script, check a file - it invokes your node through the Gateway. Seamless!"

### Node Invoke:

```bash
# From remote Gateway, invoke node capabilities
clawdbot nodes invoke macbook camera.snap
clawdbot nodes invoke macbook exec "ls ~/Downloads"
```

---

## 🔧 Advanced Configuration

### [VISUAL: Configuration editor]

**VOICEOVER:**

> "A few more advanced configurations you might want to know about."

### Memory and Context:

```json
{
  "memory": {
    "enabled": true,
    "provider": "lancedb",
    "embeddings": "openai/text-embedding-3-small",
    "indexPath": "~/.clawdbot/memory"
  }
}
```

**VOICEOVER:**

> "Enable persistent memory so your AI remembers past conversations. Uses vector embeddings for semantic search."

### Logging:

```bash
# Verbose logging for debugging
clawdbot config set logging.level "debug"
clawdbot config set logging.console true

# View logs
clawdbot logs --tail 100
```

### Extended Thinking:

```json
{
  "agent": {
    "thinking": {
      "default": "medium",
      "budget": 10000
    }
  }
}
```

**VOICEOVER:**

> "Extended thinking gives the AI more time to reason through complex problems. Set a default level and token budget."

---

## ✅ Section Recap

### [VISUAL: Advanced feature icons with checkmarks]

**VOICEOVER:**

> "We've reached power-user territory:
>
> ✓ Multi-agent routing for different contexts
> ✓ Security: pairing, allowlists, sandbox
> ✓ Tailscale for secure remote access
> ✓ Remote Gateway on VPS with local nodes
> ✓ Memory, logging, and advanced configs
>
> You now have the knowledge to build a truly sophisticated personal AI system. Whether it's a simple local setup or a distributed architecture across devices and servers - you've got the tools.
>
> One more section to go - let's wrap this all up!"

---

## 📝 Production Notes

### Demos to Consider
- Switching agents mid-conversation
- Pairing flow (approve/reject)
- Tailscale serve enabling
- Node connection (if have multiple devices)

### Security Callouts
- Never share pairing codes
- Use allowlists in production
- Enable sandbox for public channels
- Funnel needs authentication

### Architecture Diagrams
- Multi-agent routing flow
- Sandbox container diagram
- VPS + nodes architecture

### Key Timestamps
- 0:00 - Advanced features intro
- 1:00 - Multi-agent routing
- 3:30 - Security deep-dive
- 6:00 - Tailscale integration
- 7:30 - Remote Gateway setup
- 9:00 - Advanced configuration
- 9:30 - Recap
