---
label: mpv
description: Learn how to install, setup, and configure mpv
image: https://user-images.githubusercontent.com/78981416/215125796-08b99128-fe50-4d0c-b0dd-49f8828af0dc.png
---

# mpv

[mpv](https://mpv.io) is a versatile and lightweight media player with extensive customizability. It is used and recommended by most enthusiasts as it supports a wide range of video, audio, and subtitle formats. Additionally, it offers many advanced features and is available on multiple platforms.

## Installation

### Windows

==- 🔧 Installing mpv

1. Download the latest Windows build of [mpv](https://mpv.io) from [SourceForge](https://sourceforge.net/projects/mpv-player-windows/files/):
[!button size="s" variant="primary" icon="download" text="64-bit" margin="0 8 0 0"](https://sourceforge.net/projects/mpv-player-windows/files/64bit/)
[!button size="s" variant="secondary" icon="download" text="32-bit"](https://sourceforge.net/projects/mpv-player-windows/files/32bit/)
2. Once downloaded, extract the archive's contents to your specified location
!!!warning
This folder cannot be changed after installation. If you wish to change it in the future, you will need to uninstall it first.
!!!
3. Navigate to the `installer` folder and run `mpv-install.bat`. Follow the on-screen instructions to complete installation

[!embed text="Installing mpv on Windows"](/static/playback/mpv/installation-windows.mp4)

==- 📦 Installing mpv via scoop

[Scoop](https://scoop.sh) is a command line package manager for Windows. We can use it to install and manage mpv. Scoop downloads and manages packages in a portable way, keeping them neatly isolated in `C:\Users\username\scoop` and automatically adds them to your PATH.

+++ Installing scoop

    Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
    irm get.scoop.sh | iex

+++ Installing mpv

    scoop bucket add extras
    scoop install extras/mpv-git

+++ Updating mpv

    scoop update mpv-git

+++ Uninstalling mpv

    scoop uninstall mpv-git

+++

==- 🍴 Installing a fork

[mpv](https://mpv.io) can also be found in the various forks below:

- [ImPlay](https://github.com/tsl0922/ImPlay)
- [mpv.net](https://github.com/mpvnet-player/mpv.net) [!badge icon="apps" variant="info" text="Microsoft Store"](https://apps.microsoft.com/store/detail/9N64SQZTB3LM)

==-

==- 📁 Adding mpv to PATH

If you want to access mpv from the command line, you will need to add it to Windows PATH:

1. In *Windows Settings* -> *System* -> *About*, locate *Advanced System Settings*. Head to *Advanced* and click on *Environment Variables...*
2. Under *System Variables*, select *Path* and click on *Edit...*
3. Click *New* and point the new variable to the `mpv.exe` located where mpv is installed
4. Dismiss all of the dialogs by clicking `OK`

==-

### macOS

==- 🔧 Installing mpv

Download the latest macOS build of [mpv](https://mpv.io):
[!button size="s" variant="primary" icon="download" text="Stable" margin="0 8 0 0"](https://laboratory.stolendata.net/~djinn/mpv_osx/)
[!button size="s" variant="secondary" icon="download" text="Nightly"](https://github.com/jnozsc/mpv-nightly-build)

==- 🍺 Installing with Homebrew

Install [Homebrew](https://brew.sh/). Then, run the following command in your terminal:

```sh
brew install mpv
```

==-

!!!warning
[IINA](https://iina.io) is not recommended as it can display incorrect colors. We suggest sticking with the stock [mpv](https://mpv.io) player.
!!!

### Linux

Distributions usually package outdated, unmaintained, and unsupported versions of mpv. This is especially true for popular distros like Debian and Ubuntu. You are recommended to use mpv-build or third-party packages instead.

==- 🔧 Installing on various distributions

All of these packages are unofficial:

- [Arch (official package)](https://archlinux.org/packages/extra/x86_64/mpv/)
- [Gentoo (official package)](https://packages.gentoo.org/packages/media-video/mpv)
- [Arch (AUR, git package)](https://aur.archlinux.org/packages/mpv-git/)
- [Arch (AUR, mpv-build package)](https://aur.archlinux.org/packages/mpv-build-git/)
- [Debian multimedia](https://deb-multimedia.org/dists/testing/main/binary-amd64/package/mpv)
- [Ubuntu and Debian (apt repository)](https://fruit.je/apt)

==-

## Config Overview

By default, mpv's config can be found under `%APPDATA%/mpv/`. However, a folder named `portable_config` next to where `mpv.exe` is stored can override this location as the config folder.

+++ Default (`%APPDATA%`)

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

+++ Portable (`portable_config`)

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

+++ Scoop (`~/scoop/persist/mpv-git/portable_config`)

```properties

C:/Users/username/scoop/persist/mpv-git/
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

==- :icon-file-code: Generic `mpv.conf`

```properties
## Video
profile=high-quality
vo=gpu-next
gpu-api=vulkan
scale-antiring=0.5
deband=no

# Dither
# This must be set to match your monitor's bit depth
dither-depth = 8

## Behavior (personal preference)
keep-open=yes
save-position-on-quit

## Screenshots
screenshot-format=png
screenshot-dir="~/Pictures/mpv"
screenshot-template="%F-%p-%n"

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

==- :icon-info: Understanding the config

!!!
See [mpv's user manual](https://mpv.io/manual/stable) for a detailed explanation of all the options.
!!!

Option                                                                                           | Meaning
-------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
[`profile`](https://mpv.io/manual/master/#profiles)                                              | The profile to be used by mpv. This should be left at the top of your file avoid conflict with other settings.
[`vo`](https://mpv.io/manual/master/#video-output-drivers)                                       | The output driver to be used by mpv. *`gpu-next` is recommended for most modern hardware*
[`gpu-api`](https://mpv.io/manual/master/#options-gpu-api)                                       | The graphics API to be used by mpv. *`vulkan` is recommended for most modern hardware*
[`scale-antiring`](https://mpv.io/manual/master/#options-scale-antiring)                         | Sets the strength of the antiringing filter. *We recommend not setting too high of a value to prevent unwanted artifacts*
[`deband`](https://mpv.io/manual/master/#options-deband)                                         | Toggles [debanding](#debanding). *`profile=high-quality` enables deband by default and is manually disabled in the config. We recommend enabling it manually or using [auto-profiles](#auto-profiles) when needed*
[`dither-depth`](https://mpv.io/manual/master/#options-dither-depth)                             | Sets the dither depth. *This should be set to your monitor's bit depth to prevent [banding](#debanding)*
[`keep-open`](https://mpv.io/manual/master/#options-keep-open)                                   | Whether to close or leave the player open after the file finishes playing. *Use `no` if you want the player to close*
[`save-position-on-quit`](https://mpv.io/manual/master/#resuming-playback)                       | Save the current playback position on quit. When the file is reopened, mpv will resume from where it left off. *Remove this option if you do not want the player to save your position*
[`screenshot-format`](https://mpv.io/manual/master/#options-screenshot-format)                   | File format used for screenshots. *`png` is recommended for lossless quality*
[`screenshot-dir`](https://mpv.io/manual/master/#options-screenshot-dir)                         | The directory where screenshots will be saved. *Currently set to your default pictures folder (`Pictures/mpv`)*
[`screenshot-template`](https://mpv.io/manual/master/#options-screenshot-template)               | The naming scheme for screenshots. *`%F-%p` translates to `filename-timestamp`*
[`slang`](https://mpv.io/manual/master/#options-slang)                                           | Priority list of subtitle languages to use when there are multiple tracks
[`alang`](https://mpv.io/manual/master/#options-alang)                                           | Priority list of audio languages to use when there are multiple tracks
[`subs-with-matching-audio`](https://mpv.io/manual/master/#options-subs-with-matching-audio)     | Determines whether the subtitle and audio track must match their language. *Use `no` if you want to watch dubs with subtitle track*

===

## Advanced Config

mpv can be fine-tuned to meet your specific needs, from tweaking playback behavior to customizing video, audio, and subtitle settings. This section outlines the common options that can improve your experience.

!!!
This guide assumes you know the location of your config folder. *See [Config Overview](#config-overview) to see where to locate your config files.*
!!!

### Debanding

Color banding is a visual artifact that is typically seen in gradients, where the colors can be easily differentiated by the human eye. *See [Tom Scott's video explaining color banding](https://youtu.be/h9j89L8eQQk).*

<p align="center">
   <a href="https://user-images.githubusercontent.com/78981416/214381256-e5722886-57d7-4cd4-834e-edbd30b432e0.png">
   <img src="https://user-images.githubusercontent.com/78981416/214381256-e5722886-57d7-4cd4-834e-edbd30b432e0.png" alt="Banding (left) vs. No banding (right)" width="640" height="360">
   </a>
<p align="center">Banding (left) vs. No banding (right)</p>
</p>

To enable debanding in mpv, apply the following changes to your config:

+++ `mpv.conf`

```properties
## Deband
# Set deband to "no" as we only need to enable it for specific cases
deband=no
deband-iterations=4
deband-grain=48
```

!!!warning
Your deband settings should be placed after your [`profile`](https://mpv.io/manual/master/#profiles) in order to prevent conflict.
!!!

+++ `input.conf`

```properties
D cycle deband
```

+++

After applying your changes, debanding can be applied at any time by pressing `Shift` + `D`.

### Scaling

Scaling is the process of taking content that does not match your screen resolution and resizing it to fit your display. *See the [Playback Guide](/guides/playback/#scaling) for more information.*

[mpv](https://mpv.io) has a built-in profile called `high-quality` which enables better upscaling using `ewa_lanczossharp`. By default, mpv uses `lanczos` and `hermite`. *This option is necessary to enable even if you use an external shader, as it can act as a fallback.*

!!!warning
`high-quality` enables debanding by default, which is not recommended for high-quality sources. It should be followed by `deband=no`.
!!!

Scalers only work when the resolution of your video does not match your display. They do not activate if the content resolution already matches your display resolution.

+++ High-End PCs
If you use high-end hardware, we suggest using [nnedi3-nns256-win8x4](https://github.com/bjin/mpv-prescalers/blob/master/nnedi3-nns256-win8x4.hook).

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
If you use mid-range hardware, we suggest sticking to mpv's built-in `high-quality` profile.

To use the profile, add the following to the top of your `mpv.conf`:

```properties
## Video
profile=high-quality
vo=gpu-next
gpu-api=vulkan
scale-antiring=0.5
deband=no

# Dither
# This must be set to match your monitor's bit depth
dither-depth = 8
```

!!!warning
`dither-depth` should be set to match your monitor's bit depth to prevent [banding](#debanding).
!!!

This is included in the [Basic Config](#basic-config).

+++ Low-End PCs
If you use low-end hardware, we suggest sticking to mpv's built-in `fast` profile, which prioritizes performance over quality.

To use the profile, add the following to the top of your `mpv.conf`:

```properties
profile=fast
```

+++

### Subtitle Restyling

Most releases will use their own font for `.ass` subtitles. These can be manually overridden by mpv, which can help improve readability or match personal preferences.

Below are a couple of commonly used styles:

+++ Gandhi Sans

![Gandhi Sans](https://user-images.githubusercontent.com/78981416/248583226-18cece1d-4cd6-4a55-8fbf-8fa587399262.png)

[Download this font here](https://fontmeme.com/fonts/gandhi-sans-font) or use the button below:

[!button icon="download" variant="primary" text="Gandhi Sans"](/static/playback/fonts/gandhi-sans.zip)

Run the `.otf` font file to install it system-wide or put it in your `fonts` folder. Add the following to your `mpv.conf`:

```properties
## Restyle Subtitles
# Set sub-ass-override to "no" as we only need to enable it for specific cases
sub-ass-override=no
sub-ass-style-overrides=playresx=1920,playresy=1080
sub-font="Gandhi Sans"
sub-font-size=50
sub-color="#FFFFFF"
sub-border-size=2.4
sub-border-color="#FF000000"
sub-shadow-color="#A0000000"
sub-shadow-offset=0.75
sub-bold=yes
sub-ass-style-overrides=Kerning=yes
```

+++ Cabin

![Cabin](https://user-images.githubusercontent.com/78981416/248583220-ac376f74-ce9f-4c3c-800b-370355a841f0.png)

[Download this font here](https://fontmeme.com/fonts/cabin-font) or use the button below:

[!button icon="download" variant="primary" text="Cabin"](/static/playback/fonts/cabin.zip)

Run the `.ttf` font file to install it system-wide or put it in your `fonts` folder. Add the following to your `mpv.conf`:

```properties
## Restyle Subtitles

# Set sub-ass-override to "no" as we only need to enable it for specific cases
sub-ass-override=no
sub-ass-style-overrides=playresx=1920,playresy=1080
sub-font="Cabin"
sub-font-size=50
sub-color="#FFFFFFFF"
sub-border-size=2.4
sub-border-color="#FF000000"
sub-shadow-color="#A0000000"
sub-shadow-offset=0.8
sub-ass-style-overrides=Kerning=yes
```

+++

To activate it with a key, add the following to your `input.conf`, replacing `k` with the bind of your choice, if necessary (case-sensitive):

```properties
k cycle_values sub-ass-override "force" "no"
```

### Auto Profiles

Auto profiles allow users to automate actions based on certain conditions. Tasks such as [debanding](#debanding) and [subtitle restyling](#subtitle-restyling) can be applied automatically when these conditions are met, such as file names.

For instance, some seasonal releases may exhibit banding issues and use subjectively less-appealing subtitle fonts. To address this, we can create a `simulcast` auto profile, which automatically applies [debanding](#debanding) and [subtitle restyling](#subtitle-restyling) when playing releases uploaded by SubsPlease, Erai-raws, Tsundere-Raws, VARYG, or HorribleSubs.

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

!!!warning
Your auto profile(s) should be placed at the end of your `mpv.conf` in order to prevent conflict.
!!!

### Custom Scripts

mpv supports loading custom scripts, allowing you to further expand the player's functionality.

Below is a list of some popular scripts:

- [autocrop](https://github.com/mpv-player/mpv/blob/master/TOOLS/lua/autocrop.lua) - Automatically crop the video by using lavfi's cropdetect filter to detect black bars
- [autoload](https://github.com/mpv-player/mpv/blob/master/TOOLS/lua/autoload.lua) - Automatically adds all files present in the folder to a playlist
- [change-refresh](https://github.com/CogentRedTester/mpv-changerefresh) - Script to automatically change the refresh rate of the display to reflect the current video
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
