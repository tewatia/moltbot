# Part 7: Skills

> **Duration:** 10 minutes  
> **Tone:** Exploratory, hands-on, inspiring  
> **Prerequisites:** Clawdbot running with basic configuration

---

## 🎬 Opening

### [VISUAL: Grid of skill icons - home, music, productivity, development]

**VOICEOVER:**

> "Skills are what make Clawdbot incredibly versatile. Out of the box, you've got over 50 skills covering everything from smart home control to music playback, productivity apps to developer tools.
>
> And if there isn't a skill for what you need? You can create your own. Let's dive in."

---

## 🧰 What Are Skills?

### [VISUAL: Skill folder structure]

**VOICEOVER:**

> "A skill is essentially a plugin that extends what Clawdbot can do. Each skill defines tools - specific actions the AI can take - along with prompts and context that help it use those tools effectively."

### Skill Structure:

```text
skills/
├── spotify-player/
│   └── SKILL.md      # Main skill file
├── weather/
│   └── SKILL.md
├── github/
│   └── SKILL.md
└── ... 50+ more
```

**VOICEOVER:**

> "Every skill lives in its own folder with a `SKILL.md` file that describes what it does. That's it! The AI reads this file and knows how to use the skill."

---

## 📂 Built-in Skills Overview

### [VISUAL: Category icons with skill count]

**VOICEOVER:**

> "Let me walk you through the categories of built-in skills. There's a lot here, so I'll highlight the most popular ones."

---

### 🏠 Smart Home

### [VISUAL: Smart home icons - lights, speakers]

```text
openhue    - Philips Hue light control
sonoscli   - Sonos speaker control
```

**VOICEOVER:**

> "Control your Philips Hue lights - 'Turn the living room lights to warm white at 50%'. Play music on Sonos speakers - 'Play some jazz in the kitchen'."

### Demo:

```bash
# Ask Clawd to control lights
"Dim the bedroom lights to 30%"

# Response: AI uses openhue tool to set brightness
```

---

### 🎵 Music & Media

### [VISUAL: Music player icons]

```text
spotify-player - Spotify control
songsee       - Identify songs
video-frames  - Extract frames from videos
```

**VOICEOVER:**

> "Spotify player lets you control playback - 'Play Discover Weekly' or 'Skip this song'. Songsee can identify what song is playing. Video frames extracts stills from videos for analysis."

---

### 📝 Productivity

### [VISUAL: Productivity app icons]

```text
apple-notes      - Create/read Apple Notes
apple-reminders  - Manage Reminders
things-mac       - Things 3 task manager
notion           - Notion pages and databases
obsidian         - Obsidian vault access
trello           - Trello boards and cards
```

**VOICEOVER:**

> "This is probably my favorite category. Apple Notes, Reminders, Things - your native macOS apps. Plus Notion, Obsidian, and Trello for the power users."

### Demo:

```bash
# Create a note
"Create a new note with today's meeting notes"

# Add a reminder
"Remind me to call mom tomorrow at 2pm"

# Notion
"Add a new task to my Notion project board"
```

---

### 💻 Development

### [VISUAL: Developer tool icons]

```text
github    - GitHub API access
tmux      - Terminal session management
coding-agent - Code assistance
```

**VOICEOVER:**

> "GitHub integration for issues, PRs, repos. Tmux for managing terminal sessions. And the coding agent for pair programming assistance."

### Demo:

```bash
# GitHub
"Show me open PRs on my react-app repo"

# Create issue
"Create an issue titled 'Fix login bug' with details about the error"
```

---

### 🔐 Security & Utils

### [VISUAL: Security icons]

```text
1password  - 1Password vault access
weather    - Weather forecasts
himalaya   - Email access (IMAP)
```

**VOICEOVER:**

> "1Password integration - ask for credentials securely. Weather skill for forecasts. Himalaya for email access through IMAP."

---

### 📸 Camera & Vision

### [VISUAL: Camera and vision icons]

```text
camsnap   - Take photos
peekaboo  - Screen capture
openai-whisper - Audio transcription
openai-image-gen - Image generation
```

**VOICEOVER:**

> "Camera snap for photos, Peekaboo for screen capture, Whisper for transcribing audio, and image generation for creating visuals."

---

## 🛒 ClawdHub: Skill Registry

### [VISUAL: ClawdHub website]

**VOICEOVER:**

> "Not finding what you need in the built-in skills? ClawdHub is a community registry where you can discover and install skills from other users."

### Technical Steps:

