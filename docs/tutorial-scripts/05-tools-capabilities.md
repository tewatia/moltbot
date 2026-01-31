# Part 5: Tools & Capabilities

> **Duration:** 15 minutes  
> **Tone:** Empowering, practical, demo-heavy  
> **Prerequisites:** Clawdbot running with at least one channel connected

---

## 🎬 Opening

### [VISUAL: Toolbox opening with various tool icons]

**VOICEOVER:**

> "Alright, this is where Clawdbot gets really powerful. We've connected our messaging apps, we've set up our devices. Now let's talk about what Clawdbot can actually DO.
>
> We're talking about tools - browser control, running commands, file operations, and more. By the end of this section, you'll understand why people call this a personal AI 'assistant' and not just a 'chatbot'."

---

## 🌐 Browser Control

### [VISUAL: Clawdbot browser opening]

**VOICEOVER:**

> "Imagine this: you ask your AI to 'find the best flight from New York to London for next month.' Instead of just telling you to go check a website, Clawdbot can actually open a browser, navigate to a flight search, enter your criteria, and show you the results.
>
> That's browser control."

### Setup:

```bash
# Enable browser in config
clawdbot config set browser.enabled true

# Configure the control URL (where browser CDP connects)
clawdbot config set browser.controlUrl "http://127.0.0.1:18791"
```

### [VISUAL: Browser launching]

**VOICEOVER:**

> "Clawdbot uses a dedicated browser - usually Chrome or Chromium - that it controls through the Chrome DevTools Protocol. This means your personal browser stays separate; the AI gets its own sandbox to play in."

### Demo:

### [VISUAL: Asking AI to search something, watching browser act]

```bash
# Ask the AI to use the browser
clawdbot agent --message "Search for the latest news about renewable energy"
```

**VOICEOVER:**

> "Let's try it. 'Clawd, search for the latest news about renewable energy.' Watch the browser..."

### [VISUAL: Browser navigating, searching, results appearing]

**VOICEOVER:**

> "It opened a search engine, typed my query, and here are the results. Now I can say 'summarize the top 3 articles' and it'll read through them for me. Pretty incredible, right?"

### Browser Capabilities:

```text
- Navigate to URLs
- Click elements
- Fill forms
- Take screenshots
- Extract text content
- Handle basic authentication
- Manage multiple tabs
```

### Login Assistance:

```bash
# For sites that need login, use browser login helper
clawdbot browser login --profile "my-github"

# This opens browser for manual login, then saves cookies
```

**VOICEOVER:**

> "For sites that require login, you can use `browser login` to manually sign in once. Clawdbot saves the session so it can access that site automatically in the future."

---

## ⌨️ Bash/Exec Tool

### [VISUAL: Terminal commands running]

**VOICEOVER:**

> "This one's powerful and slightly dangerous - in a good way. The exec tool lets Clawdbot run commands on your machine. Rename files, check system status, run scripts - if you can do it in the terminal, Clawdbot can too."

### Technical Steps:

```bash
# Tell Clawd to run a command
clawdbot agent --message "What's my disk usage?"

# The AI will use the exec tool to run something like:
# df -h
# And interpret the results for you
```

### [VISUAL: AI running df command, showing results]

**VOICEOVER:**

> "Ask 'what's my disk usage' and Clawd runs `df -h`, reads the output, and tells you in plain English that you have 50GB free on your main drive."

### More Examples:

```bash
# File operations
"Create a new folder called 'projects' on my Desktop"
# → mkdir ~/Desktop/projects

# System info
"What processes are using the most CPU?"
# → ps aux --sort=-%cpu | head -10

# Git operations
"Show me the recent commits in this repo"
# → git log --oneline -10
```

**VOICEOVER:**

> "Create folders, check processes, run git commands - you're essentially pair programming with an AI that can actually execute code."

### Safety: Elevated Mode

```bash
# By default, potentially dangerous commands need approval
# You can toggle elevated mode per-session

# In chat:
/elevated on   # Enable full command access
/elevated off  # Back to restricted mode
```

### [VISUAL: /elevated command in chat]

**VOICEOVER:**

> "Now, you might be wondering - isn't this dangerous? What if it deletes my files?
>
> By default, Clawdbot asks for confirmation on potentially destructive commands. There's also an 'elevated mode' you can toggle for a session when you trust what you're doing."

---

## 📁 File Operations

### [VISUAL: File tree visualization]

**VOICEOVER:**

> "File operations are part of the exec tool, but they deserve special mention. Clawdbot can read, write, create, and edit files."

### Examples:

```bash
# Read a file
"What's in my .bashrc file?"

# Edit a file
"Add an alias 'gs' for 'git status' to my .zshrc"

# Create a file
"Create a Python script that prints hello world and save it as hello.py"

# Organize files
"Move all .jpg files from Downloads to Pictures/Photos"
```

