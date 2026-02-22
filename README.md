# @elvatis/openclaw-rss-feeds

OpenClaw plugin that turns any RSS/Atom feed into an automated monthly (or custom-schedule) digest - with CVE enrichment, Ghost CMS draft creation, and WhatsApp/channel notification.

## What it does

Fetches and filters RSS/Atom feeds on a configurable schedule, enriches security-related content with NVD CVE data, generates a formatted Ghost CMS draft, and notifies configured channels (WhatsApp, Telegram, etc.).

Originally built for Fortinet security advisories - now fully generic.

## Use cases

- **Security vendor feeds** (Fortinet, Cisco, Palo Alto, etc.) with CVE enrichment
- **Product release feeds** (firmware updates, changelogs)
- **News digest** (any RSS feed filtered by keyword)
- **Blog content pipeline** (auto-draft from curated feeds)

## Architecture

```
Schedule (cron)
  └── @elvatis/openclaw-rss-feeds plugin
        ├── Fetch RSS/Atom feeds
        ├── Filter by keywords + date range
        ├── Enrich with NVD CVE data (optional)
        ├── Format as HTML digest
        ├── Create Ghost CMS draft (optional)
        └── Notify configured channels
```

## Installation

```bash
openclaw plugins install @elvatis/openclaw-rss-feeds
```

## Configuration

```json
{
  "plugins": {
    "entries": {
      "openclaw-rss-feeds": {
        "config": {
          "feeds": [
            {
              "id": "fortinet",
              "name": "Fortinet Firmware",
              "url": "https://support.fortinet.com/rss/firmware.xml",
              "keywords": ["fortinet"],
              "cvssThreshold": 6.5,
              "enrichCve": true
            }
          ],
          "schedule": "0 9 1 * *",
          "output": {
            "ghostDraft": true,
            "notify": ["whatsapp:+49xxxxxxxxxx"]
          }
        }
      }
    }
  }
}
```

## Status

Work in progress. See `.ai/handoff/STATUS.md` for current build state.
