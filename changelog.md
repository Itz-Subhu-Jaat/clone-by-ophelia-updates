## v1.9.0+ — companion releases — 2026-10-01 (round 11)

- **EasyPair v1.6.0 — PARTIAL-FILE RESUME** (Products tab): interrupted transfers now **continue from the exact byte instead of starting over**. Files of 8 MB and up are received into a private `.part` buffer — when Wi-Fi drops at 80% of a 2 GB video, those bytes STAY on the receiver (with a journal entry keyed by sender+name+size plus the **SHA-256 of the first 4 KB**, so a different file with the same name can never be appended to and corrupt it). The next attempt carries that hash in the offer; the receiver answers inside its accept frame with exactly how many bytes it already holds (binding), the sender **skips precisely those bytes** and streams only the missing tail; the receiver appends and publishes the finished file to Downloads/EasyPair. A **RESUME TRANSFER card** appears on Home + Queue after a failed batch — one tap, held items, live progress that starts where it left off. Works over **every channel**: Wi-Fi, hotspot, USB cable, Wi-Fi Direct (both directions) and Bluetooth; small files keep the classic instant path (a re-send is cheaper than a copy). Partials older than 14 days self-clean on startup, nothing ever leaves the two devices. Old receivers/senders keep working unchanged (unknown fields ignored). Settings gains a TRANSFERS card explaining the behaviour. 17 new pure-JVM tests (54 total green): head hashing against Python-computed vectors, trickle-read exact skipping, the full journal lifecycle (restart survival, clock-injected cleanup, orphan sweep), offer/accept shapes and the sender/receiver offset math.

## v1.9.0+ — companion releases — 2026-10-01 (round 10)

- **Junk Cleaner v1.3.0 — SIMILAR PHOTOS** (Storage): photos that LOOK the same are now catchable even when the bytes differ — burst shots, HDR takes, WhatsApp/Telegram re-sends. Every image in the library gets a **perceptual hash** (two 64-bit fingerprints: average-hash + difference-hash over a box-downsampled luminance grid — the same technique dedicated photo-cleaners use), and matching ones are grouped with a "% alike" chip. Each group **keeps the LARGEST copy** (the least-compressed, best-quality one — the right rule for re-compressed re-sends, since WhatsApp shrinks and the original is bigger) and suggests the rest for review. Grouping is union-find over hash distances (burst chains A~B~C merge correctly), pruned via 128 similarity bands so a 5000-photo library groups in milliseconds after the fingerprint pass. Deletion is **double-confirmed**: the app's own dialog first, then **Android's own system delete sheet** on API 30+ (the same flow Files by Google uses — createDeleteRequest, nothing leaves without the OS dialog); API 26-29 uses the sanctioned WRITE_EXTERNAL_STORAGE resolver path. Live progress while fingerprinting ("Fingerprinting 320/1,245 photos…"), unreadable images are skipped and counted, and the whole pipeline is pure-JVM unit-tested (18 new tests — 35 total green: hash math, noisy-recompress tolerance, flat-image edge case found via a real floating-point accumulation bug and fixed with epsilon comparisons, transitive grouping, keeper selection, band pruning correctness).

## v1.9.0+ — companion releases — 2026-10-01 (round 9)

- **Ophelia Widgets v1.5.0 — SUNRISE IN THE CITY'S OWN CLOCK** (Products tab): the Sunrise/Sunset widget no longer assumes your phone's timezone. The city lookup now stores the city's IANA zone, so **Tokyo on an Indian phone shows Tokyo wall-clock times** — not IST. The tile gains a live **NOW line** (the city's current time, refreshed hourly) and a short **UTC offset chip** next to the city name (TOKYO +9). Scheduling is zone-aware too: the day flips at the **city's own local midnight** (the earliest of device + all placed cities), so a far-away widget rolls to its new day on time. The config screen shows the resolved zone after lookup, and older placed widgets keep working — they fall back to the phone's zone. 7 new pure JVM tests (40 total green: zone parsing fallbacks, offset formatting, 14-char label fit, exact next-midnight math for UTC/Kolkata/Tokyo, Tokyo sunrise in Tokyo clock)

## v1.9.0+ — companion releases — 2026-10-01 (round 8)

- **EasyPair v1.5.0 — ENCRYPTED RECEIVER** (Products tab): the OTHER side of encryption. New "Encrypted receiver (HTTPS)" setting in EasyPair: a self-signed certificate is generated in the **Android Keystore** (hardware-backed where the device supports it — the private key never leaves the phone) and port 53317 then serves the LocalSend REST API **behind TLS**. Our multicast announcement carries `protocol: "https"` plus the certificate's real SHA-256, so LocalSend apps on any platform and EasyPair v1.4.0+ senders **pin** us instead of trusting blindly. Default OFF (LocalSend's own default) — plain-HTTP interop is unchanged until you opt in; flipping it while visible restarts the endpoint instantly, and a keystore quirk degrades to plain HTTP with a visible note instead of breaking. Identity DTO now includes port+protocol for spec completeness. 2 new tests (37 total).

## v1.9.0+ — companion releases — 2026-10-01 (round 7)

- **EasyPair v1.4.0 — ENCRYPTED LOCALSEND** (Products tab): LocalSend receivers running their **HTTPS (encryption) mode** now work too. Announcements that say `"protocol": "https"` are honoured: the transfer goes over TLS with **REAL certificate pinning** — the peer's announced SHA-256 fingerprint must match the certificate it actually presents, or the handshake is refused with a clear reason (never trust-all, never a silent accept). Encrypted peers get a 🔒 + "encrypted" badge in the nearby list, and the fingerprint travels through both discovery paths (multicast announcements AND /register bodies). 8 new JVM tests including a real self-signed HTTPS server round-trip: pinned client passes, wrong pin is rejected mid-handshake, unpinned connection is rejected by default CA validation.

