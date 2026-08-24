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

The compiled libretro binary is still named `parallel_n64_libretro.so`, and existing core-option keys retain the `parallel-n64-*` prefix internally. This is intentional for compatibility while the project identity and restoration work move forward under the **Mupen64Plus-ReARMed** name.

## Experimental warning

This module tracks active development. Some games or renderers may still show regressions, visual issues or compatibility problems. It is intended for testing and restoration work, not yet as a drop-in replacement for every N64 setup.
