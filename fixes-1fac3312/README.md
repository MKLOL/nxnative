# Packed RGBA4 review fixes: 1fac3312

Source candidate: `1fac3312fe86e0c5a7ec6a27cfa84aba2a55aa81` on PR #993; baseline main `65501fbbaf8526f7aab0a1edd028135888590f8e`. The four synthetic failures were reproduced on the original PR head `b8adb672d245f9a3cbea9af1d2f634c618aeb25d`.

## Fixes and regressions

- Converted writeback and CPU mirrors track written mip levels, array slices and 3D planes. Independent native writes to other subresources survive.
- Attachment conversions end their encoder before auxiliary whole-layer clears begin.
- Texture-to-buffer writes invalidate converted content when their destination aliases linear texture storage.
- Integer-image fitting recognizes both RGBA4 bases and inverse RGBA4 views, preserving narrowed view ranges and writing real canonical storage.

The new fixtures run production copy/preparation/binding/render paths and verify literal guest words, physical backing pixels and untouched subresources. `t_view_swizzle` cases 447–451 cover aliased buffer writes, native/raw MRT array and 3D planes, and multi-plane clears. `t_readback` IDs 650–651 cover plain and mip/layer-selected inverse integer images with actual shader stores. Corrected case 447 uses the pool's real Metal buffer, matching production lifetime and resource identity; against original PR head it fails all eight sampled channels while guest bytes, padding and CPU-dirty checks pass. The original four failure logs are included here; source fixes and integrated fixtures are in the PR.

## Final validation

Apple M5 Max, macOS 27.0.1, Xcode 27.2, Metal 32023.922.4. Full graphics suite exits 0, including Metal validation. Selected totals: 677 view checks, 341 readback cases (342 with fast wake off), 18 stencil checks; zero failures. Tool tests: 906, 16 skips. CPU texture checks: 5,344/5,344 plus Metal format creation. Docs: 11 tests. Pinned LLVM 23.1.4 on all six modified C/Objective-C translation units: zero new findings against main. `check-summary.txt` holds selected test output; full local logs are retained separately. Narrow test-only ownership annotations explain the analyzer's C-hash-retention false positives; no lint baseline increase.

`regression-report.md` is the exact committed-candidate harness report: RPG 82,032/82,032 and 13 Sentinels 751/751 on both sides, zero changed MSL, newly failing shaders or interface failures. Neither profile has a smoke route, so harness gameplay and performance columns are unmeasured.

## Muted copied-save checks

- RPG: 187-second headless native Metal run with complete current-chain libraries and matching generated metadata. Loaded Mushroom Way, moved into a two-Goomba battle, confirmed sunlit ground/lower foliage, completed an attack (one enemy eliminated and HP19→17 after counterattack), exit 0. The two existing conservative-rasterization stub diagnostics remain; no missing/failed shader or pipeline loads. `rpg-battle.png` and `rpg-after-attack.png` are unedited snapshots.
- 13 Sentinels: 62-second copied-save title/load/Juro Kurabe protagonist-selection route, normal image, no red diagnostics or failed loads, exit 0. `13-sentinels-loaded.png` is the final snapshot.
- Both use `DM_AUDIO=1 DM_AUDIO_MUTE=1`, isolated save/temp/pipeline caches, no bundled archive and no prebuild. No audible, FPS, later-area or iOS claim is made.

The RPG and control binaries were built from the core hashes recorded in `metadata.json`; those hashes match the committed candidate. Screenshots and sanitized runtime logs contain no game files, keys, saves, extracted textures, shader outputs or binary captures. Local paths were replaced with placeholders; image pixels were not edited.
