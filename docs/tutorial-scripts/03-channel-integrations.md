# Part 3: Channel Integrations

> **Duration:** 15-20 minutes  
> **Tone:** Exciting, practical, step-by-step  
> **Prerequisites:** Clawdbot installed and running (Part 2 complete)

---

## 🎬 Opening

### [VISUAL: Montage of different messaging app icons]

**VOICEOVER:**

> "Okay, this is where things get really fun! Right now we can chat with Clawdbot through the command line or web interface. But let's be real - you want to text your AI assistant on WhatsApp, right? Or have it respond in your Telegram groups? Maybe help you out in Slack?
>
> That's exactly what we're setting up in this section. Let's connect your messaging apps!"

---

## 📱 Channel Overview

### [VISUAL: Grid of all supported channel logos]

**VOICEOVER:**

> "Clawdbot supports over 20 messaging channels. I'm going to walk you through the most popular ones:
>
> - WhatsApp - the big one, billions of users
> - Telegram - great for groups and bots
> - Discord - perfect for communities
> - Slack - for work teams
>
> And I'll mention the others so you know what's available. The setup process is similar across all of them."

---

## 💚 WhatsApp Setup

### [VISUAL: Terminal ready for WhatsApp login]

**VOICEOVER:**

> "Let's start with WhatsApp - this is probably the one most people want. Now, I need to mention something important upfront.
>
> Clawdbot uses a library called Baileys to connect to WhatsApp. This is NOT an official WhatsApp API - it's a reverse-engineered implementation. It works great, but technically it's against WhatsApp's terms of service. So use it responsibly, don't spam, and understand the risks."

### [VISUAL: Terminal showing login command]

### Technical Steps:

```bash
# Start the WhatsApp login process
clawdbot channels login
```

### [VISUAL: QR code appearing in terminal]

**VOICEOVER:**

> "When you run this command, you'll see a QR code appear in your terminal. Now grab your phone, open WhatsApp, go to Settings → Linked Devices → Link a Device, and scan this code."

### [VISUAL: Phone scanning QR code, then showing "Linked" status]

**VOICEOVER:**

> "Once you scan it, you'll see the connection establish. Your WhatsApp is now linked to Clawdbot! It's using WhatsApp's multi-device feature - the same thing that lets you use WhatsApp Web."

### Configuration:

```bash
# View current WhatsApp configuration
clawdbot config get channels.whatsapp

# Set who can message the bot (allowlist)
clawdbot config set channels.whatsapp.allowFrom '["*"]'  # Anyone (be careful!)
# or
clawdbot config set channels.whatsapp.allowFrom '["+1234567890"]'  # Specific numbers
```

### [VISUAL: Config file showing allowFrom]

**VOICEOVER:**

> "By default, Clawdbot uses something called DM pairing. This means when someone new messages you, they get a pairing code and the bot doesn't respond until you approve them. It's a security feature!
>
> You can configure an allowlist if you want specific people to always get through."

### Testing:

**VOICEOVER:**

> "Let's test it! Send a message to your own number from another phone or ask a friend to message you."

### [VISUAL: WhatsApp message being received and responded to]

**VOICEOVER:**

> "And there it is! Your AI assistant, responding on WhatsApp. Pretty magical, right?"

---

## 🔵 Telegram Setup

### [VISUAL: Telegram app and BotFather]

**VOICEOVER:**

> "Telegram is next, and this one's actually the cleanest integration because Telegram officially supports bots. We'll create a bot through BotFather - Telegram's bot management tool."

### Technical Steps:

**Step 1: Create a Bot**

```bash
# In Telegram, message @BotFather
# Send: /newbot
# Follow the prompts to name your bot
# You'll receive a token like: 123456789:ABCdefGHIjklMNOpqrsTUVwxyz
```

### [VISUAL: BotFather conversation showing token]

**VOICEOVER:**

> "Open Telegram and search for BotFather - that's the official Telegram bot for creating bots. Meta, right?
>
> Send `/newbot`, give your bot a name like 'My Clawdbot', and a username like 'myclawd_bot'. BotFather will give you a token - copy that."

**Step 2: Configure Clawdbot**

```bash
# Set the Telegram bot token
clawdbot config set channels.telegram.botToken "YOUR_BOT_TOKEN_HERE"

# Or use environment variable
export TELEGRAM_BOT_TOKEN="123456789:ABCdefGHIjklMNOpqrsTUVwxyz"
```

