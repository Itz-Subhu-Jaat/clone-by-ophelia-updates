# Clone by Ophelia — Update Channel

This is the **official update channel** for **Clone by Ophelia**, the app cloner by **Deadlock Studio**.

The app's source code lives in a **private repository**. This separate, public repository exists for two purposes: **delivering stable OTA (over-the-air) updates** to installed copies of Clone by Ophelia, and **distributing Deadlock Studio's other apps** (the Products tab inside Clone by Ophelia).

Only **built APKs and metadata** live here — no source code of any Deadlock Studio app is published in this repo.

## How updates work

Every time Clone by Ophelia opens, the app's built-in `UpdateManager` fetches the machine-readable feed at:

```
https://raw.githubusercontent.com/Itz-Subhu-Jaat/clone-by-ophelia-updates/main/latest.json
```

If the feed's `versionCode` is higher than the installed version, the app offers the update and downloads the APK from `apkUrl` (verifying it against `sha256` when present).

## Repository structure

| Path | Purpose |
|---|---|
| `latest.json` | Machine-readable update feed the app reads on every open (`versionCode`, `versionName`, `apkUrl`, `sha256`, `releaseNotes`) |
| `products.json` | Deadlock Studio app catalogue for the in-app Products tab (`id`, `name`, `tagline`, `description`, `packageName`, `versionName`, `versionCode`, `apkUrl`, `sha256`, `sizeBytes`) |
| `changelog.md` | Human-readable update log / version history |
| `apk/` | Stable release APKs (one file per app and version, e.g. `clone-by-ophelia-v1.2.0.apk`, `pic-hider-v1.0.0.apk`) |

The published branch is **`main`**.

## Publishing a new version (repo owner)

1. Build a **STABLE** release APK (buggy/experimental builds stay in the private source repo — only stable versions are published here).
2. Copy the stable release APK into `apk/`, named like `clone-by-ophelia-v<versionName>.apk`.
3. Bump `versionCode` (must be higher than the previous release) and `versionName` in `latest.json`, and update `apkUrl` and `sha256` to match the new APK file (`sha256sum apk/clone-by-ophelia-v<version>.apk`).
4. Add a `changelog.md` entry for the new version with its date and release notes.
5. Commit everything to **`main`** and push.

Every installed app will pick the update up automatically on its next open — no other infrastructure needed.
