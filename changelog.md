# Clone by Ophelia — release log

## round 10 · 2026-10-03 — the switcher fix: Discord clones stop crashing, the quick menu finally appears

**Clone by Ophelia v2.0.1 (code 13)** · sha256 `963091c6…62af74d` · 7,293,288 B
- THE BUG: on v2.0.0, Discord clones crashed and/or the account switcher never appeared (hold your profile tab → nothing). Root cause: the injected switcher module's helper classes were auto-renamed into short packages (a/, b/, i/, m/…) that collide with class names Discord itself ships — Android resolves the name from the HOST's dex first, so the switcher's calls hit Discord's code instead of its own → crash on drawer open / silently dead bootstrap. The v2.0.0 quick-menu rewrite pulled in more helpers, growing the collision set from 22 to 43 names and making the failure near-universal.
- THE FIX: every helper class in the injected dex now lives under the unique `opheliak` namespace (plus the kept `ophelia.switcher` classes) — a name no host app can ever define. A new build-time guard parses the built dex and fails the build if ANY class lands outside those namespaces, so this bug class can never ship again.
- VERIFIED: invoke-target scan of the old dex showed SlotStore/DiscordStatus/OpheliaBootstrap calling collided names (Ll/a, Lm/i, Lm/j, Li/b …); the new dex has 0 collisions against Discord's full 17,838-class set and Roblox's 26,528-class set; the real Discord stable 34712 base was patched end-to-end (provider injection, classes5.dex 86,392 B, zipalign + apksigner green, selfcheck PASS).
- ACTION FOR USERS: update the cloner, then re-clone Discord — old broken clones should be deleted. Same signing key → in-place update, registry and settings intact.

## round 9 · 2026-10-02 — OTA EVERYWHERE: Ophelia Update Center inside all 8 apps + the compass parse-fix round

Every individual app now checks the PUBLIC release feed by itself and prompts when a new version is out —
no need to come back through the cloner. Downloads are streamed + sha-256 VERIFIED in-app, so a corrupted
download can never be installed. All 8 APKs are signed with BOTH v1+v2 schemes now (same keys).

**Ophelia Compass v1.0.1 (code 2)** · sha256 `264a7fef…26914e3a` · 8,215,189 B
- THE PARSE FIX: APK now signed with BOTH v1 + v2 schemes — fixes 'there was a problem parsing the package' on devices/OEM installers that choked on v2-only APKs (delete a failed old download and grab this one).
- Ophelia Update Center: silent update check on open + 'Check for updates' in Settings; downloads are sha-256 VERIFIED in-app — a corrupt download can never be installed.
- Same signing key as v1.0.0 → installs as a normal update. Version footer now reads the real installed version.

**Ophelia Calculator v1.0.1 (code 2)** · sha256 `971314cf…b3c9672c` · 8,108,072 B
- Ophelia Update Center: the app now checks for updates BY ITSELF (silent check on open + 'Check for updates' in Settings) with sha-256 verified in-app download.
- v1+v2 signature schemes (install-compat). Honest About copy: the only network use is the optional update check.

**Ophelia Game Booster v1.0.1 (code 2)** · sha256 `f4d4ab06…a9a23d7e` · 8,102,124 B
- Ophelia Update Center: self-update check + prompt with sha-256 verified in-app download (About section).
- v1+v2 signature schemes (install-compat). Home header shows the live installed version.

**Ophelia Fit v1.0.1 (code 2)** · sha256 `2e86910c…e34b51c5` · 8,088,619 B
- Ophelia Update Center: silent check on open + 'Check for updates' row in the More tab, sha-256 verified in-app download.
- v1+v2 signature schemes (install-compat). All version labels now dynamic.

**Ophelia Junk Cleaner v2.0.1 (code 9)** · sha256 `eded9efc…db820694` · 8,344,363 B
- Ophelia Update Center on the Home tab — the cleaner updates ITSELF: silent check on open, sha-256 verified in-app download, 'Not now' snooze.
- v1+v2 signature schemes (install-compat). SAME OLD KEY — installs over v2.0.0 keeping all data/whitelist/history.

