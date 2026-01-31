# Part 3: Docker Sandboxing Fundamentals

> **Duration:** 12 minutes  
> **Tone:** Technical but accessible, empowering  
> **Goal:** Teach Docker sandboxing and when/how to use it

---

## 🎬 Opening

### [VISUAL: Container diagram - isolated box within larger system]

**VOICEOVER:**

> "Sandboxing is the game-changer. Instead of letting the AI run commands directly on your system, we run them inside Docker containers. The AI can only touch what's inside the container.
>
> Think of it like a clean room. The AI works in a sealed environment. When it's done, you control what goes in and out."

---

## 🐳 What Docker Sandboxing Does

### [VISUAL: Before/After comparison - direct access vs containerized]

**VOICEOVER:**

> "Without sandboxing, when the AI runs `ls /`, it sees your actual filesystem. Your home directory, your SSH keys, everything.
>
> WITH sandboxing, `ls /` shows a minimal Linux container. Your real files don't exist there. The AI can read, write, and execute - but only within this isolated environment."

### What Changes:

```text
WITHOUT SANDBOX:
exec "cat ~/.ssh/id_rsa" → Shows your actual SSH key 😱

WITH SANDBOX:
exec "cat ~/.ssh/id_rsa" → No such file (container filesystem) ✅
```

---

## 📦 Setting Up the Sandbox

### [VISUAL: Terminal showing setup commands]

**VOICEOVER:**

> "First, let's build the sandbox image. Clawdbot provides a script for this."

### Commands:

```bash
# Build the sandbox image
cd /path/to/clawdbot
scripts/sandbox-setup.sh

# This creates: clawdbot-sandbox:bookworm-slim
# Based on Debian Bookworm minimal

# Verify it exists
docker images | grep clawdbot-sandbox
```

**VOICEOVER:**

> "The script creates a minimal Debian-based container with just enough to run tools. It's intentionally stripped down - smaller attack surface."

---

## ⚙️ Configuration Options

### [VISUAL: Config file with sandbox section highlighted]

**VOICEOVER:**

> "Now let's configure when sandboxing kicks in. There are three modes."

### Sandbox Modes:

```json
{
  "agents": {
    "defaults": {
      "sandbox": {
        "mode": "non-main"  // or "all" or "off"
      }
    }
  }
}
```

### Mode Explained:

```text
"off"      → No sandboxing. All sessions run on host.
"non-main" → Only non-main sessions sandboxed (recommended start)
"all"      → Everything sandboxed, including your main session
```

**VOICEOVER:**

> "Let me explain 'non-main'. Your main session - when YOU are chatting directly - runs on the host. But group chats, other users, channels? Those are 'non-main' sessions and run in containers.
>
> This lets you have full access while protecting against less trusted sources."

---

## 🔍 Understanding Scope

### [VISUAL: Container allocation diagram]

**VOICEOVER:**

> "Scope controls how many containers get created."

### Scope Options:

```json
{
  "agents": {
    "defaults": {
      "sandbox": {
        "mode": "non-main",
        "scope": "session"  // or "agent" or "shared"
      }
    }
  }
}
```

```text
"session" → One container per chat session (strictest isolation)
"agent"   → One container per agent (multiple sessions share)
"shared"  → One container for ALL sandboxed sessions
```

**VOICEOVER:**

> "Session scope is the most secure - every conversation is completely isolated. But it uses more resources. Agent scope is a good middle ground. Shared is least isolation but most efficient."

---

## 📁 Workspace Access

### [VISUAL: Mount diagram showing container vs host filesystem]

**VOICEOVER:**

> "By default, the sandbox can't see your agent workspace at all. But sometimes you need the AI to work with your files. You can configure this."

### Workspace Access Levels:

```json
{
  "agents": {
    "defaults": {
      "sandbox": {
        "workspaceAccess": "none"  // or "ro" or "rw"
      }
    }
  }
}
```

```text
"none" → Sandbox workspace only (~/.clawdbot/sandboxes). Can't see your files.
"ro"   → Agent workspace mounted READ-ONLY at /agent. Can read, not write.
"rw"   → Agent workspace mounted READ-WRITE at /workspace. Full access.
```

**VOICEOVER:**

> "Start with 'none' for maximum security. If you need the AI to read your project files, use 'ro'. Only use 'rw' when you specifically need the AI to modify files in your workspace."

---

## 🌐 Network Isolation

### [VISUAL: Network diagram showing isolated container]

**VOICEOVER:**

> "Here's something important: sandbox containers have NO network access by default."

```json
{
  "agents": {
    "defaults": {
      "sandbox": {
        "docker": {
          "network": "none"  // Default - no internet
        }
      }
    }
  }
}
```

**VOICEOVER:**

> "This means sandboxed sessions can't make HTTP requests, download packages, or exfiltrate data. If you need network access - say, for a skill that calls an API - you can configure it. But the default is locked down."

### If You Need Network:

```json
{
  "agents": {
    "defaults": {
      "sandbox": {
        "docker": {
          "network": "bridge"  // Enable network (less secure)
        }
      }
    }
  }
}
```

