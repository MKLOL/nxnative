# CI readiness checks at `2f5b0e12`

The prior targeted six-translation-unit lint check missed findings in fixtures that include the shared Objective-C implementation. This follow-up checks the same full 217-translation-unit Wonder compilation database as GitHub, using LLVM 23.1.4 and Xcode 27.2.

The fixtures now check allocations and release their serial-hash registrations on success and failure paths. Two narrow ownership annotations explain analyzer false positives after the actual cleanup. Assertions are unchanged. The supported `lint_c.py tidy --update --against 65501fbb` operation only reduces the warning baseline from 271 to 268: `nvn_mtl.m` NullDereference 11→9 and `t_rt_range.m` RetainCount 1→0. No checks or workflows are disabled, and no baseline count increases.

- [Full lint scope and ratchet result](clang-tidy-scope.txt)
- [Full diagnostics](clang-tidy-diagnostics.txt)
- [Other check results](check-summary.txt)
- [Exact-candidate shader/interface harness report](regression-report.md)
- [File hashes and source revisions](metadata.json)

These are local checks. GitHub checks must finish independently before the draft is opened for review. All four runtime core files match the previously checked `1fac3312` candidate byte for byte; the [historical gameplay evidence](../fixes-1fac3312/README.md) remains labeled with that revision. There are no new gameplay, performance, audio, later-area or iOS claims.
