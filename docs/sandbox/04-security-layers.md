# Part 4: Security Layers Deep Dive

> **Duration:** 12 minutes  
> **Tone:** Technical, thorough, defense-in-depth mindset  
> **Goal:** Explain all security layers and how they stack

---

## 🎬 Opening

### [VISUAL: Castle with multiple walls and defenses]

**VOICEOVER:**

> "Security isn't about one magic setting. It's about layers. Each layer catches what the previous one missed.
>
> We've covered pairing and sandboxing. Now let's go deep on ALL the security layers Clawdbot provides and how to stack them effectively."

---

## 🧅 The Defense Layers

### [VISUAL: Onion diagram with security layers]

**VOICEOVER:**

> "Think of it like an onion. From outside to inside:"

```text
Layer 1: WHO CAN CONTACT     (DM Pairing, Allowlists)
Layer 2: WHAT TRIGGERS       (Mention Requirements, Group Policies)
Layer 3: WHERE IT RUNS       (Sandbox Mode, Scope)
Layer 4: WHAT TOOLS EXIST    (Tool Allow/Deny Policies)
Layer 5: HOW EXEC RUNS       (Exec Approvals, Elevated Gates)
```

**VOICEOVER:**

> "Each layer gates the next. Someone needs to get past ALL of them to cause harm. Let's explore each one."

---

## 🚪 Layer 1: Who Can Contact

### [VISUAL: Bouncer at door checking IDs]

**VOICEOVER:**

> "This is your first line of defense. Who is even allowed to send messages to your bot?"

### DM Policy Options:

```json
{
  "channels": {
    "whatsapp": {
      "dmPolicy": "pairing",  // "pairing" | "allowlist" | "open" | "disabled"
      "allowFrom": ["+1234567890"]  // Specific numbers
    }
  }
}
```

```text
"pairing"   → Unknown senders get a code, ignored until approved
"allowlist" → Only pre-approved senders, no pairing handshake
"open"      → Anyone can message (DANGEROUS - requires allowFrom: ["*"])
"disabled"  → DMs completely ignored
```

### Best Practice:

**VOICEOVER:**

> "Keep pairing as default. Only switch to allowlist if you want stricter control. NEVER use 'open' unless you fully understand the implications and have sandboxing in place."

### Managing Allowlists:

```bash
# View current allowlist
clawdbot config get channels.whatsapp.allowFrom

# Add someone to allowlist
clawdbot config set channels.whatsapp.allowFrom '["+1234567890", "+0987654321"]'

# Approval files are stored in:
# ~/.clawdbot/credentials/<channel>-allowFrom.json
```

---

## 📢 Layer 2: What Triggers the Bot

### [VISUAL: Group chat with @mention highlighted]

**VOICEOVER:**

> "In groups, you probably don't want every message triggering your AI. This layer controls WHAT makes the bot respond."

### Group Controls:

```json
{
  "channels": {
    "whatsapp": {
      "groups": {
        "*": {
          "requireMention": true
        },
        "family-chat": {
          "requireMention": false  // Trusted group
        }
      },
      "groupPolicy": "allowlist",  // Only listed groups accepted
      "groupAllowFrom": ["+1234567890"]  // Who can trigger in groups
    }
  }
}
```

**VOICEOVER:**

> "You can set defaults for all groups with `*`, then override for specific trusted groups. You can also restrict WHO within a group can trigger the bot, even if they're allowed to message."

---

## 📦 Layer 3: Where It Runs (Sandbox)

### [VISUAL: Container boundary diagram]

**VOICEOVER:**

> "We covered this in Part 3, but let's see how it fits the bigger picture."

### Configuration Recap:

```json
{
  "agents": {
    "defaults": {
      "sandbox": {
        "mode": "non-main",     // non-main sessions sandboxed
        "scope": "session",     // Each session isolated
        "workspaceAccess": "none"  // No host filesystem
      }
    }
  }
}
```

**VOICEOVER:**

> "Sandboxing is Layer 3 because it assumes someone got past Layers 1 and 2. Even if a malicious user is on your allowlist and mentions the bot, their commands run in a container. The blast radius is contained."

---

## 🛠️ Layer 4: Tool Policies

### [VISUAL: Tool icons with checkmarks and X marks]

**VOICEOVER:**

> "Not all tools are equal. Some are read-only. Some can modify files. Some can run arbitrary commands. Tool policies let you control what's even available."

### Global Tool Policies:

```json
{
  "tools": {
    "allow": ["*"],  // Allow all by default
    "deny": ["browser", "exec", "process"]  // Except these
  }
}
```

### Per-Agent Policies:

```json
{
  "agents": {
    "list": [
      {
        "id": "family",
        "tools": {
          "allow": ["read", "sessions_list"],
          "deny": ["write", "edit", "exec", "browser"]
        }
      }
    ]
  }
}
```

### Sandbox-Specific Policies:

```json
{
  "tools": {
    "sandbox": {
      "tools": {
        "allow": ["read", "write"],
        "deny": ["browser", "nodes", "gateway"]
      }
    }
  }
}
```

**VOICEOVER:**

> "You can set policies globally, per-agent, and specifically for sandboxed sessions. They combine - if a tool is denied anywhere in the chain, it's unavailable."

### Tool Groups:

