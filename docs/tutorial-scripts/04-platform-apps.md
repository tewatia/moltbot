# Part 4: Platform Apps

> **Duration:** 10 minutes  
> **Tone:** Exciting, hands-on, visual  
> **Prerequisites:** Clawdbot installed and configured (Parts 1-3 complete)

---

## 🎬 Opening

### [VISUAL: Split screen showing Mac, iPhone, and Android]

**VOICEOVER:**

> "So far we've been chatting through messaging apps and the command line. But Clawdbot has some awesome companion apps that take things to the next level.
>
> Imagine speaking to your AI assistant with your voice, having it push information to a live Canvas on your screen, or snapping a photo from your phone and asking 'What is this?'
>
> That's what we're exploring now - the Clawdbot apps for Mac, iOS, and Android."

---

## 🖥️ macOS App: Clawdbot.app

### [VISUAL: Applications folder showing Clawdbot.app]

**VOICEOVER:**

> "Let's start with the Mac app. This lives in your menu bar and gives you quick access to all of Clawdbot's features."

### Installation

**VOICEOVER:**

> "If you built from source, you can package the app with a script. Otherwise, grab the release from GitHub."

### Technical Steps:

```bash
# If building from source
cd /path/to/clawdbot
./scripts/package-mac-app.sh

# The app will be in the build folder
# Drag to Applications
```

### [VISUAL: Menu bar showing Clawdbot icon]

**VOICEOVER:**

> "Once installed, you'll see a little icon in your menu bar - that's Clawdbot. Click it and you get a dropdown with all your options."

### Features:

```text
Menu Bar Features:
- Gateway status and control
- Start/Stop Gateway
- Voice Wake toggle
- Talk Mode overlay
- WebChat quick access
- Debug tools
```

**VOICEOVER:**

> "From here you can see if the Gateway is running, start or stop it, enable voice features, and access debug tools. It's like having a dashboard always available."

---

## 🎙️ Voice Wake

### [VISUAL: Microphone icon activating]

**VOICEOVER:**

> "This is one of my favorite features. Voice Wake lets you talk to Clawdbot just by saying a wake word - kind of like 'Hey Siri' or 'Alexa', but for your personal AI."

### Setup:

```bash
# Voice Wake requires ElevenLabs for high-quality voice
# Set your API key
clawdbot config set tts.elevenlabs.apiKey "your-elevenlabs-key"

# Enable Voice Wake
clawdbot config set voicewake.enabled true
```

### [VISUAL: macOS permissions dialog for microphone]

**VOICEOVER:**

> "You'll need to grant microphone permissions. macOS will prompt you the first time. Accept it, and Voice Wake starts listening."

### Demo:

### [VISUAL: Person saying wake word, response appears]

**VOICEOVER:**

> "Now just say... 'Hey Clawd' - and watch what happens."

### [VISUAL: Voice visualization, then spoken response]

**VOICEOVER:**

> "It heard us! Ask a question, and it responds with voice. You're basically having a conversation with your AI. No keyboard needed."

---

## 💬 Talk Mode

### [VISUAL: Talk Mode overlay appearing]

**VOICEOVER:**

> "Talk Mode is the visual companion to Voice Wake. When activated, an overlay appears on your screen showing the conversation in real-time."

### Technical Steps:

```bash
# Enable Talk Mode
clawdbot config set talk.enabled true

# Configure appearance
clawdbot config set talk.position "bottom-right"
```

### [VISUAL: Talk Mode showing transcription and response]

**VOICEOVER:**

> "You see what you said transcribed, and the AI's response streams in. It's a beautiful, focused conversation interface that appears when you need it and disappears when you don't."

---

## 📱 iOS Node

### [VISUAL: iPhone with Clawdbot app]

**VOICEOVER:**

> "Now let's look at the iPhone app. It's called a 'node' because it connects to your main Gateway and adds mobile capabilities."

### Features:

```text
iOS Features:
- Canvas surface (visual workspace)
- Voice Wake on mobile
- Camera snap and video
- Screen recording
- Location access
- Bonjour pairing (auto-discovery)
```

**VOICEOVER:**

> "What can you do with the iOS node? Take a photo and ask Clawd to identify it. Share your location so it can recommend nearby restaurants. Push content to the Canvas and view it on your phone. Even record your screen for troubleshooting help."

### Setup:

### [VISUAL: iPhone and Mac on same network]

**VOICEOVER:**

> "First, make sure your iPhone and Gateway are on the same network. The iOS app uses Bonjour to automatically discover your Gateway."

### [VISUAL: Pairing screen on iOS app]

**VOICEOVER:**

> "Open the app, and it should find your Gateway. Tap to pair, and you're connected!"

### Demo: Camera Snap

### [VISUAL: Taking a photo of an object, sending to Clawd]

