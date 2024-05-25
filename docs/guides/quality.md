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

!!!
Looking for fansubbed seasonal anime? Check [FansubDB](https://fansubdb.com)'s list of release groups that are working on current shows.
!!!

## Releases

With so many options for every show, it can be hard to find which one to download.

Although most [re-encoded releases](#types-of-releases) (marked in <span style="color:#E53E3E">red</span> on [Nyaa](https://nyaa.si/help#torrent-colors)) should be avoided, the best release group varies per show, and there's no foolproof way to tell without [comparing the different sources](#quality-comparisons).

[SeaDex](https://releases.moe) is a quick and easy way to find the best releases for your shows. It is actively maintained to stay up-to-date on what's best, backed by quality comparisons.
