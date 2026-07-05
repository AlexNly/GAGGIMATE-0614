# GaggiMate data backup — GAGGIMATE#0614

Shot history, brew profiles, and settings exported from my GaggiMate
(Gaggia espresso machine controller, https://gaggimate.eu), serial GAGGIMATE#0614.

## Contents

- `shots/` — 57 shots as binary `.slog` files (format: 512-byte header,
  magic `SHOT`, 26-byte samples at 250 ms — see `src/display/models/shot_log_format.h`
  in the GaggiMate repo) plus matching `.json` shot notes. `index.json` is the
  shot list as returned by the `req:history:list` WebSocket API.
- `profiles/` — all brew profiles as JSON (GaggiMate profile schema),
  including the Automatic Pro v3 family and Adaptive v2.
- `settings.json` — machine settings from `GET /api/settings`.
  WiFi/AP/Home-Assistant credentials are redacted.

## Restore

- **Shots**: copy `shots/*.slog` and `shots/*.json` into `/h/` on the SD card
  (or upload when booted from internal flash), then run
  `{"tp":"req:history:rebuild"}` against `ws://gaggimate.local/ws`.
- **Profiles**: send each JSON via `{"tp":"req:profiles:save","profile":{...}}`.
- **Settings**: POST the desired keys to `/api/settings` (re-enter redacted
  credentials manually).
