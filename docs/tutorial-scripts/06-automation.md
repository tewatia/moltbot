# Part 6: Automation

> **Duration:** 10 minutes  
> **Tone:** Empowering, practical, "set it and forget it"  
> **Prerequisites:** Clawdbot running with tools enabled

---

## 🎬 Opening

### [VISUAL: Clock/calendar icons with automation arrows]

**VOICEOVER:**

> "What if Clawdbot could work for you even when you're not actively chatting with it? Check your calendar every morning, summarize your emails, monitor a website for changes, or remind you about important dates.
>
> That's automation. Let's set it up."

---

## ⏰ Cron Jobs

### [VISUAL: Clock face with schedule markers]

**VOICEOVER:**

> "Cron jobs are scheduled tasks that run automatically. You know how some apps send you a 'daily digest' email? With Clawdbot, you can create your own."

### Setup:

```bash
# Enable cron in config
clawdbot config set cron.enabled true
```

### Configuration Example:

```json
{
  "cron": {
    "enabled": true,
    "jobs": [
      {
        "id": "morning-briefing",
        "schedule": "0 8 * * *",
        "message": "Give me a morning briefing: weather, calendar highlights, and top news.",
        "channel": "telegram",
        "to": "+1234567890"
      },
      {
        "id": "daily-reminder",
        "schedule": "0 17 * * 1-5",
        "message": "Remind me to log my work hours",
        "channel": "slack",
        "to": "#personal"
      }
    ]
  }
}
```

### [VISUAL: Config file being edited]

**VOICEOVER:**

> "Cron uses the standard cron format - minute, hour, day of month, month, day of week. The first example runs at 8 AM every day. The second runs at 5 PM on weekdays.
>
> The 'message' is what gets sent to the AI, and 'channel' plus 'to' specify where the response should go."

### Schedule Reference:

```text
# Cron format: minute hour day-of-month month day-of-week
0 8 * * *      # Every day at 8:00 AM
0 9 * * 1      # Every Monday at 9:00 AM
*/30 * * * *   # Every 30 minutes
0 0 1 * *      # First day of every month
```

**VOICEOVER:**

> "Here's a quick reference. Every morning at 8? `0 8 * * *`. Every Monday at 9? Add a 1 at the end for Monday. Every 30 minutes? `*/30 * * * *`. You get the idea."

### Demo:

### [VISUAL: Setting up a test cron that runs in 1 minute]

```bash
# For testing, set a job to run soon
# Edit config with job running in 1 minute
clawdbot config set cron.jobs '[{"id":"test","schedule":"* * * * *","message":"Say hello!","channel":"webchat"}]'

# Restart gateway
clawdbot gateway restart

# Wait for it to run...
```

**VOICEOVER:**

> "Let's set up a test. I'll create a job that runs every minute and says hello. Restart the gateway, and... wait for it... there! The message came through automatically."

### Practical Use Cases:

```text
Morning Briefing:
- Weather forecast
- Calendar events for the day
- Unread emails summary
- Top news headlines

Work Automation:
- Daily standup reminders
- Weekly report generation
- Log hours reminder

Personal:
- Medication reminders
- Weekly meal planning
- Birthday wish reminders
```

---

## 🪝 Webhooks

### [VISUAL: Webhook flow diagram]

**VOICEOVER:**

> "Webhooks let external services trigger Clawdbot. Someone submits a form on your website? Webhook. A payment comes through Stripe? Webhook. An alert fires in your monitoring system? You guessed it."

### Setup:

```bash
# Enable webhooks
clawdbot config set webhooks.enabled true

# Set a security token
clawdbot config set webhooks.token "your-secret-token"

# Webhook endpoint becomes available at:
# POST http://127.0.0.1:18789/hooks/trigger
```

### [VISUAL: Config showing webhook settings]

**VOICEOVER:**

> "Enable webhooks in config and set a secret token for security. Now you have an endpoint that external services can call."

### Sending a Webhook:

```bash
# From any service, POST to this endpoint
curl -X POST http://127.0.0.1:18789/hooks/trigger \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer your-secret-token" \
  -d '{
    "message": "New user signed up: john@example.com",
    "channel": "slack",
    "to": "#signups"
  }'
```

### [VISUAL: Curl command and Slack message appearing]

**VOICEOVER:**

> "Here's how to trigger it. Send a POST request with your message, the channel, and where to deliver. The AI can process the incoming data and respond accordingly."

### Integration Ideas:

```text
- GitHub: Notify about stars, issues, PRs
- Stripe: Summarize new payments
- Zapier: Connect 1000+ services
- Custom apps: Your own integrations
```

**VOICEOVER:**

> "Connect it to GitHub for repository activity, Stripe for payment notifications, or use Zapier to connect literally thousands of services. The possibilities are endless."

---

## 📧 Gmail Pub/Sub

### [VISUAL: Gmail logo with notification bell]

**VOICEOVER:**

