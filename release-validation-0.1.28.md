# SourceVR 0.1.28 release validation

Prepared October 2, 2026 for [SourceVR 0.1.28](https://github.com/tinsarfal/SourceVR/releases/tag/0.1.28).

Source commit: `d6c0a068426cdb9baed0106816e826e688bdd625`. Engine version: `0.5.5+d6c0a068`. Build identity: `d6c0a068426c`. Android versionCode: `50501348`, higher than the published 0.1.27 code `50401346`.

The changelog retains the October 1 draft and includes the subsequent committed changes. Its baseline is the source embedded in the published 0.1.27 APK, `63316332ff03d1b6442e45bd2f48035b3202e2bf`. That APK's SHA-256 matches GitHub, and its source tree matches the local `sourcevr-0.1.27` tag.

- All seven native game profiles built successfully with optimized Android arm64 release flags. Native build identity, dependency closure and authored asset staging passed for each profile.
- Quest/PICO Full/Core APKs passed the release audits for public release metadata `0.1.28`, current build identity, the expected game catalog, native dependencies, 16 KiB alignment and the pinned production signing certificate. The three legacy save-transfer APKs were also built and signer-audited.
- Launcher tests: 179 passed, with three optional live-network tests skipped. Importer tests: 244 passed. The local Portal retail menu-map fixtures ran with the configured Steam installations.
- Versioning and packaging tests: 88 passed, plus 30 subtests. Shared storage access tests: 11 passed.
- All 36 focused Python regression runs and eight standalone C++ targets passed, including gravity glove gestures and catching, outline color and rendering state, settings migration, portal crossings and carried-object targets, camera/bridge physics, audio, map-loading and decal checks. Relevant harnesses also exercised optimized fast-math and sanitizer builds. The entire unrelated host suite was not run.
- All four APKs were checked for macOS Finder metadata; none contains it. Their SHA-256 checksums are published with the release.
- 190 original native libraries and their checksums are archived across all seven profiles for crash symbolization, alongside source differences, build configurations, audit logs and test receipts. Matching unstripped Android bootstrap and JNI bridge symbols are also archived.

The carried-object target harness retains its original production bodies, asserted conditions and negative control; expected C++ assertion failures use `exit(1)` to avoid macOS Crash Reporter suspension. Its release-only wrapper's import path was corrected, and the final check passed separately.

Fresh headset installation/gameplay testing and PICO hardware testing were not performed: no headset was connected during this preparation. Earlier device evidence for the outline and Portal 2 crossing fixes is preserved with the local source reports and build records.
