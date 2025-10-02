---
label: mpv
description: Learn how to install, setup, and configure mpv
image: https://user-images.githubusercontent.com/78981416/215125796-08b99128-fe50-4d0c-b0dd-49f8828af0dc.png
---

# mpv

[mpv](https://mpv.io) is a versatile and lightweight media player with extensive customizability. It is used and recommended by most enthusiasts as it supports a wide range of video, audio, and subtitle formats. Additionally, it offers many advanced features and is available on multiple platforms.

## Installation

### Windows

==- Option #1: 🔧 Installing mpv

1. Download the latest Windows build of [mpv](https://mpv.io) from [mpv-winbuild](https://github.com/zhongfly/mpv-winbuild/releases/latest). *For most users, this should be `mpv-x86_64-YYYYMMDD-git-abcxyz.7z`*
2. Once downloaded, extract the archive's contents to your specified location
!!!warning
This folder cannot be changed after installation. If you wish to change it in the future, you will need to uninstall it first.
!!!
3. Navigate to the `installer` folder and run `mpv-install.bat`. Follow the on-screen instructions to complete installation

[!embed text="Installing mpv-winbuild"](/static/tutorials/mpv/mpv-winbuild-installation.mp4)

==- Option #2: 🍨 Installing mpv via Scoop

[Scoop](https://scoop.sh) is a command line package manager for Windows. Unlike other installation methods, Scoop downloads and manages packages in a portable way, keeping them neatly isolated in `%userprofile%/scoop` and automatically adding them to your PATH.

Scoop can be installed using their install script in a PowerShell window:

```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
irm get.scoop.sh | iex
```

Then, install mpv using `scoop`:

```powershell
scoop bucket add extras
scoop install extras/mpv-git
```

+++ Updating mpv

```powershell
scoop update mpv-git
```

+++ Uninstalling mpv

```powershell
scoop uninstall mpv-git
```

+++

==- Option #3: 🍴 Installing a fork

!!!info
We suggest sticking with the official mpv player as forks tend to lag behind in updates.
!!!

[mpv](https://mpv.io) can also be found in the various forks below:

- [mpc-qt](https://github.com/mpc-qt/mpc-qt)
- [mpv.net](https://github.com/mpvnet-player/mpv.net) [!badge icon="apps" variant="info" text="Microsoft Store"](https://apps.microsoft.com/store/detail/9N64SQZTB3LM)

===

==- 📁 Adding mpv to PATH

If you want to access mpv from the command line, you will need to add it to Windows PATH:

1. In *Windows Settings* -> *System* -> *About*, locate *Advanced System Settings*. Head to *Advanced* and click on *Environment Variables...*
2. Under *System Variables*, select *Path* and click on *Edit...*
3. Click *New* and point the variable to the folder where `mpv.exe` is located/installed
4. Launch a new terminal window. *mpv can be accessed from the command line using `mpv`*

![Adding mpv to PATH](/static/tutorials/mpv/mpv-path.png)

==-

### macOS

==- Option #1: 🔧 Installing mpv

Download the latest macOS build of [mpv](https://mpv.io):
[!button size="s" variant="primary" icon="download" text="Stable" margin="0 8 0 0"](https://laboratory.stolendata.net/~djinn/mpv_osx/)
[!button size="s" variant="secondary" icon="download" text="Nightly"](https://github.com/jnozsc/mpv-nightly-build)

==- Option #2: 🍺 Installing with Homebrew

Install [Homebrew](https://brew.sh/). Then, run the following command in your terminal:

```sh
brew install mpv
```

==-

!!!warning
[IINA](https://iina.io) is not recommended as it can display incorrect colors. We suggest sticking with the stock [mpv](https://mpv.io) player.
!!!

### Linux

!!!warning
Distributions usually package outdated, unmaintained, and/or unsupported versions of mpv. We recommend using mpv-build or third-party packages instead.
!!!

==- 🔧 Installing mpv with unofficial packages

The following packages are not maintained by official mpv developers:

- [Arch](https://archlinux.org/packages/extra/x86_64/mpv/)
- [Gentoo](https://packages.gentoo.org/packages/media-video/mpv)
- [Arch](https://aur.archlinux.org/packages/mpv-git/) [!badge variant="secondary" text="AUR"] [!badge variant="secondary" text="git"]
- [Arch](https://aur.archlinux.org/packages/mpv-build-git/) [!badge variant="secondary" text="AUR"] [!badge variant="secondary" text="mpv-build"]
- [Debian](https://deb-multimedia.org/dists/testing/main/binary-amd64/package/mpv)
- [Ubuntu/Debian](https://fruit.je/apt) [!badge variant="secondary" text="APT"]

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

+++ Scoop (`%userprofile%/scoop/persist/mpv-git`)

```properties
.
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
scale-antiring=0.6

## Behavior (personal preference)
keep-open=yes
save-position-on-quit

## Screenshots
screenshot-format=png
screenshot-dir="~/Pictures/mpv"
screenshot-template="%F-%p-%n"
screenshot-high-bit-depth=no

## Language Priority
## Sub
## Add enm before eng for honorifics
slang=eng,en
alang=jpn,ja

## Dub
#slang=zxx,eng,en
#alang=eng,en
#subs-with-matching-audio=forced
```

==- :icon-info: Understanding the config

!!!
See [mpv's user manual](https://mpv.io/manual/stable) for a detailed explanation of all the options.
!!!

Option                                                                                           | Meaning
-------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
[`profile`](https://mpv.io/manual/master/#profiles)                                              | The profile to be used by mpv. This should be left at the top of your file avoid conflict with other settings.
[`vo`](https://mpv.io/manual/master/#video-output-drivers)                                       | The output driver to be used by mpv. *`gpu-next` is recommended for most modern hardware*.
[`scale-antiring`](https://mpv.io/manual/master/#options-scale-antiring)                         | Sets the strength of the antiringing filter. *We recommend not setting too high of a value to prevent unwanted artifacts*.
[`keep-open`](https://mpv.io/manual/master/#options-keep-open)                                   | Whether to close or leave the player open after the file finishes playing. *Use `no` if you want the player to close*.
[`save-position-on-quit`](https://mpv.io/manual/master/#resuming-playback)                       | Save the current playback position on quit. When the file is reopened, mpv will resume from where it left off. *Remove this option if you do not want the player to save your position*.
[`screenshot-format`](https://mpv.io/manual/master/#options-screenshot-format)                   | File format used for screenshots. *`png` is recommended for lossless quality*.
[`screenshot-dir`](https://mpv.io/manual/master/#options-screenshot-dir)                         | The directory where screenshots will be saved. *Currently set to your default pictures folder (`Pictures/mpv`)*.
[`screenshot-template`](https://mpv.io/manual/master/#options-screenshot-template)               | The naming scheme for screenshots. *`%F-%p` translates to `filename-timestamp`*.
[`slang`](https://mpv.io/manual/master/#options-slang)                                           | Priority list of subtitle languages to use when there are multiple tracks.
[`alang`](https://mpv.io/manual/master/#options-alang)                                           | Priority list of audio languages to use when there are multiple tracks.
[`subs-with-matching-audio`](https://mpv.io/manual/master/#options-subs-with-matching-audio)     | Determines whether the subtitle and audio track must match their language.

===

## Advanced Config

mpv can be fine-tuned to meet your specific needs, from tweaking playback behavior to customizing video, audio, and subtitle settings. This section outlines the common options that can improve your experience.

!!!
This guide assumes you know the location of your config folder. *See [Config Overview](#config-overview) to see where to locate your config files.*
!!!

### Debanding

Color banding is a visual artifact that is typically seen in gradients,
where the colors can be easily differentiated by the human eye. 
*See [Tom Scott's video explaining color banding](https://youtu.be/h9j89L8eQQk).*

<p align="center">
   <a href="https://user-images.githubusercontent.com/78981416/214381256-e5722886-57d7-4cd4-834e-edbd30b432e0.png">
   <img src="https://user-images.githubusercontent.com/78981416/214381256-e5722886-57d7-4cd4-834e-edbd30b432e0.png" alt="Banding (left) vs. No banding (right)" width="640" height="360">
   </a>
<p align="center">Banding (left) vs. No banding (right)</p>
</p>

mpv includes built-in debanding with sensible defaults, so no additional
configuration is required. You can enable it anytime during playback by
pressing `b` (default keybind). If the default deband is inadequate for a
specific video, you may need to experiment with the [`--deband-*`](https://mpv.io/manual/master/#options-deband)
options to find what works best.

!!!warning
Keep in mind that stronger settings will cause a loss of detail and
should be reserved for situations where the loss of detail is acceptable for reducing banding.
!!!

### Scaling

Scaling is the process of taking content that does not match your screen resolution and resizing it to fit your display. *See the [Playback Guide](/guides/playback/#scaling) for more information.*

[mpv](https://mpv.io) has a built-in profile called `high-quality` which enables better upscaling using `ewa_lanczossharp`. By default, mpv uses `lanczos` and `hermite`. *This option is necessary to enable even if you use an external shader, as it can act as a fallback.*

Scalers only work when the resolution of your video does not match your display. They do not activate if the content resolution already matches your display resolution.

+++ High-End PCs

If you use high-end hardware, we recommend using the following shaders:

- [ArtCNN_C4F32.glsl](https://github.com/Artoriuz/ArtCNN/blob/main/GLSL/ArtCNN_C4F32.glsl) for higher quality sources
- [nnedi3-nns128-win8x4.hook](https://github.com/bjin/mpv-prescalers/blob/master/compute/nnedi3-nns128-win8x4.hook) for lower quality sources

Download both the shader files and place them in your `shaders` folder.

Next, add the following to your `input.conf`, replacing `g` with the bind of your choice, if necessary (case-sensitive):

```properties
g cycle-values glsl-shaders "~~/shaders/nnedi3-nns128-win8x4.hook" "~~/shaders/ArtCNN_C4F32.glsl" ""
```

To toggle the shader, press `g` during playback to select the suitable shader.

+++ Mid-Range PCs

If you use mid-range hardware, we recommend sticking to mpv's built-in `high-quality` profile.

To use the profile, add the following to the top of your `mpv.conf`:

```properties
profile=high-quality
vo=gpu-next
scale-antiring=0.6
```

This is included in the [Basic Config](#basic-config).

+++ Low-End PCs

If you use low-end hardware, we recommend sticking to mpv's default profile, which aims for a balance between quality and performance. No other changes need to be made as it is used by default.

+++ Potato PCs

If you use very low-end hardware, we recommend sticking to mpv's `fast` profile, which prioritizes performance over quality.

To use the profile, add the following to the top of your `mpv.conf`:

```properties
profile=fast
```

+++

### Subtitle Restyling

Most releases will use their own font for `.ass` subtitles. 
These can be manually overridden by mpv, which can help improve readability or match personal preferences.

!!!warning
Restyling subtitles may lead to incorrect rendering in some cases.
!!!

Below are a couple of commonly used styles:

==- :icon-project-roadmap: Gandhi Sans

![Gandhi Sans](/static/tutorials/mpv/gandhi-sans-cropped.png)

[Download this font here](https://fontmeme.com/fonts/gandhi-sans-font) or use the button below:

[!button icon="download" variant="primary" text="Gandhi Sans"](/static/playback/fonts/gandhi-sans.zip)

Run the `.otf` font file to install it system-wide or put it in your `fonts` folder. Add the following to your `mpv.conf`:

```properties
# Set sub-ass-override to "no" as we only need to enable it for specific cases
sub-ass-override=no
sub-ass-style-overrides=playresx=1920,playresy=1080
sub-font="Gandhi Sans"
sub-font-size=50
sub-color="#FFFFFF"
sub-margin-y=40
sub-border-size=2.4
sub-border-color="#FF000000"
sub-shadow-color="#A0000000"
sub-shadow-offset=0.75
sub-bold=yes
sub-ass-style-overrides=Kerning=yes
```

==- :icon-project-roadmap: Cabin

![Cabin](/static/tutorials/mpv/cabin-cropped.png)

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
sub-margin-y=40
sub-border-size=2.4
sub-border-color="#FF000000"
sub-shadow-color="#A0000000"
sub-shadow-offset=0.8
sub-ass-style-overrides=Kerning=yes
```

==- :icon-project-roadmap: Cabin F

![Cabin F](/static/tutorials/mpv/cabin-f-cropped.png)

!!!secondary
This is a modified version of Cabin made by *@astolfo69 (RaptoR)* in the [SeaDex Discord server](https://discord.com/invite/jPeeZewWRn).
!!!

Download this font using the button below:

[!button icon="download" variant="primary" text="Cabin F"](/static/playback/fonts/cabin-f.zip)

Run the `.ttf` font file to install it system-wide or put it in your `fonts` folder. Add the following to your `mpv.conf`:

```properties
## Restyle Subtitles
# Set sub-ass-override to "no" as we only need to enable it for specific cases
sub-ass-override=no
sub-ass-style-overrides=playresx=1920,playresy=1080
sub-font="Cabin F"
sub-font-size=50
sub-color="#FFFFFFFF"
sub-margin-y=40
sub-border-size=2.4
sub-border-color="#FF000000"
sub-shadow-color="#A0000000"
sub-shadow-offset=0.8
sub-ass-style-overrides=Kerning=yes
```

===

To activate it with a key, add the following to your `input.conf`, replacing `k` with the bind of your choice, if necessary (case-sensitive):

```properties
k cycle_values sub-ass-override "force" "no"
```

### Auto Profiles

Auto profiles allow users to automate actions based on certain conditions. Tasks such as [debanding](#debanding) and [subtitle restyling](#subtitle-restyling) can be applied automatically when these conditions are met, such as file names.

For instance, some seasonal releases may exhibit banding issues and use subjectively less-appealing subtitle fonts. To address this, we can create a `simulcast` auto profile, which automatically applies [debanding](#debanding) and [subtitle restyling](#subtitle-restyling) when playing releases uploaded by SubsPlease, Erai-raws, Tsundere-Raws, VARYG, or HorribleSubs.

Add the following to the end of your `mpv.conf`:

```properties
[crunchyroll]
profile-cond=filename:match("SubsPlease") or filename:match("Erai%-raws") or filename:match("HorribleSubs")
profile-restore=copy
sub-ass-use-video-data=aspect-ratio

[simulcast]
profile-cond=(function(a)for b,c in ipairs(a)do if filename:match(c)then return true end end end)({"SubsPlease","Erai%-raws","Tsundere%-Raws","%-VARYG","HorribleSubs","SubsPlus%+", "Yameii"})
profile-restore=copy
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

==- :icon-gear: Installation

1. Navigate to your mpv [config directory](#config-overview)
2. Locate the `scripts` folder. *You may need to create this folder if it doesn't exist*
3. Drag your script file(s) (e.g. `.lua`) into the folder

Your scripts are automatically loaded when you launch mpv.
*If mpv is currently open, you will need to relaunch it in order for your script(s) to take effect.*

==-

### Skins

You can customize how mpv looks using skins. These are subject to personal preference, so find what works best for you.

Below is a list of some popular skins:

- [mfpbar](https://codeberg.org/NRK/mpv-toolbox/src/branch/master/mfpbar)
- [ModernX](https://github.com/zydezu/ModernX)
- [mpv-osc-tethys](https://github.com/Zren/mpv-osc-tethys)
- [mpv-progressbar](https://github.com/torque/mpv-progressbar)
- [oscc](https://github.com/longtermfree/oscc)
- [uosc](https://github.com/tomasklaen/uosc)