```json
{
  "tools": {
    "deny": ["group:system", "group:fs-write"]
  }
}
```

```text
group:system    → exec, process, gateway...
group:fs-write  → write, edit, apply_patch...
group:fs        → read, write, edit, apply_patch...
group:browser   → browser-related tools
```

---

## ✅ Layer 5: Exec Approvals

### [VISUAL: Approval prompt appearing on screen]

**VOICEOVER:**

> "This is the final gate. Even if exec is allowed, you can require approval for each command."

### Exec Approval Levels:

```json
// ~/.clawdbot/exec-approvals.json
{
  "defaults": {
    "security": "allowlist",  // "deny" | "allowlist" | "full"
    "ask": "on-miss",         // "off" | "on-miss" | "always"
    "askFallback": "deny"     // What to do if UI unavailable
  }
}
```

```text
Security:
  "deny"      → Block all exec
  "allowlist" → Only pre-approved commands
  "full"      → Allow everything

Ask:
  "off"       → Never prompt
  "on-miss"   → Prompt when command not in allowlist
  "always"    → Prompt every single time
```

### Command Allowlists:

```json
{
  "agents": {
    "main": {
      "allowlist": [
        { "pattern": "/opt/homebrew/bin/rg" },
        { "pattern": "~/Projects/**/bin/*" },
        { "pattern": "/usr/local/bin/*" }
      ]
    }
  }
}
```

**VOICEOVER:**

> "Build up an allowlist of safe commands over time. The UI shows you what commands were run and lets you add them with one click. Start with 'ask: always' and build your allowlist organically."

### Safe Bins (Stdin-Only):

```text
Default safe bins that work without explicit allowlist:
jq, grep, cut, sort, uniq, head, tail, tr, wc

These only operate on stdin - no file arguments allowed.
```

---

## 🧩 How Layers Stack

### [VISUAL: Flow diagram showing request passing through layers]

**VOICEOVER:**

> "Let's trace a message through all the layers."

### Example Flow:

```text
1. Message arrives from Telegram group
   → Layer 1: Is this group on allowlist? ✅ Yes
   
2. Was bot mentioned?
   → Layer 2: requireMention=true, was mentioned? ✅ Yes
   
3. Determine execution environment
   → Layer 3: non-main session → Sandboxed ✅
   
4. AI wants to use exec tool
   → Layer 4: exec allowed in sandbox? ✅ Yes
   
5. AI wants to run "rm -rf /"
   → Layer 5: Command in allowlist? ❌ No
   → Layer 5: Ask the user? User denies ❌
   
RESULT: Command blocked at Layer 5
```

**VOICEOVER:**

> "Even though the message got through four layers, the final exec approval caught the dangerous command. That's defense in depth."

---

## 📊 Configuration Examples

### [VISUAL: Config file examples]

### Family-Safe Setup:

```json
{
  "channels": {
    "whatsapp": {
      "dmPolicy": "allowlist",
      "allowFrom": ["+1234567890", "+0987654321"],
      "groups": { "*": { "requireMention": true } }
    }
  },
  "agents": {
    "defaults": {
      "sandbox": { "mode": "non-main", "workspaceAccess": "none" }
    }
  },
  "tools": {
    "deny": ["exec", "browser", "process"]
  }
}
```

**VOICEOVER:**

> "This setup: only family can message, groups require mention, non-main is sandboxed, no dangerous tools."

### Power User Setup:

```json
{
  "channels": {
    "telegram": { "dmPolicy": "pairing" }
  },
  "agents": {
    "defaults": {
      "sandbox": { "mode": "non-main", "workspaceAccess": "ro" }
    }
  },
  "tools": {
    "elevated": {
      "enabled": true,
      "allowFrom": { "telegram": ["@yourusername"] }
    }
  }
}
```

**VOICEOVER:**

> "More flexible: you can elevate from sandbox, workspace is readable, pairing protects new users."

---

## 🔍 Verifying Your Setup

### [VISUAL: Security audit output]

```bash
# Run the security audit
clawdbot security audit --deep

# Check sandbox status
clawdbot sandbox explain

# Check tool policy for an agent
clawdbot sandbox explain --agent family

# View effective config
clawdbot config show --json | jq '.agents.defaults.sandbox'
```

**VOICEOVER:**

> "Use these commands to verify your setup. The security audit catches common misconfigurations. Sandbox explain shows effective policies."

---

## ➡️ Next: Fully Hardened Setup

### [VISUAL: Maximum security fortress]

**VOICEOVER:**

> "We've covered all the layers. In Part 5, we'll put it all together into a FULLY hardened, nearly risk-free configuration. For those who want maximum security."

---

## 📝 Production Notes

### Diagrams
- Onion layers of security
- Flow diagram through all layers
- Tool policy inheritance
- Exec approvals flow

### Commands to Demo
- `clawdbot security audit`
- `clawdbot sandbox explain`
- Config editing examples
- Exec approval prompts

### Key Timestamps
- 0:00 - Defense layers overview
- 1:30 - Layer 1: Who can contact
- 3:30 - Layer 2: What triggers
- 5:00 - Layer 3: Where it runs
- 6:30 - Layer 4: Tool policies
- 8:30 - Layer 5: Exec approvals
- 10:30 - How layers stack
- 11:30 - Configuration examples
