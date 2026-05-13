# ☀️ Morning Brain — Personalized AI Morning Digest Agent

An n8n automation agent that delivers a personalized AI-generated morning briefing to your inbox every day at 8am — completely free.

## What it does

Every morning at 8:00 AM, this agent:
1. Fetches real-time weather for your city (Chennai) from Open-Meteo
2. Pulls the top 3 tech headlines from TechCrunch RSS feed
3. Sends all data to Llama 3.3 (via Groq API) to compose a warm, personalized digest
4. Delivers the final briefing to your Gmail inbox automatically

## Sample Output

```
☀️ Good morning, Pushpanjali!

Today in Chennai: 32°C, clear skies 🌈

Here are your top 3 tech headlines:
1️⃣ AI agents are revolutionizing software development 🤖
2️⃣ Open source models are closing in on GPT-4 📈
3️⃣ India's tech startup ecosystem sees record funding 🚀

As you start your day, remember: "Builders don't wait for the
perfect moment, they make the moment perfect." 💡

---
This email was sent automatically with n8n
```

## Tech Stack

| Tool | Purpose | Cost |
|------|---------|------|
| n8n (self-hosted, Docker) | Workflow automation | Free |
| Open-Meteo API | Real-time weather | Free, no key needed |
| TechCrunch RSS | Tech headlines | Free |
| Groq API (Llama 3.3-70b) | AI digest composition | Free tier |
| Gmail OAuth2 | Email delivery | Free |

## Workflow Architecture

```
Schedule Trigger (8am daily)
        |
        ├──→ Weather (Open-Meteo HTTP Request)
        |               |
        └──→ RSS Read   |
            (TechCrunch)|
                        ↓
                  Compose Digest
                  (Groq / Llama 3.3)
                        |
                        ↓
                     Limit (1)
                        |
                        ↓
                  Gmail (Send)
```

## Setup Instructions

### Prerequisites
- n8n running locally via Docker
- A Google account (for Gmail)
- A free Groq API account

### Step 1 — Clone this repo
```bash
git clone https://github.com/yourusername/morning-brain-agent
```

### Step 2 — Import the workflow
1. Open n8n (localhost:5678)
2. Click **New Workflow** → **...** menu → **Import from file**
3. Select `morning_brain.json`

### Step 3 — Set up credentials

**Groq API:**
1. Sign up at console.groq.com (free)
2. Create an API key
3. In the Compose Digest node, replace `YOUR_GROQ_KEY` in the URL

**Gmail OAuth2:**
1. Go to console.cloud.google.com
2. Create a new project → enable Gmail API
3. Create OAuth 2.0 credentials (Web application)
4. Add redirect URI: `http://localhost:5678/rest/oauth2-credential/callback`
5. Paste Client ID and Secret into n8n Gmail credential

### Step 4 — Configure your location
In the Weather node URL, update the coordinates:
```
latitude=YOUR_LAT&longitude=YOUR_LON
```
Find your coordinates at latlong.net

### Step 5 — Publish
Click **Publish** in n8n — the agent will run every day at 8am automatically.

## Customization

- **Change city:** Update latitude/longitude in the Weather node URL
- **Change news source:** Replace the RSS feed URL with any RSS feed
- **Change time:** Edit the Schedule Trigger from 8am to your preferred time
- **Change name:** Update "Pushpanjali" in the Groq prompt to your name

## Part of a 3-Agent Portfolio

This is Agent 2 in a series of n8n agents built for the CentrAlign AI internship application:

- **Agent 1** — Student Profile Analyzer (Form → Gemini → Notion)
- **Agent 2** — Morning Brain Daily Digest (Weather + News + Groq → Gmail) ← you are here
- **Agent 3** — Coming soon

## Author

**Chennuru Pushpanjali**
First-year B.Tech AI Student, Sai University Chennai (2025–2029)
Building real AI agents from day one.

---
Built with n8n • Powered by Groq • Delivered by Gmail
