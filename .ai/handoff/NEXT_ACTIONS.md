# openclaw-rss-feeds — Next Actions

> Prioritized by strategic importance. Top = do first.

## P1 — Research (Sonar)
- [ ] Research: rss-parser npm package (most popular RSS parser for Node.js)
- [ ] Research: NVD CVE API v2.0 usage from TypeScript
- [ ] Research: Ghost Admin API JWT from Node.js (existing pattern in bash script)
- [ ] Research: openclaw plugin scheduling API (cron-like, from plugin manifest)

## P2 — Architecture (Opus)
- [ ] Define config schema: feeds[], schedule, cvssThreshold, output (ghost + notify)
- [ ] Define internal pipeline: fetch → filter → enrich → format → output
- [ ] Decide: Python subprocess vs. pure TypeScript (prefer TS for portability)

## P3 — Implementation (Sonnet)
- [ ] Create package.json + tsconfig.json + openclaw.plugin.json
- [ ] Port NVD CVE fetch to TypeScript
- [ ] Port RSS parser to TypeScript (rss-parser npm)
- [ ] Port HTML formatter to TypeScript
- [ ] Implement Ghost draft creation (TS)
- [ ] Implement scheduled execution via plugin API
- [ ] Write tests (feed parsing, CVE enrichment, HTML output)

## P4 — Docs + Publish
- [ ] Update README.md with final config examples
- [ ] npm publish @elvatis/openclaw-rss-feeds
- [ ] Blog article: "Building a security digest plugin for OpenClaw"
- [ ] Submit to OpenClaw community plugins page (PR)
