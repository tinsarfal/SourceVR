# SourceVR

Native VR ports of Half-Life 2, Portal, Portal 2, Episode One, and Episode Two for
standalone Meta Quest and PICO headsets. Builds are published on the
[Releases](../../releases) page.

The release does not include the retail depots needed to play. You need your own
Steam copy of every game you want to play.

> **Portal 2 is highly experimental and is not ready for standard gameplay.**
> Treat it as an early test build, not as a campaign-ready release.

> **PICO support is experimental, untested, and may not work.**

## Requirements

- A Meta Quest or PICO headset with Developer Mode enabled (developed and tested on Quest 3).
- Half-Life 2 on Steam's **`steam_legacy`** branch (PatchVersion `8491853`). In
  Steam, open *Half-Life 2 → Properties → Betas*, select `steam_legacy`, and let
  the game update. These files are also required by Portal, Episode One, and
  Episode Two.
- Portal on Steam if you want to play Portal.
- Portal 2 on Steam if you want to test the experimental Portal 2 support.
- Episode One and/or Episode Two from the `steam_legacy` Half-Life 2 installation
  if you want to play them.
- Enough headset space for the APK, its staged VR files, and your retail game
  folders. The installer shows the exact staging requirement; keep at least 1 GiB
  of extra free space during installation and updates.

## Install or update

The upcoming 0.1.21 release is still in preparation. See the
[draft changelog](release-notes-0.1.21.md).

The main APK contains one launcher for all five games and uses the package
`com.sourcevrport.hl2vr`. You can install it with SideQuest, adb, or another APK
sideloading tool. To install or update it with adb:

```sh
adb install -r SourceVR-<version>.apk
```

Use `-r` for updates so Android preserves your existing app data. Do not uninstall
an existing SourceVRPort app to work around an update or signing error.

### Existing Portal, Episode One, and Episode Two players

Older releases installed Portal and each episode as separate Android apps. If you
have one of those apps, transfer its saves and settings **before** installing the
unified APK.

> **Do not uninstall the old app first.** Uninstalling deletes the only app-owned
> copy of its saves that Android lets the transfer tool read.

For each old Portal or episode app you have installed:

1. Update it in place with the matching transfer APK.

   ```sh
   adb install -r SourceVRPort-save-transfer-portal-<version>.apk
   adb install -r SourceVRPort-save-transfer-ep1-<version>.apk
   adb install -r SourceVRPort-save-transfer-ep2-<version>.apk
   ```

2. Open the transfer tool from **Library → Unknown Sources**, grant its requested
   file access, and choose **Copy and verify user data**.
3. Wait for the transfer to report success. If you have more than one old app,
   complete each transfer.
4. Install the unified `SourceVR` APK with `adb install -r`.
5. Launch each transferred game and confirm its saves before removing a transfer
   app.

The transfer APKs contain no game or engine. Fresh installs do not need them.

## Import your game content

On first launch, grant **Allow access to manage all files**. SourceVRPort keeps one
shared copy of your retail folders at `/sdcard/SourceVRPort/common/`:

| Folder | Needed by |
|---|---|
| `hl2` | Half-Life 2, Portal, Episode One, and Episode Two |
| `platform` | Half-Life 2, Portal, Episode One, and Episode Two |
| `portal` | Portal |
| `episodic` | Episode One and Episode Two |
| `ep2` | Episode Two |
| `update` | Portal 2 |
| `portal2_dlc2` | Portal 2 |
| `portal2_dlc1` | Portal 2 |
| `portal2` | Portal 2 |
| `portal2_platform` | Portal 2 (imported from its `platform` folder) |

You can copy folders to the headset with adb, MTP, SideQuest, or another
file-transfer tool. Then choose **Import a folder…** in SourceVRPort and select an
individual game folder or a parent folder containing several of them. Repeat
until every folder required by the games you want is installed.

### Half-Life 2 and the Episodes