**Ophelia Pic Hider v2.0.1 (code 7)** · sha256 `5e7dfa88…8bb937f1` · 9,232,419 B
- Ophelia Update Center in Settings → About: silent check on open + manual 'Check', sha-256 verified in-app download.
- v1+v2 signature schemes (install-compat). SAME OLD KEY — vault data untouched.
- Disguise safety: update prompts NEVER appear inside the calculator disguise.

**Ophelia Widgets v2.0.1 (code 9)** · sha256 `0ad67fbc…1c888f1f` · 8,443,922 B
- Ophelia Update Center in the widget browser: silent check on open + 'CHECK' row, sha-256 verified in-app download.
- v1+v2 signature schemes (install-compat). SAME OLD KEY — all configured widgets survive the update.
- Widget config screens stay interruption-free.

**Ophelia EasyPair v1.6.2 (code 9)** · sha256 `05d02be6…8c5689c4` · 2,218,868 B
- Ophelia Update Center in Settings → About: silent check on open + manual 'Check', sha-256 verified in-app download.
- v1+v2 signature schemes (install-compat). SAME OLD KEY — installs over 1.6.1.
- Transfers keep priority: an incoming-transfer prompt always wins over the update dialog.
## round 8 · 2026-10-02 — the signature-continuity round: widgets/Junk Cleaner/Pic Hider 2.0.0 + 4 NEW apps

**Ophelia Widgets v2.0.0 (code 8)** · sha256 `9275c237…27094a2` · 8,402,551 B
- COMPLETE UI REDESIGN: every widget painted with a colorful gradient/glass/solid panel (works on any launcher).
- Background TRANSPARENCY slider (0-100%) on every single widget + 6 gradient presets, 12 accent swatches, custom RGB, auto text contrast.
- Live-preview config screen — tweak the skin, watch the real widget re-render before saving.
- All 38 widget kinds now configurable (12 had no config screen before).

**Junk Cleaner v2.0.0 (code 8)** · sha256 `68c8b95e…466783e` · 8,315,963 B
- Full UI redevelopment: animated storage-health ring, colorful category grid, scan → review → clean → done.
- NEW: Whitelist (long-press any found item), cleaning history + total-freed stats, day streaks.
- NEW: 0-byte empty files + APK residue (orphan vs installed vs bundle) categories.
- Empty-folder cleaning preserved (race-safe, children-first) — and it really works.

**Pic Hider v2.0.0 (code 6)** · sha256 `c4d91732…f33e1e4e` · 9,171,850 B
- THE permission fix: READ_MEDIA_IMAGES/VIDEO + partial-access handling + rationale card + full MediaStore gallery browser.
- Import works with OR without permission (system photo picker path — zero permission needed).
- Calculator disguise mode (secret sequence unlocks the vault) + share-sheet import from any app.

**Ophelia Calculator v1.0.0 (code 1) — NEW APP** · sha256 `09aa52da…d3d3eca2` · 8,063,240 B
- Scientific + basic pads, DEG/RAD, live preview, history tape, memory keys, unit converter (8 categories / 69 units), date math, percent-of semantics (200+10% = 220), 4 themes, haptics. 69 unit tests.

**Ophelia Compass v1.0.0 (code 1) — NEW APP** · sha256 `b0adbd4c…f9bd9144` · 8,186,752 B
- Sensor-fused heading (rotation-matrix + complementary filter, no backspin), 3 dial themes, level bubble, calibration detector, true north (GPS declination), bearing lock + waypoints, field meter, haptics. 16 unit tests.

**Ophelia Game Booster v1.0.0 (code 1) — NEW APP** · sha256 `d14501d3…9a53eaf3` · 8,057,181 B
- BOOST with REAL before/after availMem delta, draggable FPS overlay, DND-on-launch (auto-revert), per-game session tracking, profiles, whitelist. 22 unit tests.

**Ophelia Fit v1.0.0 (code 1) — NEW APP** · sha256 `a2ec1b30…d00101db3e` · 8,059,453 B
- 85-exercise library, routine builder + set-by-set logger, GPS run tracker, step counter, Tabata/HIIT timer, volume charts, Epley PRs, body log, JSON export. 50 unit tests.

