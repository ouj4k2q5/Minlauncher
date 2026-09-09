
# MinLauncher

A personal fork of [Olauncher](https://github.com/tanujnotes/Olauncher) — a minimal,
ad-free Android home screen launcher.

## Fork notice

This repository is a **modified version** of
[Olauncher](https://github.com/tanujnotes/Olauncher) by
[tanujnotes](https://github.com/tanujnotes), forked at upstream commit
[`1d438f8`](https://github.com/tanujnotes/Olauncher/commit/1d438f8)
(`versionCode 112`, `v6.9.1`).

**Modifications have been made by ouj4k2q5 since 2026-09-08.**

This is an independent, unofficial fork. It is **not** affiliated with, endorsed by,
or supported by the original author. Please do **not** report issues with this fork
to the upstream project.

If you are looking for the original, well-maintained launcher, use
[Olauncher](https://github.com/tanujnotes/Olauncher) — available on
[F-Droid](https://f-droid.org/packages/app.olauncher) and
[Google Play](https://play.google.com/store/apps/details?id=app.olauncher).

## Why this fork exists

- Learning project: understanding a real-world Android codebase end to end
- Remove every piece of promotional content, including the ones that only appear after
  the app has been installed for a while
- Stop the app needing network access at all
- Raise `minSdk` so each feature has one code path instead of two

## Changes from upstream

The Kotlin sources went from 5,868 lines across 28 files to 5,185 across 26, and the
app no longer declares any network permission.

### Identity

- Renamed the app to `MinLauncher`
- Changed `applicationId` to `io.github.ouj4k2q5.minlauncher`
- Removed the upstream author's funding configuration and app-store metadata

### Removed — promotion

- The "More features…" row, which upsold the paid Pro Launcher
- The settings footer that linked to the author's other apps, alternating between two of
  them depending on whether the millisecond clock happened to be even
- The share, rate and X/Twitter rows
- **The timed prompt state machine.** This is the part that made removing the rows above
  insufficient on their own: a `UserState` machine advanced through
  `START → WALLPAPER → REVIEW → RATE → SHARE` as the install aged and popped a dialog at
  each step — a wallpaper pitch after 10 minutes, a review request after an hour, a rate
  request after 7 days, a share request after 14 days. It was triggered from three places
  (closing the app drawer, closing settings, tapping the home screen), so the dialogs
  would have kept appearing on a timer
- The new-year greeting dialogs, which shared the same entry point
- Outbound links to the author's blog, privacy policy, developer page and the
  "Not working?" help page

### Removed — network access

- The daily photo wallpaper, which downloaded a JSON index from the author's GitHub gist,
  fetched a photo over HTTP and reapplied it every four hours via WorkManager
- The `INTERNET` permission and the WorkManager dependency that existed only to serve it

What replaces it is the plain-colour path upstream already had: a solid wallpaper that
follows the theme — black under the dark theme, white under the light one. Since the
window is transparent and `windowShowWallpaper` is on, the wallpaper *is* the app
background, so it is repainted whenever the theme changes.

### Removed — dependencies and dead code

- `androidx.lifecycle:lifecycle-extensions`, deprecated since 2019, replaced by explicit
  `lifecycle-runtime-ktx` and `lifecycle-livedata-ktx` declarations
- `com.google.android.material`, which had no references anywhere in the source or
  resources
- Device-admin based screen locking. Android 9 added
  `AccessibilityService.GLOBAL_ACTION_LOCK_SCREEN` and before that `DevicePolicyManager`
  was the only option, so upstream carried both. `minSdk 30` makes only one reachable, so
  `DeviceAdmin`, its `res/xml/policies.xml` policy declaration and the manifest receiver
  with its `BIND_DEVICE_ADMIN` permission are gone
- Unused strings, dropped from `values/` and all 20 translated locales

### Changed

- Raised `minSdk` from 24 to 30, then removed the version-conditional branches it made
  unreachable. Lint's `ObsoleteSdkInt` count went from 36 to 1. Status bar show/hide
  collapsed to `WindowInsetsController` alone, and the pinned-shortcut and pin-item calls
  lost their `@RequiresApi` annotations
- `kotlinx-coroutines-android` is now declared explicitly. The code imports
  `Dispatchers`, `launch` and `withContext` but had been relying on
  `lifecycle-viewmodel-ktx` to supply them transitively
- The theme switcher is hidden on e-ink displays, where `MainActivity` forces the light
  theme anyway, so the control could not have taken effect
- `enableJetifier` removed — `./gradlew checkJetifier` confirms no dependency uses legacy
  support libraries, so it only slowed the build down

## Building

```bash
git clone https://github.com/ouj4k2q5/MinLauncher.git
cd MinLauncher
./gradlew assembleDebug
```

Requirements:

| | |
|---|---|
| JDK | 17 |
| Gradle | 8.11.1 (via wrapper) |
| Android Gradle Plugin | 8.9.1 |
| `compileSdk` / `targetSdk` | 36 |
| `minSdk` | 30 (Android 11) |

The debug build uses `applicationIdSuffix ".debug"`, so it can be installed alongside a
release build — or alongside upstream Olauncher.

```bash
./gradlew test    # unit tests
./gradlew lint    # Android Lint
```

## Installing

No published releases yet — build from source as shown above. `./gradlew assembleDebug`
produces a debug-signed APK at `app/build/outputs/apk/debug/app-debug.apk` that installs
on a real device with `adb install`.

`./gradlew assembleRelease` currently produces an **unsigned** APK, which cannot be
installed.

## Privacy

This app collects no analytics, sends no telemetry, and **cannot reach the network at
all** — it declares no `INTERNET` permission, so network access is not merely unused but
technically unavailable.

The seven permissions it does declare are all launcher functionality:

| Permission | Why |
|---|---|
| `QUERY_ALL_PACKAGES` | Required to list the installed apps a launcher must show |
| `SET_WALLPAPER` | Applying the solid-colour wallpaper |
| `PACKAGE_USAGE_STATS` | Optional; only used to display today's screen time |
| `EXPAND_STATUS_BAR` | Opening the notification shade on swipe down |
| `REQUEST_DELETE_PACKAGES` | Uninstalling an app from the drawer |
| `ACCESS_HIDDEN_PROFILES` | Private Space support on Android 15+ |
| `SET_ALARM` | Opening the clock app when the clock is tapped |

The optional accessibility service is used **only** to turn the screen off with a
double-tap gesture, and collects nothing. Screen time is computed on the device from
`UsageStatsManager` and never leaves it.


## Contributing

This is a personal fork maintained for my own use, so feature requests are unlikely to
be accepted. Bug reports are welcome via
[Issues](https://github.com/ouj4k2q5/MinLauncher/issues).

Improvements that also apply to the original project are better sent
[upstream](https://github.com/tanujnotes/Olauncher) so everyone benefits.

## License

```
Copyright (C) 2020-2025 tanujnotes and Olauncher contributors
Copyright (C) 2026 ouj4k2q5

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <https://www.gnu.org/licenses/>.
```

Licensed under [GNU GPL v3](LICENSE), the same license as the original Olauncher.
The full license text is in [`LICENSE`](LICENSE).

### Trademarks

"Olauncher" and its icon are identifiers of the original project. This fork uses a
different name and application ID to avoid confusion. GPLv3 grants rights to the
source code; it does not grant rights to names or logos.

## Credits

All credit for the original design and implementation goes to
[tanujnotes](https://github.com/tanujnotes). If you find this fork useful, please
consider supporting the original author:

- [GitHub Sponsors](https://github.com/sponsors/tanujnotes)
- [Buy Me a Coffee](https://www.buymeacoffee.com/tanujnotes)