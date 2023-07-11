---
label: Quality
order: -1
description: Learn what to look for to get the best quality
image: https://user-images.githubusercontent.com/78981416/215166522-1d7358e8-bec2-4a54-a9ec-71deab646e56.gif
---

# Quality

## Video

==- Understanding the Different Parts
- **Container** - The file itself, containing various video, audio, and subtitle streams. *They are typically stored as `.mkv` or `.mp4`.*
    - You can losslessly convert between them with tools such as [ffmpeg](https://ffmpeg.org/download.html).
- **Codec** - The compression format used for the video stream and the biggest factor in compatibility with your system. *HEVC (H.265) and AVC (H.264) are the main ones.*
- **Bit depth** - The maximum colors that can be stored in the video. *Typically in 8-bit, or 10-bit for high-quality anime encodes.*
    - Converting an 8-bit source to 10-bit might seem counterintuitive if you know a little about transcoding and data loss, *[but it gives a better result at smaller sizes](https://yukisubs.files.wordpress.com/2016/10/why_does_10bit_save_bandwidth_-_ateme.pdf).*
- **Frame rate** - The frequency at which frames are displayed. *This will usually be 23.976fps.*
    - Many TVs use [interpolation](https://en.wikipedia.org/wiki/Motion_interpolation) to turn this into 60 fps giving you an artificial sense of smoothness. *This is not recommended for anime and should be disabled in settings. 60fps encodes should be avoided.*
- **Level/profile** - The maximum resolution and bitrate specified within the AVC/HEVC standard. *A higher level/profile means lower compatibility and more processing power required to decode.*
    - *See [Levels for AVC (H.264)](https://en.wikipedia.org/wiki/Advanced_Video_Coding#Levels) and [Levels for HEVC (H.265)](https://en.wikipedia.org/wiki/High_Efficiency_Video_Coding_tiers_and_levels#Levels).*

==- Reading a Video File
The file name for a video will typically look like one of the following:

![](https://files.catbox.moe/dxyv69.png)

Key                                                           | Meaning                                                           
--------------------------------------------------------------|-----------------------------------------------------------
![#ed8796](https://placehold.co/15x15/ed8796/ed8796.png) 1    | The name of the group who created the release.                     
![#f5a97f](https://placehold.co/15x15/f5a97f/f5a97f.png) 2    | The anime show title.                                        
![#eed49f](https://placehold.co/15x15/eed49f/eed49f.png) 3    | The season/episode number.
![#a6da95](https://placehold.co/15x15/a6da95/a6da95.png) 4    | The source where the release was taken from. *Typically BD (Blu-ray/JPBD) or WEB.*
![#91d7e3](https://placehold.co/15x15/91d7e3/91d7e3.png) 5    | The resolution of the video file.
![#8aadf4](https://placehold.co/15x15/8aadf4/8aadf4.png) 6    | The video codec. *Typically HEVC or AVC.*
![#c6a0f6](https://placehold.co/15x15/c6a0f6/c6a0f6.png) 7    | The audio codec. *Dual audio means the release contains two audio streams, typically the dubbed version and original.*
![#f4dbd6](https://placehold.co/15x15/f4dbd6/f4dbd6.png) 8    | The CRC32 checksum.
![#f5bde6](https://placehold.co/15x15/f5bde6/f5bde6.png) 9    | The container format. *Typically `.mkv` or `.mp4`.*

### Encoding

Quality is subjective, and there is a difference between flaws which need to be fixed and the preferences of the end user. *Generally, the best release is an encode which fixes the flaws of a raw BDMV while simultaneously not altering the original material.*

The official BDMV is an encode of the source Blu-ray. As a result, it often suffers from issues like banding, blocking, noise, aliasing, etc. These can originate from a variety of reasons. For example, a lot of anime is natively 720p and is upscaled to 1080p for the Blu-ray. *Even if issues aren't present in the BDMV, they can appear in the encode due to compression.*

These issues can be fixed through filtering, a step which comes before encoding. The video is filtered with tools like [Vapoursynth](https://www.vapoursynth.com) before passing it on to the encoder.

See the links below for more information:
- [Examples of problems](http://bakashots.me/guide/index.php)
- [Encoding guides and more examples](https://guide.encode.moe/encoding/video-artifacts.html)
- [Advanced encoding guide](https://silentaperture.gitlab.io/mdbook-guide/introduction.html)
- [Mini encodes and audio](https://kokomins.wordpress.com/2019/10/10/anime-encoding-guide-for-x265-and-why-to-never-use-flac/)

### Types of Releases

==- BDMV
A simple complete copy of the original Blu-ray, used as a source for making other releases or encodes. *These are not useful for watching.*

JPBDs often have better quality than USBDs because of more bitrate devoted to the video instead of multiple (dub) audio tracks. *In some cases, Blu-rays from other countries can also have the best quality.*

These can be found on [U2](https://u2.dmhy.org) (Private), [Skyeysnow](https://skyeysnow.com) (Private, Open Signup) and [Nyaa](https://nyaa.si) (Public). *Release groups don't matter for BDMVs because they're all the same unless the files are corrupted.*

==- BDRemux
A losslessly packaged version of the BDMV put into `.mkv` files for ease. *Similar to BDMVs, they can have large file sizes.*

==- BDRip
An encode made directly from the BDMV/BDRemux. *These are for watching and are considered the best quality.*

Most can be found on [Nyaa](https://nyaa.si), with some rare stuff on places like [RuTracker](https://rutracker.org). The direct encode is usually a raw without subtitles and is used by muxers or fansubbers to make a release. 

Roughly:
- **Excellent Quality** - Beatrice-Raws, Kawaiika-Raws, Raws-Maji, SCY
- **Good Quality** - VCB-Studio
- **Okay Quality** - ANK-raws, LowPower-Raws, Moozzi2, Reinforce, Snow-raws

Fansub groups like Coalgirls, Commie, Doki, etc. also have their own encodes, but their main contribution is subtitles. These subtitles are taken by mux groups and combined with good video and multiple subtitle/audio options to make a complete release. Some will use their own encodes. *They'll usually mention all sources used for the release in the description.*

**Examples of good mux groups:** ARC, Arid, CTR, Drag, Kametsu, KH, Mysteria, NH, OZR, pog42, UDF, YURI

==- WEB-DL
Until the Blu-rays are released, this is the only source available for new airing anime. *This is not the same as a WEBRip.*

Reliable groups upload direct WEB-DLs from official streaming sources such as [Crunchyroll](https://www.crunchyroll.com) or [HIDIVE](https://www.hidive.com). The quality will be the same no matter which group releases these. *The most reliable one right now is SubsPlease.*

==- Re-encode
Re-encodes are encodes of a BDRip or WEB source. Quality is dependent on the source used. *A re-encode of a BDMV would typically have better quality than a re-encode of a BDRip.*

Re-encoding is generally considered a bad practice due to quality loss, as encoding is a lossy process, and information is lost at every stage. *On [Nyaa](https://nyaa.si), these are marked in red.*

==- Mini Encode
Mini encodes are releases designed to be space and bandwidth saving while retaining quality. *These are not the same as re-encodes, as they encode from the original BD or WEB source.*

For BD releases:
- **Good Quality** - Judas (new)
    - *Judas (new) is roughly mid 2020 and later.*
- **Decent Quality** - Akihitosubs, DB, Ember, Nep_Blanc
- **Bad Quality** - Cerberus, Cleo, Judas (old), Reaktor
    - *Cerberus and Reaktor often pick good subtitle sources.*
- **Worst Quality** - bonkai77, DaddySubs, DKB,  FFA, HR, SSA, Tenrai-Sensei, YuiSubs, and other groups using NVENC

For airing anime, we recommend sticking with SubsPlease or Erai-Raws (1080p and 720p), as they are generally better than most mini encodes of WEB releases.

See the comparisons below:
- [Jujutsu Kaisen S2](https://slow.pics/c/HZeCzBjs) - Web (SubsPlease 1080p/720p, VARYG) vs. Minis (A-L, Anime Time, ASW, Breeze, DKB, Judas, NanakoRaws, Sokudo, Valenciano
- [SukiMega](https://slow.pics/c/vpcExtLb) - Web (SubsPlease 1080p/720p) vs. Minis (Anime Time, ASW, DKB, EMBER, Judas, Valenciano)

===

### BD vs WEB

The BD is usually always a better source than WEB. *There's no reason to get a WEB-sourced encode once the BD is out.*

The advantages of BD and what you're missing out on with WEB are:

1. **Better Quality** - The BD episode is around 6GB compared to 1.3GB on WEB. This makes it a better source even for mini encodes, as they have more data to work with. *A same size encode made from both sources will show the BD version to be superior.*

2. **No Censorship** - The BDs will remove or reduce censorship that may be present on WEB releases, ranging from minor changes within scenes to whole new scenes being added.

3. **Additional Content** - A BD release allows the studios to fix any mistakes made due to budget/time/airing duration constraints and they can add as much content as they want.

4. **Improvements** - Fully redrawn scenes, extra details, shading improvements, removed dimming/brightness changes, etc.

See the comparisons below between BD and WEB:
- [Demon Slayer](https://slow.pics/c/UMxyTZ7T)
- [Saiki Kusuo no Ψ-nan](https://slow.pics/c/GxJxekoN)
- [Toaru Kagaku no Accelerator EP 5](https://slow.pics/c/Z0DF2PlI)
- [Toaru Kagaku no Accelerator EP 6](https://slow.pics/c/CpGp3GIv)

## Audio

### Codecs
Audio codecs are divided into lossless (DTS-HDMA, FLAC, TrueHD) and lossy formats (AAC, MP3, Opus). *While lossless raw video will be multiple GBs per minute, audio is more manageable when it comes to size, and you'll see many options with lossless audio.*

Lossless audio is typically unnecessary for most sound systems. Even with the best audiophile grade setup, most users will find it impossible to make out the difference. *However, some exceptional music samples exist, which can be used to differentiate lower-bitrate lossy audio by listening to small extracted parts repeatedly.* 

!!!
We suggest checking out these [online ABX tests](http://abx.digitalfeed.net) with your setup.

Alternatively, you can use the [ABX Comparator plugin for foobar2000](https://www.foobar2000.org/components/view/foo_abx) with [this guide for setting it up](https://www.head-fi.org/threads/setting-up-an-abx-test-simple-guide-to-ripping-tagging-transcoding.655879/#post-9268096.). ABX Comparator allows you to compare any two tracks and produces a verifiable log.

*If you manage to complete it with a decent probability, feel free to join our [Discord](https://discord.gg/snackbox) to talk about it!*
!!!

A good benchmark for audio bitrates (stereo/2.0) is:
- 128 kbps for Opus
- 160 kbps for AAC
- 192 kbps for MP3

For surround audio, multiply with the number of stereo pairs.

## Subtitles

Most anime releases will use `.ass` subtitles, as it allows for better styling options compared to others like `.srt`. *However, this styling can breaks when there is incompatibility somewhere in the playback process.*

Fansubs use a variety of fonts in their subtitles. These are bundled within the `.mkv` files as attachments or provided separately in a folder. Fonts included separately can be installed or placed in your player's fonts folder for a quick solution. They should be muxed in for perfect compatibility.

### Fansubs

!!!
Looking for the best fansub release? *Check out the following indexes:*
- [A Certain Fansubber's Index](https://index.fansubcar.tel)
- [SeaDex](https://releases.moe)
- [Sneedex](https://sneedex.moe)
!!!

Fansubs are fan-produced versions of subtitles. They can often be better than official subtitles as they retain the phrasing and style of the original media.

For most newer fansubbed anime, fansubs will often use an edited version of official subs from sources like [Crunchyroll](https://www.crunchyroll.com). *These edits range from removing excessive localization to reverting translations back to their original language.*

[AnimeTosho](https://animetosho.org) is a useful resource for grabbing just the subtitles from any release on [Nyaa](https://nyaa.si). They can be downloaded under the *Subtitles* section of a release.

![](https://files.catbox.moe/wbychc.gif)