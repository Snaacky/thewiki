---
label: HTPC (on a budget)
order: -2
description: Anime on the Big Screen
image: /static/tohsaka.gif
---

# Why HTPC?

There are plenty of reasons to want to use a HTPC over a smart TV or other premade TV box. Among them, there is [poor subtitle rendering](https://cdn.discordapp.com/attachments/930734323353677854/1137931348510785586/Kodi_20230806_190720_Record.mp4), [poor software support](https://github.com/mpv-android/mpv-android/issues/12), or limited filtering and upscaling options.

As of the writing of this guide, any less than $50 leaves you with the Amazon Fire Stick 4K Max as probably the best option, as most PCs cheaper than this will not handle things much better, and in some cases may be even worse.

## Specs Rundown

There are two main considerations for an Anime HTPC. The biggest, will be the CPU. You will need to decide if you want to use the integrated GPU (iGPU) or if you want to install an external dedicated GPU (dGPU).

+++ dGPU

- You plan to use shaders
- You want to use filters like deband
- You have a 4K TV and want to upscale

+++ iGPU

- You are on a tighter budget
- You want this to be as small as possible
- You have a 1080p TV or will send your 4K TV a 1080p signal
+++
Once you have decided if you will be using a dGPU, you can pick your CPU.

### CPU choice

To keep things cheap, this guide will be using used hardware as examples, however you are free to look at brand new hardware instead.

A good base for any HTPC is a used small form factor office machine, such as a Dell Optiplex, Lenovo ThinkCentre, or HP ProDesk. Look on sites like [ebay.com](https://ebay.com) for these machines at good prices.
If you chose to rely on iGPU performance for your HTPC, you can look at MFF or micro desktops. These machines are very tiny and are very quiet, however, they do not have room for a dGPU.
!!!warning
If you decided on iGPU, avoid 6th generation Intel or older. These CPUs are known to have [issues hardware decoding HEVC](https://github.com/mpv-player/mpv/issues/12154), and software decoding will have a performance impact on subtitles.
!!!

Good Canidates:

==- Dedicated GPU

![OptiPlex 5040](https://github.com/guyman624/thewiki/assets/82007920/bc6a1940-3cda-4bf6-94cd-5c14f72e79a1)
This machine has plenty of ram preinstalled, a beefier CPU, and SSD storage included. All it needs is a GPU.

Keep your eyes peeled for machines that already have a GPU installed
![gpu preinstalled](https://github.com/guyman624/thewiki/assets/82007920/d5ad6390-b4e4-4f31-ad16-0ef33f7411c0)

Once you have picked a machine, you can move onto picking a GPU.

The two most popular options are going to be the GT 1030 and the RX 550. If you plan to play 4K or HDR videos, you should pick the GT 1030. Otherwise, the RX 550 should be fine.
The most important consideration is the video decoder, and supported resolutions. NVidia has a chart [here](https://developer.nvidia.com/video-encode-and-decode-gpu-support-matrix-new) (look at NVDEC not NVENC). For AMD, check TechPowerUp for the Unified Video Decoder version, then check [here](https://en.wikipedia.org/wiki/Unified_Video_Decoder#Format_support) for supported codecs.
![gpu](https://github.com/guyman624/thewiki/assets/82007920/6aca03ab-155d-499a-b38c-aadd7c9d0004)
!!!warning
SFF machines will need a low profile GPU and bracket.
!!!

==- Integrated GPU
![MFF desktop](https://github.com/guyman624/thewiki/assets/82007920/5cbe1a4e-7b82-4136-992c-4427597487da)
This machine includes the power cord, has a decent amount of ram, and an included SSD. This is an ideal canidate if size is a concern.

===

## Software

### Windows

I won't go over installing Windows in this guide, but install Windows as you normally would on any other computer. You are also free to install any debloat scripts, such as AtlasOS or ReviOS to increase performance. Make sure you install the appropriate graphics driver for your system, whether it be NVidia, AMD, or Intel.

### MPV

#### Config

We already have a [guide](/../tutorials/mpv/) on setting up mpv, however you may realize that the config there is slow on your machine. Here, I will provide the configuration I use for my 6100T + GT 1030 HTPC.

==- HTPC config

```yaml
#### General
ontop=yes
blend-subtitles=video
hwdec=auto-safe
gpu-api=vulkan
target-colorspace-hint=yes

scale=ewa_lanczos
dscale=hermite
cscale=catmull-rom

deband=no
deband-iterations=4
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
target-contrast=inf
target-trc=pq
target-prim=bt.2020
target-peak=700    ##IF YOU HAVE AN HDR TV, ADJUST THIS TO THE 10% PEAK

## Above makes use of vo=gpu-next for HDR Content, and vo=gpu for SDR. This is a requirement for blend-subtitles=video.
```

==-

In my input.conf I have a single line, for deband.

```yaml
BACK cycle deband
```

BACK refers to the back button on my mouse.

#### Scripts

The most important script for TV viewing will be [changerefresh](https://github.com/CogentRedTester/mpv-changerefresh).
This script automatically sets the TV to refresh rate of the video, to prevent judder. Make sure you set `auto = true` for automatic switching.
It also requires [nircmd](https://www.nirsoft.net/utils/nircmd-x64.zip), you can just copy nircmd.exe to C:\Windows.
