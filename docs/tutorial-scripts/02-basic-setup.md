# Part 2: Basic Setup

> **Duration:** 10-15 minutes  
> **Tone:** Patient, encouraging, step-by-step  
> **Prerequisites:** Node.js 22+, Terminal familiarity

---

## 🎬 Opening

### [VISUAL: Clean terminal window]

**VOICEOVER:**

> "Alright, let's get our hands dirty! In this section, we're going to install Clawdbot, run through the onboarding wizard, and have our first conversation with our new AI assistant.
>
> Don't worry if you're not super technical - I'll walk you through every step."

---

## ✅ Prerequisites Check

### [VISUAL: Show Node.js version check in terminal]

**VOICEOVER:**

> "First, let's make sure we have what we need. Clawdbot requires Node.js version 22 or higher. Let's check what version you have."

### Technical Steps:

```bash
# Check Node.js version
node --version
# Expected output: v22.x.x or higher
```

### [VISUAL: Show the version number output]

**VOICEOVER:**

> "If you see version 22 or higher, you're good to go! If not, head over to nodejs.org and grab the latest LTS version. On Mac, you can also use Homebrew - just run `brew install node`."

### If Node is not installed:

```bash
# macOS with Homebrew
brew install node

# Or download from nodejs.org and use the installer
```

---

## 📦 Installing Clawdbot

### [VISUAL: Terminal with npm install command]

**VOICEOVER:**

> "Now for the fun part - installing Clawdbot. It's just one command."

### Technical Steps:

```bash
# Install Clawdbot globally using npm
npm install -g clawdbot@latest

# If you prefer pnpm (faster)
pnpm add -g clawdbot@latest
```

### [VISUAL: Show installation progress and completion]

**VOICEOVER:**

> "This installs Clawdbot globally on your system, so you can run it from anywhere. The `@latest` ensures you get the newest version.
>
> Depending on your internet speed, this takes about a minute. You'll see some packages being downloaded - that's totally normal."

### Verification:

```bash
# Verify installation
clawdbot --version
```

**VOICEOVER:**

> "Let's verify it worked. Run `clawdbot --version` and you should see the version number. Perfect!"

---

## 🧙 The Onboarding Wizard

### [VISUAL: Terminal ready for onboard command]

**VOICEOVER:**

> "Now comes the magic. Clawdbot has an onboarding wizard that walks you through the entire setup. It'll help you configure your AI model, set up your workspace, and even install a background service so Clawdbot runs automatically."

### Technical Steps:

```bash
# Run the onboarding wizard
clawdbot onboard --install-daemon
```

### [VISUAL: Show the wizard starting with welcome message]

**VOICEOVER:**

> "The `--install-daemon` flag tells it to also set up a background service. On Mac, this uses launchd. On Linux, it's systemd. This means Clawdbot will start automatically when you log in - super convenient."

---

## 🔐 Authentication Options

### [VISUAL: Auth options appearing in wizard]

**VOICEOVER:**

> "The first thing the wizard asks is how you want to authenticate with your AI model. You've got several options here, and I'll explain each one."

### Option 1: Anthropic OAuth (Recommended)

**VOICEOVER:**

> "If you have a Claude Pro or Max subscription, this is the way to go. It uses OAuth - the same kind of login you use for 'Sign in with Google'. A browser window will open, you sign into your Anthropic account, and you're done.
>
> This is my personal recommendation because Claude handles complex tasks really well and has great context understanding."

### [VISUAL: Browser opening for OAuth, then returning to terminal]

### Option 2: OpenAI Codex OAuth

**VOICEOVER:**

> "If you're a ChatGPT Plus or Pro subscriber, you can use OpenAI's OAuth flow. Same idea - browser opens, you sign in, and your subscription is connected."

### Option 3: API Keys

**VOICEOVER:**

> "The classic way. If you have API keys from Anthropic, OpenAI, or other providers, you can enter them directly. These are pay-per-use, so you're charged based on how much you use the API."

### Technical Steps (API Key example):

```bash
# If you chose API key, the wizard will ask you to paste it
# The key is stored securely in ~/.clawdbot/clawdbot.json

# You can also set it via environment variable
export ANTHROPIC_API_KEY="your-key-here"
# or
export OPENAI_API_KEY="your-key-here"
```

---

## 📁 Workspace Configuration

### [VISUAL: Wizard asking about workspace location]

**VOICEOVER:**

> "Next, the wizard asks where you want your workspace. The workspace is where Clawdbot stores your agent configuration, skills, and other files."

### Technical Steps:

```bash
# Default workspace location
~/clawd/

# The wizard creates these files:
# ~/clawd/AGENTS.md   - Agent instructions
# ~/clawd/SOUL.md     - Personality/style
# ~/clawd/TOOLS.md    - Tool descriptions
```

**VOICEOVER:**

> "The default is `~/clawd/` in your home directory. You can change this, but the default works great for most people. The wizard will create some starter files for you - don't worry about these for now, we'll explore them later."

---

## 🔧 Gateway Configuration

### [VISUAL: Gateway configuration options]

**VOICEOVER:**

> "The Gateway is the heart of Clawdbot - it's the service that runs in the background and connects everything together. The wizard will configure it to run on port 18789 by default."

### Technical Steps:

```bash
# Gateway runs on this URL
ws://127.0.0.1:18789

# Configuration is stored in
~/.clawdbot/clawdbot.json
```

**VOICEOVER:**

> "You'll also be asked if you want to enable certain features like browser control. For now, I'd recommend accepting the defaults. We can always change these later."

---

## 🎉 Installation Complete!

### [VISUAL: Wizard completion message with success indicators]

