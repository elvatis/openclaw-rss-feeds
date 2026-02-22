# @elvatis_com/openclaw-rss-feeds

OpenClaw plugin for automated RSS/Atom feed digests with CVE enrichment and Ghost CMS integration.

## Features

- **Feed Management** - Add, remove, and list RSS/Atom feeds via agent tools
- **Scheduled Digests** - Cron-based fetching with configurable intervals
- **CVE Enrichment** - Automatically cross-references security feeds with NVD/CISA KEV data
- **Ghost CMS Drafts** - Generates formatted blog post drafts in Ghost
- **Smart Filtering** - Keyword and severity-based filtering to reduce noise

## Installation

```bash
npm install @elvatis_com/openclaw-rss-feeds
```

## Configuration

Add to your `openclaw.json`:

```json
{
  "plugins": {
    "openclaw-rss-feeds": {
      "feeds": [
        {
          "url": "https://www.cert-bund.de/rss20neu",
          "label": "CERT-Bund",
          "category": "security"
        }
      ],
      "schedule": "0 9 1 * *",
      "ghost": {
        "url": "https://your-blog.com",
        "adminApiKey": "your-key"
      },
      "enrichCve": true,
      "keywords": ["critical", "zero-day", "ransomware"]
    }
  }
}
```

## Agent Tools

| Tool | Description |
|---|---|
| `rss_list_feeds` | List all configured feeds |
| `rss_add_feed` | Add a new RSS/Atom feed |
| `rss_remove_feed` | Remove a feed by label or URL |
| `rss_fetch_now` | Trigger an immediate fetch for one or all feeds |
| `rss_digest` | Generate a digest from recent entries |
| `rss_search` | Search feed entries by keyword |

## How It Works

1. Feeds are fetched on schedule (or on-demand via agent tool)
2. New entries are filtered by keywords and deduplication
3. Security feeds get CVE enrichment (CVSS scores, CISA KEV status)
4. A formatted digest is generated (Markdown or Ghost draft)
5. Agent is notified with a summary

## Development

```bash
git clone https://github.com/homeofe/openclaw-rss-feeds
cd openclaw-rss-feeds
npm install
npm run build
npm run test
```

## License

MIT
