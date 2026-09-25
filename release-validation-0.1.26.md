# SourceVR 0.1.26 validation

The four signed APKs and `SHA256SUMS-0.1.26.txt` are in this directory. The
earlier local 0.1.26 Quest and PICO APKs, with their original checksum file,
are preserved in `.release-build-0.1.26/previous-local-0.1.26/`.

| APK | Game catalog | Bytes |
|---|---|---:|
| `SourceVR-0.1.26.apk` | Seven games, including Entropy : Zero | 772,118,088 |
| `SourceVR-pico-0.1.26.apk` | Seven games, including Entropy : Zero | 772,118,088 |
| `SourceVRCore-0.1.26.apk` | Original six games | 740,904,894 |
| `SourceVRCore-pico-0.1.26.apk` | Original six games | 740,904,894 |

All seven native profiles built and staged. All four APK artifact audits passed
with `--require-current-version`, including the release certificate, 16 KiB
alignment, profile and headset routing, payload hashes, and matching native and
Android build stamps. `scripts/gen_version.py --check` passed after the build.
All APKs have application ID `com.sourcevrport.hl2vr`, Android version code
50301341, engine version `0.5.3+6512c863-dirty`, and build hash
`6512c8637531-dirty-298283153678`. This version code is higher than the
previous local 0.1.26 build's 50201341.

The Quest and PICO comparisons each found 1,313 shared runtime assets with
identical bytes between SourceVR and Core. Core contains no Entropy Zero native
profile, overlay, or cover bitmap. SourceVR contains the official Steam cover,
verified by its pinned SHA-256. The Portal 2 client and server native blobs
have new hashes compared with the earlier local APK; the built server binary
contains `Portal2_UpdateWeightedCubePaintPower` and
`CPropWeightedCube::SetPaintedSkin`. The Portal 2 entity and paint audits passed.

Logs, build metadata, and the catalog comparison are in `.release-build-0.1.26/`.
The SourceVR Quest APK was installed successfully on a Quest 3 and Android
reported version code 50301341. Launch and gameplay have not been verified.
Portal 2 and PICO remain experimental.
