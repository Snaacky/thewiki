---
label: Quality
order: -1
---

# Video

**Container** - .mkv and.mp4, these contain various video, audio, and subtitle streams within them. Even the exact same video can be placed in different containers. You can losslessly convert between them with [ffmpeg](https://ffmpeg.org/download.html).

**Codec** - This is what most people mean when they confuse it with the container. The codec is the biggest factor in compatibility with your hardware and system. HEVC(h265/x265) and AVC(h264/x264) are the main ones.

**Bit depth** - 8-bit/10-bit - Most high quality anime encodes are 10-bit. Converting an 8-bit source to 10-bit might seem counterintuitive if you know a little about transcoding and data loss, [but it gives a better result at smaller sizes.](https://yukisubs.files.wordpress.com/2016/10/why_does_10bit_save_bandwidth_-_ateme.pdf)

**Frame rate** - This will usually be 23.976 FPS. Many TVs use [interpolation](https://en.wikipedia.org/wiki/Motion_interpolation) to turn this into 60 fps giving you an artificial sense of smoothness. This is not recommended for anime and should be disabled in settings. 60 FPS encodes are even worse and should be avoided.

**Level/Profile** - These are specifications within the h264/h265 standard which give an idea of compatibility and specify the maximum resolution and bitrate, for example h264 4.0 = 1080p 30fps 20 Mbps. Higher level/profile = lower compatibility = more processing power needed to decode.

The full information for a video will look like -
x264 &emsp;&emsp;High &emsp; &emsp;10 &emsp;&emsp;&emsp;&emsp;.mkv
(codec)&emsp;(profile)&emsp;(bit depth)&emsp;(container)

## Quality

Quality might be somewhat subjective. The best release for the purpose of this guide is an encode which fixes flaws of the raw bdmv while simultaneously not altering the original material too much. There is a difference between obvious flaws which need to be fixed and your subjective preference of how you like the encode. If something apparently looks "better" because it unnecessarily alters the video, then it's not a good indication of quality. These statements might seem too complicated or a bit vague, I'll try to explain in brief, but this is beyond the scope of a guide for beginners and more into the realm of encoding.

The official BDMV, from the blurays, is itself an encode of the actual source. It often suffers from issues like banding, blocking, noise, aliasing, etc. These might be originating from a variety of reasons. For example, a lot of anime is native 720p and is upscaled to 1080p for the bluray. Even if issues aren't present in the bdmv, they can appear in the encode because of compression. This fixing is called filtering, a step which comes before encoding. The video is filtered with Avisynth/Vapoursynth before passing it on to the encoder.

- [Examples of problems](http://bakashots.me/guide/index.php)

- [Encoding guides and more examples](https://guide.encode.moe/encoding/video-artifacts.html)

- [Advanced encoding guide](https://silentaperture.gitlab.io/mdbook-guide/introduction.html)

- [Mini encodes and audio](https://kokomins.wordpress.com/2019/10/10/anime-encoding-guide-for-x265-and-why-to-never-use-flac/)

## Types of releases

**BDMV** - A simple complete copy of the bluray. It's used as a source for making another release or encoding, this is not useful for watching.

These can be found on U2 (Chinese-Private), Skyeysnow (Chinese-Open) and nyaa (usually without seeds). Release groups don't matter for bdmvs because they're all the same unless the files are corrupted. JPBDs often have better quality than USBD because of more bitrate devoted to the video instead of multiple(dub) audio tracks. In some cases, blurays from Italy or other countries might have the best quality.

**BDRemux** - The BDMV is losslessly packaged into mkv files for ease. The sizes are huge, just like bdmvs.

**BDRip** - An encode made directly from the BDMV/BDRemux. This is the only kind of encode considered good quality.

Most can be found on nyaa with some rare stuff on rutracker(Russian). The direct encode is usually a raw without subtitles and is used by muxers or fansubbers to make a release. The groups can be roughly rated as -

- Excellent - Beatrice-Raws, Kawaiika-Raws, SCY, Raws-Maji

- Good - VCB-Studio

- Okay - Snow-raws, ANK-raws, Reinforce, LowPower-Raws, Moozzi2

Fansub groups like Coalgirls, Commie, Doki etc. also have their own encodes, but their main contribution is subtitles. These subtitles are taken by mux groups and combined with good video and multiple subtitle/audio options to make a complete release. Sometimes they use their own encodes. They'll usually mention all sources used for the release in the description.

Examples of good mux groups are - OZR, Kametsu, CTR, YURI, pog42, Arid, ARC, Mysteria, Drag, UDF, NH, KH.

**WEB-DL** - Until the blurays are released, this is the only source available for new airing anime. Note that this is not a WEBRip, as in a screen capture. The ones released by reliable groups are always direct WEB-DLs from sources like Crunchyroll or Funimation. The quality will be the same no matter which group releases these, however they often go down and reliability is a factor when you want airing anime in the shortest time possible. The most reliable one right now is SubsPlease.

**Re-encode** - Marked red on nyaa, these are usually encodes of a BDRip, WEB source or even worse. Encoding is a lossy proceess and information is lost at every stage. The same encode made from the BDMV instead of a BDRip would've been better quality. Re-encoding is considered a bad practice.

**Mini encode** - These are around 500mb or lower, they can be either a BDRip or a re-encode. Despite a bad reputation, the minis which are not re-encodes can be a decent option for those low on storage or bandwidth. Mini groups -

- Good - Judas (new)

- Decent - Akihitosubs, DB, Ember, Nep_Blanc

- Bad - Judas (old), Cleo, Reaktor, Cerberus. Their video might be bad, but Cerberus and Reaktor often pick good subtitle sources.

- Trash - bonkai77, DaddySubs, Tenrai-Sensei, DKB, HR, SSA, FFA, YuiSubs and any other group using NVENC.

Judas (new) is roughly mid 2020 and later.
Note that this tier list is valid only for BD sourced encodes and not airing anime. The order of quality for WEB releases is -

`Subsplease/Erai 1080p > Subsplease/Erai 720p > All mini encodes.`

## BD vs WEB

The BD is usually always a better source than WEB. There's no reason to get a WEB sourced encode once the BD is out. The advantages of BD and what you're missing out on with WEB are -

1. Better Quality - The average episode on the BD is 6GB compared to 1.3GB on WEB. This makes it a better source even for mini encodes, as compression is better when you have more data to work with. A same size encode made from both sources will show the BD version to be superior.

2. No Censorship - The BDs will remove or reduce censorship wherever possible, this can range from minor changes within scenes to whole new scenes being added.

3. Additional Content - Censorship is not the only situation where content is added. A BD release allows the studios to fix any mistakes made due to budget/time/airing duration constraints and they can add as much content as they want. An example of this is Fate UBW, you can check the difference in episode durations.

4. Animation Improvements - Fully redrawn scenes, extra details, shading improvements, removed dimming/brightness changes etc.

Here are some comparisons -

- [Toaru Kagaku no Accelerator EP 5](https://slow.pics/c/Z0DF2PlI)
- [Toaru Kagaku no Accelerator EP 6](https://slow.pics/c/CpGp3GIv)
- [Saiki Kusuo no Ψ-nan](https://slow.pics/c/GxJxekoN)
- [Demon Slayer](https://slow.pics/c/UMxyTZ7T)

# Audio

Codecs - Audio codecs are divided into lossless (FLAC, TrueHD, DTS-HDMA) and lossy (aac, opus, mp3). While lossless raw video will be multiple GBs per minute, audio is more manageable in size, and you'll see many options with lossless audio.

Lossless audio is unnecessary for most sound systems. Even with the best audiophile grade setup, it's nearly impossible to make out the difference. However, some exceptional music samples exist, which can be used to differentiate lower-bitrate lossy audio by listening to small extracted parts repeatedly. If you want to try, check out these [online ABX tests](http://abx.digitalfeed.net) or [Foobar plugin](https://www.foobar2000.org/components/view/foo_abx) with [this guide for setting it up.](https://www.head-fi.org/threads/setting-up-an-abx-test-simple-guide-to-ripping-tagging-transcoding.655879/#post-9268096.) This foobar plugin lets you ABX any two tracks of your choice and produces a verifiable log. If you manage to complete it with a decent probability, do join our [discord](https://discord.gg/snackbox) for some interesting conversation.

A good benchmark for bitrates (stereo/2.0) is -

- 128 kbps for opus

- 160 kbps for aac

- 192 kbps for mp3

For surround audio, multiply with the number of stereo pairs.

# Subtitles

Most anime subtitles are in the .ass format, it has better styling options compared to srt. This styling often breaks when there is incompatibility somewhere in the playback process.

Fansubs use a variety of fonts in subtitles. These are bundled within the .mkv files as attachments or provided separately in a folder. The ones given separately can be installed on windows or placed in your player's fonts folder for a quick solution. They should be muxed in for perfect compatibility.

## Fansubs

Older anime was subbed by a variety of fansub groups. Every good release will mention the sources used for subtitles, the edits made, and often describe their choice of subtitles. A very good resource for fansub reviews was MyAnimeList. They removed the fansub reviews section a few years ago towards their goal of legitimization, since fansubs are associated with piracy. Luckily, the data was archived and [can be brought back on the MAL page itself through this](https://www.reddit.com/r/anime/comments/9kq1ch/bringing_fansubs_back_on_mal/). Fair warning, the reviews are mostly filled with hate and trolls but often give you an idea of which group used which script. You can also make out the kind of translation and how much it is localized.

Localization means changing cultural references and puns to fit the english context, but too often it becomes more about the American context and fansubs get riddled with memes. A degree of localization is always involved with translation to make sure the dialogue flows smoothly, but too much of it might be a problem for some. These two websites provide information and comparisons between various groups doing any anime -

- https://www.crymore.net

- https://fansub.co

Most newer fansubbed anime is some variation of edited official subs from CR or Funi. These edits are usually range between excessive localization by completely eradicating and replacing the idea of Japan with America or untranslation by changing random phrases and lines back to Japanese. Most actual fansubs lie somewhere in between these extremes. After looking at the work of a few groups, you'll be able to figure out who has a tendency to do what.

AnimeTosho is a very useful resource for grabbing just the subtitles from any release on nyaa. You search the title as it is on nyaa and after opening the page you'll see `All Attachments` near the bottom. This will download all subtitles and fonts extracted from that release. It's also useful to get the mediainfo when the uploader hasn't posted it, to do this, just click on any of the mkv files in the torrent and you'll get details about it. Animetosho is also a DDL site which uploads torrents from nyaa to various hosting services. There's also an NZB option, but none of the file hosts last for long.
