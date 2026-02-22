# P4 Review — Opus (Architecture)

**Reviewer:** Opus (Architect) | **Date:** 2026-02-22 | **Commit reviewed:** 42b2f04

## ADR Compliance

- **D1 Pure TS:** ✅ Correct. No Python, no subprocess for logic. All deps are npm packages (rss-parser, jsonwebtoken, node-cron).
- **D2 Scheduling:** ✅ Correct. `node-cron` in `registerService` with `start()`/`stop()`. `cron.validate()` is checked. Manual trigger via `registerTool('rss_run_digest')`. Both optional.
- **D3 Notification:** ✅ Correct. CLI subprocess via `openclaw message send`. Format `channel:target` as per ADR.
- **D4 CVE optional:** ✅ Correct. Per-feed via `enrichCve: true` + `keywords[]`. `nvdApiKey` optional. Works without key with 6s rate-limit delay.
- **D5 Sequential:** ✅ Correct. Feeds are processed sequentially in `for...of`. 6s sleep between NVD requests.
- **D6 Config Schema:** ✅ Correct. `PluginConfig` interface matches the ADR schema exactly: `feeds[]`, `schedule?`, `lookbackDays?`, `ghost?{url, adminKey}`, `notify?[]`, `nvdApiKey?`.

## Plugin API Compliance

- `registerService({id, start, stop})` — ✅ matches OpenClaw Plugin Spec
- `registerTool({name, description, parameters, execute})` — ✅ matches Plugin Spec
- Default export `(api) => void` — ✅ correct
- `api.logger.info/warn/error` — ✅ used consistently (except cveFetcher, see Minor)

## Issues Found

### Critical (fixed in this commit)

1. **Shell injection in notifier.ts** — `execSync()` with string concatenation and `shellEscape()` is vulnerable. If a `notify` target or the message payload contains special characters, an attack vector could exist despite escaping (single-quote escaping has edge cases in different shells).
   - **Fix:** Replaced with `execFileSync()` (no shell subprocess, arguments are passed directly as an array). Shell injection is structurally impossible with this approach.

2. **Ghost publish without outer try/catch** — `publishDraft()` has an internal try/catch, but if `makeGhostToken()` throws before the HTTP call (e.g. invalid `adminKey` format), the error was caught internally. Nevertheless: defensive wrapping added in the caller (`runDigest`), since Ghost failures should be non-fatal per ADR.
   - **Fix:** Outer try/catch around the entire Ghost block in `runDigest()`.

### Minor (nice-to-have, not a blocker)

1. **cveFetcher uses `console.warn` instead of `api.logger`** — Inconsistent with ADR point 5 (structured logging via `api.logger`). cveFetcher has no access to `api.logger` because it only receives primitive params. Recommendation: pass logger as an optional parameter or refactor in v0.2.

2. **`getFirmwareType()` heuristic is Fortinet-specific** — The Major/Feature/Patch detection is based on the last version part (`0`=Major, `2`/`4`=Feature, rest=Patch). This works for Fortinet but not generically. Acceptable for v0.1 (Fortinet-only).

3. **No `openclaw.plugin.json` manifest** — The Plugin Spec expects a manifest with `configSchema` and `uiHints`. `package.json` has `openclaw.extensions`, but the JSON schema for config validation is missing. Should be added before npm publish.

4. **`dryRun` mode creates a new `PluginApi` object with spread** — Works, but `api` could have non-enumerable properties. A `skipPublish` flag in the `runDigest` parameter would be cleaner.

5. **No timeout/retry on RSS fetch** — `rss-parser` has `timeout: 20000`, but no retry. A single retry could help with flaky feeds.

## Recommendation

**NEEDS_FIXES** → Critical fixes (shell injection, Ghost try/catch) were applied directly in this commit.

After fixes: **APPROVED for v0.1.0-beta**. Track minor points as issues.