**VOICEOVER:**

> "And just like that, you're done! The wizard has:
> - Configured your AI model authentication
> - Created your workspace
> - Set up the Gateway service
> - Installed a background daemon
>
> Let's verify everything is running."

### Technical Steps:

```bash
# Check if Gateway is running
clawdbot gateway status

# You should see something like:
# ✓ Gateway is running on port 18789
```

**VOICEOVER:**

> "If you see that green checkmark, you're golden!"

---

## 💬 Your First Conversation

### [VISUAL: Terminal with agent command]

**VOICEOVER:**

> "Let's have our first chat with Clawdbot! There are a few ways to do this, but let's start with the command line."

### Technical Steps:

```bash
# Send a message to your AI assistant
clawdbot agent --message "Hello! What can you help me with?"
```

### [VISUAL: Show the AI response streaming in terminal]

**VOICEOVER:**

> "Watch the response stream in. Pretty cool, right? Your personal AI assistant is now running on YOUR machine, ready to help with whatever you need."

### More Examples:

```bash
# Ask a question
clawdbot agent --message "What's the capital of France?"

# Ask for help with something specific
clawdbot agent --message "Help me write a professional email declining a meeting"

# Use extended thinking for complex tasks
clawdbot agent --message "Analyze the pros and cons of remote work" --thinking high
```

**VOICEOVER:**

> "You can ask questions, get help with tasks, or even use the `--thinking` flag for more complex problems where you want the AI to really think it through."

---

## 🌐 The Control UI

### [VISUAL: Browser opening to localhost:18789]

**VOICEOVER:**

> "There's also a web interface! Open your browser and go to localhost port 18789."

### Technical Steps:

```bash
# Open in browser
open http://127.0.0.1:18789
```

### [VISUAL: Show the Control UI with WebChat]

**VOICEOVER:**

> "This is the Control UI. You can chat with your assistant here using WebChat, see what sessions are active, check the status of your channels, and configure settings. It's like mission control for your AI assistant."

---

## 📂 What Got Created?

### [VISUAL: Finder/file explorer showing directories]

**VOICEOVER:**

> "Let's take a quick peek at what the wizard created on your system."

### Technical Steps:

```bash
# State directory - where Clawdbot stores its data
ls ~/.clawdbot/
# clawdbot.json  - Configuration file
# credentials/   - Auth tokens
# sessions/      - Conversation history
# logs/          - Log files

# Workspace directory - your agent's home
ls ~/clawd/
# AGENTS.md  - Agent behavior
# SOUL.md    - Personality
# TOOLS.md   - Tool descriptions
```

**VOICEOVER:**

> "In your home directory, you now have `.clawdbot` - that's where all the state lives. Configuration, credentials, session logs. And you have `clawd` - that's your workspace where you can customize your agent's behavior."

---

## 🔧 Quick Configuration Tips

### [VISUAL: Showing config file in editor]

**VOICEOVER:**

> "Before we move on, let me show you a couple of quick configuration tips."

### Technical Steps:

```bash
# View your configuration
clawdbot config show

# Change the default model
clawdbot config set agents.defaults.model "anthropic/claude-sonnet-4-20250514"

# View a specific setting
clawdbot config get agents.defaults.model
```

**VOICEOVER:**

> "You can use `clawdbot config` to view and change settings without editing files directly. Super handy!"

### Example Configuration (~/.clawdbot/clawdbot.json):

```json
{
  "agent": {
    "model": "anthropic/claude-opus-4-5"
  },
  "agents": {
    "defaults": {
      "workspace": "~/clawd"
    }
  },
  "gateway": {
    "port": 18789
  }
}
```

---

## 🏥 Troubleshooting: clawdbot doctor

### [VISUAL: Running doctor command]

**VOICEOVER:**

> "If something doesn't seem right, Clawdbot has a built-in doctor command that checks your setup."

### Technical Steps:

```bash
# Run diagnostics
clawdbot doctor
```

### [VISUAL: Show doctor output with checks]

**VOICEOVER:**

> "It checks your configuration, verifies credentials, looks for common issues, and gives you actionable fixes. If you ever get stuck, this is your first stop."

---

## ✅ Section Recap

### [VISUAL: Checklist graphic]

**VOICEOVER:**

> "Awesome! Let's recap what we accomplished:
>
> ✓ Installed Node.js 22+
> ✓ Installed Clawdbot globally
> ✓ Ran the onboarding wizard
> ✓ Configured AI authentication
> ✓ Set up our workspace
> ✓ Had our first conversation!
>
> You now have a fully working Clawdbot installation. But right now, we're only chatting through the command line. Let's fix that!
>
> In the next section, we're going to connect your REAL messaging apps - WhatsApp, Telegram, Discord, and more. Get ready to text your AI assistant like it's a friend!"

---

## 📝 Production Notes

### Terminal Commands to Show
- `node --version`
- `npm install -g clawdbot@latest`
- `clawdbot onboard --install-daemon`
- `clawdbot agent --message "Hello!"`
- `clawdbot doctor`

### Potential Issues to Address
- Node version too old (show how to upgrade)
- Permission errors on global install (suggest using `sudo` or fixing npm permissions)
- OAuth browser not opening (show manual URL flow)

### Transitions
- Start: Show terminal ready for commands
- End: Show Control UI, then tease messaging channels

### Key Timestamps
- 0:00 - Prerequisites check
- 2:00 - Installation
- 4:00 - Onboarding wizard start
- 6:00 - Authentication options
- 8:00 - Workspace setup
- 10:00 - First conversation
- 12:00 - Control UI tour
- 14:00 - Recap and next steps
