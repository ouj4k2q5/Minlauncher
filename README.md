
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
- Remove all promotional content — links to the author's other apps, the pro-version
  upsell, and the timed rate/share prompts — for a strictly prompt-free experience
- Replace the network-backed daily photo wallpaper with a plain solid colour that
  follows the theme, so the app needs no internet access at all
- Raise `minSdk` to simplify version-conditional code paths

## Changes from upstream

### Identity
- Renamed the app to `MinLauncher`
- Changed `applicationId` to `io.github.ouj4k2q5.minlauncher`
- Removed the upstream author's funding configuration and app-store metadata

### Removed
- _(planned)_ All in-app promotion: the "More features…" pro upsell, the footer links
  to the author's other apps, the rate/share entries, and the timed prompt state
  machine that surfaced review/rate/share dialogs after 10 minutes / 1 hour / 7 days /
  14 days of use
- _(planned)_ Remaining outbound links to the author's blog, privacy policy and
  developer page
- _(planned)_ The network-backed daily photo wallpaper, along with the `INTERNET`
  permission and the WorkManager dependency
- _(planned)_ Deprecated `androidx.lifecycle:lifecycle-extensions` dependency and the
  unused `com.google.android.material` dependency
- _(planned)_ Device-admin based screen locking, which `minSdk 30` makes redundant

### Changed
- _(planned)_ Raised `minSdk` from 24 to 30 and dropped the version-conditional
  branches it made unreachable
- _(planned)_ The theme switcher is hidden on e-ink displays, where the light theme is
  forced anyway

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
| `minSdk` | 24 |

The debug build uses `applicationIdSuffix ".debug"`, so it can be installed alongside a
release build.

```bash
./gradlew test    # unit tests
./gradlew lint    # Android Lint
```

## Installing

No published releases yet — build from source as shown above.

## Privacy

This app collects no analytics and sends no telemetry.

Two Android permissions deserve explanation:

- `QUERY_ALL_PACKAGES` — required to list the installed apps a launcher must show
- `PACKAGE_USAGE_STATS` — optional; used only to display today's screen time

The optional accessibility service is used **only** to turn the screen off with a
double-tap gesture, and collects nothing.

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