---
name: AniZen MPV config precedence
description: Why custom MPV configuration must be loaded from the player initialization callback.
---

AniZen's MPV config editor must persist the preference value regardless of external-storage permission. MPV's `BaseMPVView.initialize()` sets `config-dir` and loads the user's `mpv.conf`/`input.conf` during native initialization; no second runtime reload is needed.

**Why:** The editor previously only called `pref.set()` when all-files external storage permission was granted. On private-storage devices, the next player start rewrote the edited file with the stale preference value, making every custom option appear ignored.

**How to apply:** Call `pref.set(newValue)` outside the permission-specific external-file branch. Continue writing the external copy when available, but rely on the configured MPV directory for loading.