**Signing continuity**: all three upgraded apps are signed with the SAME keys as the versions you already have installed — normal in-place updates, nothing to uninstall. The four new apps are fresh installs with their own keys.

---
## round 7 · 2026-10-01 — Roblox supported + Discord quick menu + the family-wide bug-scan round

**Clone by Ophelia v2.0.0 (code 12)** · sha256 `ebf35a55…58470ff` · 7,293,184 B
- Roblox moved to the SUPPORTED list (username+password login, no signature gate at login).
- The split-screen / small-screen unlock: Roblox ships `resizeableActivity=false`, `smallScreens=false`, `requiresSmallestWidthDp=300` — every clone rewrites all three (verified on the real 2.740.931 APK with aapt2 + apksigner).
- The account switcher is now a Discord-only quick menu: hold your profile tab → SET STATUS (Online/Idle/DND/Invisible, real API calls with an honest open-your-profile fallback) + SWITCH ACCOUNTS below. Non-Discord clones stay 100% inert.
- Patcher selfcheck grew the resize tests (locked-manifest asset + end-to-end).

**Pic Hider v1.3.1 (code 5)** · sha256 `f4fa0e2c…fe03d79` · 7,876,059 B
- The durability pass: atomic verified vault creation with rollback, self-heal for half-written vaults, streaming imports, off-main unlocks, reboot-safe photo frames state.

**Junk Cleaner v1.4.1 (code 7)** · sha256 `9a6c4245…156307f5` · 7,549,634 B
- MediaStore-aware deletes (no gallery ghosts), Android 10 legacy storage + grant sheet, folder-safety hardening, overview refresh after every clean.

**Ophelia Widgets v1.5.1 (code 7)** · sha256 `a338439d…4784c01` · 7,474,074 B
- Photo frames survive reboots (pick-time copy), single-source battery/weather scheduling, reboot-proof re-arms, thread-safe dot rendering, working currency arrows.

**EasyPair v1.6.1 (code 8)** · sha256 `4cf89171…6e5153e` · 2,193,509 B
- The resume head-hash buffer-offset fix (fragmented reads hashed the wrong bytes — resume never engaged), truncation guard on small files, group-owner send cleanup.

---
## v1.9.0+ — companion releases — 2026-10-01 (round 12)

- **Junk Cleaner v1.4.0 — SIMILAR VIDEOS** (Storage): the photo trick, one level up — **re-sent and re-encoded copies of the SAME video clip** are the heavy hitters of storage waste, and now they group up. Four **keyframes are sampled across each video's timeline** (at 1/5, 2/5, 3/5 and 4/5 of the runtime) and each frame is fingerprinted with the proven photo pipeline (aHash + dHash over box-downsampled luminance). Two clips group when their **durations agree** (±1.5 s or ±8 %, whichever is larger — a re-encode keeps length, a trimmed re-cut has a genuinely different one and is deliberately left alone) AND at least **3 of the 4 frames find a matching counterpart in the other video, in BOTH directions** (the both-ways rule stops a static/slideshow clip from swallowing a real edit that happens to share one frame). Frames match against ANY slot — keyframes can sit a beat differently after a re-encode — and **flat frames (black screens) are dropped** from fingerprints, so two clips that merely open on black can never false-group. Keep rule identical to photos: **keeper = the largest file** (the least-compressed original — WhatsApp shrinks, the original is bigger). Decoding stays fast on real libraries: `getScaledFrameAtTime` decodes straight to thumbnail size (a 4K clip never materialises a full-res frame), the newest 600 videos are walked with live progress ("Fingerprinting 40/812 videos…"), and an empty DURATION column on old databases is healed from MediaMetadataRetriever. Deletion is double-confirmed as always: the app's dialog, then **Android's own system delete sheet** on API 30+ (createDeleteRequest); API 26-29 uses the sanctioned resolver path. 20 new pure-JVM tests (55 total green): duration tolerance windows, re-encode bit-flip tolerance, one-way-agreement rejection, keyframe misalignment, transitive re-send chains through three chats, shuffled-input determinism, keeper selection, effective-duration fallback, and the both-thresholds frame rule.

