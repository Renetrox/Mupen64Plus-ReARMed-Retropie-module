# Mupen64Plus-ReARMed RetroPie module

Experimental RetroPie-Setup scriptmodule for **Mupen64Plus-ReARMed**.

This module installs the current ReARMed development branch and registers it in RetroPie as `lr-mupen64plus-rearmed`.

## Current status

Mupen64Plus-ReARMed is still in an experimental restoration phase. The goal is to recover the fast legacy rendering paths and configuration options that became neglected or lost over time, especially for modest ARM hardware and older PCs.

At the current stage:

- **Glide64 is the default renderer** used by this RetroPie module.
- **Rice remains available** from RetroArch Core Options.
- **Frameskip is functional with Glide64 and Rice.**
- The renderer-independent frameskip option is installed **disabled by default**, so users can enable it per game when useful.
- The native legacy `gles2n64` frameskip option is also available and disabled by default.
- Compatibility and renderer restoration work are still ongoing.

Main core repository:

https://github.com/Renetrox/Mupen64Plus-ReARMed

## Installation on RetroPie

The scriptmodule must be copied into RetroPie-Setup's `scriptmodules/libretrocores` directory.

```bash
cd /tmp
git clone https://github.com/Renetrox/Mupen64Plus-ReARMed-Retropie-module.git
sudo cp Mupen64Plus-ReARMed-Retropie-module/lr-mupen64plus-rearmed.sh \
  ~/RetroPie-Setup/scriptmodules/libretrocores/
sudo chmod +x ~/RetroPie-Setup/scriptmodules/libretrocores/lr-mupen64plus-rearmed.sh
```

Then launch RetroPie-Setup:

```bash
cd ~/RetroPie-Setup
sudo ./retropie_setup.sh
```

Go to:

**Manage packages → Manage experimental packages → lr-mupen64plus-rearmed → Install from source**

After installation, `lr-mupen64plus-rearmed` will be added as an N64 emulator in RetroPie/runcommand.

## ROMs

Place Nintendo 64 ROMs in:

```text
~/RetroPie/roms/n64
```

Supported cartridge extensions include:

```text
.z64 .n64 .v64
```

## Default core options set by the module

```text
GFX Plugin: Glide64
GFX Plugin Accuracy: Low
Screen Size: 640x480
Frame Skip: Disabled
gles2n64 Frame Skip: Disabled
```

Frameskip can then be enabled from RetroArch Core Options when needed. Rice can also be selected there instead of Glide64.

## Notes

The installer accepts both binary names during the transition:

- Current `frameskip-modern` branch: `parallel_n64_libretro.so`.
- Proposed updated core in [Mupen64Plus-ReARMed PR #1](https://github.com/Renetrox/Mupen64Plus-ReARMed/pull/1): `mupen64plus_rearmed_libretro.so`.

It installs **only the binary produced by the checked-out source**, never renames it over a stock ParaLLEl-N64 core. After a successful installation of the new `mupen64plus_rearmed_libretro.so`, the script automatically removes an existing `parallel_n64_libretro.so` **only inside this module's own install directory**. It leaves the old file alone when the new binary has not been installed, during removal, or under the separate official ParaLLEl-N64 module directory. The existing `parallel-n64-*` RetroArch core-option keys remain unchanged for compatibility with saved settings. Do not rename the core-option keys just to match the binary filename.

The script preserves pre-existing `$biosdir/gles2n64rom.conf` content and only supplies its 28 ROM presets when the file does not exist. The gles2n64 frameskip default is **`0`** (disabled); `disabled` is not a valid value for that renderer's native frameskip.

This repository is the **canonical RetroPie installer** for ReARMed. The standalone `mupen64plus-frameskip` module in RetroPie-Extra is a different emulator and must not be overwritten or removed.

## Experimental warning

This module tracks active development. Some games or renderers may still show regressions, visual issues or compatibility problems. It is intended for testing and restoration work, not yet as a drop-in replacement for every N64 setup.

## Save states

The core implements manual save/load states. Configure installs metadata for
`mupen64plus_rearmed_libretro.so` into RetroPie's standard core-info directory,
`/opt/retropie/configs/all/retroarch/cores`, and requests an info-cache refresh.
It declares only `basic` support; rewind, run-ahead and deterministic netplay
are not promised. The stock `parallel_n64_libretro.info` is left untouched.
Run the module's **Configuration / options** step after updating this script;
recompiling the core is unnecessary for this metadata change.

If RetroArch uses a custom **Settings > Directory > Core Info** location, copy
`mupen64plus_rearmed_libretro.info` from the module installation directory into
that location and restart RetroArch. An update of RetroArch's core-info checkout
can remove locally added info files; rerun this module's configuration afterwards.

If the menu is still missing, test with a game running:

1. Under Settings > Core, temporarily enable **Bypass Core Info Save States
   Features** (`core_info_savestate_bypass`). Advanced settings may need enabling.
   Reopen the Quick Menu. If Save State appears, metadata/cache detection is the
   blocker. Turn the bypass off after this test; do not save it globally.
2. If it remains hidden, check Quick Menu visibility, core/game overrides and
   achievement restrictions. These are separate from core serialization.
3. With a keyboard, try F2 (save) and F4 (load), subject to your configured
   hotkey-enable requirement. Test on a spare state slot. Record the notification
   or verbose RetroArch log if it fails.

The metadata correction has not been validated by saving/loading a game on the
target Orange Pi. It does not establish the cause of every missing-menu report.

FreeJ2ME-Plus is different: its libretro serialization size is zero and both
serialization functions return false. Do not apply a `savestate = "true"`
metadata workaround to it. Its normal in-game RMS saves remain available;
adding arbitrary-point states requires a Java runtime snapshot implementation.