### [VISUAL: Config being set in terminal]

**VOICEOVER:**

> "Now we tell Clawdbot about this token. You can set it in the config or use an environment variable - either works."

**Step 3: Restart Gateway**

```bash
# Restart to pick up new configuration
clawdbot gateway restart
```

**VOICEOVER:**

> "Restart the Gateway so it connects with the new token."

### Testing:

### [VISUAL: Finding and messaging the bot in Telegram]

**VOICEOVER:**

> "Now find your bot in Telegram - search for the username you created - and send it a message. You should get a response from your AI assistant!"

### Group Configuration:

```json
{
  "channels": {
    "telegram": {
      "botToken": "your-token",
      "groups": {
        "*": {
          "requireMention": true
        }
      }
    }
  }
}
```

**VOICEOVER:**

> "If you add your bot to groups, you can configure whether it responds to every message or only when mentioned. I recommend `requireMention: true` for groups so it doesn't spam every conversation."

---

## 🟣 Discord Setup

### [VISUAL: Discord Developer Portal]

**VOICEOVER:**

> "Discord is fantastic for communities, and setting up a bot here gives Clawdbot some extra powers - like slash commands and reactions."

### Technical Steps:

**Step 1: Create a Discord Application**

```text
1. Go to discord.com/developers/applications
2. Click "New Application"
3. Name it "Clawdbot" or whatever you like
4. Go to "Bot" section
5. Click "Add Bot"
6. Enable these intents:
   - MESSAGE CONTENT INTENT
   - SERVER MEMBERS INTENT
   - PRESENCE INTENT
7. Copy the bot token
```

### [VISUAL: Discord developer portal walkthrough]

**VOICEOVER:**

> "Head to the Discord Developer Portal and create a new application. Under the Bot section, add a bot, enable the message content intent - this is important so it can read messages - and copy the token."

**Step 2: Configure Clawdbot**

```bash
# Set Discord token
clawdbot config set channels.discord.token "YOUR_DISCORD_BOT_TOKEN"

# Or use environment variable
export DISCORD_BOT_TOKEN="your-token-here"
```

**Step 3: Invite Bot to Server**

```text
1. Go to OAuth2 → URL Generator
2. Select "bot" scope
3. Select needed permissions (Send Messages, Read Message History, etc.)
4. Copy the generated URL
5. Open URL in browser and select your server
```

### [VISUAL: Bot joining server]

**VOICEOVER:**

> "Generate an invite URL in the OAuth2 section, open it in your browser, and add the bot to your server. You should see Clawdbot appear in your member list!"

### Restart and Test:

```bash
# Restart gateway
clawdbot gateway restart

# In Discord, type:
@Clawdbot Hello! Can you help me?
```

### [VISUAL: Discord message and response]

**VOICEOVER:**

> "Mention your bot in any channel and watch it respond. Discord has great support for formatting, so your AI responses look really nice here."

---

## 💼 Slack Setup

### [VISUAL: Slack app directory]

**VOICEOVER:**

> "Slack is essential for work communication. Setting up Clawdbot here is a bit more involved because Slack uses App tokens, but I'll walk you through it."

### Technical Steps:

**Step 1: Create a Slack App**

```text
1. Go to api.slack.com/apps
2. Click "Create New App" → "From scratch"
3. Name it and select your workspace
4. Go to "Socket Mode" → Enable it
5. Go to "OAuth & Permissions"
6. Add Bot Token Scopes:
   - app_mentions:read
   - channels:history
   - chat:write
   - im:history
   - im:write
7. Install to workspace
8. Copy the Bot Token (xoxb-...)
9. Go to "Basic Information" → App-Level Tokens
10. Generate token with "connections:write" scope
11. Copy the App Token (xapp-...)
```

### [VISUAL: Slack API dashboard walkthrough]

**VOICEOVER:**

> "Create a new Slack app, enable Socket Mode - this lets us receive messages without setting up a public webhook - and configure the OAuth scopes. You need two tokens: the Bot Token that starts with xoxb, and the App Token that starts with xapp."

**Step 2: Configure Clawdbot**