Copy the required folders from your `steam_legacy` Half-Life 2 installation:

- Windows: `C:\Program Files (x86)\Steam\steamapps\common\Half-Life 2\`
- macOS: `~/Library/Application Support/Steam/steamapps/common/Half-Life 2/`

Import `hl2` and `platform` for Half-Life 2. Also import `episodic` for Episode One,
and import both `episodic` and `ep2` for Episode Two.

### Portal

Portal needs folders from two different Steam installations:

1. Import `hl2` and `platform` from the **`steam_legacy` Half-Life 2** installation
   above. Do not substitute the same-named folders from Portal's installation.
2. Import `portal` from your Portal installation:

   - Windows: `C:\Program Files (x86)\Steam\steamapps\common\Portal\`
   - macOS: `~/Library/Application Support/Steam/steamapps/common/Portal/`

Once all three folders are installed, select Portal in the launcher and wait for
its content check to report ready.

### Portal 2 (highly experimental)

> Portal 2 is not ready for standard gameplay. Install it only if you want to
> test the current experimental support.

1. Find your Portal 2 installation:

   - Windows: `C:\Program Files (x86)\Steam\steamapps\common\Portal 2\`
   - macOS: `~/Library/Application Support/Steam/steamapps/common/Portal 2/`

2. Copy `update`, `portal2_dlc2`, `portal2_dlc1`, `portal2`, and `platform` from
   that installation to the headset.
3. Choose **Import a folder…** and select the Portal 2 parent folder to import all
   five folders together, or import them one at a time. SourceVRPort stores Portal
   2's `platform` folder as `portal2_platform` so it cannot overwrite the
   `steam_legacy` Half-Life 2 `platform` folder.

The importer verifies files before activating them and preserves saves, settings,
installed mods, and generated data when retail content is re-imported. A complete
installation from another Steam build can be allowed per game; modified or damaged
content can also be force-launched if you explicitly accept the warning.

## Install content mods

SourceVRPort can mount content-only mods containing maps, materials, models, scripts,
sounds, or VPK archives. Transfer each mod from your computer into the appropriate
public `custom` folder:

| Game | Mod folder on the headset |
|---|---|
| Half-Life 2 | `/sdcard/SourceVRPort/common/hl2/custom/` |
| Portal | `/sdcard/SourceVRPort/common/portal/custom/` |
| Portal 2 (experimental) | `/sdcard/SourceVRPort/common/portal2/custom/` |
| Episode One | `/sdcard/SourceVRPort/common/episodic/custom/` |
| Episode Two | `/sdcard/SourceVRPort/common/ep2/custom/` |

A mod in the Half-Life 2 folder is also mounted by Portal and both Episodes. Use a
game's own folder when the mod should apply only to that game. Portal 2 uses only
its own folder from this table.

**Copy the enclosing mod folder, not its contents directly into `custom`.** Each
immediate child of `custom` must be one complete mod folder or one VPK:

```text
# Correct
common/hl2/custom/MyMod/materials/...
common/hl2/custom/MyMod/models/...
common/hl2/custom/MyMod/scripts/...
common/hl2/custom/MyMod/sound/...

# Wrong: these folders are one level too shallow and will be skipped
common/hl2/custom/materials/...
common/hl2/custom/models/...
common/hl2/custom/scripts/...
common/hl2/custom/sound/...
```

For example:

```sh
adb shell mkdir -p /sdcard/SourceVRPort/common/hl2/custom
adb push "/path/to/MyMod" /sdcard/SourceVRPort/common/hl2/custom/MyMod
```

For a Steam Workshop download, copy the entire numbered item folder so
`content_dir.vpk` and all `content_000.vpk`, `content_001.vpk`, … parts stay together
inside one folder.

Mods that include their own `client.dll` or `server.dll` game code are not supported.

## Play

Open SourceVRPort from **Library → Unknown Sources**, choose a game, and press
**Play** once its content check reports ready.