```bash
# Enable ClawdHub
clawdbot config set clawdhub.enabled true

# Search for skills
clawdbot skills search "calendar"

# Install a skill
clawdbot skills install @username/google-calendar

# View installed skills
clawdbot skills list
```

### [VISUAL: Search results on ClawdHub]

**VOICEOVER:**

> "Enable ClawdHub in config, then search for skills. Found one you like? Install it with `clawdbot skills install`. It downloads to your workspace and is immediately available."

### Website:

**VOICEOVER:**

> "You can also browse at clawdhub.com to see what's available. Rating, documentation, examples - it's all there."

---

## 🛠️ Creating Your Own Skill

### [VISUAL: Text editor with SKILL.md]

**VOICEOVER:**

> "The real power is creating your own skills. Let's build one together - a simple weather skill."

### Step 1: Create the Folder

```bash
# Navigate to your workspace
cd ~/clawd/skills

# Create skill folder
mkdir my-weather
cd my-weather
```

### Step 2: Write SKILL.md

```markdown
---
name: my-weather
description: Get current weather for any location
tools:
  - name: get_weather
    description: Fetch current weather conditions
    parameters:
      location:
        type: string
        required: true
        description: City name or coordinates
---

# My Weather Skill

This skill provides current weather information.

## Usage

Ask about weather in any location:
- "What's the weather in Tokyo?"
- "Is it raining in London?"
- "Temperature in New York?"

## Implementation

When the `get_weather` tool is called, run this command:

```bash
curl -s "wttr.in/${location}?format=%C+%t+%h+%w"
```

This returns conditions, temperature, humidity, and wind.
```

### [VISUAL: SKILL.md file content]

**VOICEOVER:**

> "The frontmatter defines metadata - name, description, and tools with their parameters. The body explains how to use the skill and what commands to run. That's all you need!"

### Step 3: Test It

```bash
# Restart gateway to pick up new skill
clawdbot gateway restart

# Test in chat
"What's the weather in Paris?"
```

### [VISUAL: Weather response appearing]

**VOICEOVER:**

> "Restart the gateway, ask about weather, and... there it is! The AI used our custom skill. From zero to working skill in about 5 minutes."

---

## 🔧 Advanced Skill Features

### [VISUAL: Advanced skill configuration]

**VOICEOVER:**

> "Skills can get more sophisticated. Here are some advanced features:"

### Environment Variables:

```markdown
---
name: my-api-skill
env:
  - API_KEY: required
---
```

**VOICEOVER:**

> "Require environment variables for API keys."

### Dependencies:

```markdown
---
dependencies:
  - npm:axios
  - brew:jq
---
```

**VOICEOVER:**

> "Specify npm packages or system tools that need to be installed."

### Authenticated Tools:

```markdown
---
tools:
  - name: make_request
    auth:
      type: oauth
      provider: google
---
```

**VOICEOVER:**

> "Request OAuth authentication for tools that need it."

---

## 💡 Skill Ideas

### [VISUAL: Lightbulb with brainstorming ideas]

**VOICEOVER:**

> "Need inspiration? Here are some skill ideas:"

```text
Personal:
- Custom recipe lookup
- Fitness tracker integration
- Habit monitoring

Work:
- Jira/Linear ticket management
- Meeting transcription
- Time tracking

Fun:
- Movie recommendations
- Game inventory
- Pet care reminders

Smart Home:
- Custom automation sequences
- Energy monitoring
- Security camera snapshots
```

---

## ✅ Section Recap

### [VISUAL: Skill icons with checkmarks]

**VOICEOVER:**

> "Skills are the secret sauce:
>
> ✓ 50+ built-in skills covering major use cases
> ✓ Smart home, music, productivity, development
> ✓ ClawdHub for community skills
> ✓ Create your own in minutes
>
> You're not limited to what Clawdbot ships with. If you can write a command or call an API, you can make a skill for it.
>
> Next up: Advanced features. Multi-agent routing, security deep-dive, remote gateways, and more power-user stuff. Let's go!"

---

## 📝 Production Notes

### Skills to Demo
- One smart home (Hue if available)
- One productivity (Apple Notes or Reminders)
- One developer (GitHub)
- Custom skill creation

### ClawdHub
- Have account ready for demo
- Maybe pre-install a skill to show

### Custom Skill Demo
- Weather example is simple and visual
- Works without API keys (uses wttr.in)

### Key Timestamps
- 0:00 - What are skills
- 1:00 - Built-in skills overview
- 2:30 - Smart home demo
- 3:30 - Productivity skills
- 4:30 - Developer skills
- 5:30 - ClawdHub
- 6:30 - Creating custom skill
- 8:30 - Advanced features
- 9:30 - Recap
