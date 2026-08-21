---
name: AniZen MPV config precedence
description: Why custom MPV configuration must be loaded from the player initialization callback.
---

AniZen's MPV initialization is asynchronous. Commands sent immediately after `initialize()` can execute before AniZen's `initOptions()` defaults, allowing those defaults to overwrite user configuration. User `mpv.conf` and `input.conf` must be loaded from `postInitOptions()`, after all built-in options have been applied.

**Why:** A custom configuration can appear completely ignored when the load command races the initialization callback; this affects subtitles, filters, color controls, and playback settings together.

**How to apply:** Keep the config paths on the MPV view before initialization, then issue `load-config` and `load-input-conf` in `postInitOptions()`, skipping empty files.