# Changelog — Clone by Ophelia

All notable stable releases of Clone by Ophelia (Deadlock Studio). The latest version is always described in [latest.json](latest.json), the feed the app checks on every open.

## v1.7.1+ — companion releases — 2026-10-01 (round 2)

- **Junk Cleaner v1.1.0** (Products tab): NEW **BIG FILES** — the 25 largest files on shared storage, each with a per-file confirm-delete. NEW **DUPLICATES** — byte-identical copies found by a 3-stage engine (size buckets → 64 KB head hash → full content MD5), so two files with the same name but different content never group, and two files with different names but identical content always do. One-tap "Clean copies" keeps the first copy of each group and removes the rest, after a full review dialog. 9 new JVM tests (17 total)
- **Per-product changelogs**: products.json now carries a structured `changelog` array for every product (version · date · notes) — the release hub timeline reads the same data

## v1.7.1 — 2026-10-01

- **HONEST COMPATIBILITY SYSTEM — only truly cloneable apps are called supported now.** The "Popular" section is now **SUPPORTED**: apps verified to install, launch AND log in as re-signed clones — Discord (your flagship), WhatsApp, Telegram, Instagram, Facebook, Messenger, Snapchat, X, Reddit, Signal, Threads and Spotify. The "Popular Games" section is gone: those games were never honest candidates
- **Free Fire and friends are marked NOT SUPPORTED — with the real reason.** Anti-cheat engines (Free Fire, BGMI, PUBG Mobile, Roblox, all Supercell games, Mobile Legends, Genshin Impact, Minecraft) verify the APK signature at runtime, so clones crash on launch — exactly what you saw when you tried FF. Every Android-declared game gets this protection automatically, and Google-only sign-in apps (YouTube, Gmail, Maps, Photos) are blocked too (their login refuses clone signatures). Tapping a blocked app explains WHY instead of silently doing nothing
- **"Untested" apps ask first.** Everything else in ALL APPS now carries an untested badge and shows a one-tap confirmation with a heads-up before cloning — no more surprise crashes from apps we never verified
- **Ophelia Widgets v1.3.0 — the DOT MATRIX batch** (Products tab): 8 new Nothing-style widgets, 31 total. App Shortcut (any installed app as a dot tile), App Folder (2x2), Month Calendar (today lit in Ophelia red), Step Counter (hardware pedometer), Now Playing (transport keys that steer whatever is playing + live track titles), Web Search (Google/YouTube/DuckDuckGo/Bing), AI Bar (Gemini/ChatGPT/Copilot/Claude) and Contact Dial (tap-to-call tile)
- **Pic Hider v1.3.0** (Products tab): 'gallery won't open' fixed — phones whose gallery picker silently fails now get an instant one-tap file-browser fallback banner; 'PIN won't save' fixed — vault creation writes are synchronous (an aggressive memory killer can never eat a fresh setup), setup failures show a clear message instead of crashing, and the lock screen always keeps a manual Unlock button
- **Junk Cleaner v1.0.1** (Products tab): the crash-on-open is root-caused and fixed (Compose host now extends ComponentActivity — verified in the shipped dex)

## v1.7.0 — 2026-10-01

- **THE SWITCHER NOW LIVES INSIDE THE CLONED APP — properly this time.** No second home-screen icon, no notification hub, nothing that behaves like a second app. Every clone with the account switcher shows a small **draggable Ophelia ⚡ bubble** whenever the clone is open (it rides along on every screen). Tap it: capture the login you're using, switch to a saved login, done — without ever leaving the app. The old design (external "Quick Switch" + a launcher icon) is completely removed
- **Switching is crash-free now.** All state copying happens on a background thread (the old version froze and then died mid-switch), the process-detection bug that left Discord running while its files were being swapped is fixed, and every switch ends with a clean hard restart — the app comes back fully reloaded on the new account. Switching away also **auto-saves** the login you're leaving, so hopping back is instant
- **NEW — EDIT INSTALLED CLONES.** Every clone card has an **Edit** button now: change the clone's name and icon any time. Installs as an in-place update — data, logins, chats and saved switcher accounts all survive
- **FIXED — UNINSTALL.** The delete button on a clone card now opens the real Android uninstall confirmation (it silently did nothing before)
- **FIXED — DEADLOCK STUDIO CREDIT.** Never printed inside an app's own logo again (your Discord logo stays clean). The credit sits where it belongs: in the big teal→rose gradient box pinned to the bottom of the home screen and the switcher panel
- **NEW Deadlock Studio app — EasyPair** (Products tab): offline high-speed sharing between two nearby phones — Wi-Fi Direct, hotspot, same Wi-Fi, USB cable (USB tethering) and Bluetooth. Send photos, videos, audio, files, whole folders, installed apps and clipboard text with no limit; files land in Downloads/EasyPair. 1.3 MB, privacy-first (only visible while you choose to be)
- **Ophelia Widgets v1.2.0**: the launcher icon is now the real Ophelia artwork — the character + widgets lockup — instead of the plain wordmark

## v1.6.0 — 2026-09-30

- **THE BIG ONE — ACCOUNT SWITCHER INSIDE EVERY CLONE.** Every new clone ships with a second home-screen icon: **"Discord Alt ⚡ Switch"** (your clone's name + ⚡). Open it and you get the Ophelia Quick Switch screen: tap **CAPTURE CURRENT ACCOUNT** while logged in with account A, then log out / log in as account B and capture again — from then on, switching between the two logins is ONE TAP. One clone, many accounts, **no need to create more clones just for more accounts** (exactly what you asked for). The switcher snapshots the clone's saved login state and restores it on switch — fully offline, nothing uploaded anywhere
- **Quick Switch notification** (home screen toggle): a silent persistent notification with one-tap buttons for your most-used clones — switch instances from anywhere via the notification shade
- Old clones can be upgraded in place: tap **↻ (update)** on their card and they're rebuilt **with the switcher injected** — no reinstall of your accounts needed if you stay logged in during the update
- **Ophelia Widgets v1.1.0** in the Products tab: 11 new widgets (total 23) — the neon Ophelia brand set (Ophelia Clock, crimson Battery ring, ornate Signature lockup), Year Progress, Day Progress, Moon Phase, World Clock, Storage gauge, RAM monitor, Tally counter, Decision spinner — plus the new studio wordmark icon and the "Ophelia Widgets" name
- **NEW Deadlock Studio app — Junk Cleaner** (Products tab): one-tap Smart Clean with full review before deletion, a system cache boost that uses the official Android storage API (the same lever Google's Files app uses — no root), and a per-app cache dashboard. Lightweight, honest, 3 permissions all explained in the app
- Engine: brand-new AXML node-injection capability — the manifest patcher can now ADD components (the switcher activity + permission), verified end-to-end on the real Discord 347.12 APK in CI

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
