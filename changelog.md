# Changelog — Clone by Ophelia

All notable stable releases of Clone by Ophelia (Deadlock Studio). The latest version is always described in [latest.json](latest.json), the feed the app checks on every open.

## v1.5.0 — 2026-09-30

- **The icon editor is now a proper crop screen** (the design you asked for): a dark "Edit Image" editor with a centered square crop window, rule-of-thirds grid, white frame, pinch/drag to position the photo, a zoom slider with live percentage, and a Rotate button (90° steps) plus a Shape button that previews how launchers will mask your icon (square / rounded / squircle / circle)
- **Fixed the Deadlock Studio credit being cut in half on Android 12+**: modern Android circle-masks the splash logo, and the credit used to sit outside the mask. It now lives in the always-visible centre of the splash — "DEVELOPED BY / DEADLOCK STUDIO" shows completely, including the last letters, on every Android version
- **NEW Deadlock Studio app — Cool Widgets**: 12 home screen widgets in 7 categories (digital clock, analog clock, big date, countdown, battery ring, weather for any city you type, sticky note, daily quote, flashlight, quick search, settings shortcuts, photo frame). Only 2 permissions in the whole app, both explained up front. Install it from the Products tab
- Engine: the credit-strip downscaler now samples every pixel of the studio lockup exactly once (integer rounding used to silently drop the right edge of the text)

## v1.4.0 — 2026-09-30

- **New app identity — the Ophelia brand package.** The app's internal package name is now `com.ophelia.appcloner` (the old build carried a leftover internal name). Because Android treats a different package as a different app, **install this update and then uninstall the old "Clone by Ophelia" icon** — both will appear on your launcher until you remove the old one. Your cloned apps are NOT affected; they keep working exactly as before
- **~3x bigger Deadlock Studio credit** on clone loading screens: a bold two-line "DEVELOPED BY / DEADLOCK STUDIO" lockup with gradient + glow — clearly visible on every splash
- **Google sign-in / passkey awareness while cloning**: the configure screen now shows a heads-up card when the target app's only login is Google (YouTube, Gmail, Google Photos…) and a tip card for apps with clone-friendly logins (Discord / WhatsApp / Telegram QR login). Help → "Google sign-in & passkeys" got per-app guidance
- Pic Hider **v1.2.0** in the Products tab: the photo-import and custom-icon crashes are fixed at the root (the vault used to lock itself the moment the gallery picker opened), fingerprint unlock now also works on Class-2 sensors (most budget face-unlock phones), the PIN setup / lock / change screens were rebuilt as a professional step-by-step wizard, and the app gained a 21-test self-check that runs on every release

## v1.3.0 — 2026-09-30

- **Custom icons fixed for real** — clones now get genuine adaptive icons: your photo keeps full quality (no more black or blurry icons), and every icon variant of the app is replaced, including Discord's in-app icon styles (matte dark and friends)
- **Icon editor redesigned**: cleaner full-screen editor with live launcher previews (circle / squircle / rounded) so you see exactly what your home screen will show — no more confusing crop box
- Fixed "Download failed: Failed to find configured root" when installing Pic Hider from the Products tab
- Downloaded APKs (app updates and studio products) are deleted automatically after the install finishes — your storage no longer fills up
- Pre-installed apps that were updated from the Play Store (YouTube, Amazon, PhonePe, …) can now be cloned like any normal app; only true system apps stay dimmed
- About screen: new researched sections on why some apps show "Get the app from Google Play" and how to sign in to clones without Google (email, QR, one-time links)
- Pic Hider v1.1.0 in the Products tab: **app disguise** — switch its launcher name and icon to Calculator, Notes or Gallery, or pin a custom shortcut with any name and any picture

## v1.2.0 — 2026-09-30

- New app icon: the official Ophelia brand mark
- **Products tab** (bottom bar): install Deadlock Studio's other apps — starting with **Pic Hider**, a PIN-encrypted private photo & video vault with fingerprint unlock
- System apps (Play Store, Gallery, …) are dimmed and can't be picked anymore — they can't be re-signed, so they fail confusingly at install
- Curated apps that aren't installed show an "install app" tile that opens their Play Store page in one tap, then they're cloneable
- Clones now carry a bold, glowing "Developed by Deadlock Studio" credit on their loading screen by default (removable only via the collapsed Advanced section)
- Apps that declare a shared user id are rejected with a clear reason instead of Android's confusing install error
- "Update clone" now tells you to reinstall the source app from the Play Store when it's missing

## v1.1.0 — 2026-09-29


- Full-screen icon editor: zoom in and out, drag your photo anywhere, pick the background fill — the icon exports exactly as you frame it
- Icons now export edge-to-edge with rounded corners, like real app icons (fixes icons looking tiny or dark)
- Uninstalled clones are removed from your list automatically
- Curated apps & games: WhatsApp, Telegram, Facebook, Snapchat, Instagram, Free Fire, BGMI/PUBG, Clash of Clans, Brawl Stars and more
- Loading screen credit: a small glowing 'Developed by Deadlock Studio' on clones
- In-app updates: the app now checks this repo for new versions on every open

## v1.0.0

- Initial release
- Clone Discord and Roblox: run multiple independent instances side by side
- Unlimited clones — create as many as you want
- Custom name & icon for every clone
- Clone by Ophelia branding, by Deadlock Studio
