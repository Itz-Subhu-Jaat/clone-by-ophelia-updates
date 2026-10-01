# Clone by Ophelia — release log

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