---

## 🔧 Practical Example: Minimal Enable

### [VISUAL: Config file being edited]

**VOICEOVER:**

> "Let's configure a practical sandbox setup."

### Configuration:

```json
{
  "agents": {
    "defaults": {
      "sandbox": {
        "mode": "non-main",
        "scope": "session",
        "workspaceAccess": "none",
        "docker": {
          "network": "none"
        }
      }
    }
  }
}
```

**VOICEOVER:**

> "This setup:
> - Sandboxes all non-main sessions (groups, other users)
> - Gives each session its own container
> - Provides no workspace access
> - Blocks network
>
> Your main session still has full access, but everyone else is in a sealed box."

---

## 🔓 The Escape Hatch: Elevated Mode

### [VISUAL: Elevated mode toggle illustration]

**VOICEOVER:**

> "Sometimes you're in a sandboxed session but legitimately need host access. That's where elevated mode comes in."

### Using Elevated:

```bash
# In chat:
/elevated on    # Run exec on host (with exec approvals)
/elevated full  # Run exec on host, skip approvals (dangerous!)
/elevated off   # Back to sandbox
```

**VOICEOVER:**

> "The `/elevated on` command lets you break out of the sandbox for a specific session when you need to. But it respects exec approvals - you'll still get prompted for dangerous commands.
>
> `/elevated full` bypasses even that. Only use it when you absolutely know what you're doing."

### Configuration:

```json
{
  "tools": {
    "elevated": {
      "enabled": true,
      "allowFrom": {
        "whatsapp": ["+1234567890"],  // Only these users can elevate
        "telegram": ["@yourusername"]
      }
    }
  }
}
```

**VOICEOVER:**

> "You can restrict who's even allowed to USE elevated mode. Don't let random approved users escape the sandbox."

---

## 📊 Risk Level With Sandboxing

### [VISUAL: Risk matrix showing improvement]

**VOICEOVER:**

> "With sandboxing properly configured, you've moved to LOW-MEDIUM risk."

### What's Protected:

```text
✅ Non-main sessions can't access your filesystem
✅ Network access is blocked by default
✅ Each session is isolated from others
✅ Mistakes are contained to the container
✅ Elevated mode is gated by allowlist
```

### What's Still Possible:

```text
⚠️ Main session still has full access
⚠️ Users on elevated allowlist can escape
⚠️ Container breakouts (rare but theoretical)
⚠️ Anything you explicitly bind mount
```

---

## 🔗 Custom Bind Mounts (Advanced)

### [VISUAL: Bind mount diagram]

**VOICEOVER:**

> "Sometimes you need to expose specific host paths to the sandbox. Use bind mounts carefully."

### Configuration:

```json
{
  "agents": {
    "defaults": {
      "sandbox": {
        "docker": {
          "binds": [
            "/home/user/source:/source:ro",  // Read-only source code
            "/home/user/data:/data:rw"       // Read-write data dir
          ]
        }
      }
    }
  }
}
```

### Security Warning:

```text
⚠️ NEVER bind:
- ~/.ssh
- ~/.*credentials*
- /var/run/docker.sock (gives container host control!)
- Anything with secrets
```

**VOICEOVER:**

> "Bind mounts BYPASS sandboxing for those specific paths. Use them sparingly and always prefer read-only mode unless write access is truly necessary."

---

## ✅ Sandbox Verification

### [VISUAL: Terminal showing sandbox explain command]

**VOICEOVER:**

> "How do you verify sandboxing is working? Clawdbot has a built-in command."

```bash
# Check sandbox status
clawdbot sandbox explain

# For a specific session
clawdbot sandbox explain --session agent:main:group-12345

# As JSON (for scripting)
clawdbot sandbox explain --json
```

**VOICEOVER:**

> "This shows you the effective sandbox mode, which tools are blocked inside the sandbox, and what configuration is applying. Use it to verify your setup."

---

## ➡️ Next: Security Layers

### [VISUAL: Multiple security layer icons]

**VOICEOVER:**

> "Sandboxing contains blast radius. But we can add more layers - pairing, allowlists, tool policies, exec approvals. Each layer adds defense in depth.
>
> In Part 4, we'll stack these layers and see how they work together."

---

## 📝 Production Notes

### Commands to Demo
- `scripts/sandbox-setup.sh`
- `docker images | grep clawdbot-sandbox`
- `clawdbot sandbox explain`
- Configuration editing
- `/elevated on` in chat

### Diagrams
- Container isolation visualization
- Workspace access levels comparison
- Scope comparison (session vs agent vs shared)
- Risk matrix improvement

### Key Timestamps
- 0:00 - What sandboxing does
- 2:00 - Setting up the sandbox image
- 3:30 - Mode configuration
- 5:00 - Scope options
- 6:30 - Workspace access
- 8:00 - Network isolation
- 9:00 - Elevated escape hatch
- 10:30 - Risk assessment
- 11:30 - Verification
