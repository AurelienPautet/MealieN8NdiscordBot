# To Mealie Recipe Extractor

## Overview

This n8n automation allows you to drop a link into a Discord channel and instantly receive a formatted `schema.org/Recipe` JSON object. It is designed to work with:

- **TikTok** (Video transcripts + Captions)  
- **Instagram** (Reels/Post captions + Transcripts)  
- **YouTube** (Video Transcripts)  
- **Standard Websites** (HTML Parsing)

The automation uses AI (Google Gemini) to intelligently parse unstructured data (like video transcripts) and format it into a clean, importable JSON recipe that **Mealie** can easily import.

It also automatically translates the recipe into **French** (configurable).

---

## Cost & Usage (Free Tiers)

This workflow is designed to be extremely cost-effective. You can likely run this automation **over 1,000 times a month for free**:

- **Google Gemini**: The model used (`Gemini 2.5 Flash`) has a generous free tier that covers massive request volumes.
- **Apify**: Offers a monthly free credit (usually $5), which renews every month. Since this workflow primarily scrapes text and transcripts (lightweight data), the cost per run is negligible.

---

## Key Features

### Multi-Platform Support
Automatically detects the link type and routes it to the appropriate scraper.

### AI-Powered Parsing
Uses **Gemini 2.5 Flash** to extract ingredients and instructions from messy, unstructured video transcripts.

### Smart Output
- If the recipe is short (< 4000 characters):  
  → Sent as a Discord code block.
- If the recipe is long:  
  → Uploaded as a `.json` file to avoid Discord character limits.

### Error Handling
Notifies you in Discord if extraction fails (e.g., invalid link, missing content).

---

## Prerequisites

To use this workflow, you need accounts for the following services:

| Service        | Required? | Details |
|---------------|----------|--------|
| **n8n**       |          | Self-hosted or Cloud instance |
| **Discord Bot** |          | Bot Token & Channel ID (where links will be posted) |
| **Apify Account** |        | API Key + access to required actors |
| **Google Gemini API** |    | For AI parsing and translation |

---

## Setup Instructions

### 1. Import Workflow
- Download the `To Mealie Recipe.json` file.
- Import it into your n8n instance.

---

### 2. Configure Credentials

Set up credentials in n8n for:

- **Discord Bot Trigger API** – For listening to messages  
- **Discord Bot API** – For sending replies  
- **Apify API** – To access scraping actors  
- **Google Gemini (PaLM) API** – For AI parsing & translation

---

### 3. Configure Nodes

#### A. Discord Trigger

- **Listen to**: Update the Channel IDs to match your target Discord channel.
- **Bot Permissions**: Ensure your bot has:
  - `View Channels`
  - `Send Messages`
  in the channel.

#### B. Apify Actors (Crucial)

This workflow relies on specific Apify Actors. You must have access to them (or replace with your preferred actors):

| Platform       | Apify Actor(s) |
|---------------|----------------|
| **TikTok**    | `clockworks/tiktok-scraper` + `scrape-creators/best-tiktok-transcripts-scraper` |
| **Instagram** | `apify/instagram-post-scraper` + `tictechid/anoxvanzi-Transcriber` |
| **YouTube**   | `karamelo/youtube-transcripts` |

> Ensure all actors are active and have valid API keys.

#### C. Language Settings

- The node named **LANGUAGE** (on the left) is currently hardcoded to **French**.
- To change language:
  1. Open the **LANGUAGE** node.
  2. Change `"french"` to `"english"` or your preferred language (e.g., `"spanish"`, `"german"`).

> Note: Language change affects both output and translation.

---

## How to Use

1. Go to the configured Discord channel.
2. Paste a link (e.g., a TikTok recipe video).
3. Wait: The bot will reply with:  
   *"Le traitement de la recette a commencé "*
4. Result:
   - After **10–30 seconds**, you’ll receive:
     - A formatted JSON code block (if short), or
     - A `.json` file attachment (if long).
5. Copy the JSON and import it directly into **Mealie**.

---

## Troubleshooting

| Issue                     | Solution |
|--------------------------|---------|
| "Error processing recipe" | Check n8n logs. Common causes: <br> - Apify actor failed (e.g., private video) <br> - Gemini refused to generate content (e.g., sensitive content) |
| Wrong Language        | Ensure you updated the **LANGUAGE** node. |
| Discord Trigger not firing | Verify: <br> - Bot is in the channel <br> - Has `View Channels` and `Send Messages` permissions <br> - Channel is public or the bot has access to private channels |

---

## Pro Tips

- Test with a known working recipe first (e.g., a TikTok with clear captions).
- For better accuracy, ensure videos have captions enabled.
- Use **short, clear recipe links** to reduce parsing time.
- Monitor n8n logs for detailed error messages.

---

> This tool turns chaotic video content into structured, importable recipes — saving you time and effort.  
> Whether you're building a recipe database or curating content, **To Mealie Recipe Extractor** is your go-to automation.
