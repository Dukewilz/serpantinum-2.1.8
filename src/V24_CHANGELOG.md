# v24 / Serpantinum 2.1.8 compatibility 1

Donor: ilyamiro/serpantinum `22f3036ec69cc549c959eab73a40ceb3462aeda1` (2.1.8).
Current-user baseline: Dukewilz/serpantinum-2.1.6 `35fa1a3` and its recorded source history.

## Install

Update Serpantinum using its official installer to **2.1.8 first**. This package is a v24 overlay, not the distribution/dependency updater. It supports leftover v24 files from the supplied repository after that update. It does not rely on the stale src/version.txt label.

```bash
unzip serpantinum-v24-218-compat1.zip
cd serpantinum-v24-218-compat1
bash install.sh --check
bash install.sh --restart
```

If detection selects the wrong runtime, pass the same explicit path to both commands:

```bash
bash install.sh --target "$HOME/.local/share/serpantinum/src" --check
bash install.sh --target "$HOME/.local/share/serpantinum/src" --restart
```

Use an ordinary terminal in the running desktop session. Python 3, bash and QuickShell must be installed. Without `--restart`, installation only writes source; restart Serpantinum yourself. Restart uses process ownership and exact runtime paths, never a global `pkill`. The existing daemon is retained. Startup is checked through the main QuickShell IPC endpoint; this is a load check, not a complete visual check of every popup.

## Rollback

```bash
bash rollback.sh --check
bash rollback.sh --restart
```

Backups live in the target runtime's `.v24-218-state`. Keep this directory until satisfied. Rollback refuses to overwrite files modified after installation. Do not install another version over an active backup if you intend to roll it back later.

## Changes

- Port v24 onto 2.1.8's split Bar settings and native module/time settings. Retain native workspace-count semantics and smart-autohide fixes.
- Keep Inner Pill controls separate from the three CAVA visualizer controls.
- Replace font-size percentage offsets with shared `CenteredIcon`, using the bundled Iosevka font and Qt tight glyph bounds with baseline compensation. Applied to IconButton, ClickButton, FillButton, system quick actions and profile switches. Legacy per-call offsets are ignored in automatic mode to avoid double correction.
- Fix UTF-16 surrogate splitting in profile switches. Supplementary-plane Nerd icons stay intact; labels get separate bounded layout slots.
- Remove the v24 Wallpaper transition controls and their settings writes. WallpaperEngine is byte-identical to the 2.1.8 donor; the picker selects the native fade transition. Existing unused `theme.wallpaperTransition` JSON is ignored, not destructively removed.
- Preserve current/custom UI backgrounds and their opacity/blur/ambient pipeline.
- Guard 91 payload files using Git blob identities from 41 official snapshots (2.1.6–2.1.8), 9 supplied-repository snapshots, and the final payload. Package bytes additionally use SHA-256. Mixed recognized states are accepted per file. Unknown edits are rejected with paths and SHA-256 values before source writes; there is no force switch.
- Do not write Calendar, native Dock, WidgetSync, widget presets, settings.json, compositor configuration or installed daemon. These retain the state left by the official updater. Old unreferenced TaskbarDock/PopupController files are not shipped or activated.

## Validation and limits

- All port QML parsed by Qt 6.11 qmlformat; shell scripts checked with bash -n.
- Actual offscreen Qt rendering: 9 icons at 14/22/32 pixels, 27 combinations, visible bounds centered within 1 pixel.
- Install/check/idempotence/rollback tested on release 91326a6, latest donor 22f3036 and a mixed current-user/2.1.8 tree. Restored source bytes compared exactly, including untouched files.
- Unknown edits rejected before source changes. Startup success/failure paths tested with mocked session lifecycle; failure restores previous source.
- No live Hyprland/NVIDIA session was available. Offscreen rendering verifies the shared glyph component, not the complete desktop. Review TopBar, Guide and profile buttons after installation.
- Upstream revisions after 22f3036 or uncommitted local edits can require a new compatibility check. The installer does not silently accept future changes.

Sources: https://github.com/ilyamiro/serpantinum and https://github.com/Dukewilz/serpantinum-2.1.6 . Qt metrics: https://doc.qt.io/qt-6/qml-qtquick-textmetrics.html . Original project license is included.
