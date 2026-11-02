# Super Mario RPG RGBA4 lighting evidence

Baseline: `65501fbbaf8526f7aab0a1edd028135888590f8e`.
Candidate: `b8adb672d245f9a3cbea9af1d2f634c618aeb25d`.
Super Mario RPG update 1.0.1 / v65536, Apple M5 Max, macOS 27.0.1, Xcode 27.2, Metal 32023.922.4.

These unedited host snapshots and sanitized runtime logs support the RGBA4 source PR. This separate evidence branch contains no game files, keys, shader outputs, extracted textures, saves or binary captures. The source PR contains only its code, tests and documentation.

## Matched three-Goomba comparison

- `smrpg-upstream-65501fbb.png`: freshly built pristine upstream, dark arena.
- `smrpg-rgba4-fix.png`: exact proposed source fix, sunlight restored.
- Same isolated copied checkpoint, arena, three-Goomba formation, camera, 3 Chain / 10% gauge, 19/20 HP and 7/10 FP. Animation phases differ.
- These initial runs used a genuine unchanged-chain 552-shader diagnostic subset with conservative uniform metadata fallbacks; no missing/failed shader or pipeline loads. Runtime durations were 341 s baseline and 259 s candidate, exit 0.
- The corresponding `smrpg-baseline-01-runtime.txt` and `smrpg-candidate-01-runtime.txt` retain diagnostics with local paths sanitized.

## Full-library recheck

After the full current-chain build succeeded, both hosts were checked again using all 82,032 base shaders plus generated variants and matching generated constant-buffer metadata. The original checkpoint was copied separately again.

- `smrpg-upstream-full-paratroopas.png`: baseline, two-Paratroopa outdoor encounter, near-black ground and lower foliage.
- `smrpg-rgba4-full-goombas.png`: committed candidate host, two-Goomba encounter, normal sunlight. One attack completed, eliminating a Goomba; the enemy counterattack reduced HP from 19 to 17.
- These are separate enemy formations and animation phases. They verify the defect and corrected lighting with complete libraries; the primary pair above supplies the matched three-Goomba presentation.
- Runtime durations: 177 s baseline and 224 s candidate. Both exit 0. No missing or failed shader/pipeline loads. Both retain the known unimplemented `SetSubpixelPrecisionBias` and `SetConservativeRasterDilate` diagnostics. The candidate also logs a test-driver `DM_KEYS unknown key name: quit`: `quit` is not a control command; both hosts were subsequently stopped with SIGTERM and exit 0. That line is an operator input error, not a rendering regression.
- `smrpg-upstream-full-runtime.txt` and `smrpg-rgba4-fix-full-runtime.txt` are the sanitized full-library logs.

All runs use `DM_AUDIO=1 DM_AUDIO_MUTE=1`, isolated save/temp/pipeline caches, and headless native Metal snapshots. No audible assessment or frame-rate claim is made. The scope is copied-save load, field movement and these battles; later areas and iOS remain untested.

## Automated checks

`regression-report.md` is the exact harness report against committed candidate `b8adb672`: RPG 82,032/82,032 and 13 Sentinels 751/751 on both sides, no new failures, changed MSL or interface failures. Neither game has a harness smoke route; this report does not claim gameplay boots or FPS measurements. The manual runs are documented above and in the source PR.

`metadata.json` records hashes of the evidence files. Screenshots are byte-identical copies of the host PNGs, with no brightness adjustment or other editing.