```bash
# On the Gateway, you can invoke iOS nodes remotely
clawdbot nodes list
# Should show your iOS device

clawdbot nodes invoke ios-device camera.snap
# Takes a photo and sends it to the current session
```

**VOICEOVER:**

> "Let's try the camera. Ask Clawd: 'What's in this photo?' and snap a picture. The image is sent to your AI, and you get an analysis back. Super useful for identifying plants, products, or getting help with anything visual."

---

## 🤖 Android Node

### [VISUAL: Android phone with Clawdbot app]

**VOICEOVER:**

> "Android gets the same love! The Android node has similar capabilities."

### Features:

```text
Android Features:
- Canvas surface
- Talk Mode
- Camera (photos and video)
- Screen capture
- Optional SMS access
```

**VOICEOVER:**

> "Same story - Canvas, camera, screen capture. Plus, on Android you can optionally enable SMS access, which opens up some interesting automation possibilities."

### Setup:

```bash
# Android also uses Bonjour for discovery
# Make sure your device and Gateway are on the same WiFi

# View connected nodes
clawdbot nodes list
```

### [VISUAL: Android pairing with Gateway]

**VOICEOVER:**

> "Pair the same way - open the app, it finds your Gateway, tap to connect. Now your Android phone is a Clawdbot node!"

---

## 🖼️ The Canvas

### [VISUAL: Canvas showing pushed content]

**VOICEOVER:**

> "Let's talk about the Canvas. This is a visual workspace that Clawdbot can push content to. Think of it like a shared screen where your AI can show you things."

### What Can Appear on Canvas:

```text
- Charts and graphs
- Code snippets
- Images and diagrams
- Interactive forms
- Custom HTML/UI
```

### Technical Steps:

```bash
# Ask Clawd to push something to Canvas
clawdbot agent --message "Show me a chart of global temperature over the last 100 years"

# Canvas updates in real-time on any connected device
```

### [VISUAL: Canvas updating with chart]

**VOICEOVER:**

> "The Canvas uses something called A2UI - Agent to UI. Your AI assistant can actually generate and update visual content. Ask for a chart, and it appears. Request a diagram, boom - it's on your screen."

### Demo:

### [VISUAL: Asking for content, watching Canvas update]

**VOICEOVER:**

> "Let's try it. 'Clawd, show me a timeline of major AI milestones since 2010.' Watch the Canvas... there it is! An interactive timeline that the AI just generated for us."

---

## 🔧 Node Commands

### [VISUAL: Terminal showing node commands]

**VOICEOVER:**

> "You can control your mobile nodes remotely from the command line. Here are some useful commands."

### Technical Steps:

```bash
# List all connected nodes
clawdbot nodes list

# Describe a node's capabilities
clawdbot nodes describe ios-phone

# Invoke a capability
clawdbot nodes invoke ios-phone camera.snap
clawdbot nodes invoke android-phone screen.record --duration 30s
clawdbot nodes invoke ios-phone location.get

# Send a notification to mobile
clawdbot nodes invoke android-phone notify --message "Time for your meeting!"
```

**VOICEOVER:**

> "`clawdbot nodes list` shows what's connected. `describe` shows what a node can do. And `invoke` triggers an action. Get location, snap a photo, record screen, send notifications - all remotely."

---

## ✅ Section Recap

### [VISUAL: All three devices side by side]

**VOICEOVER:**

> "Amazing! Let's recap:
>
> ✓ macOS menu bar app for quick control
> ✓ Voice Wake - talk to your AI naturally
> ✓ Talk Mode - visual conversation overlay
> ✓ iOS node with camera, location, Canvas
> ✓ Android node with same capabilities
> ✓ Canvas for visual AI content
> ✓ Node commands for remote control
>
> You've now got Clawdbot on all your devices, working together as one system. The Gateway is the brain, your phone is the eyes and ears, and the Canvas is the shared workspace.
>
> Next up, let's look at what Clawdbot can actually DO with all this power - browser control, running commands, and more!"

---

## 📝 Production Notes

### Demos to Record
- Menu bar dropdown and options
- Voice Wake actual demo (show microphone activation)
- Talk Mode overlay appearing
- iOS camera snap → AI response
- Canvas updating with generated content

### Permissions to Show
- macOS microphone permission
- iOS camera and location permissions
- Android notification permission

### Hardware Needed
- Mac with menu bar visible
- iPhone for iOS demo
- Android phone (or emulator)

### Key Timestamps
- 0:00 - Platform overview
- 1:00 - macOS app installation
- 2:30 - Voice Wake setup and demo
- 4:30 - Talk Mode
- 5:30 - iOS node features
- 7:00 - Android node
- 8:00 - Canvas demo
- 9:30 - Node commands
