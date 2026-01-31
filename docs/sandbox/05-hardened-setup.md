# Part 5: Fully Hardened, Risk-Free Configuration

> **Duration:** 10 minutes  
> **Tone:** Authoritative, comprehensive, "this is the safest way"  
> **Goal:** Provide a production-ready maximum security configuration

---

## 🎬 Opening

### [VISUAL: Vault door opening to reveal secure setup]

**VOICEOVER:**

> "Everything we've learned, combined into one setup. This is Clawdbot at maximum security - where even aggressive threat actors would struggle to cause harm.
>
> This isn't for everyone. It sacrifices convenience for safety. But if you need to run Clawdbot in a sensitive environment, this is how you do it."

---

## 🏰 The Maximum Security Architecture

### [VISUAL: Complete architecture diagram]

**VOICEOVER:**

> "Here's our target architecture:
>
> - Gateway runs inside Docker (not just tools - the entire gateway)
> - All sessions sandboxed with strict policies
> - Read-only filesystem access
> - No network for sandboxed sessions
> - Exec completely disabled or heavily gated
> - Explicit allowlists everywhere
> - No elevated escape hatch"

---

## 🐳 Step 1: Run Gateway in Docker

### [VISUAL: Docker container diagram]

**VOICEOVER:**

> "The first step is containerizing the Gateway itself. Not just tools - the whole thing."

### Docker Compose Setup:

```yaml
# docker-compose.yml
version: '3.8'
services:
  clawdbot:
    image: clawdbot/gateway:latest
    container_name: clawdbot
    restart: unless-stopped
    volumes:
      - ./config:/home/clawdbot/.clawdbot:rw
      - ./workspace:/home/clawdbot/clawd:rw
    environment:
      - CLAWDBOT_STATE_DIR=/home/clawdbot/.clawdbot
      - ANTHROPIC_API_KEY=${ANTHROPIC_API_KEY}
    ports:
      - "127.0.0.1:18789:18789"  # Loopback only!
    security_opt:
      - no-new-privileges:true
    read_only: false  # Gateway needs some write access
    user: "1000:1000"  # Non-root
```

**VOICEOVER:**

> "The Gateway container exposes only the loopback port. Host files are volume-mounted explicitly. The container runs as non-root. Any compromise is contained to the container."

### Build and Run:

```bash
# Create directories
mkdir -p config workspace
chmod 700 config workspace

# Start the containerized gateway
docker-compose up -d

# View logs
docker-compose logs -f clawdbot
```

---

## 🔒 Step 2: Maximum Security Configuration

### [VISUAL: Config file with security sections highlighted]

**VOICEOVER:**

> "Now the configuration. This is the most locked-down setup possible."

### Complete Configuration:

```json
{
  "gateway": {
    "mode": "local",
    "bind": "loopback",
    "port": 18789,
    "auth": {
      "mode": "token",
      "token": "${CLAWDBOT_GATEWAY_TOKEN}"
    }
  },
  
  "channels": {
    "whatsapp": {
      "dmPolicy": "allowlist",
      "allowFrom": ["+YOUR_NUMBER_ONLY"],
      "groups": {
        "*": {
          "requireMention": true,
          "disable": true
        }
      },
      "groupPolicy": "disabled"
    },
    "telegram": {
      "dmPolicy": "allowlist",
      "allowFrom": ["@yourusername_only"],
      "groups": {
        "*": { "disable": true }
      }
    }
  },
  
  "agents": {
    "defaults": {
      "sandbox": {
        "mode": "all",
        "scope": "session",
        "workspaceAccess": "ro",
        "docker": {
          "network": "none",
          "readOnlyRoot": true
        }
      }
    }
  },
  
  "tools": {
    "allow": ["read", "sessions_list", "session_status"],
    "deny": ["exec", "process", "write", "edit", "apply_patch", "browser", "nodes", "gateway", "cron", "canvas", "image"],
    "elevated": {
      "enabled": false
    }
  },
  
  "session": {
    "dmScope": "per-channel-peer"
  },
  
  "discovery": {
    "mdns": { "mode": "off" }
  },
  
  "logging": {
    "redactSensitive": "tools"
  }
}
```

---

