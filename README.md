eBay Deal Finder

Welcome to my scraper! Configure your webhook in the config.json. In the settings in the web interface in index.html, everything else can be easily configured without coding, and I'm pretty sure you can add the webhook. Good luck!

A stealth eBay scraper that monitors search results and fires Discord webhook alerts when new listings appear under your price ceiling.

Built with Puppeteer + stealth plugin to survive eBay's bot detection. Includes a real-time web dashboard via Express + WebSockets.

Features

Puppeteer Stealth Scraping: Bypasses eBay bot detection using advanced evasion techniques.

Configurable Alerts: Set search queries, price ceilings, and keyword filters.

Discord Webhook Integration: Fires rich embeds directly to your server.

Deduplication: Uses a local tracker to ensure you only get alerted once per unique listing.

Real-time Dashboard: Monitor the scraper and update settings via a web UI.

Dry Run Mode: Test your configuration without sending actual Discord messages.

Setup

cd ebay-public-scraper
npm install

cp .env.example .env
# Edit .env — add your Discord webhook URL at minimum


Get a Discord webhook URL: Server Settings → Integrations → Webhooks → New Webhook → Copy URL

Running

# Dashboard mode (recommended) — web UI + scraper
npm start

# Scraper only (no dashboard)
npm run scraper

# Dry run — test without sending Discord alerts
npm run dry-run


The dashboard is available at http://localhost:3060 (or your custom PORT).

Configuration

Settings can be managed via .env or config.json. Note: config.json takes precedence over .env and can be updated live via the dashboard.

Variable

Default

Description

DISCORD_WEBHOOK_URL

required

Discord webhook for alerts

SEARCH_QUERY

nvidia rtx 3060 12gb

eBay search query

MAX_PRICE

250

Max total price (item + shipping)

SCAN_INTERVAL_MINUTES

10

How often to scan

PROJECT_NAME

Deal Hunter

Label shown in Discord embeds

PORT

3060

Dashboard port

DRY_RUN

false

Log alerts to console instead of sending

Project Structure

ebay-public-scraper/
├── index.js          # Scraper-only entry point
├── server.js         # Dashboard + scraper entry point
├── src/
│   ├── scraper.js    # Puppeteer stealth browser engine
│   ├── parser.js     # URL builder + listing filter
│   ├── tracker.js    # Deduplication (seen listings)
│   └── discord.js    # Webhook notification sender
├── public/
│   └── index.html    # Real-time dashboard UI
├── .env.example      # Environment variable template
└── data/             # Runtime state (gitignored)


Notes

Scraping Stability: eBay changes their DOM periodically. If results drop to zero, check the CSS selectors in src/scraper.js.

Bot Detection: The stealth plugin helps, but isn't magic. Keeping the scan interval at 10+ minutes is recommended.

Persistence: data/seen_listings.json saves your history across restarts. Delete this file if you want to reset the "seen" list.