# SourceVR

Native VR ports of Half-Life 2, Episode One, and Episode Two for standalone Meta
Quest headsets. Builds are published on the [Releases](../../releases) page.

The release does not include the retail depots needed to play. You need your own
Steam copy of every game you want to play.

## Requirements

- A Meta Quest headset with Developer Mode enabled (developed and tested on Quest 3).
- Half-Life 2 on Steam's **`steam_legacy`** branch (PatchVersion `8491853`). In
  Steam, open *Half-Life 2 → Properties → Betas*, select `steam_legacy`, and let
  the game update.
- Episode One and/or Episode Two from that same installation if you want to play them.
- Enough headset space for the APK, its staged VR files, and your retail game
  folders. The installer shows the exact staging requirement; keep at least 1 GiB
  of extra free space during installation and updates.

## Install or update

The main APK contains one launcher for all three games and uses the package
`com.sourcevrport.hl2vr`. You can install it with SideQuest, adb, or another APK
sideloading tool. To install or update it with adb:

```sh
adb install -r SourceVRPort-hl2Episodes-<version>.apk
```

Use `-r` for updates so Android preserves your existing app data. Do not uninstall
an existing SourceVRPort app to work around an update or signing error.

### Existing Episode One and Episode Two players

Releases before 0.1.14 installed each episode as a separate Android app. If you
have one of those versions, transfer its saves and settings **before** installing
the unified APK.

> **Do not uninstall the old episode app first.** Uninstalling deletes the only
> app-owned copy of its saves that Android lets the transfer tool read.

For each old episode app you have installed:

1. Update it in place with the matching transfer APK.

   ```sh
   adb install -r SourceVRPort-save-transfer-ep1-<version>.apk
   adb install -r SourceVRPort-save-transfer-ep2-<version>.apk
   ```

2. Open the transfer tool from **Library → Unknown Sources**, grant its requested
   file access, and choose **Copy and verify user data**.
3. Wait for the transfer to report success. If both old episode apps are installed,
   complete both transfers.
4. Install the unified `hl2Episodes` APK with `adb install -r`.
5. Launch each episode and confirm its saves before removing a transfer app.

The transfer APKs contain no game or engine. Fresh installs do not need them.

## Import your game content

On first launch, grant **Allow access to manage all files**. SourceVRPort keeps one
shared copy of your retail folders at `/sdcard/SourceVRPort/common/`:

| Folder | Needed by |
|---|---|
| `hl2` | all three games |
| `platform` | all three games |
| `episodic` | Episode One and Episode Two |
| `ep2` | Episode Two |

Copy those folders from your Steam installation:

- Windows: `C:\Program Files (x86)\Steam\steamapps\common\Half-Life 2\`
- macOS: `~/Library/Application Support/Steam/steamapps/common/Half-Life 2/`

You can copy them to the headset with adb, MTP, SideQuest, or another file-transfer
tool. Then choose **Import a folder…** in SourceVRPort and select an individual game
folder or a parent folder containing several of them. Repeat until every folder
required by the games you want is installed.

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
| Episode One | `/sdcard/SourceVRPort/common/episodic/custom/` |
| Episode Two | `/sdcard/SourceVRPort/common/ep2/custom/` |

A mod in the Half-Life 2 folder is also mounted by both Episodes. Use a game's own
folder when the mod should apply only to that game.

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