## 📋 What This Configuration Does

### [VISUAL: Checklist appearing]

**VOICEOVER:**

> "Let's break down what each section achieves:"

```text
GATEWAY:
✅ Loopback only - no network exposure
✅ Token authentication required
✅ mDNS discovery disabled

CHANNELS:
✅ DM allowlist - only YOU can message
✅ Groups completely disabled
✅ No pairing for strangers

SANDBOX:
✅ ALL sessions sandboxed (including main)
✅ Per-session isolation
✅ Read-only workspace access
✅ No network for containers
✅ Read-only root filesystem

TOOLS:
✅ Only safe tools allowed (read, session info)
✅ No exec, write, browser, or system tools
✅ Elevated mode completely disabled

SESSION:
✅ DM sessions isolated per peer
```

---

## 🔐 Step 3: Exec Approvals (If You Must Have Exec)

### [VISUAL: Exec approval config]

**VOICEOVER:**

> "If you absolutely need exec capability, here's how to gate it maximally."

### Ultra-Restrictive Exec Config:

```json
// ~/.clawdbot/exec-approvals.json
{
  "version": 1,
  "defaults": {
    "security": "deny",
    "ask": "always",
    "askFallback": "deny",
    "autoAllowSkills": false
  },
  "agents": {
    "main": {
      "security": "allowlist",
      "ask": "always",
      "allowlist": [
        { "pattern": "/usr/bin/cat" },
        { "pattern": "/usr/bin/ls" },
        { "pattern": "/opt/homebrew/bin/rg" }
      ]
    }
  }
}
```

**VOICEOVER:**

> "Security defaults to deny. Every single command prompts for approval. Only explicitly allowlisted binaries can even be considered. No skill auto-allow. This is maximum paranoia."

---

## 🌐 Step 4: Network Hardening

### [VISUAL: Network isolation diagram]

**VOICEOVER:**

> "Network exposure is often the biggest risk. Here's how to lock it down."

### Host Firewall (UFW Example):

```bash
# Block all incoming by default
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Allow SSH only from specific IP (if needed)
sudo ufw allow from 192.168.1.100 to any port 22

# Clawdbot gateway should NOT be exposed
# It binds to loopback only

# Enable firewall
sudo ufw enable
```

### Tailscale (If Remote Access Needed):

```json
{
  "tailscale": {
    "serve": {
      "enabled": true,
      "funnel": false  // NO public access
    }
  },
  "gateway": {
    "auth": {
      "allowTailscale": true  // Tailscale identity auth
    }
  }
}
```

**VOICEOVER:**

> "If you need remote access, use Tailscale Serve - NOT Funnel. Serve keeps access within your private tailnet. Funnel would expose to the internet."

---

## 📁 Step 5: Filesystem Protection

### [VISUAL: Permission diagram]

**VOICEOVER:**

> "Lock down the filesystem hosting Clawdbot."

### Permission Hardening:

```bash
# State directory
chmod 700 ~/.clawdbot
find ~/.clawdbot -type f -exec chmod 600 {} \;
find ~/.clawdbot -type d -exec chmod 700 {} \;

# Workspace (if using volume mounts)
chmod 700 ~/clawd
find ~/clawd -type f -exec chmod 600 {} \;

# Config file specifically
chmod 600 ~/.clawdbot/clawdbot.json

# Credentials
chmod 700 ~/.clawdbot/credentials
chmod 600 ~/.clawdbot/credentials/*
```

### SELinux/AppArmor (Advanced):

```bash
# Example AppArmor profile (conceptual)
# /etc/apparmor.d/clawdbot
profile clawdbot {
  # Allow read of config
  /home/*/.clawdbot/** r,
  
  # Allow write only to specific paths
  /home/*/.clawdbot/sessions/** rw,
  /home/*/.clawdbot/logs/** rw,
  
  # Deny everything else
  deny /** w,
}
```

---

## ✅ Verification Checklist

### [VISUAL: Security audit running]

```bash
# 1. Run security audit
clawdbot security audit --deep

# 2. Verify sandbox config
clawdbot sandbox explain

# 3. Test tool restrictions
clawdbot agent --message "run ls /"
# Should be blocked

# 4. Verify network isolation
docker exec clawdbot curl google.com
# Should fail (no network)

# 5. Check permissions
ls -la ~/.clawdbot

# 6. Verify allowlists
clawdbot config get channels.whatsapp.allowFrom
```