## v1.9.0 — 2026-10-01 (round 6)

- **Clone by Ophelia v1.9.0 — STUDIO UPDATE CENTER**: the Products tab is now a real update center for the whole family. Every app card shows its **full release history** (each version, its date, what changed) pulled live from the official OTA feed, plus a clear badge: **UPDATE ready** / **UP TO DATE** / **NEW**. A status strip sums up the family (updates ready, installed count, last-checked time, manual refresh). **Real launcher icons** on every card (Widgets, Junk Cleaner, EasyPair composited from their actual adaptive icons), the tab renamed **Products → Studio** with a family-grid icon, and every install row marked **sha256-verified** — because it is. Cloning engine untouched.

## v1.8.0+ — companion releases — 2026-10-01 (round 5)

- **EasyPair v1.3.0 — FULL LOCALSEND INTEROP** (Products tab): the **real LocalSend app on any PC or phone now sees this device automatically**. EasyPair joins the LocalSend multicast group (224.0.0.167:53317) and announces itself exactly like a LocalSend device — so LocalSend on Windows/macOS/Linux/iOS/Android lists your phone as a target and **sends files straight to it over HTTP REST** (protocol v2.2: prepare-upload → upload, sha256-verified, files land in Downloads/LocalSend and appear in the Received Files screen). It works BOTH ways: **EasyPair also sends to any LocalSend receiver** the same way — LocalSend devices show up in the nearby list with a 📡 badge. Both wire generations are supported (classic token + current sessionId/per-file-token), verified against the LocalSend source. 27 new JVM protocol tests (announcement parsing, both upload query formats, chunked + content-length HTTP bodies, end-to-end round trip)

## v1.8.0+ — companion releases — 2026-10-01 (round 4)

- **Clone by Ophelia v1.8.0**: **THE SWITCHER IS NOW THE DISCORD WAY.** The floating bubble + its button (which crashed the clone on open) are completely gone — replaced by exactly what you asked: **HOLD the app's own profile tab** (bottom-right corner) and the Ophelia **accounts drawer** slides up — logged-in account, one-tap SWITCH, **ADD ACCOUNT** (saves this login, restarts logged out for the next one — no logout ever), rename/delete via long-press. Nothing is injected as an activity or secondary process anymore: the whole drawer is a plain overlay in the app's own window and main process — the smallest possible footprint, one less thing ROMs can crash on. **Custom logos fixed for real:** ANY picture of ANY width is now contain-fitted whole into the launcher's visible safe zone with edge-extension around it — the entire logo shows, zero blank space, no cut-off wordmarks. **The delete button on clone cards is removed** — uninstall clones the normal Android way (hold the icon → Uninstall); stale list entries clean themselves up
- **EasyPair v1.2.0** (Products tab): the **photos/videos tap crash is fixed** (the system photo picker was being asked for more items than Android's 100-item limit allows — it threw SecurityException at launch; now 50 + every picker launch is guarded). NEW **in-app gallery** (LocalSend-style): multi-select tile grid with real thumbnails. **The app now asks for real permissions** — Photos & videos / Music & audio / Nearby devices show properly on Android's permissions page (fixes the "kisi app ne koi permission nahi mangi" report), managed from a new Permissions card in Settings. **Discovery rebuilt on the LocalSend pattern** (studied their v1.18.2 source + protocol v2.2): receivers UDP-announce every 2s and answer probes — devices appear by themselves on same Wi-Fi AND phone hotspots. **Your artwork is now the app icon**
- **Junk Cleaner v1.2.0** (Products tab): **real runtime media permissions** (Photos/Videos/Audio — one-tap card, listed properly on the app-info page) and **your artwork as the launcher icon**
# Changelog — Clone by Ophelia

All notable stable releases of Clone by Ophelia (Deadlock Studio). The latest version is always described in [latest.json](latest.json), the feed the app checks on every open.

## v1.7.1+ — companion releases — 2026-10-01 (round 3)

- **Ophelia Widgets v1.4.0 — the SECOND dot batch** (Products tab): 7 new widgets, **38 total**. **Game Score** — a two-team scoreboard with +/− keys per side and a centre reset. **Days Since** — the count-up mirror of Countdown (days since you quit, started, moved in). **Week Number** — ISO week + a Monday-to-Sunday dot strip with today lit red. **Uptime** — time since last boot, refreshed hourly. **Hydro** — hydration tracker: tap to log a glass, the dot strip fills, optional gentle reminder notifications (opt-in, notification permission asked once at configure). **Currency Rate** — live rates for any pair (USD→INR etc.) via a keyless open API, refreshed every 6 hours, tap refreshes instantly, ↑↓ change arrows. **Sunrise/Sunset** — for any city, computed OFFLINE with NOAA solar equations after a one-time lookup (polar day/night honestly says so). 9 new pure-math JVM tests (34 total green)
- **EasyPair v1.1.0** (Products tab): NEW **Received Files screen** — every incoming file is registered with the URI the system handed us, so you can browse everything that landed in Downloads/EasyPair, **tap to open**, **re-share onward** (single file or Share-all) and **delete for good** with a confirm. History screen gains a one-tap "Browse received files" jump
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
