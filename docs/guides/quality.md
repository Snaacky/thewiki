---
label: Quality
order: -1
description: Learn what to look for to get the best quality
image: https://user-images.githubusercontent.com/78981416/215166522-1d7358e8-bec2-4a54-a9ec-71deab646e56.gif
---

# Quality

## Video

Commonly used terminology:

- **Container** - The file itself, containing various video, audio, and subtitle streams. *They are typically stored as `.mkv` or `.mp4`*
- **Codec** - The compression format used for the video stream and the biggest factor in compatibility with your system. *HEVC (H.265) and AVC (H.264) are the main ones*
- **Bit depth** - The maximum colors that can be stored in the video. *Typically in 8-bit, or 10-bit for high-quality anime encodes*
  - Converting an 8-bit source to 10-bit might seem counterintuitive, *[but it can give better results at smaller sizes](https://yukisubs.files.wordpress.com/2016/10/why_does_10bit_save_bandwidth_-_ateme.pdf)*
- **Frame rate** - The frequency at which frames are displayed. *This will usually be close to 23.976 fps (24000/1001)*
  - Many TVs will use [interpolation](https://en.wikipedia.org/wiki/Motion_interpolation) to convert low frame rate content to a higher frame rate like 60 fps, giving you an artificial sense of smoothness. *This is not recommended for anime and should be disabled in settings. 60 fps encodes should also be avoided*
- **Level/profile** - The maximum resolution and bitrate specified within the AVC/HEVC standard. *A higher level/profile means lower compatibility and more processing power required to decode*
  - *See [Levels for AVC (H.264)](https://en.wikipedia.org/wiki/Advanced_Video_Coding#Levels) and [Levels for HEVC (H.265)](https://en.wikipedia.org/wiki/High_Efficiency_Video_Coding_tiers_and_levels#Levels)*

==- :icon-info: Understanding the file name

The file name for a release will typically look like one of the following:

![Example file names of common releases](/static/quality/example-releases.png)

Key                                                        | Meaning
-----------------------------------------------------------|-----------------------------------------------------------
![#ed8796](https://placehold.co/14x14/ed8796/ed8796.png) 1 | The name of the group/person who created the release
![#f5a97f](https://placehold.co/14x14/f5a97f/f5a97f.png) 2 | The title of the anime
![#eed49f](https://placehold.co/14x14/eed49f/eed49f.png) 3 | The season/episode number
![#a6da95](https://placehold.co/14x14/a6da95/a6da95.png) 4 | The source where the video was taken from. *Typically BD (Blu-ray) or WEB*
![#91d7e3](https://placehold.co/14x14/91d7e3/91d7e3.png) 5 | The resolution of the video stream
![#8aadf4](https://placehold.co/14x14/8aadf4/8aadf4.png) 6 | The video codec. *Typically HEVC or AVC*
![#c6a0f6](https://placehold.co/14x14/c6a0f6/c6a0f6.png) 7 | The audio codec. *Dual audio means the release contains two audio streams, typically the dubbed and original*
![#f4dbd6](https://placehold.co/14x14/f4dbd6/f4dbd6.png) 8 | The CRC32 checksum
![#f5bde6](https://placehold.co/14x14/f5bde6/f5bde6.png) 9 | The container format. *Typically `.mkv` or `.mp4`*

===

### Encoding

Quality is subjective to an extent, and there is a difference between flaws that need to be fixed and the preferences of the end user.

Generally, the best release is an encode which fixes the flaws of an untouched Blu-ray (BDMV) while simultaneously not altering the original material. There are objective flaws called **artifacts**, a term used to broadly describe defects or foreign, unwanted elements in a video. There can be several causes, ranging from lossy compression, improper conversions, to post-processing adjustments like sharpening and resampling. *You can read about artifacts in detail [here](https://guide.encode.moe/encoding/video-artifacts.html).* On the other hand, end user preferences like excessive sharpening or contrast are destructive and go against the idea of a good encode.

The official BDMV is an encode of the original source master. Subsequently, it often suffers from various issues (e.g. aliasing, banding, blocking, noise) originating due to a variety of reasons. For example, a lot of anime is natively 720p and is upscaled to 1080p for the Blu-ray. *Even if issues aren't present in the BDMV, they can appear in an encode due to compression.*

These issues can be fixed through filtering, a step which comes before encoding. The video is filtered with tools like [VapourSynth](https://github.com/vapoursynth/vapoursynth) before passing it on to the encoder. Since encoding is a lossy process, filtering is necessary to improve the video quality along with transparent encoder settings. *As a result, any unfiltered encodes are by definition worse than the source.*

See the links below for more information:

- [Examples of problems](http://bakashots.me/guide/index.php)
- [Encoding guides and more examples](https://guide.encode.moe/encoding/video-artifacts.html)
- [Advanced encoding guide](https://silentaperture.gitlab.io/mdbook-guide/introduction.html)
- [Mini encodes and audio](https://kokomins.wordpress.com/2019/10/10/anime-encoding-guide-for-x265-and-why-to-never-use-flac/)

!!!warning
The encoding guides above should give you a general idea of the workflow, *but do note they are heavily outdated and incomplete.*
!!!

### Types of Releases

+++ BDMV

Also known as: [!badge variant="info" text="Blu-ray Disc" margin="0 8 0 0"]
[!badge variant="info" text="Untouched Blu-ray"]

A complete copy of the original Blu-ray used as a source for making other releases or encodes, including menus, extras, adverts, etc. *These are not practical for watching.*

Japanese Blu-rays often have better quality than other countries as they allocate far more bitrate to the video. This is not necessarily intentional, but a side effect of having fewer episodes per disc and not including dub tracks, in contrast to other regions which tend to cram more episodes onto a single disc and have large 5.1 dubs.

There are cases where regional discs do offer better quality. This generally applies when Japanese discs have authoring issues, resulting in the alternatives being better by comparison. For instance, Italian discs from Dynit frequently offer the best video even at lower bitrates due to good filtering and better-optimized encode settings.

These can be mainly found on [U2](https://u2.dmhy.org) (private), however [Nyaa](https://nyaa.si) (public) and [Skyey Snow](https://skyeysnow.com) (private, open signup) will have some too. *Release groups don't matter for BDMVs because they are 1:1 copies of the disc and should instead be chosen based on region.*

+++ BD Remux

Also known as: [!badge variant="info" text="Remux" margin="0 8 0 0"]
[!badge variant="info" text="Blu-ray Remux"]

A losslessly packaged version of the BDMV put into `.mkv` files, allowing for slightly lower sizes (`.mkv` files have less overhead than `.m2ts`), much better ease of use, lossless compression of audio, and bundling tracks from other sources.

A Blu-ray remux is generally the best version to get quality-wise, unless the original Blu-ray introduces several problems and/or a properly filtered release exists.

+++ BD Encode

Also known as: [!badge variant="info" text="BD" margin="0 8 0 0"]
[!badge variant="info" text="BDRip" margin="0 8 0 0"]
[!badge variant="info" text="Blu-ray"]

An encode made directly from the original Blu-ray or remux. Generally, the goal is to retain visual transparency to the source while reducing file size, however many good encoders will aim to make their release better than the source via filtering.

While Blu-ray remuxes are generally the best option, encodes can sometimes be more preferred over remuxes depending on the show, such as fixing video artifacts introduced from a bad Blu-ray. Additionally, they tend to be considerably smaller over remuxes (around 1-2 GB an episode vs. 6GB), without sacrificing too much in quality compared to other sources such as mini encodes.

Most can be found on [Nyaa](https://nyaa.si), with some rare stuff on places like [RuTracker](https://rutracker.org).

+++ WEB-DL

A WEB-DL is a file losslessly downloaded from an official streaming service, such as Amazon, Crunchyroll, HIDIVE, Netflix, etc. As a result, they are a 1:1 copy of the official stream.

Until the Blu-rays are released, this is the only and best source available for most airing anime. The quality of a WEB-DL from the same service will be identical no matter which group releases it. *Some exceptions exist, such as Netflix with multiple quality profiles and Amazon with multiple regions/services.*

WEB-DLs can be obtained from groups like [SubsPlease](https://subsplease.org) or [Erai-Raws](https://www.erai-raws.info). Both rip from [Crunchyroll](https://www.crunchyroll.com) and have fast release times, with the difference being that SubsPlease rips only the English subs, while Erai-Raws rips all the subs.

+++ WEBRip

WEBRips are transcodes of the official stream. This terminology is broad and is associated with either screen recordings of the content or encodes of a WEB-DL. *Because of this, WEBRips are generally not recommended, especially if a WEB-DL already exists.*

Most WEBRips will be lower quality than their WEB-DL equivalent. However, some groups will attempt to improve upon a WEB-DL by filtering it to fix issues.

Some encoders will merge multiple WEB sources, resulting in significantly better quality than a single source, and sometimes even beating Blu-rays.

+++ Re-encode

Re-encodes are encodes of an existing Blu-ray or WEB encode. These are common on [streaming sites](/sourcing/streaming/), where they convert the original encode to a supported file format while also trying to save space, but can also be found in some mini encodes.

The process of re-encoding is generally considered a bad practice due to quality loss, as encoding is a lossy process and information is lost at every stage. As a result, it introduces artifacts like blocking and banding. *Because of this, re-encodes are not recommended if you want the best quality and should be avoided.*

On [Nyaa](https://nyaa.si), these are marked in <span style="color:#E53E3E">red</span>. *You can see the difference with re-encodes from the comparisons below.*

+++ Mini Encode

Mini encodes are releases designed to save on space and bandwidth while retaining some quality. *These are not the same as re-encodes, as some minis encode from the original BD or WEB source.*

While re-encoding is a bad process, re-encoded minis will often use the original WEB source. With Blu-rays, some will use the best encode/raw available for their release. They also usually opt for alternative and efficient codecs such as HEVC and apply various filters to compensate. As a result, they can be a better option when compared to [streams](/sourcing/streaming/) of similar size or bad encodes.

Some may not notice the differences between a good mini encode and its alternatives. *See how minis compare in the quality comparisons below.*

+++

==- :icon-file-media: Quality Comparisons

!!!
Most streams and minis are re-encodes. These are marked in <span style="color:#E53E3E">red</span>. *See [Types of Releases](#types-of-releases).*
!!!

{.compact}
Show                                                      | Type                                    | Sources
----------------------------------------------------------|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
[Dokyuu Hentai HxEros](https://slow.pics/c/PZRxqAsh)      | [!badge variant="primary" text="BD"]    | **Raw:** Reinforce <br>**Official:** HorribleSubs/Crunchyroll <br>**Stream:** <span style="color:#E53E3E">animepahe</span>, <span style="color:#E53E3E">AniWave (9anime)</span>, <span style="color:#E53E3E">Anitaku (Gogoanime)</span> <br>**Mini:** <span style="color:#E53E3E">EMBER</span>
[Jujutsu Kaisen S2](https://slow.pics/c/HZeCzBjs)         | [!badge variant="secondary" text="WEB"] | **Official:** NanakoRaws, SubsPlease/Crunchyroll 1080p/720p, VARYG/Netflix <br>**Mini:** <span style="color:#E53E3E">A-L</span>, <span style="color:#E53E3E">Anime Time</span>, <span style="color:#E53E3E">ASW</span>, Breeze, DKB, <span style="color:#E53E3E">Judas</span>, Sokudo, <span style="color:#E53E3E">Valenciano</span>
[Masamune-kun no Revenge R](https://slow.pics/c/rj3QjRMA) | [!badge variant="secondary" text="WEB"] | **Encode:** smol <br>**Official:** SubsPlease/Crunchyroll <br>**Stream:** <span style="color:#E53E3E">animepahe</span>, <span style="color:#E53E3E">HiAnime (AniWatch)</span>, <span style="color:#E53E3E">AniWave</span>, <span style="color:#E53E3E">Anitaku (Gogoanime)</span> <br>**Mini:** <span style="color:#E53E3E">ASW</span>, DKB, <span style="color:#E53E3E">EMBER</span>, <span style="color:#E53E3E">Judas</span>
[Oshi no Ko](https://slow.pics/c/6HqApHsn)                | [!badge variant="secondary" text="WEB"] | **Encode:** Setsugen <br>**Official:** SubsPlease/Crunchyroll <br>**Stream:** <span style="color:#E53E3E">HiAnime (Zoro)</span>, <span style="color:#E53E3E">AniWave (9anime)</span>, <span style="color:#E53E3E">Anitaku (Gogoanime)</span>
[Senran Kagura](https://slow.pics/c/QLtX61qx)             | [!badge variant="primary" text="BD"]    | **Raw:** BDMV <br>**Encode:** Snow-Raws <br>**Stream:** <span style="color:#E53E3E">animepahe</span>, <span style="color:#E53E3E">HiAnime (Zoro)</span>, <span style="color:#E53E3E">AniWave (9anime)</span>, <span style="color:#E53E3E">Anitaku (Gogoanime)</span>
[Space Dandy](https://slow.pics/c/d5hU8mnp)               | [!badge variant="primary" text="BD"]    | **Encode:** MTBB <br>**Mini:** <span style="color:#E53E3E">AnimeRG</span>, Cleo 1080p/720p, Commie 720p, DHD
[SukiMega](https://slow.pics/c/vpcExtLb)                  | [!badge variant="secondary" text="WEB"] | **Official:** SubsPlease/Crunchyroll 1080p/720p <br>**Mini:** <span style="color:#E53E3E">Anime Time</span>, <span style="color:#E53E3E">ASW</span>, DKB, <span style="color:#E53E3E">EMBER</span>, <span style="color:#E53E3E">Judas</span>, <span style="color:#E53E3E">Valenciano</span>
[Vinland Saga S2](https://slow.pics/c/GjhwBwo3)           | [!badge variant="secondary" text="WEB"] | **Encode:** Foxtrot <br>**Official:** SubsPlease/Crunchyroll <br>**Stream:** <span style="color:#E53E3E">Anitaku (Gogoanime)</span> <br>**Mini:** <span style="color:#E53E3E">Anime Time</span>, <span style="color:#E53E3E">ASW</span>, Breeze, DKB, <span style="color:#E53E3E">EMBER</span>, <span style="color:#E53E3E">Judas</span>, <span style="color:#E53E3E">Trix</span>

===

### Blu-ray vs WEB

Generally, Blu-rays are usually better overall compared to WEB sources. With Blu-rays, you can expect improvements such as:

1. **Better Quality** - An average Blu-ray episode is around 6GB compared to 1.3GB for a WEB episode. This makes it a better source even for mini encodes, as they have more data to work with; *a same size encode made from both sources will show the Blu-ray version to be superior*

2. **No Censorship** - Blu-rays will often remove or reduce censorship that may be present on WEB releases, ranging from minor changes within scenes to whole new scenes being added

3. **Additional Content** - Blu-rays allow studios to add as much content as they want and fix any previous mistakes made in the WEB release due to budget/time/airing duration constraints

4. **Improvements** - Fully redrawn scenes, add extra details, introduce shading improvements, dimming/brightness changes, etc.

However, there are some cases where a WEB source will outperform the Blu-ray. A bad Blu-ray may introduce blurring, use aggressive sharpening filters, or include other visual artifacts that may affect your viewing experience.

See the comparisons between Blu-ray and WEB sources:

- [Demon Slayer](https://slow.pics/c/UMxyTZ7T)
- [Saiki Kusuo no Ψ-nan](https://slow.pics/c/GxJxekoN)
- [Toaru Kagaku no Accelerator EP 5](https://slow.pics/c/Z0DF2PlI)
- [Toaru Kagaku no Accelerator EP 6](https://slow.pics/c/CpGp3GIv)

## Audio

### Codecs

Audio codecs are divided into lossless (DTS-HD MA, FLAC, TrueHD) and lossy formats (AAC, MP3, Opus). While lossless video will be multiple GBs per minute, lossless audio is more manageable when it comes to size, and you'll see many releases utilizing it.

Lossless audio is typically unnecessary for the majority of sound systems. Even with the best audiophile-grade setup, most users will find it impossible to make out the difference between lossless and good lossy audio. *However, some exceptional music samples exist, which can be used to differentiate lower-bitrate lossy audio by listening to small extracted parts repeatedly.*

!!!
We suggest checking out these [online ABX tests](http://abx.digitalfeed.net) with your setup.

Alternatively, you can use the [ABX Comparator plugin for foobar2000](https://www.foobar2000.org/components/view/foo_abx) with [this guide for setting it up](https://www.head-fi.org/threads/setting-up-an-abx-test-simple-guide-to-ripping-tagging-transcoding.655879/#post-9268096.). ABX Comparator allows you to compare any two tracks and produce a verifiable log.

*If you manage to complete it with a decent probability, feel free to join our [Discord](https://discord.gg/snackbox) to talk about it!*
!!!

A good baseline for lossy audio bitrates is:

Format | Stereo (2.0, 2 channels) | Surround (5.1, 6 channels)
-------|--------------------------|-----------------------------
AAC    | 160 kb/s                 | 480 kb/s
MP3    | 192 kb/s                 | 576 kb/s
Opus   | 128 kb/s                 | 384 kb/s

!!!info
For surround audio, multiply them by the number of stereo pairs (2 channels).
!!!

## Subtitles

Most anime releases will use `.ass` subtitles, as it allows for better styling options compared to alternatives like `.srt`. *However, this styling breaks when there is incompatibility somewhere during playback.*

Fansubs use a variety of fonts in their subtitles. These are embedded within the `.mkv` files as attachments or provided separately in a folder. Fonts included separately can be installed on your system or placed in your player's fonts folder for a quick solution. They should be muxed in for perfect compatibility.

### Fansubs

Fansubs are fan-produced versions of subtitles. They will often edit the official subs from sources like [Crunchyroll](https://www.crunchyroll.com) and perform modifications such as retiming, adding OP/ED translations, typesetting signs, restyling, etc. Often they will also edit the dialogue to make it more/less localized (depending on the political persuasion of the group) and fix any perceived issues. Some fansubbers will opt for an OTL (original translation), meaning they make the entire track from scratch.

While these edits can be an improvement over the original subs, *they may also introduce more errors than they fix.* A degree of localization is always involved with translation to make sure the dialogue flows smoothly. Localization is the process of changing cultural references and puns to fit the target audience's context. Fansubs can range from excessive localization by completely eradicating and replacing the idea of Japan, to the untranslation of random phrases and lines back to Japanese, damaging the concept of subtitles. Most good fansubs lie somewhere in between these extremes. After looking at the work of these groups, you'll be able to figure out who tends to do what.

==- :icon-desktop-download: Getting subtitles with AnimeTosho

[AnimeTosho](https://animetosho.org) is a useful resource for grabbing subtitles from any release on [Nyaa](https://nyaa.si). *Note that only uploads that are less than 16GB will have downloadable attachments on AnimeTosho.*

They can be downloaded under the *Subtitles* section of a release:

![AnimeTosho](/static/quality/animetosho.gif)

===

## Airing releases

With so many releases available for each show, choosing the best one can be overwhelming. Here is a general guide to recommended release groups  
For completed shows you should instead check [SeaDex](https://releases.moe).  It is actively maintained to stay up-to-date on what's best, backed by quality comparisons

### Notable WEB-DL Groups
<small>This information is current as of 13th August 2026 and applies to recent releases from around this period onward. Older rips **will** have issues not mentioned here</small>

{.compact}
| Group | Pub | LayRes | Style | Matrix | Name | Chap | Audio | Dub | Sub | Source | Majin | Notes |
|:------|:------:|:---------:|:-------:|:------:|:------:|:--------:|:-----:|:---:|:---------:|:------:|:-----:|:------|
| AnoZu | :x: | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark: | Native<br>*AMZN* | Dual | All | CR | 7.5Mb/s+ | Primarily on Seedpool<br>AMZN audio is only for dual releases |
| Yameii | :white_check_mark: | :white_check_mark: | N/A | :white_check_mark: | :white_check_mark: | :white_check_mark: | Native | Dub | SDH | CR | Never | CR: Rips the dub stream (sometimes different) |
| ToonsHub | :white_check_mark: | :white_check_mark: | :white_check_mark:- | :white_check_mark: | :white_check_mark: | :white_check_mark: | Native | Dual | All | All | 7.1Mb/s+ | Restyle sometimes messes with TS |
| Kitsune | :x: | :white_check_mark: | :white_check_mark:- | :white_check_mark: | :white_check_mark: | :white_check_mark: | AMZN | Dual | All | All | Never | Aither internal<br>Restyle frequently messes with TS |
| VARYG | :white_check_mark: | :white_check_mark: | :x: | :white_check_mark: | :white_check_mark: | :white_check_mark: | Native | Dual | All | All | 8Mb/s+ | Almost always has missing/wrong fonts<br>Naming occasionally broken |
| SubsPlease | :white_check_mark: | :x: | :white_check_mark:- | :white_check_mark: | :x: | :x: | Native- | Sub | Eng | CR<br>HIDI | Never | Restyle sometimes messes with TS<br>CR: Uses lower quality AAC 128Kbps |
| Erai-raws | :white_check_mark: | :white_check_mark: | :x: | :x: | :x: | :white_check_mark: | Native+ | Sub | All | All | 6Mb/s+? | Slow to start ripping new seasons<br>NF: Uses higher quality xHE-AAC 192Kbps |

<small>

**Public:** Releases publicly available on Nyaa, non-public releases can be found on the private trackers listed in the notes  
**LayoutRes:** Required for typesetting to display correctly. Releases without LayoutRes set will have broken perspective. See [Adding LayoutRes to ASS Files](/advanced/muxing/#adding-layoutres-to-ass-files)  
**Restyle:** Changes the default font to something more visually appealing. See [Anime WEB-DL Groups Font comparison](https://slow.pics/c/lez5c1GP)  
**Matrix:** The subtitle colorspace, not setting this will result in wrong colors for subtitles authored in BT.709 (generally foreign subs)  
**Naming:** Uses official titles only and includes useful information such as the source, release type, and codecs  
**Majin:** Experimental CR stream; quality varies by title. Bitrate shown is the minimum to prefer Majin over 8Mbps. Groups not bitrate gating will have inferior quality
</small>

### Notable WEB-DL Fixers
These groups fix issues with web sources and should always be picked over the untouched source

{.compact}
| Group | Notes |
|:------|:------|
| **SubsPlus+** | Automated improvements to HiDive subtitles, muxed with a manually selected superior video source |
| **Unfucked**, **Subsmix**, **TSPlease** | Merges typesetting from other languages when CR doesn't have any<br>See [Crunchyroll is destroying its subtitles](https://daiz.moe/crunchyroll-is-destroying-its-subtitles-for-no-good-reason/) article and [Crunchyroll Pipelines & Formats](https://docs.google.com/spreadsheets/d/1YSXp5jxPE4LMAyaPH5sl9xXbZiSjnD_zRRZbZ26Rk44/htmlview#gid=221235503) spreadsheet |


### Notable Fansub Groups
The notable currently active fansub groups, these releases generally improve on the source's video quality, subtitle quality, or both

{.compact}
| | | |
|:---|:---|:---|
| **9volt**<br>[Nyaa](https://nyaa.si/?f=0&c=0_0&q=%22%5B9volt%5D%22) · [nekoBT](https://nekobt.to/groups/4398244388404) | **Asakura**<br>[Nyaa](https://nyaa.si/?f=0&c=0_0&q=%22%5BAsakura%5D%22) · [nekoBT](https://nekobt.to/groups/7310842786873) | **BlackRose**<br>[Nyaa](https://nyaa.si/?f=0&c=0_0&q=%22%5BBlackRose%5D%22) · [nekoBT](https://nekobt.to/groups/9496112614974) |
| **cappybara**<br>[Nyaa](https://nyaa.si/?f=0&c=0_0&q=%22%5BCappybara%5D%22) · [nekoBT](https://nekobt.to/groups/4528651404085) | **Chihiro**<br>[Nyaa](https://nyaa.si/?f=0&c=0_0&q=%22%5BChihiro%5D%22) · [nekoBT](https://nekobt.to/groups/7513738503473) | **Commie**<br>[Nyaa](https://nyaa.si/?f=0&c=0_0&q=%22%5BCommie%5D%22) · [nekoBT](https://nekobt.to/groups/7434523383604) |
| **CrappySubs**<br>[Nyaa](https://nyaa.si/?f=0&c=0_0&q=%22%5BCrappySubs%5D%22) · [nekoBT](https://nekobt.to/groups/7657200696888) | **Cyan**<br>[Nyaa](https://nyaa.si/?f=0&c=0_0&q=%22%5BCyan%5D%22) · [nekoBT](https://nekobt.to/groups/4529225239867) | **derpie**<br>[Nyaa](https://nyaa.si/?f=0&c=0_0&q=%22%5Bderpie%5D%22) · [nekoBT](https://nekobt.to/groups/4637277057591) |
| **FLE**<br>[Nyaa](https://nyaa.si/?f=0&c=0_0&q=%22%5BFLE%5D%22) · [nekoBT](https://nekobt.to/groups/4661198055985) | **Freehold**<br>[Nyaa](https://nyaa.si/?f=0&c=0_0&q=%22%5BFreehold%5D%22) · [nekoBT](https://nekobt.to/groups/7271873158193) | **GJM**<br>[Nyaa](https://nyaa.si/?f=0&c=0_0&q=%22%5BGJM%5D%22) · [nekoBT](https://nekobt.to/groups/4416382069043) |
| **Half-Baked**<br>[Nyaa](https://nyaa.si/?f=0&c=0_0&q=%22%5BHalf-Baked%5D%22) | **Kaizoku**<br>[Nyaa](https://nyaa.si/?f=0&c=0_0&q=%22%5BKaizoku%5D%22) · [nekoBT](https://nekobt.to/groups/8438338633525) | **Kaleido-subs**<br>[Nyaa](https://nyaa.si/?f=0&c=0_0&q=%22%5BKaleido-subs%5D%22) · [nekoBT](https://nekobt.to/groups/4397444928050) |
| **Lazyleido**<br>[Nyaa](https://nyaa.si/?f=0&c=0_0&q=%22%5BLazyleido%5D%22) · [nekoBT](https://nekobt.to/groups/6220083224625) | **MTBB**<br>[Nyaa](https://nyaa.si/?f=0&c=0_0&q=%22%5BMTBB%5D%22) · [nekoBT](https://nekobt.to/groups/7364000585776) | **RaiN**<br>[Nyaa](https://nyaa.si/?f=0&c=0_0&q=%22%5BRaiN%5D%22) |
| **Reza**<br>[Nyaa](https://nyaa.si/user/Reza27) | **Salchow**<br>[Nyaa](https://nyaa.si/?f=0&c=0_0&q=%22%5BSalchow%22) · [nekoBT](https://nekobt.to/groups/7310206892857) | **sam**<br>[Nyaa](https://nyaa.si/?f=0&c=0_0&q=%22%5Bsam%5D%22) · [nekoBT](https://nekobt.to/groups/7466298080819) |
| **sgt**<br>[Nyaa](https://nyaa.si/?f=0&c=0_0&q=%22%5Bsgt%5D%22) · [nekoBT](https://nekobt.to/groups/8111921135162) | **Starbez**<br>[Nyaa](https://nyaa.si/user/Starbez) · [nekoBT](https://nekobt.to/groups/7292250934068) | **Vodes**<br>[Nyaa](https://nyaa.si/?f=0&c=0_0&q=%22%5BVodes%5D%22) · [nekoBT](https://nekobt.to/groups/5949517558321) |

<small>

Check [FansubDB](https://fansubdb.com)'s list of release groups that are working on current shows.
</small>

### Web tier list
{.compact}
| Tier | Source | Bitrate | Audio | Subs | Notes |
|:----:|:-------|:-------:|:-----:|:----:|:------|
| 1 | Crunchyroll(**CR**) | 2pass: 8Mb/s<br>Majin: VBR | AAC:192Kb/s | ASS | New "Majin" VBR stream generally offers better quality unless the bitrate is much lower |
| 2 | Disney+(**DSNP**) | VBR | AAC:128Kb/s | SRT | Generally the best option when CR doesn't have the license |
| 3 | Netflix(**NF**) | 2pass: 5Mb/s | AAC128Kb/s<br>*AAC:192Kb/s* | SRT | Has different streams for DV (Generally best), HDR, HEVC, AV1<br>xHE-AAC 192Kbps audio stream is typically unripped due to bad compatibility | 
| 4 | Amazon(**AMZN**) | VBR | DDP:224Kb/s | SRT | Has many different services and streams that can offer wildly different quality<br>Tends to be much more competitive in grainy shows where it offers higher bitrates |
| 5 | HIDIVE(**HIDI**) | 2pass: 5.3Mb/s | AAC:128Kb/s | ASS | Wrong frame rate (24.0fps) resulting in a dupe frame (stutter) every 1000 frames (42 seconds)<br>Uses a very bad AAC encoder resulting in much worse quality than other services |
