---
label: MPV
description: Installation, setup, and configuration for MPV
image: https://user-images.githubusercontent.com/78981416/215125796-08b99128-fe50-4d0c-b0dd-49f8828af0dc.png
---

# TEEST

# MPV

[MPV](https://mpv.io/) is a versatile and lightweight media player with extensive customization options. It supports a wide variety of video file formats, audio and video codecs, subtitle types, offers advanced features, and is compatible with multiple platforms.

## Installation

### Windows

==- Manually Installing MPV

1. Grab the topmost file from [here](https://sourceforge.net/projects/mpv-player-windows/files/64bit/).

2. Extract it into a folder. This folder cannot be changed after installation. If you want to change it after installation, you'll have to uninstall and then reinstall it in the new location.

3. Navigate to the `installer` folder.

4. Inside the `installer` folder you'll find `mpv-install.bat`, run this as an administrator. If you want to read about what `mpv-install.bat` does, visit [here](https://github.com/rossy/mpv-install).

5. After it's done, you'll get a prompt to open Control Panel and set MPV as the default player.

That's it for the basic installation of MPV. You can use it without doing anything else.

==- Adding MPV to PATH

1. Open the Start Search, type in `env`, and choose `Edit the system environment variables`.
2. Click the `Environment Variables…` button.
3. Under the `System Variables` section (the lower half), find the row with `Path` in the first column, and click `edit`.
4. The `Edit environment variable` UI will appear. Here, you can click `New` and add the path of the directory containing `mpv.exe`.
5. Dismiss all of the dialogs by choosing `OK`. Your changes are saved.

==- Automatically Installing MPV with Scoop

1. Open PowerShell

2. Run the following commands in PowerShell to install [scoop](https://scoop.sh/)

   ```powershell
   Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
   ```

   ```powershell
   irm get.scoop.sh | iex
   ```

   This is a one time thing to install scoop. Scoop will be installed to `C:\Users\<YOUR USERNAME>\scoop` by default.

3. Now install mpv via running the following commands

   ```powershell
   scoop bucket add extras
   ```

   ```powershell
   scoop install mpv
   ```

   That's it. This will not only install MPV, it'll also add it to your PATH which makes MPV accessible via terminal and create [`portable-config`](#additional-filesfolders) folder for you. This is where your [custom config](#custom-config) will go.
   Scoop installs each app under its own directory and keeps them contained so your MPV is installed in `C:\Users\<YOUR USERNAME>\scoop\apps\mpv` by default.

4. To update MPV in future just run the command below

   ```powershell
   scoop update mpv
   ```

5. To uninstall just run

   ```powershell
   scoop uninstall mpv
   ```

   This will uninstall MPV entirely but preserve your `portable-config` in `C:\Users\<YOUR USERNAME>\scoop\persist\mpv\portable_config`.
   If you also want to delete persisting data such as the config, run:

   ```powershell
   scoop uninstall mpv --purge
   ```

==- Preconfigured MPV portable build

If you don't want to setup MPV yourself, you can grab the preconfigured MPV portable build linked below. It has the same settings as described in this guide. All you need to do is download it and follow these installation instructions.
[!file MPV Portable Build](https://mega.nz/folder/11QCTZgR#sdsjUYkIieGjVR09mnpYSw)

===

### MacOS

Grab MacOS builds from [here](https://laboratory.stolendata.net/~djinn/mpv_osx/)

#### Homebrew

You will need [homebrew](https://brew.sh/) installed for this and then run the following command in your terminal.

```sh
brew install mpv
```

### Linux

```sh
apt install mpv
```

## Additional files/folders

The default path for mpv config is `%APPDATA%/mpv/` but a folder named `portable_config` next to the `mpv.exe` overrides this. Here's a brief overview of files/folders you may want inside either of these folders -

- `mpv.conf` - MPV user settings.
- `input.conf` - custom keybind settings. You can see the default key bindings [here](https://user-images.githubusercontent.com/78981416/233719424-c442e27d-bd40-4138-912a-746a2b14493b.png) and syntax [here](https://mpv.io/manual/master/#input-conf).
- `fonts` - Font files in this directory are used by mpv/libass for subtitles.
- `scripts` - This directory is used to load custom scripts, usually `.lua` files. You can find scripts [here](https://github.com/mpv-player/mpv/wiki/User-Scripts).
- `shaders` - This directory is used to load custom shaders like NNEDI3, Ravu, FSRCNNX, etc.
- `script-opts` - A complementary folder to `scripts`, required by some `.lua` scripts to store user settings.

These will not be present by default, you'll have to create them as needed. Once you're done it'll look like:

```Folder structure
.
├── mpv.exe
└── portable_config
    ├── fonts
    ├── script-opts
    ├── scripts
    ├── shaders
    ├── mpv.conf
    └── input.conf
```

For more details, visit [here](https://mpv.io/manual/master/#files)

## Basic Config

MPV is a great player out-of-the-box but you can customize it further to make it even better. You can personalize settings, key bindings, and appearance. You can also fine-tune playback behavior, add new features through scripts, or optimize visual and audio settings.

We'll start with a generic basic config that'll be good enough for most people:

```properties
## Video

profile=gpu-hq
vo=gpu-next
gpu-api=vulkan
hwdec=auto-safe

## Behavior

keep-open=yes
save-position-on-quit

## Scaler

#change ewa_lanczos to spline36 if your system can't handle ewa_lanczos
scale=ewa_lanczos
dscale=mitchell
cscale=ewa_lanczos

## Screenshots

screenshot-format=png
screenshot-high-bit-depth=no
screenshot-png-compression=9
screenshot-directory="~/Pictures/MPV"
screenshot-template="%F-%p"

## Language Priority

# Sub
# Add enm before eng for honorifics
slang=eng,en
alang=jpn,ja

# Dub
# uncomment this section to prefer English Dub with subtitles for English Dub
#alang=eng,en
#slang=zxx,eng,en
#subs-with-matching-audio=no
```

==- What do these options do?

Below are some brief explanations of what some of the options do. Visit [MPV docs](https://mpv.io/manual/stable) for a detailed explanation of all the options.

- **profile=gpu-hq** - Allows for higher-quality playback
- **vo=gpu-next** -  General purpose, customizable, GPU-accelerated video output driver. It supports extended scaling methods, dithering, color management, custom shaders, HDR, and more
- **gpu-api=vulkan** - Use Vulkan graphics API
- **hwdec=auto-safe** - Enable hardware decoding
- **keep-open=yes** - Pause the player instead of terminating when playing or seeking beyond the end of the file, and there is no next file to be played
- **save-position-on-quit** - Save the current playback position on quit. When this file is played again later, the player will seek to the old playback position on start
- **scale=ewa_lanczos** - Use `ewa_lanczos` for upscaling
- **dscale=mitchell** - Use `mitchell` for downscaling
- **cscale=ewa_lanczos** - As `scale`, but for interpolating chroma information
- **screenshot-format=png** - Use `.png` for screenshots. `.png` is a lossless format.
- **screenshot-high-bit-depth=no** - Creates unnecessarily large screenshots without any real benefits, hence disabled
- **screenshot-png-compression=7** -  0-9, higher means better compression. This will affect the file size of the written screenshot file and the time it takes to write a screenshot
- **screenshot-directory="~/Pictures/MPV"** - The place where your screenshots will be stored. `~/Pictures/MPV` automatically gets expanded to `C:\Users\<YOUR USERNAME>\Pictures\MPV`
- **screenshot-template="%F-%p"** - Filename format for your screenshots. `%F` here means the filename and `%p` means the timestamp
- **slang=eng,en** - Priority list of subtitle languages to use
- **alang=jpn,ja** - Priority list of subtitle languages to use
- **subs-with-matching-audio=no** - Necessary if you want to watch dubs with signs/songs track

===

## Advanced Config

With advanced configuration options, you can fine-tune MPV to meet your specific needs, from tweaking playback behavior to customizing video, audio, and subtitle settings. You can do a lot here and covering all of it here is impossible, so we'll just go over some of the common ones. A lot of the values here should be good enough but you may need to adjust things depending on your sources.

### Debanding

Banding is a visual artifact, visual artifacts should never be in a video.
Debanding is the process of removing said banding.
Here's a 6-minute explanation of what causes banding: [Why dark video is a terrible mess - Tom Scott](https://youtu.be/h9j89L8eQQk)

==- Example of banding

![Banding (bad) vs No Banding (good)](https://user-images.githubusercontent.com/78981416/214381256-e5722886-57d7-4cd4-834e-edbd30b432e0.png "Banding (bad) vs No Banding (good)")

===

```properties
## Deband

# Set to "no" as we only need to enable it for specific cases
deband=no
deband-iterations=4
deband-threshold=48
deband-grain=48
```

Now go to your `input.conf`, located in `portable_config`. If it doesn't exist, you can make it. Open `input.conf` and add the following line to your `input.conf`.

```properties
D cycle deband
```

Now you can press `shift + d` to enable or disable debanding.

### Restyling subtitles

Restyling subtitles refers to the process of modifying the appearance of subtitles to improve readability or match personal preferences. Below are two commonly used styles that you can use or change them to your own liking.

=== Style 1: Gandhi Sans

[![GandhiSans](https://user-images.githubusercontent.com/78981416/248583226-18cece1d-4cd6-4a55-8fbf-8fa587399262.png "GandhiSans")](https://user-images.githubusercontent.com/78981416/248583226-18cece1d-4cd6-4a55-8fbf-8fa587399262.png "GandhiSans")

```properties
## Restyle Subtitles

# Set to "no" as we only need to enable it for specific cases
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

==- Style 2: Cabin

[![Cabin](https://user-images.githubusercontent.com/78981416/248583220-ac376f74-ce9f-4c3c-800b-370355a841f0.png "Cabin")](https://user-images.githubusercontent.com/78981416/248583220-ac376f74-ce9f-4c3c-800b-370355a841f0.png "Cabin")

```properties
## Restyle Subtitles

# Set to "no" as we only need to enable it for specific cases
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

===

Now go to your `input.conf`, located in `portable_config`. If it doesn't exist, you can make it. Open `input.conf` and add the following line to your `input.conf`.

```properties
k cycle_values sub-ass-override "force" "no"
```

Now you can press `k` to enable or disable style override.

### Auto Profiles

Auto profiles in MPV allow users to automate actions based on specific conditions. By defining profiles and their corresponding conditions, tasks such as debanding and restyling subtitles can be automatically applied based on factors like filenames.

For instance, seasonal releases may exhibit banding issues and less appealing subtitles. To address this, we can create a `[simulcast]` auto-profile in MPV. It will automatically apply debanding and restyle the subtitles when the filename includes specific keywords, such as the group tag.

```properties
## Auto-profiles

[simulcast]
profile-cond=string.match(p.filename, "SubsPlease") or string.match(p.filename, "Erai%-raws") or string.match(p.filename, "Tsundere%-Raws") or string.match(p.filename, "%-VARYG") or string.match(p.filename, "HorribleSubs")
profile-restore=copy
sub-fix-timing=yes
sub-ass-override=force
deband=yes
```

Simply copy and paste the above at the end of your `mpv.conf`.

### Upscaling

If you're watching content that's a lower resolution than your screen, you can consider using a high-quality scaler like `nnedi3-nns256-win8x4` which you can download from [here](https://github.com/bjin/mpv-prescalers/blob/master/nnedi3-nns256-win8x6.hook). Do note that this will be way more taxing on your system.

You can add it directly to your `mpv.conf`

```properties
glsl-shaders="~~/shaders/nnedi3-nns256-win8x4.hook"
```

Or, you can bind it to a key in your `input.conf`

```properties
G change-list glsl-shaders toggle "~~/nnedi3-nns256-win8x4.hook"
```

!!!
Scalers only work when the resolution of your video does not match your display. If the resolution of the video you're playing matches your display's resolution, scalers will remain unused.
!!!

### QoL Changes

By default, mouse wheel up and down seek the video instead of changing the volume. You can change this behavior so that mouse wheel up and down control the volume by adding the following to your `input.conf`

```properties
WHEEL_UP      add volume 2
WHEEL_DOWN    add volume -2
```

### Popular Scripts

MPV offers powerful scripting capabilities, allowing the player to do almost anything. Check out a variety of popular user scripts linked below or [here](https://github.com/mpv-player/mpv/wiki/User-Scripts) for additional functionality beyond the core MPV player.

- [autoload](https://github.com/mpv-player/mpv/blob/master/TOOLS/lua/autoload.lua) - Automatically adds all the files present in the folder in a playlist
- [mpv-playlistmanager](https://github.com/jonniek/mpv-playlistmanager) - Script to create and manage playlists
- [mpv-webm](https://github.com/ekisu/mpv-webm) - WebM maker for mpv
- [pause-when-minimize](https://github.com/mpv-player/mpv/blob/master/TOOLS/lua/pause-when-minimize.lua) - Pauses playback when minimizing the window, and resumes playback if it's brought back again
- [thumbfast](https://github.com/po5/thumbfast) - Thumbnailer script for mpv
- [trackselect](https://github.com/po5/trackselect) - Script to select tracks based on their title

### Skins

You can customize how MPV looks by using skins. These are completely up to your preference so you can look through some of the ones linked below.

- [ModernX](https://github.com/cyl0/ModernX)
- [mfpbar](https://codeberg.org/NRK/mpv-toolbox/src/branch/master/mfpbar)
- [mpv-osc-modern](https://github.com/maoiscat/mpv-osc-modern)
- [oscc](https://github.com/longtermfree/oscc)
- [progressbar](https://github.com/torque/mpv-progressbar)
- [tethys](https://github.com/Zren/mpv-osc-tethys)
- [uosc](https://github.com/tomasklaen/uosc)
