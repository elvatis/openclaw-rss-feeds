# openclaw-rss-feeds — Status

> Last updated: 2026-02-22 (initial setup)
> Phase: P0 — Project initialized, core logic already exists

## Project Overview

**Package:** `@elvatis/openclaw-rss-feeds`
**Repo:** https://github.com/homeofe/openclaw-rss-feeds
**Purpose:** Generic RSS/Atom feed digest plugin — fetch, filter, enrich (CVE), draft, notify.

## Build Health

| Component         | Status       | Notes                                             |
| ----------------- | ------------ | ------------------------------------------------- |
| Repo / Structure  | (Verified)   | Initialized 2026-02-22                            |
| Core logic        | (Verified)   | Exists in cron/scripts/fortinet-monthly-digest.sh |
| Plugin manifest   | (Unknown)    | Not yet created                                   |
| TypeScript port   | (Unknown)    | Python logic needs porting to TS                  |
| Config schema     | (Unknown)    | Not yet defined                                   |
| Tests             | (Unknown)    | Not yet created                                   |
| npm publish       | (Unknown)    | Not yet published                                 |

## Key Decision

Core logic already implemented as a bash+Python script (`fortinet-monthly-digest.sh`).
Strategy: port the Python logic to TypeScript, make feed URL / keywords / CVSSthreshold / output configurable.

## Existing Logic (to port)

1. NVD CVE fetch (keyword + date range + CVSS threshold)
2. RSS/Atom feed parse (date filtering, dedup)
3. HTML digest formatting
4. Ghost CMS draft creation (JWT auth)
5. WhatsApp notification via openclaw message CLI