```bash
# Set both tokens
clawdbot config set channels.slack.botToken "xoxb-..."
clawdbot config set channels.slack.appToken "xapp-..."

# Or use environment variables
export SLACK_BOT_TOKEN="xoxb-..."
export SLACK_APP_TOKEN="xapp-..."
```

**Step 3: Enable Events**

```text
1. Go to "Event Subscriptions" → Enable Events
2. Subscribe to bot events:
   - app_mention
   - message.im
3. Save changes
```

### [VISUAL: Event subscriptions configuration]

**VOICEOVER:**

> "Enable event subscriptions and subscribe to `app_mention` and `message.im`. This tells Slack to notify Clawdbot when someone mentions it or sends a DM."

### Testing:

```bash
# Restart gateway
clawdbot gateway restart

# In Slack, type:
@Clawdbot What's on my schedule today?
```

### [VISUAL: Slack message and response]

**VOICEOVER:**

> "Restart the gateway, then mention your bot in Slack. You'll get a response right in your workspace. Now you can get AI help without leaving your work chat!"

---

## 📋 Other Channels Overview

### [VISUAL: Grid of remaining channel logos]

**VOICEOVER:**

> "Those are the big four, but Clawdbot supports many more. Let me quickly run through them:"

### Signal
- Requires signal-cli installed separately
- Very privacy-focused
- Great for secure communications

### iMessage (macOS only)
- Works through the Messages app
- Requires macOS with Messages signed in
- Great for Apple ecosystem users

### Microsoft Teams
- Enterprise-grade setup
- Requires Azure Bot Framework registration
- Perfect for corporate environments

### Matrix
- Open, decentralized protocol
- Self-hosted friendly
- Great for privacy-conscious organizations

### Google Chat
- Requires Google Workspace
- Good for G Suite organizations

### BlueBubbles
- Alternative iMessage integration
- Requires BlueBubbles server

### Zalo
- Popular in Vietnam
- Both business and personal accounts

### WebChat
- Built into the Gateway
- No additional setup needed
- Access at localhost:18789

---

## 🔒 Security: DM Pairing

### [VISUAL: Pairing code example]

**VOICEOVER:**

> "Before we wrap up, let's talk about security. By default, Clawdbot uses DM pairing. When someone new messages you, they get a short code and the bot doesn't respond until you approve them."

### Technical Steps:

```bash
# View pending pairing requests
clawdbot pairing list

# Approve a pairing request
clawdbot pairing approve telegram ABC123

# The sender is now added to your allowlist
```

**VOICEOVER:**

> "This prevents random strangers from eating up your AI tokens! You can manage pairing with the `clawdbot pairing` commands."

### Alternative: Open DMs

```json
{
  "channels": {
    "telegram": {
      "dmPolicy": "open",
      "allowFrom": ["*"]
    }
  }
}
```

**VOICEOVER:**

> "If you want anyone to be able to message, you can set `dmPolicy` to open. Just be aware that anyone who finds your bot can then use it - and that uses your API credits."

---

## ✅ Section Recap

### [VISUAL: Checklist graphic with channel logos]

**VOICEOVER:**

> "That was a lot, but look what we accomplished!
>
> ✓ Connected WhatsApp via QR code
> ✓ Set up a Telegram bot through BotFather
> ✓ Created a Discord application and added it to a server
> ✓ Configured a Slack app with socket mode
> ✓ Learned about DM pairing security
>
> You can now text your AI assistant on any of these platforms. Send a question from WhatsApp while walking the dog, get help in Slack during a work meeting, or have it moderate your Discord server.
>
> But wait, there's more! In the next section, we'll look at the companion apps for Mac, iPhone, and Android. Imagine talking to your AI assistant with your voice..."

---

## 📝 Production Notes

### Channels to Demo Live
- WhatsApp QR scan (most visual impact)
- Telegram BotFather flow
- Discord slash commands (if time permits)

### Screen Recording Tips
- Use test accounts where possible
- Blur any sensitive phone numbers
- Pre-create bot accounts to save time

### Potential Issues
- WhatsApp QR expires (need to refresh)
- Discord intents not enabled (common mistake)
- Slack tokens mixed up (bot vs app token)

### Key Timestamps
- 0:00 - Channel overview
- 2:00 - WhatsApp setup
- 7:00 - Telegram setup
- 11:00 - Discord setup
- 15:00 - Slack setup
- 18:00 - Other channels overview
- 19:00 - Security and recap
