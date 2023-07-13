---
label: Quality
order: -1
description: Learn what to look for to get the best quality
image: https://user-images.githubusercontent.com/78981416/215166522-1d7358e8-bec2-4a54-a9ec-71deab646e56.gif
---

# Quality

## Video

==- Understanding the Different Parts

- **Container** - The file itself, containing various video, audio, and subtitle streams. *They are typically stored as `.mkv` or `.mp4`*
- **Codec** - The compression format used for the video stream and the biggest factor in compatibility with your system. *HEVC (H.265) and AVC (H.264) are the main ones*
- **Bit depth** - The maximum colors that can be stored in the video. *Typically in 8-bit, or 10-bit for high-quality anime encodes*
  - Converting an 8-bit source to 10-bit might seem counterintuitive if you know a little about transcoding and data loss, *[but it can give better results at smaller sizes](https://yukisubs.files.wordpress.com/2016/10/why_does_10bit_save_bandwidth_-_ateme.pdf)*
- **Frame rate** - The frequency at which frames are displayed. *This will usually be 23.976 fps*
  - Many TVs will use [interpolation](https://en.wikipedia.org/wiki/Motion_interpolation) to convert low frame rate content to a higher framerate like 60 fps, giving you an artificial sense of smoothness. *This is not recommended for anime and should be disabled in settings. 60 fps encodes should be avoided*
- **Level/profile** - The maximum resolution and bitrate specified within the AVC/HEVC standard. *A higher level/profile means lower compatibility and more processing power required to decode*
  - *See [Levels for AVC (H.264)](https://en.wikipedia.org/wiki/Advanced_Video_Coding#Levels) and [Levels for HEVC (H.265)](https://en.wikipedia.org/wiki/High_Efficiency_Video_Coding_tiers_and_levels#Levels)*

==- Reading the File Name

The file name for a release will typically look like one of the following examples:

![](https://files.catbox.moe/hn6135.png)

Key                                                        | Meaning
-----------------------------------------------------------|-----------------------------------------------------------
![#ed8796](https://placehold.co/14x14/ed8796/ed8796.png) 1 | The name of the group/person who created the release.
![#f5a97f](https://placehold.co/14x14/f5a97f/f5a97f.png) 2 | The title of the anime.
![#eed49f](https://placehold.co/14x14/eed49f/eed49f.png) 3 | The season/episode number.
![#a6da95](https://placehold.co/14x14/a6da95/a6da95.png) 4 | The source where the release was taken from. *Typically BD (Blu-ray/JPBD) or WEB.*
![#91d7e3](https://placehold.co/14x14/91d7e3/91d7e3.png) 5 | The resolution of the video file.
![#8aadf4](https://placehold.co/14x14/8aadf4/8aadf4.png) 6 | The video codec. *Typically HEVC or AVC.*
![#c6a0f6](https://placehold.co/14x14/c6a0f6/c6a0f6.png) 7 | The audio codec. *Dual audio means the release contains two audio streams, typically the dubbed and original.*
![#f4dbd6](https://placehold.co/14x14/f4dbd6/f4dbd6.png) 8 | The CRC32 checksum.
![#f5bde6](https://placehold.co/14x14/f5bde6/f5bde6.png) 9 | The container format. *Typically `.mkv` or `.mp4`.*

===

### Encoding

Quality is subjective to an extent, and there is a difference between flaws which need to be fixed and the preferences of the end user. Generally, the best release is an encode which fixes the flaws of a raw BDMV while simultaneously not altering the original material. There are objective flaws in videos called artifacts. The term “artifact” is used to broadly describe defects or foreign, unwanted elements in a video. There can be any number of causes ranging from lossy compression, improper conversions, to post-processing adjustments like sharpening and resampling. You can read about artifacts in detail [here](https://guide.encode.moe/encoding/video-artifacts.html). On the other hand, end user preferences like unnecessary sharpening or boosting contrast are destructive and go against the idea of a good encode.

The official BDMV is an encode of the original source master. As a result, it often suffers from issues like banding, blocking, noise, aliasing, etc. These can originate from a variety of reasons. For example, a lot of anime is natively 720p and is upscaled to 1080p for the Blu-ray. *Even if issues aren't present in the BDMV, they can appear in the encode due to compression.*

These issues can be fixed through filtering, a step which comes before encoding. The video is filtered with tools like [Vapoursynth](https://www.vapoursynth.com) before passing it on to the encoder. Since encoding is a lossy process, filtering is necessary to improve the video quality along with transparent encoder settings. This means that pretty much all unfiltered encodes are worse than the source.

See the links below for more information:

- [Examples of problems](http://bakashots.me/guide/index.php)
- [Encoding guides and more examples](https://guide.encode.moe/encoding/video-artifacts.html)
- [Advanced encoding guide](https://silentaperture.gitlab.io/mdbook-guide/introduction.html)
- [Mini encodes and audio](https://kokomins.wordpress.com/2019/10/10/anime-encoding-guide-for-x265-and-why-to-never-use-flac/)

!!!
The encoding guides above give you a general idea about things but are heavily outdated and incomplete
!!!

### Types of Releases

==- BDMV
A simple complete copy of the original Blu-ray, used as a source for making other releases or encodes. *These are not useful for watching.*

JPBDs often have better quality than USBDs because of more bitrate devoted to the video instead of multiple (dub) audio tracks. In some cases, Blu-rays from other countries can also have the best quality.

These can be found on [U2](https://u2.dmhy.org) (Private), [Skyeysnow](https://skyeysnow.com) (Private, Open Signup) and [Nyaa](https://nyaa.si) (Public). *Release groups don't matter for BDMVs because they're all the same unless the files are corrupted.*

==- BDRemux
A losslessly packaged version of the BDMV put into `.mkv` files for ease. Similar to BDMVs, they can have large file sizes.

==- BDRip
An encode made directly from the BDMV/BDRemux. Encodes can either be better than the source or be aimed at lower filesizes by sacrificing quality.

Most can be found on [Nyaa](https://nyaa.si), with some rare stuff on places like [RuTracker](https://rutracker.org). The direct encode is usually a raw without subtitles and is used by muxers or fansubbers to make a release.

==- WEB-DL
WEB-DL refers to a file losslessly ripped from a streaming service, such as Netflix, Amazon Video, Crunchyroll, etc. Until the Blu-rays are released, this is the only source available for new airing anime. WEB-DLs are a 1:1 copy of the official stream.

Reliable groups upload direct WEB-DLs from official streaming sources such as [Crunchyroll](https://www.crunchyroll.com) or [HIDIVE](https://www.hidive.com). The quality of a WEB-DL from the same service will be identical no matter which group released it. *The most reliable one right now is [SubsPlease](https://subsplease.org).*

WEB-DLs are usually your best source for most airing anime which you can get from [SubsPlease](https://subsplease.org) or [Erai-Raws](https://www.erai-raws.info/). Both of them rip Crunchyroll with the difference being SubsPlease only rips the english subs while Erai-Raws rips all the subs.

==- WEBRip
Unlike WEB-DLs, WEBRips are not a 1:1 copy of the official stream. Generally, most of the WEBRips you'll see will be lower quality than it's WEB-DL equivalent but this is not always true and there are groups that improve upon a WEB-DL by filtering it to fix issues.

==- Re-encode
Re-encodes are encodes of a BDRip or WEB source. Quality is dependent on the source used. A re-encode of a BDMV would typically have better quality than a re-encode of a BDRip.

Re-encoding is generally considered a bad practice due to quality loss, as encoding is a lossy process, and information is lost at every stage. *On [Nyaa](https://nyaa.si), these are marked in red.*

==- Mini Encode
Mini encodes are releases designed to be space and bandwidth saving while retaining quality. *These are not the same as re-encodes, as they encode from the original BD or WEB source.*

You can check out some mini encode comparisons below:

- [Jujutsu Kaisen S2](https://slow.pics/c/HZeCzBjs) - Web (SubsPlease/Crunchyroll 1080p/720p, VARYG/Netflix) vs. Minis (A-L, Anime Time, ASW, Breeze, DKB, Judas, NanakoRaws, Sokudo, Valenciano)
- [SukiMega](https://slow.pics/c/vpcExtLb) - Web (SubsPlease/Crunchyroll 1080p/720p) vs. Minis (Anime Time, ASW, DKB, EMBER, Judas, Valenciano)

===

### BluRay vs WEB

BluRays are generally always a better source than WEB. *Do note, there are cases where WEB is better than the BluRay for a variety of reasons but for the sake of this guide we are generalizing a bit.*

The advantages of BD and what you're missing out on with WEB are:

1. **Better Quality** - The BD episode is around 6GB compared to 1.3GB on WEB. This makes it a better source even for mini encodes, as they have more data to work with; *a same size encode made from both sources will show the BD version to be superior*

2. **No Censorship** - The BDs will remove or reduce censorship that may be present on WEB releases, ranging from minor changes within scenes to whole new scenes being added

3. **Additional Content** - A BD release allows the studios to fix any mistakes made due to budget/time/airing duration constraints and they can add as much content as they want

4. **Improvements** - Fully redrawn scenes, extra details, shading improvements, removed dimming/brightness changes, etc.

See the comparisons below between BD and WEB:

- [Demon Slayer](https://slow.pics/c/UMxyTZ7T)
- [Saiki Kusuo no Ψ-nan](https://slow.pics/c/GxJxekoN)
- [Toaru Kagaku no Accelerator EP 5](https://slow.pics/c/Z0DF2PlI)
- [Toaru Kagaku no Accelerator EP 6](https://slow.pics/c/CpGp3GIv)

## Audio

### Codecs

Audio codecs are divided into lossless (DTS-HD MA, FLAC, TrueHD) and lossy formats (AAC, MP3, Opus). While lossless raw video will be multiple GBs per minute, audio is more manageable when it comes to size, and you'll see many options with lossless audio.

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

Fansubs are fan-produced versions of subtitles. They will often edit the official subs from sources like [Crunchyroll](https://www.crunchyroll.com) and add things like opening/ending lyrics, song translations, typesetted signs, etc.

Fansub edits can either be an improvement or they might introduce more errors than they fix. There's also the topic of localization. Localization means changing cultural references and puns to fit the english context. A degree of localization is always involved with translation to make sure the dialogue flows smoothly. Fansubs can range from excessive localization by completely eradicating and replacing the idea of Japan to untranslation by changing random phrases and lines back to Japanese making the very concept of subtitles useless. Most of the good fansubs lie somewhere in between these extremes. After looking at the work of a few groups, you'll be able to figure out who has a tendency to do what.

[AnimeTosho](https://animetosho.org) is a useful resource for grabbing just the subtitles from any release on [Nyaa](https://nyaa.si). They can be downloaded under the *Subtitles* section of a release. *Do note that usually only uploads that are less than 16GB will have downloadable attachments on AnimeTosho.*

==- Using AnimeTosho
![AnimeTosho](https://files.catbox.moe/wbychc.gif)

===

## How do I find the best release?

With so many options for every show, it can be hard to find which one to download. Although releases marked red on Nyaa means that the release is bad, the opposite isn't true. A green release does not mean it's a good release. The best release group varies per show and there's really no way to tell without actually comparing the different sources ([example](https://slow.pics/c/J8ow4zap)).

To find the best release of a show, you can visit: [!button variant="info" icon="https://files.catbox.moe/6fax26.png" target="blank" size="xs" text="SeaDex"](https://releases.moe/)

SeaDex is an actively maintained sheet of best releases. Different sources are first compared and then a release is picked and added on the sheet.
