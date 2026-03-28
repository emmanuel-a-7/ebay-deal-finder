# eBay Deal Finder

A stealth eBay scraper that monitors search results and fires Discord webhook alerts when new listings appear under your price ceiling.

Built with Puppeteer + stealth plugin to survive eBay's bot detection. Includes a real-time web dashboard via Express + WebSockets.

## Features

- Puppeteer stealth scraping (bypasses eBay bot detection)
- Configurable search query, price ceiling, required/excluded keywords
- Discord webhook alerts with rich embeds
- Deduplication — only alerts on listings you haven't seen before
- Real-time dashboard at `localhost:PORT`
- Dry run mode for testing without sending alerts

## Setup

```bash
cd ebay-public-scraper
npm install

cp .env.example .env
# Edit .env — add your Discord webhook URL at minimum
```

**Get a Discord webhook URL:** Server Settings → Integrations → Webhooks → New Webhook → Copy URL

## Running

```bash
# Dashboard mode (recommended) — web UI + scraper
npm start

# Scraper only (no dashboard)
npm run scraper

# Dry run — test without sending Discord alerts
npm run dry-run
```

Dashboard available at `http://localhost:3060` (or whatever PORT you set).

## Configuration

Set via `.env` or `config.json` (see `.env.example` for all options):

| Variable | Default | Description |
|---|---|---|
| `DISCORD_WEBHOOK_URL` | required | Discord webhook for alerts |
| `SEARCH_QUERY` | `nvidia rtx 3060 12gb` | eBay search query |
| `MAX_PRICE` | `250` | Max total price (item + shipping) |
| `SCAN_INTERVAL_MINUTES` | `10` | How often to scan |
| `PROJECT_NAME` | `Deal Hunter` | Label shown in Discord embeds |
| `PORT` | `3060` | Dashboard port |
| `DRY_RUN` | `false` | Log alerts to console instead of sending |

`config.json` takes precedence over `.env` and can also be updated live via the dashboard API.

## Project Structure

```
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
```

## Notes

- eBay scraping works until it doesn't — eBay changes their DOM periodically. If results drop to zero, check the selectors in `src/scraper.js`.
- The stealth plugin helps a lot but isn't magic. Rate limiting your scan interval (10+ minutes) reduces ban risk.
- `data/seen_listings.json` persists dedup state across restarts. Delete it to reset.