---

## 📊 Risk Assessment: Minimal

### [VISUAL: Green security indicator]

**VOICEOVER:**

> "With this configuration, your attack surface is minimal."

```text
Threat: Strangers message the bot
Defense: Allowlist-only DMs, no groups
Risk: ✅ Eliminated

Threat: Content-based prompt injection
Defense: Read-only tools, no exec, no browser
Risk: ✅ Limited to information disclosure (read-only)

Threat: AI makes a mistake
Defense: All sandbox, no write tools
Risk: ✅ Contained to sandbox

Threat: Container escape
Defense: Non-root, read-only root, no network
Risk: ✅ Extremely unlikely

Threat: Network intrusion
Defense: Loopback only, token auth, firewall
Risk: ✅ Eliminated
```

---

## ⚖️ Trade-offs

### [VISUAL: Balance scale - security vs functionality]

**VOICEOVER:**

> "Let's be honest about what you give up:"

```text
CANNOT DO:
- Run arbitrary commands
- Modify files
- Browse the web
- Control smart home devices
- Execute complex automation
- Let others use your bot

CAN DO:
- Read files in workspace
- Answer questions
- Summarize content
- Process text
- View session histories
- Chat assistance

SUMMARY: Safe chat assistant, not a power tool
```

---

## 🔄 Graduated Security Levels

### [VISUAL: Security level slider]

**VOICEOVER:**

> "Not everyone needs maximum security. Here's a quick reference for graduated levels."

```text
LEVEL 1 - MAXIMUM SECURITY (This Video)
- Gateway in Docker
- All sandbox, no exec
- Read-only tools
- Allowlist-only DMs
- Use case: Untrusted environments, production

LEVEL 2 - HIGH SECURITY
- Host gateway, sandbox non-main
- Exec with approval + allowlist
- Read/write tools with caution
- DM pairing
- Use case: Shared family device

LEVEL 3 - MODERATE SECURITY
- Host gateway, sandbox non-main
- Exec with approval on-miss
- Most tools enabled
- DM pairing
- Use case: Personal power user

LEVEL 4 - CONVENIENCE MODE
- Host gateway, sandbox off
- All tools, elevated enabled
- DM pairing
- Use case: Single trusted user, development
```

---

## 🎯 Final Recommendations

### [VISUAL: Key takeaways graphic]

**VOICEOVER:**

> "Whatever level you choose:
>
> 1. **Start restrictive, loosen carefully**. It's easier to add permissions than revoke them.
>
> 2. **Run security audit regularly**. `clawdbot security audit` catches drift.
>
> 3. **Understand your threat model**. Who might message your bot? What could they try?
>
> 4. **Backup before changes**. Export config, save your setup.
>
> 5. **Stay updated**. Security improvements happen with each release.
>
> You now have the knowledge to run Clawdbot at any security level you need. Choose wisely, and happy automating!"

---

## 🔚 Series Wrap-up

### [VISUAL: Series overview with checkmarks]

**VOICEOVER:**

> "That's the complete security series:
>
> ✓ Part 1: Understanding the risks
> ✓ Part 2: Global install and basic mitigations
> ✓ Part 3: Docker sandboxing fundamentals
> ✓ Part 4: All security layers explained
> ✓ Part 5: Maximum security configuration
>
> You're now equipped to run Clawdbot safely. Thanks for watching, and stay secure!"

---

## 📝 Production Notes

### Files to Create
- Complete docker-compose.yml
- Maximum security config.json
- exec-approvals.json example

### Commands to Demo
- docker-compose up
- clawdbot security audit --deep
- Testing tool restrictions
- Permission verification

### Diagrams
- Complete architecture
- Risk elimination chart
- Security level comparison

### Key Timestamps
- 0:00 - Maximum security overview
- 1:30 - Gateway in Docker
- 3:30 - Configuration deep-dive
- 6:00 - Exec approvals
- 7:30 - Network hardening
- 8:30 - Verification
- 9:30 - Trade-offs and recommendations
