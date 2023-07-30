---
label: mpv
description: Learn how to install, setup, and configure mpv
image: https://user-images.githubusercontent.com/78981416/215125796-08b99128-fe50-4d0c-b0dd-49f8828af0dc.png
---

# mpv

[mpv](https://mpv.io) is a versatile and lightweight media player with extensive customizability. It is used and recommended by most users as it supports a wide range of video file formats, audio and video codecs, and subtitle types. Additionally, it offers many advanced features and is available on multiple platforms.

## Installation

### Windows

==- Installing the Source Binary

1. Download the latest Windows build of [mpv](https://mpv.io) from [SourceForge](https://sourceforge.net/projects/mpv-player-windows/files/):
[!button size="s" variant="primary" icon="download" text="64-bit" margin="0 8 0 0"](https://sourceforge.net/projects/mpv-player-windows/files/64bit/)
[!button size="s" variant="secondary" icon="download" text="32-bit"](https://sourceforge.net/projects/mpv-player-windows/files/32bit/)
2. Once downloaded, extract the archive's contents to your specified location
!!!warning
This folder cannot be changed after installation. If you wish to change it in the future, you will need to uninstall it first.
!!!
3. Navigate to the `installer` folder and run `mpv-install.bat`. Follow the on-screen instructions to complete installation

![Installing mpv](https://files.catbox.moe/ly721g.gif)

==- Installing a Pre-Configured Build

If you don't want to setup mpv yourself, a portable build of mpv is available below. It is pre-configured to have the settings described in the [Basic Config](#basic-config) and [Advanced Config](#advanced-config).

[!file mpv Portable Build](https://mega.nz/folder/11QCTZgR#sdsjUYkIieGjVR09mnpYSw)

==- Installing a Fork

[mpv](https://mpv.io) can also be found in the various forks below:

- [ImPlay](https://github.com/tsl0922/ImPlay)
- [mpv.net](https://github.com/mpvnet-player/mpv.net) [!button size="xs" variant="primary" icon="apps" text="Microsoft Store" target="blank" margin="0 8 0 0"](https://apps.microsoft.com/store/detail/9N64SQZTB3LM)

===

==- Adding mpv to PATH

If you want to access mpv from the command line, you will need to add it to Windows PATH:

1. In *Windows Settings* > *System* > *About*, locate *Advanced System Settings*. Head to *Advanced* and click on *Environment Variables...*
2. Under *System Variables*, select *Path* and click on *Edit...*
3. Click *New* and point the new variable to the `mpv.exe` located where mpv is installed
4. Dismiss all of the dialogs by clicking `OK`

===

### macOS

==- Installing the Source Binary

Download the latest macOS build of [mpv](https://mpv.io):
[!button size="s" variant="primary" icon="download" text="Stable" margin="0 8 0 0"](ttps://laboratory.stolendata.net/~djinn/mpv_osx/)
[!button size="s" variant="secondary" icon="download" text="Nightly"](https://github.com/jnozsc/mpv-nightly-build)

==- Installing with Homebrew

Install [Homebrew](https://brew.sh/). Then, run the following command in your terminal:

```sh
brew install mpv
```

===

### Linux

==- Debian (APT)

Run the following command in your terminal:

```sh
apt install mpv
```

===

## Config Overview

By default, mpv's config can be found under `%APPDATA%/mpv/`. However, a folder named `portable_config` next to where `mpv.exe` is stored can override this location as the root folder.

+++ Default Root (`%APPDATA%`)

```properties
.
└── mpv/
    ├── fonts/
    ├── script-opts/
    ├── scripts/
    ├── shaders/
    ├── input.conf
    └── mpv.conf
```

+++ Modified Root (`portable_config`)

```properties
.
├── mpv.exe
└── portable_config/
    ├── fonts/
    ├── script-opts/
    ├── scripts/
    ├── shaders/
    ├── input.conf
    └── mpv.conf
```

+++

File           | Meaning
---------------|---------------------------------------------------------------------------------------------------------------------------
`fonts/`       | Directory for custom font files. *These are typically used for subtitles*
`scripts/`     | Directory for [custom scripts](#custom-scripts). *These are usually `.lua` scripts*
`shaders/`     | Directory for custom shaders used in [scaling](/guides/playback/#scaling), [filtering](/guides/playback/#filtering), etc.
`script-opts/` | Directory used by custom scripts to store additional options/configs
`mpv.conf`     | General user configuration for mpv
`input.conf`   | Keybind settings for mpv

Some folders may not be present by default and will need to be created.

*For more details, see [mpv's documentation on files](https://mpv.io/manual/master/#files).*

## Basic Config

mpv is a great player out-of-the-box which can be extensively customized to your liking.

We recommend taking your time to create your own config. If you want to get up and running quickly, we suggest using the generic `mpv.conf` config below:

```properties
## Video

profile=gpu-hq
vo=gpu-next
gpu-api=vulkan

## Behavior

keep-open=yes
save-position-on-quit

## Scaler

# If you encounter problems using ewa_lanczos, change to spline36
scale=ewa_lanczos
dscale=mitchell
cscale=ewa_lanczos

## Screenshots

screenshot-format=png
screenshot-high-bit-depth=no
screenshot-png-compression=9
screenshot-directory="~/Pictures/mpv"
screenshot-template="%F-%p"

## Language Priority

# Sub
# Add enm before eng for honorifics
slang=eng,en
alang=jpn,ja

# Dub
# Uncomment this section to prefer English dub with subtitles for English dub
#slang=zxx,eng,en
#alang=eng,en
#subs-with-matching-audio=no
```

==- Understanding the Config

!!!
See [mpv's user manual](https://mpv.io/manual/stable) for a detailed explanation of all the options.
!!!

Option                                                                                           | Meaning
-------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
[`profile`](https://mpv.io/manual/stable/#profiles)                                              | The profile to be used by mpv. *We recommend `gpu-hq` for high-quality playback*
[`vo`](https://mpv.io/manual/stable/#video-output-drivers)                                       | The output driver to be used by mpv. *We recommend `gpu-next` for most modern hardware*
[`gpu-api`](https://mpv.io/manual/stable/#options-gpu-api)                                       | The graphics API to be used by mpv. *We recommend `vulkan` for high-end hardware*
[`keep-open`](https://mpv.io/manual/stable/#options-keep-open)                                   | Whether to close or leave the player open after the file finishes playing. *Use `no` if you want the player to close*
[`save-position-on-quit`](https://mpv.io/manual/stable/#resuming-playback)                       | Save the current playback position on quit. When the file is reopened, mpv will resume from where it left off. *Remove this option if you do not want the player to save your position*
[`scale`](https://mpv.io/manual/stable/#options-scale)                                           | The [upscale](#scaling) filter. *We recommend `ewa_lanczos` for best quality*
[`dscale`](https://mpv.io/manual/stable/#options-dscale)                                         | The [downscale](#scaling) filter. *We recommend `mitchell` for best quality*
[`cscale`](https://mpv.io/manual/stable/#options-cscale)                                         | The [scale](#scaling) for interpolating chroma information. *We recommend `ewa_lanczos` for best quality*
[`screenshot-format`](https://mpv.io/manual/stable/#options-screenshot-format)                   | File format used for screenshots. *We recommend `png` for lossless quality*
[`screenshot-high-bit-depth`](https://mpv.io/manual/stable/#options-screenshot-high-bit-depth)   | Use a bit depth similar to the source for screenshots. *Leave this at `no` as it creates unnecessarily large files*
[`screenshot-png-compression`](https://mpv.io/manual/stable/#options-screenshot-png-compression) | The compression level for `.png` screenshots. *Can be set between `0` to `9`; a higher number means better compression and longer output time*
[`screenshot-directory`](https://mpv.io/manual/stable/#options-screenshot-directory)             | The directory to save screenshots to. *Currently set to your user's `Pictures/mpv/` folder*
[`screenshot-template`](https://mpv.io/manual/stable/#options-screenshot-template)               | The naming scheme for screenshots.
[`slang`](https://mpv.io/manual/stable/#options-slang)                                           | Priority list of subtitle languages to use when there are multiple tracks
[`alang`](https://mpv.io/manual/stable/#options-alang)                                           | Priority list of audio languages to use when there are multiple tracks
[`subs-with-matching-audio`](https://mpv.io/manual/stable/#options-subs-with-matching-audio)     | Determines whether the subtitle and audio track must match their language. *Use `no` if you want to watch dubs with subtitle track*

===

## Advanced Config

mpv can be fine-tuned to meet your specific needs, from tweaking playback behavior to customizing video, audio, and subtitle settings. This section outlines the common options that can improve your experience.

!!!
This guide assumes you know the root folder for your mpv config. *See [Config Overview](#config-overview) to see where to locate your config files.*
!!!

### Debanding

Color banding is a visual artifact that is typically seen in gradients, where the colors can be easily differentiated by the human eye. *See [Tom Scott's video explaining color banding](https://youtu.be/h9j89L8eQQk).*

![Banding (left) vs. No banding (right)](https://user-images.githubusercontent.com/78981416/214381256-e5722886-57d7-4cd4-834e-edbd30b432e0.png)

To enable debanding in mpv, apply the following changes to your config:

+++ `mpv.conf`

```properties
## Deband

# Set deband to "no" as we only need to enable it for specific cases
# If you wish to apply debanding automatically on startup, set it to "yes" (not recommended)
deband=no
deband-iterations=4
deband-threshold=48
deband-grain=48
```

+++ `input.conf`

```properties
D cycle deband
```

+++

After applying your changes, debanding can be applied at any time by pressing `Shift` + `D`.

### Scaling

Scaling is the process of taking content that does not match your screen resolution and resizing it to fit your display. *See the [Playback Guide](/guides/playback/#scaling) for more information.*

!!!
Scalers only work when the resolution of your video does not match your display. They do not activate if the content resolution already matches your display resolution.
!!!

+++ High-End PCs
For those with high-end hardware, we recommend using [nnedi3-nns256-win8x4](https://github.com/bjin/mpv-prescalers/blob/master/nnedi3-nns256-win8x4.hook).

Download the shader file and place it in your `shaders` folder.

To use the shader, add the following to your `mpv.conf`:

```properties
glsl-shaders="~~/shaders/nnedi3-nns256-win8x4.hook"
```

To activate it with a key, add the following to your `input.conf`, replacing `G` with the bind of your choice, if necessary (case-sensitive):

```properties
G change-list glsl-shaders toggle "~~/shaders/nnedi3-nns256-win8x4.hook"
```

+++ Mid-Range PCs
For those with mid-range hardware, we recommend using [nnedi3-nns128-win8x4](https://github.com/bjin/mpv-prescalers/blob/master/nnedi3-nns128-win8x4.hook).

Download the shader file and place it in your `shaders` folder.

To use the shader, add the following to your `mpv.conf`:

```properties
glsl-shaders="~~/shaders/nnedi3-nns128-win8x4.hook"
```

To activate it with a key, add the following to your `input.conf`, replacing `G` with the bind of your choice, if necessary (case-sensitive):

```properties
G change-list glsl-shaders toggle "~~/shaders/nnedi3-nns128-win8x4.hook"
```

+++ Low-End PCs
For those with low-end hardware, we recommend sticking to mpv's built-in scalers.

In your `mpv.conf`, add the following:

```properties
vo=gpu-next
scale=ewa_lanczos
dscale=mitchell
cscale=ewa_lanczos
```

This is included in the [Basic Config](#basic-config).

If these are too heavy for your system, delete these lines from your config. mpv will use `spline36` by default with `profile=gpu-hq`.
+++

### Subtitle Restyling

Most releases will use their own font for `.ass` subtitles. These can be manually overridden by mpv, which can help improve readability or match personal preferences.

Below are a few commonly used styles:

+++ Gandhi Sans

![Gandhi Sans](https://user-images.githubusercontent.com/78981416/248583226-18cece1d-4cd6-4a55-8fbf-8fa587399262.png)

Download this font from [Font Meme](https://fontmeme.com/fonts/gandhi-sans-font/), or use the button below:

[!button icon="download" variant="primary" text="Gandhi Sans"](https://fontmeme.com/fonts/download/305566/gandhi-sans.zip)

Run the `.otf` font file to install it system-wide or put it in your `fonts` folder. Add the following to your `mpv.conf`:

```properties
## Restyle Subtitles

# Set sub-ass-override to "no" as we only need to enable it for specific cases
# If you wish to use it for all video, set it to "yes"
sub-ass-override=no
sub-ass-force-style=playresx=1920,playresy=1080
sub-font="Gandhi Sans"
sub-font-size=50
sub-color="#FFFFFF"
sub-border-size=2.4
sub-border-color="#FF000000"
sub-shadow-color="#A0000000"
sub-shadow-offset=0.75
sub-bold=yes
sub-ass-force-style=Kerning=yes
```

+++ Cabin

![Cabin](https://user-images.githubusercontent.com/78981416/248583220-ac376f74-ce9f-4c3c-800b-370355a841f0.png)

Download this font from [Font Meme](https://fontmeme.com/fonts/cabin-font/), or use the button below:

[!button icon="download" variant="primary" text="Cabin"](https://fontmeme.com/fonts/download/25391/cabin.zip)

Run the `.ttf` font file to install it system-wide or put it in your `fonts` folder. Add the following to your `mpv.conf`:

```properties
## Restyle Subtitles

# Set sub-ass-override to "no" as we only need to enable it for specific cases
# If you wish to use it for all video, set it to "yes"
sub-ass-override=no
sub-ass-force-style=playresx=1920,playresy=1080
sub-font="Cabin"
sub-font-size=50
sub-color="#FFFFFFFF"
sub-border-size=2.4
sub-border-color="#FF000000"
sub-shadow-color="#A0000000"
sub-shadow-offset=0.8
sub-ass-force-style=Kerning=yes
```

+++

To activate it with a key, add the following to your `input.conf`, replacing `k` with the bind of your choice, if necessary (case-sensitive):

```properties
k cycle_values sub-ass-override "force" "no"
```

### Auto Profiles

Auto profiles allow users to automate actions based on certain conditions. Tasks such as [debanding](#debanding) and [subtitle restyling](#subtitle-restyling) can be applied automatically when these conditions are met, such as file names.

For instance, some seasonal releases may exhibit banding issues and less-appealing subtitles. To address this, we can create a `simulcast` auto profile, which will automatically apply debanding and restyle the subtitles when the filename includes specific keywords, such as the group tag.

Add the following to the end of your `mpv.conf`:

```properties
## Auto profiles

[simulcast]
profile-cond=string.match(p.filename, "SubsPlease") or string.match(p.filename, "Erai%-raws") or string.match(p.filename, "Tsundere%-Raws") or string.match(p.filename, "%-VARYG") or string.match(p.filename, "HorribleSubs")
profile-restore=copy
sub-fix-timing=yes
sub-ass-override=force
deband=yes
```

In this example, the `simulcast` auto profile will automatically apply when you play a release created by SubsPlease, Erai-raws, Tsundere-Raws, VARYG, or HorribleSubs.

### Quality of Life

#### Volume

By default, scrolling the mouse wheel up and down seek the video instead of changing the volume. This behavior can be changed by adding the following to your `input.conf`:

```properties
WHEEL_UP      add volume 2
WHEEL_DOWN    add volume -2
```

### Custom Scripts

mpv allows you to load custom scripts, allowing you to further expand the player's functionality.

Below is a list of some popular scripts:

- [autoload](https://github.com/mpv-player/mpv/blob/master/TOOLS/lua/autoload.lua) - Automatically adds all files present in the folder to a playlist
- [mpv-playlistmanager](https://github.com/jonniek/mpv-playlistmanager) - Script to create and manage playlists
- [mpv-webm](https://github.com/ekisu/mpv-webm) - WebM maker for mpv
- [pause-when-minimize](https://github.com/mpv-player/mpv/blob/master/TOOLS/lua/pause-when-minimize.lua) - Pauses playback when minimizing the window, and resumes playback when brought back
- [thumbfast](https://github.com/po5/thumbfast) - Display thumbnails when scrubbing video (may be needed for some [Skins](#skins))
- [trackselect](https://github.com/po5/trackselect) - Select tracks based on their title

### Skins

You can customize how mpv looks using skins. These are subject to personal preference, so find what works best for you.

Below is a list of some popular skins:

- [mfpbar](https://codeberg.org/NRK/mpv-toolbox/src/branch/master/mfpbar)
- [ModernX](https://github.com/cyl0/ModernX)
- [mpv-osc-modern](https://github.com/maoiscat/mpv-osc-modern)
- [mpv-osc-tethys](https://github.com/Zren/mpv-osc-tethys)
- [mpv-progressbar](https://github.com/torque/mpv-progressbar)
- [oscc](https://github.com/longtermfree/oscc)
- [uosc](https://github.com/tomasklaen/uosc)
