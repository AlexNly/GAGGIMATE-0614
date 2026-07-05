# GAGGIMATE#0614 — shot journal & machine backup

**Browse the shot journal: https://alexnly.github.io/GAGGIMATE-0614/**

This repo is the living archive of my GaggiMate espresso machine
(https://gaggimate.eu, serial GAGGIMATE#0614). After every shot,
[gaggibot](https://github.com/AlexNly/gaggibot) downloads the shot log and my
notes from the machine, refreshes the brew profiles and settings, regenerates
the journal site and pushes a commit — so shots, profiles and settings are
version-controlled and survive firmware updates/downgrades, SD card failure,
or the machine itself.

## Contents

- `shots/` — every shot as a binary `.slog` file (512-byte header, magic
  `SHOT`, 26-byte samples at 250 ms; format spec in
  [`gaggibot/slog.py`](https://github.com/AlexNly/gaggibot/blob/main/src/gaggibot/slog.py))
  plus matching `.json` shot notes.
- `profiles/` — all brew profiles as JSON, including the
  [Automatic Pro](https://modsmthng.github.io/Automatic-Pro/v3/) v3 family.
- `settings.json` — machine settings from `GET /api/settings`
  (WiFi/AP/Home-Assistant credentials redacted).
- `docs/` — the generated static journal site (GitHub Pages).

## Restore onto a machine

- **Shots**: copy `shots/*.slog` and `shots/*.json` into `/h/` on the SD card
  (or upload when booted from internal flash), then run
  `{"tp":"req:history:rebuild"}` against `ws://gaggimate.local/ws`.
- **Profiles**: send each JSON via `{"tp":"req:profiles:save","profile":{...}}`.
- **Settings**: POST the desired keys to `/api/settings` (re-enter redacted
  credentials manually).

Want this for your own machine? See [gaggibot](https://github.com/AlexNly/gaggibot).
