---
label: HTPC
order: -2
description: Learn how to setup a home-theater system for watching anime
image: /static/tohsaka.gif
---

# HTPC

A home-theater PC (HTPC) is a system specifically designed to host/stream media, such as anime TV shows and movies.

There are plenty of reasons you may want to consider using a HTPC over a smart TV or other premade TV box, such as [bad subtitle rendering](/static/playback/ass-subtitles-issue.mp4), [poor software support](https://github.com/mpv-android/mpv-android/issues/12), or limited filtering and upscaling options.

!!!
If you are on a strict budget ($50 or less), we suggest getting an [Amazon Fire TV Stick 4K Max](https://www.amazon.com/dp/B08MQZXN1X), as most PCs at this price will provide a suboptimal experience.
!!!

## Specifications

There are two main things to consider when building an anime HTPC: the [GPU](#gpu) and the [CPU](#cpu). The information below will help you decide on what to choose for your setup and budget.

### GPU

Generally, a dedicated graphics card (dGPU) will almost always be better than the integrated option (iGPU). *However, you may want to consider whether your home-theater setup will necessitate one if you're on a tighter budget:*

+++ Dedicated

A dGPU is a dedicated piece of hardware which will be used to handle video decoding. Cards in this category are usually from AMD or NVIDIA.

You may want to consider a dGPU if:

- You plan to use shaders
- You want to use filters like [debanding](/tutorials/mpv/#debanding)
- You have a high-resolution display (e.g. 2160p/4K or higher) and want to upscale

+++ Integrated

An iGPU is an integrated piece of hardware on your [CPU](#cpu) which will be used to handle video decoding. CPUs in this category are usually from Intel or AMD.

You may want to consider an iGPU if:

- You are on a tighter budget
- You want a smaller computer/footprint
- You have a lower-resolution display (e.g. 1080p) or plan to send your high-resolution display a lower-resolution signal (e.g. 2160p/4K -> 1080p)

+++

### CPU

Once you've decided on a type of [GPU](#gpu) for your HTPC, you can decide on what CPU you will need.

For most budget-friendly setups, we recommend picking up and repurposing a used/refurbished small form factor office machine, such as a *Dell OptiPlex*, *Lenovo ThinkCentre*, or *HP ProDesk*. These can be commonly found on sites like [eBay](https://ebay.com) at good prices.

Below are some examples of good candidates for HTPC setups to help you determine a good system:

==- :icon-rocket: Systems using a dGPU

![An example listing for a Dell OptiPlex 5040](/static/htpc/example-optiplex.png)

This is an example of a good option to choose for your HTPC. As shown, this machine includes a capable CPU, plenty of RAM preinstalled, and an SSD. It is only missing a dedicated graphics card.

You should also keep your eyes peeled for machines that have a dGPU preinstalled:

![A dedicated graphics card preinstalled in a Dell OptiPlex](/static/htpc/example-optiplex-dgpu.png)

If it includes a dGPU, you might not need to find one for your HTPC. Check to see beforehand if it can handle the various codecs/video you plan to run on it.

Once you have picked a machine, you can move onto picking a GPU. We recommend choosing relatively recent, low-powered graphics cards as this will allow you to handle most modern shows without issue. Some examples of popular budget cards are:

- AMD Radeon RX 550
- NVIDIA GT 1030 (GDDR5) [!badge variant="info" text="Preferred for 4K/HDR"]
- NVIDIA Quadro P600 [!badge variant="warning" text="Mini DP to HDMI adapter required"]

The most important factor to consider when choosing a GPU is the video decoder hardware and its supported resolutions.

!!!warning
Lower quality GPUs will often use lower quality fans or lock them to run at high speeds. This can result in unwanted noise, which may impact your listening experience.
!!!

- For AMD, look up the card on [TechPowerUp](https://www.techpowerup.com/gpu-specs) and search for the *Unified Video Decoder* version. [Check this number here](https://en.wikipedia.org/wiki/Unified_Video_Decoder#Format_support) to see if it supports your codecs
- For NVIDIA, see their [Video Encode and Decode GPU Support Matrix](https://developer.nvidia.com/video-encode-and-decode-gpu-support-matrix-new) -> **NVDEC**

![An example listing for a NVIDIA GeForce GT 1030 from MSI](/static/htpc/example-nvidia-gpu.png)

!!!warning
Small form factor machines will require a low-profile GPU and bracket. Make sure to double-check the height of the card prior to installation, as passively-cooled cards tend to use larger heatsinks that may not fit in your case.
!!!

==- :icon-cpu: Systems using an iGPU

If you are choosing to use an iGPU for your HTPC, you can also look for MFF or micro desktops. These machines will have a smaller footprint and are very quiet. *Due to their size, however, they will not have room for a dGPU if you plan to upgrade later on.*

Intel is generally the choice of CPU in this category, as there is a much broader selection of processors with integrated graphics. Additionally, you can take advantage of Intel Quick Sync Video for hardware decoding media.

We recommend choosing 7th Generation Intel processors or newer to support playing the latest video codecs. [See the full list of codecs supported by various Intel generations.](https://wikipedia.org/wiki/Intel_Quick_Sync_Video#Hardware_decoding_and_encoding)

!!!warning
Avoid systems running 6th Generation Intel processors or older. These CPUs are known to [not include proper support for HEVC content and encounter issues hardware decoding](https://github.com/mpv-player/mpv/issues/12154), and software decoding will have a performance impact on subtitles.
!!!

![An example listing for a HP ProDesk 600](/static/htpc/example-prodesk.png)

This is an example of a good option to choose for your HTPC. As shown, this machine includes a capable CPU, decent amount of RAM preinstalled, and an SSD. This is an ideal candidate if size is a concern.

==-

## Operating System

### Windows

For most HTPC users, Windows is the recommended operating system. *Optionally, you may choose to install debloat scripts to increase performance:*

- [AtlasOS](https://atlasos.net)
- [ReviOS](https://revi.cc)

!!!
Make sure you install the appropriate graphics driver for your system:

[!button variant="secondary" icon="download" text="AMD" margin="0 8 0 0"](https://www.amd.com/en/support)
[!button variant="secondary" icon="download" text="Intel" margin="0 8 0 0"](https://www.intel.com/content/www/us/en/download-center)
[!button variant="secondary" icon="download" text="NVIDIA"](https://www.nvidia.com/download/index.aspx)
!!!

### Linux

!!!warning
HDR is not supported in X11 and Wayland. If you plan on playing HDR content, you will need to use Kodi's built-in player.
!!!

Linux is another common option for HTPCs. If you chose to go down this route, most of the setup remains the same, however file paths will be different.

## Player

### mpv

[mpv](/tutorials/mpv) is the most recommended media player for HTPC setups.

#### Config

Depending on your hardware, the [example mpv configs](/tutorials/mpv/#basic-config) may not suit your hardware. Below is an example configuration catered specficially towards HTPC setups:

==- :icon-file-code: HTPC `mpv.conf` config

```properties
# General
ontop=yes
profile=high-quality
blend-subtitles=video
hwdec=auto-safe
gpu-api=vulkan
target-colorspace-hint=yes

# Deband
deband=no
deband-iterations=2
deband-threshold=64
deband-range=20
deband-grain=64

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

[SDR]
profile-cond=p["video-params/primaries"] and p["video-params/primaries"] ~= "bt.2020"
vo=gpu

[HDR]
profile-cond=p["video-params/primaries"] == "bt.2020"
vo=gpu-next
target-contrast=inf ##inf is for OLED, for LCD get the contrast value from rtings or similar
target-trc=pq
target-prim=dci-p3
target-peak=700    ## If you have an HDR display, adjust this to the 10% peak

## Above makes use of vo=gpu-next for HDR Content, and vo=gpu for SDR. This is a requirement for blend-subtitles=video
## blend-subtitles=video is used to avoid performance issues with rendering 1080p subtitles at 2160p.
```

!!!
`deband` should be toggled as needed and is set to `off` in the config. [See how to bind debanding to a hotkey.](/tutorials/mpv/#input-conf)
!!!

==-

#### Scripts

Most modern TVs are capable of changing refresh rate to match the frame rate of the content, which removes [judder](/guides/playback#explaining-judder).

However this is not default behaviour in Windows, and as such we recommend [change-refresh](https://github.com/CogentRedTester/mpv-changerefresh), a script that automatically accomplishes this.

!!!
Make sure you set `auto` to `yes` for automatic switching in `changerefresh.conf`.
!!!

!!!warning
Some users experience issues with the [change-refresh](https://github.com/CogentRedTester/mpv-changerefresh) script, such as the refresh rate not changing. If this is the case, you can try setting `detect_display_resolution = false` and manually setting `original_width` and `original_height` to your respective display's specifications in `changerefresh.conf`.
!!!

!!!warning
[nircmd](https://www.nirsoft.net/utils/nircmd-x64.zip) is required for [change-refresh](https://github.com/CogentRedTester/mpv-changerefresh). Download and copy `nircmd.exe` to your `Windows` folder (i.e. `C:\Windows`).
!!!

### Kodi

For HTPC setups, we recommend using [Kodi](https://kodi.tv/download/windows/).

Kodi is a home-theater software which neatly displays your library and allows for easy navigation with a remote, while utilising mpv as the player for playing content.

#### Library

Kodi offers many ways to import your content, with the easiest being the [PlexKodiConnect](https://github.com/croneter/PlexKodiConnect) addon, a plugin that allows you to access your existing Plex library through Kodi.

Alternatively, you can use network shares like SMB or local drives such as an internal/external HDD.

#### mpv External Player Config

After installing Kodi, go to `%appdata%\Kodi\userdata`, make a file called `playercorefactory.xml` and paste the following into it, replacing `Path\To\mpv.exe` with the location of your `mpv.exe`:

==- :icon-file-code: `playercorefactory.xml`

```xml
<playercorefactory>
  <players>
    <player name="mpv" type="ExternalPlayer" audio="true" video="true">
      <filename>Path\To\mpv.exe</filename>
      <hidexbmc>true</hidexbmc>
      <hideconsole>false</hideconsole>
      <warpcursor>topright</warpcursor>
      <playcountminimumtime>1200</playcountminimumtime>
    </player>
  </players>
  <rules action="prepend">
    <rule filetypes="*" filename="*" player="mpv"/>
  </rules>
</playercorefactory>
```

==-

## Control

Using a keyboard and mouse to control your system is not optimal for home theater usage. Instead, you should consider using an alternative input device:

- CEC Adapter [!badge variant="primary" text="Recommended"]
- Combo keyboard and trackpad devices
- USB remotes

Although the most expensive option, a CEC adapter is one of the best ways to control your HTPC. This allows you to use your TV's OEM remote control to send commands to your computer, including but not limited to directional pad, select button, color buttons, and more, depending on your specific make and model of TV.

We recommend using the [Pulse-Eight CEC Adapter](https://www.pulse-eight.com/p/104/usb-hdmi-cec-adapter), which can often be found cheaper on second-hand marketplaces.

!!!secondary
Although not officially rated to do so, the Pulse-Eight CEC Adapter has been proven to somewhat work with 4K 120Hz signals, though your milage may vary.
!!!

+++ CEC

Pulse-Eight hosts builds for [libCEC](https://libcec.pulse-eight.com) on their website. libCEC includes the `cec-tray` application, a tool that converts CEC button presses on a comapatible remote to keyboard commands in Windows, which can be used to control media player applications such as Kodi and mpv.

==- :icon-gear: Automatic startup

1. Locate your libCEC installation folder. *By default, this can be found in `C:\Program Files (x86)\Pulse-Eight\USB-CEC Adapter\x64\netfx\`*
2. Create a shortcut to `cec-tray.exe`. Place this shortcut in your startup folder
   - Press `Win` + `R` and type `shell:startup` to find your startup folder
   - You can also manually go to this folder by going to `C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup` in File Explorer

==-

#### CEC with Media Players

+++ mpv

To add CEC support to mpv, add the following lines to your mpv's `input.conf`:

==- :icon-file-code: CEC `input.conf` config

```properties
# Left D-pad - Seek backward by 5 seconds
KP_LEFT seek -5
# Right D-pad - Seek forward by 5 seconds 
KP_RIGHT seek 5
# F2 - Toggle debanding
F2 cycle deband
# F4 - Cycle through subtitles (backwards)
F4 cycle sub down
# F1 - Cycle through audio tracks
F1 cycle audio
# Select - Pause/play
ENTER cycle pause
# Backspace - Exit mpv
BS quit
```

==-

+++ Kodi

Kodi includes support for CEC adapters by default. *Unfortunately, this plugin cannot be used to pass CEC commands to mpv.*

To disable the plugin:

1. Launch Kodi
2. Go to *Settings* > *System* > *Input* > *Peripherals* > *libCEC*
3. Disable the plugin
4. Relaunch/exit Kodi

The `cec-tray` application will still automatically detect `kodi.exe` and quit to prevent a conflict with the plugin. To avoid this, rename `kodi.exe` (by default, this can be found in located at `C:\Program Files\Kodi`) to another name, such as `kodi1.exe`. This will prevent `cec-tray` from quitting when Kodi is opened.

+++ AutoHotkey

[AutoHotkey](https://www.autohotkey.com) is a scripting language for macros. This guide will show you how to use AHK to launch Kodi using your TV remote.

#### Installation

Install v1.1 of [AutoHotkey](https://www.autohotkey.com). Follow the on-screen instructions to install AHK onto your system.

#### Script

Create a new text file called `kodi.ahk` with the following:

==- :icon-file-code: Kodi `.ahk` script

```properties
$F4::
IfWinExist, ahk_exe mpv.exe
{
    ifWinNotActive, ahk_exe mpv.exe
    {
        WinActivate
        return
    }
    Send {F4}
    return
}
 
IfWinNotExist, ahk_exe kodi1.exe
{
    Run, C:\Program Files\Kodi\kodi1.exe  ; Replace with the full path if necessary
    return
}
 
IfWinExist, ahk_exe kodi1.exe
{
    WinActivate
    return
}
Send {F4}
return
```

!!!secondary
Credits to Jimbo for the script.
!!!

==-

This script checks if `mpv.exe` or `kodi1.exe` are running:

- If they are not running, `kodi1.exe` is ran
- If they are currently running, the `F4` key is passed through to the running program. *This allows using the remote to launch Kodi, without sacrificing a button*

You will need to modify this code if you want to run something other than `kodi1.exe` (i.e. you used a different file name for the Kodi executable or want to launch a different program) or listen for a key other than `F4`.

To run this script, double-click the `.ahk` file. Alternatively, you can place it in your startup folder to run it automatically when `cec-tray` starts.

+++

+++
