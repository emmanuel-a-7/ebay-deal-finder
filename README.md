eBay Deal Finder

-Note. This code was created in part with Gemni AI. This project is currently depreciated as version 1.1 has replaced it with quality-of-life changes.

Welcome to my scraper! Configure your webhook in the config.json. In the settings in the web interface in index.html, everything else can be easily configured without coding, and I'm pretty sure you can add the webhook. Good luck!

A stealth eBay scraper that monitors search results and gives alerts when listing become avaliable that meet your criterion.

Built with Puppeteer + stealth plugin to survive eBay's bot detection. Includes a real-time web dashboard via Express + WebSockets. 

Features:
Puppeteer Stealth Scraping: Bypasses eBay bot detection using advanced evasion techniques.
Configurable Alerts: Set search queries, price ceilings, and keyword filters.
Discord Webhook Integration: Notifies listing numbers directly to your server.
Deduplication: Uses a local tracker to ensure you only get alerted once per unique listing.
Real-time Dashboard: Monitor the scraper and update settings via a HTML and JavaScript web UI.
Dry Run Mode: Test your configuration without sending actual Discord messages.

Setup
cd ebay-public-scraper
npm install

cp .env.example .env
# Edit .env — add your Discord webhook URL to send notifications

Get a Discord webhook URL: Server Settings – Integrations - Webhooks - New Webhook - Copy URL

Running
# Dashboard mode (recommended) — web UI + scraper and Notifications
npm start

# Scraper only (no dashboard)
npm run scraper

# Dry run — test without sending Discord alerts
npm run dry-run


The dashboard is available at http://localhost:3060 using port 3060, and I believe it should work with live server.

Configuration:
Settings can be managed via .env or config.json. Note: config.json takes precedence over .env and can be updated live via the dashboard.
1. Variable
2. Default
3. Description
4. DISCORD_WEBHOOK_URL
5. SEARCH_QUERY (ex. nvidia rtx 3060 12gb)
6. MAX_PRICE
7. Max total price (item + shipping)
8. SCAN_INTERVAL_MINUTES


Notes

Scraping Stability: eBay changes their anti-bot periodically. If results drop to zero, check the CSS selectors in src/scraper.js.
Bot Detection: The stealth plugin helps, but isn't magic. Keeping the scan interval at 10+ minutes allows for successful scraping.
Persistence: data/seen_listings.json saves your history across restarts. Delete this file if you want to reset the seen listings.