### [VISUAL: Files being organized by AI]

**VOICEOVER:**

> "Ask it to organize your downloads, edit configuration files, or create new scripts. It reads the current content, understands what you want, and makes the changes."

### Safety Notes:

**VOICEOVER:**

> "A word of caution - always be specific about what you want changed. And maybe keep backups of important files until you trust the flow. The AI is smart, but it's still following your instructions."

---

## 🔗 Session Tools (Multi-Agent)

### [VISUAL: Multiple agent bubbles communicating]

**VOICEOVER:**

> "Here's something unique to Clawdbot - session tools. These let different AI sessions communicate with each other."

### Commands:

```bash
# List active sessions
# In chat or via CLI
clawdbot sessions list

# See another session's history
clawdbot sessions history --session work-agent

# Send a message between sessions
clawdbot sessions send --to family-agent --message "Add milk to the grocery list"
```

### [VISUAL: Message passing between sessions]

**VOICEOVER:**

> "Imagine you have a 'work' agent and a 'personal' agent. From work, you suddenly remember you need groceries. Instead of switching contexts, just say 'Tell my family agent to add milk to the shopping list.' The work agent sends a message to the family agent."

### Use Cases:

```text
- Delegating tasks between specialized agents
- Sharing context across sessions
- Coordinating work across different domains
```

**VOICEOVER:**

> "It's like having a team of AI assistants that can coordinate. Pretty cool for power users who want specialized agents for different areas of life."

---

## 🕐 Chat Commands

### [VISUAL: Slash commands in chat]

**VOICEOVER:**

> "You've already seen `/elevated`, but there are more chat commands that control your session."

### Available Commands:

```text
/status   - See current session status (model, tokens used)
/new      - Start a fresh session
/reset    - Same as /new
/compact  - Summarize and compress the session context
/think    - Set thinking level (off, low, medium, high)
/verbose  - Toggle verbose output
/usage    - Show token usage
```

### [VISUAL: Running various commands in chat]

**VOICEOVER:**

> "These work in any chat interface - WhatsApp, Telegram, WebChat. `/status` shows what model you're using and how many tokens you've consumed. `/new` starts fresh. `/compact` is great when your conversation is getting long - it summarizes everything so you can keep going without hitting context limits."

### Thinking Levels:

```bash
/think high
# Now ask a complex question...
"Analyze the pros and cons of different database architectures for a social media app"
```

**VOICEOVER:**

> "The `/think` command is special. Set it to 'high' for complex problems. The AI takes more time to reason through the problem step by step. Great for coding, analysis, or decision-making."

---

## 🎯 Practical Demo: Full Workflow

### [VISUAL: Complete workflow demonstration]

**VOICEOVER:**

> "Let me show you a full workflow putting it all together."

### Scenario: Creating a Project

```bash
# Step 1: Create project structure
"Create a new Node.js project called 'my-api' with an src folder and a basic package.json"

# Step 2: Write some code
"Add an Express server in src/index.js with a health check endpoint"

# Step 3: Test it
"Run npm install and start the server"

# Step 4: Research
"Open the browser and find the Express.js documentation for middleware"

# Step 5: Extend
"Based on the docs, add a logging middleware to our server"
```

### [VISUAL: Each step happening in real-time]

**VOICEOVER:**

> "Watch this: Create project - done. Write code - here's an Express server. Run it - npm install, start, boom it's running. Need docs? Browser opens Express.js. Add middleware - code is updated.
>
> In five minutes, with just natural language, we built a working API server."

---

## ✅ Section Recap

### [VISUAL: Tool icons with checkmarks]

**VOICEOVER:**

> "We covered a lot of ground! Let's recap:
>
> ✓ Browser control - AI surfs the web for you
> ✓ Bash/exec - run any terminal command
> ✓ File operations - read, write, organize files
> ✓ Session tools - agents talking to agents
> ✓ Chat commands - control your session inline
>
> These tools are what transform Clawdbot from a chatbot into a true assistant. It's not just answering questions - it's actually doing things on your behalf.
>
> Next up: automation! Cron jobs, webhooks, and email triggers. Let's make this thing run on autopilot."

---

## 📝 Production Notes

### Demos to Record
- Browser opening and searching (visual impact!)
- Running a command, showing results
- Creating and editing a file
- Chat commands in action
- Full workflow (project creation)

### Safety Callouts
- Emphasize sandbox/approval model
- Show what happens when command needs confirmation
- Mention that main session has full access

### Terminal Preparation
- Have a clean directory for file demos
- Pre-install any needed packages
- Test browser control works before recording

### Key Timestamps
- 0:00 - Tools overview
- 1:00 - Browser control setup
- 4:00 - Browser demo
- 6:00 - Bash/exec tool
- 8:00 - File operations
- 10:00 - Session tools
- 12:00 - Chat commands
- 14:00 - Full workflow demo