> "Here's a powerful one for Gmail users. Gmail Pub/Sub lets Clawdbot get notified in real-time when new emails arrive. Perfect for summarizing important messages or filtering urgent ones."

### Prerequisites:

```text
1. Google Cloud account
2. Gmail API enabled
3. Pub/Sub API enabled
4. OAuth credentials
```

### Setup Overview:

```bash
# Step 1: Enable Gmail API in Google Cloud Console
# Step 2: Enable Pub/Sub API
# Step 3: Create a topic (projects/your-project/topics/gmail)
# Step 4: Create a push subscription
# Step 5: Configure Clawdbot with credentials

clawdbot config set gmail.pubsub.enabled true
clawdbot config set gmail.pubsub.topicName "projects/your-project/topics/gmail"
```

### [VISUAL: Google Cloud Console configuration]

**VOICEOVER:**

> "The setup involves Google Cloud - enable APIs, create a Pub/Sub topic, and set up a subscription. I won't go through every step here, but check the docs at docs.clawd.bot/automation/gmail-pubsub for the full walkthrough."

### What You Can Do:

```text
- "Summarize unread emails from today"
- Automatic triage of urgent vs routine
- Auto-respond to certain patterns
- Forward key emails to other channels
```

**VOICEOVER:**

> "Once set up, you can ask for email summaries, get notified about important senders, or even have the AI triage your inbox. 'Any urgent emails from my boss?' - Clawdbot can tell you immediately."

---

## 🔄 Polling (Heartbeat)

### [VISUAL: Heartbeat pulse animation]

**VOICEOVER:**

> "Not everything needs webhooks. For simpler checks, Clawdbot supports polling - checking something repeatedly at an interval."

### Configuration:

```json
{
  "polling": {
    "jobs": [
      {
        "id": "website-monitor",
        "interval": "5m",
        "message": "Check if example.com is responding. If it's down, alert me.",
        "channel": "telegram"
      }
    ]
  }
}
```

**VOICEOVER:**

> "Set up a polling job with an interval like '5m' for 5 minutes or '1h' for hourly. Unlike cron which runs at specific times, polling runs continuously at intervals."

### Use Cases:

```text
- Monitor website uptime
- Check stock prices
- Track new items on a page
- Periodic backups
- Service health checks
```

---

## 🎯 Practical Example: Full Automation Stack

### [VISUAL: Complete automation workflow diagram]

**VOICEOVER:**

> "Let me show you what a full automation setup might look like for a solo developer."

### Example Configuration:

```json
{
  "cron": {
    "enabled": true,
    "jobs": [
      {
        "id": "morning-standup",
        "schedule": "0 9 * * 1-5",
        "message": "Morning! Check my GitHub notifications, calendar for today, and any unread Slack messages. Give me a 1-minute summary.",
        "channel": "telegram",
        "to": "+1234567890"
      },
      {
        "id": "evening-review",
        "schedule": "0 18 * * 1-5",
        "message": "End of day! Summarize what I accomplished in git commits, create a list of open items for tomorrow.",
        "channel": "slack",
        "to": "#work-log"
      }
    ]
  },
  "webhooks": {
    "enabled": true,
    "token": "your-secret-token"
  }
}
```

### [VISUAL: Timeline of automated messages throughout the day]

**VOICEOVER:**

> "At 9 AM, Clawdbot sends you a morning briefing on Telegram with GitHub notifications, calendar events, and Slack summaries. At 6 PM, it posts a work log to your Slack with git activity and open items.
>
> You've just automated your daily standups and journaling. That's what I call working smarter!"

---

## ✅ Section Recap

### [VISUAL: Automation icons with checkmarks]

**VOICEOVER:**

> "Automation is what turns Clawdbot from a tool you use into a system that works for you. Here's what we covered:
>
> ✓ Cron jobs for scheduled tasks
> ✓ Webhooks for external triggers
> ✓ Gmail Pub/Sub for real-time email
> ✓ Polling for continuous checks
>
> Set it once, and it runs forever. Morning briefings, daily reminders, website monitoring - all automated.
>
> Next up: Skills! We're going to explore the 50+ built-in skills and learn how to create your own. Smart home control, music, productivity apps - let's go!"

---

## 📝 Production Notes

### Demos to Prepare
- Cron job running live (use short interval for demo)
- Curl webhook triggering a message
- Show Gmail notification arriving (if setup complete)

### Config Files to Show
- Cron job configuration
- Webhook configuration
- Gmail Pub/Sub (mention setup is involved)

### Tools/Services Needed
- Terminal for curl
- Test channel (WebChat works well)
- Optional: Google Cloud account for Gmail demo

### Key Timestamps
- 0:00 - Automation overview
- 1:00 - Cron jobs explained
- 4:00 - Cron demo
- 5:30 - Webhooks setup
- 7:00 - Webhook demo
- 8:00 - Gmail Pub/Sub overview
- 9:00 - Polling
- 9:30 - Full example and recap
