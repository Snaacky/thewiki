---
label: Muxing
---

Matroska(mkv) is a very versatile container. It can contain multiple streams of video, audio, subtitles, and other attachments. The process of taking these streams, adding or removing some, and bundling them into a new mkv is called muxing. In general, you could be muxing any format, but for anime we'll mostly be dealing with mkv. It's useful when you want to use subtitles from a different release with what you already have downloaded, or to remove the extra english audio tracks to save space. Note that this is a lossless process different from encoding and takes only a few seconds.

[Mkvtoolnix](https://mkvtoolnix.download/) is the best tool for all kinds of muxing. The equivalent cli option is mkvmerge (installed with mkvtoolnix) or ffmpeg. The process can also be done in batch for a whole folder at once. Example (paste in cmd in that folder) -

`FOR /F "tokens=*" %G IN ( 'dir /b *.mkv ') DO ffmpeg -n -i "%G" -map 0:v:0 -map 0:a:1 -map 0:s:2 -map 0:t? -c copy "%~nG .mkv"`

This will copy the first video, second audio, third subtitle stream and all attachments, which are usually the japanese ones in dual audio releases. You can check the stream number with mediainfo. The index starts from zero so `-map 0:s:0` would select the first subtitle stream. `-map 0:t?` copies the attachments(fonts) if any exist. Understanding more about the [ffmpeg map option](https://trac.ffmpeg.org/wiki/Map) is helpful here as that is the only part you need to change.

# Video

Ideally, the video should be an encode that's an improvement over the source video but realistically that isn't always the case since sometimes no good encode exists. This guide won't be tackling the topic of encoding since there's already one [here](https://guide.encode.moe/encoding/preparation.html).

## General

- WEB-DLs are usually the best source for shows which haven't gotten a BluRay (usually airing or recently ended).
- An encode of the BluRay that's better than the source will trump the source.
- BluRay Remux trumps WEB-DL/WEBRip and Bad Encodes.
- Avoid mini encodes as these are targeted to save size by sacrificing quality. Some popular mini encode groups to avoid are:

```
Anime Time, AnimeKaizoku, AnimeRG, BakedFish, Bonkai, CBB/CherryBomB, Cleo, DB, DeadFish,
Edge, EMBER, Hakata Ramen, Hi10, iPUNISHER, Judas, Kanjouteki, M@nI, MiniFreeza,
MiniTheatre, Mr.Deadpool, NemDiggers, NoobSubs, project-gxs, SSA, youshikibi
```

## Video Source

Unless you are making your own encode, you'll have to stick to the available options. To get started:

- Download the different raws/encodes you find along with the source (BDMV/Remux) and make a [comparison](/tutorials/comparison).
- Look at the comparison to decide the best source and if you are having trouble picking one, feel free to join the [SeaDex Discord](https://discord.com/invite/jPeeZewWRn) where pixel peepers will check it out and help you decide.

# Audio

## Recommended Tools

- [eac3to](https://forum.doom9.org/showthread.php?t=125966) with [updated libraries](https://mega.nz/#!dFAmEC4Y!WMTQvzLkfTDHPfhTXURSLaFWbmDMVaq3dKfk4ucjYrI) for extracting and transcoding.
- [SoX](http://sox.sourceforge.net/) for resampling and bit depth reduction.
- [opus-tools](https://opus-codec.org/downloads/) for transcoding lossless audio to Opus.
- [acsuite](https://github.com/OrangeChannel/acsuite) for frame-based cutting/trimming/splicing of audio files using VapourSynth clip information.
- [sync-audio-tracks](https://github.com/alopatindev/sync-audio-tracks) calculates a delay between two audios and produces a shifted audio.
- [downsampler-threaded](https://gitlab.com/beep_street/downsampler-threaded) is a multi-process sox frontend for automatically resampling FLAC files.

## General

Either lossless or lossy audio can be used for a good release but with a few things to keep in mind:

- Avoid including the lossless audio tracks like PCM, DTS-HD, DTS-HD MA, and TrueHD from BDMVs/Remuxes. These tracks should always be transcoded to either FLAC (lossless compression) or a lossy codec with appropriate bitrate.
  - **Note**: _Do not transcode object-based codecs like DTS:X or Dolby Atmos to FLAC as Dolby Atmos is an extension to TrueHD and DTS:X is an extension to DTS-HD MA. Their metadata will be ignored or stripped out during conversion._
- Audio tracks should either be converted to 16-bit FLAC because 24-bit FLAC is bloated with no benefits or to a lossy audio codec like Opus with an appropriate bitrate like 128-192Kb/s for Stereo tracks and 256-320Kb/s for multi-channel tracks to achieve transparency.
- Never downmix multichannel audio to stereo.
- Never transcode a lossy track.

## eac3to

Once you have eac3to with updated libraries, you can start with this simple command:

```batch
eac3to ExampleAnime.mkv
```

Or, if it's a BDMV

```batch
eac3to /path/to/BDMV/
```

This will return all the tracks in the file.

For example:

```batch
D:\MHA>eac3to "My Hero Academia the Movie- The Heroes Rising 2018 1080p BluRay Hybrid-REMUX AVC TrueHD 5.1 Dual Audio -ZR-.mkv"
MKV, 1 video track, 3 audio tracks, 3 subtitle tracks, 1:44:09, 24p /1.001
1: h264/AVC, English, 1080p24 /1.001 (16:9)
2: TrueHD, English, 5.1 channels, 48kHz
   "Surround 5.1"
3: AC3, English, 5.1 channels, 448kbps, 48kHz, 24ms
   "Surround 5.1"
4: DTS Master Audio, Japanese, 5.1 channels, 24 bits, 48kHz
   (core: DTS, 5.1 channels, 1509kbps, 48kHz)
   "Surround 5.1"
5: Subtitle (PGS), English
6: Subtitle (PGS), English, "Signs & Songs"
7: Subtitle (PGS), Japanese
```

Now you can select, extract and convert them all at once by using the numbers corresponding to each track.

```batch
eac3to ExampleAnime.mkv 2: "ENG.5.1.flac" 3: "ENG.5.1.*" 4: "JPN.5.1.flac"
```

Batch command:

```batch
for %I in (*.mkv) do eac3to "%I" 2: "%~nI.ENG.5.1.flac" 3: "%~nI.ENG.5.1.*" 4: "%~nI.JPN.5.1.flac"
```

Shell command:

```shell
for file in *.mkv; do eac3to "${file}" 2: "${file%.*}.ENG.5.1.flac" 3: "${file%.*}.ENG.5.1.*" 4: "${file%.*}.JPN.5.1.flac"; done

```

This will select the second and fourth tracks and transcode both of them losslessly to FLAC but the third lossy track will be extracted without any transcoding because no codec was specified.

For example:

```batch
D:\MHA>eac3to  "My Hero Academia the Movie- The Heroes Rising 2018 1080p BluRay Hybrid-REMUX AVC TrueHD 5.1 Dual Audio -ZR-.mkv" 2: "English 5.1.flac" 3: "English 5.1.*" 4: "Japanese 5.1.flac"
------------------------------------------------------------------------------
MKV, 1 video track, 3 audio tracks, 3 subtitle tracks, 1:44:09, 24p /1.001
1: h264/AVC, English, 1080p24 /1.001 (16:9)
2: TrueHD, English, 5.1 channels, 48kHz
   "Surround 5.1"
3: AC3, English, 5.1 channels, 448kbps, 48kHz, 24ms
   "Surround 5.1"
4: DTS Master Audio, Japanese, 5.1 channels, 24 bits, 48kHz
   (core: DTS, 5.1 channels, 1509kbps, 48kHz)
   "Surround 5.1"
5: Subtitle (PGS), English
6: Subtitle (PGS), English, "Signs & Songs"
7: Subtitle (PGS), Japanese
[a02] Extracting audio track number 2...
[a03] Extracting audio track number 3...
[a04] Extracting audio track number 4...
[a02] Decoding with libav/ffmpeg...
[a03] Applying (E-)AC3 delay...
[a04] Decoding with libDcaDec DTS Decoder...
[a02] Encoding FLAC with libFlac...
[a03] A remaining delay of -8ms could not be fixed.
[a04] Encoding FLAC with libFlac...
[a02] Creating file "English 5.1.flac"...
[a03] Creating file "English 5.1.ac3"...
[a04] Creating file "Japanese 5.1.flac"...
[a02] Removing TrueHD dialog normalization...
[a02] Original audio track, L+R+C+SL+SR: max 20 bits, average 17 bits.
[a02] Original audio track, LFE: constant bit depth of 16 bits.
[a04] The original audio track has a constant bit depth of 24 bits.
Video track 1 contains 149592 frames.
eac3to processing took 7 minutes, 28 seconds.
Done.
```

Here the second and fourth lossless tracks get transcoded to FLAC while the lossy AC3 track remains the same.

## SoX

If you're planning to include lossless audio from the source, downconvert and dither 24-bit lossless tracks to 16-bit FLAC for the sake of reducing bloat as 24-bit is significantly larger with no benefits over 16-bit FLAC.

Here's the recommended command:

```batch
sox input.flac -R -G -b 16 output.flac rate -v -L 48000 dither
```

Batch command:

```batch
for %I in (*.flac) do sox "%I" -G -b 16 "%~nI.flac" rate -v -L 48000 dither
```

Shell command:

```shell
for file in *.flac; do sox -S "${file}" -R -G -b 16 "${file%.*}.flac" rate -v -L 48000 dither; done
```

## opus-tools

If you're planning to include lossy audio, it's recommended to use Opus with a bitrate of 128-192Kb/s for Stereo tracks and 256-320Kb/s for multi-channel tracks to achieve transparency.

Here are the recommended commands:

```batch
opusenc --bitrate 192 Stereo_track.flac Stereo_track.opus
```

```batch
opusenc --bitrate 320 multichannel_track.flac multichannel_track.opus
```

Batch command:

```batch
for %I in (*.flac) do opusenc --bitrate 192 "%I" "%~nI.opus"
```

Shell command:

```shell
for file in *.flac; do opusenc --bitrate 192 "${file}" "${file%.*}.opus"; done
```

# Subtitles

## Recommended Tools

- [arch1t3cht/Aegisub](https://github.com/arch1t3cht/Aegisub) for everything subtitle work.
- [SubKt](https://github.com/Myaamori/SubKt) is a highly configurable toolkit for fansubbing automation.
- [Myaamori-Aegisub-Scripts](https://github.com/TypesettingTools/Myaamori-Aegisub-Scripts) includes several helpful scripts for processing `.ASS` subtitles.
- [Prass](https://github.com/tp7/Prass) is another Console processor for `.ASS` subtitles.
- [tp7/Sushi](https://github.com/tp7/Sushi) is an automatic subtitle shifter based on audio.
- [FichteFoll/Sushi](https://github.com/FichteFoll/Sushi) is the python 3 fork of [tp7/Sushi](https://github.com/tp7/Sushi).

## General

- Multiple Subtitle tracks take negligible space and provide more options so there's no reason not to include them.
- Always include the official subtitle track.
- Always include fansubs if they exist and are of good quality.
- `Honorifics` track must be tagged as `enm`.
- Must include a `Signs and Songs` track for English dub.
- It's recommended to check the subtitle track for obvious grammatical errors, punctuation errors, typos, etc.
- Check the subtitle tracks for scene bleeds, i.e, when something extends to a frame it shouldn't.
- Pick the default subtitle track by diffing the different options and finding the best one. This can be done via sites like [Diffchecker](https://www.diffchecker.com/) or [evadiff](https://meitantei.org/public/evadiff/).
- Here's a [subdigest](https://github.com/TypesettingTools/Myaamori-Aegisub-Scripts#sub-digest) script to get clean text file output of .ASS subtitles for diff checking:

Batch command:

```batch
for %I in (*.ass) do python -m subdigest -i "%I" --selection-set "text" "\\p" --remove-selected --selection-set style "Sign|OP|ED|op|ed|kfx|Karaoke|Eyecatch|Signs" --remove-selected --remove-all-tags --remove-comments --get-field text -o "%~nI.txt"
```

Shell command:

```shell
for file in *.ass do; python -m subdigest -i "${file}" --selection-set "text" "\\p" --remove-selected --selection-set style "Sign|OP|ED|op|ed|kfx|Karaoke|Eyecatch|Signs" --remove-selected --remove-all-tags --remove-comments --get-field text -o "${file%.*}.txt"; done
```

## Styling

- The dialogue style must be readable without being distracting.
- Feel free to restyle bad styling. Here's a reference for some good [dialogue fonts](https://i.imgur.com/qQUFf7n.png).
- You can also copy styling from existing groups like [GJM](https://nyaa.si/user/GoodJobMedia), [Kaleido](https://nyaa.si/user/Kaleido-subs), or [DDY](https://nyaa.si/user/DameDesuYo).
- Try to match the styling of previous seasons of the same show to maintain consistency. Although this isn't mandatory and should be avoided if previous season releases had bad styling.

# Advanced

## Ordered Chapters/mkv linking

On some releases(like coalgirls), the OP/ED are removed from the episodes and placed into separate files. These files are then linked to the appropriate position in the episodes where they are supposed to play. While this is a great idea to save space, unfortunately, it doesn't work on a lot of players and the OP/ED is never played. If you notice this while watching anime, OCs are the most probable cause.

[Explanation for ordered chapters](https://mod16.org/hurfdurf/?p=8)

Fix ordered chapters using [UnlinkMKV](https://github.com/gnoling/UnlinkMKV)

## CRC32

This is used to verify that the files you have are the correct ones, or whether they match with the version released by the uploader. CRCs will change if any part of the file changes, even if you only change one letter in one tag in the file. This allows us to identify files that were changed from the original source or corrupted at some point. However, just renaming the file will not change it. Apart from the numbers included within [] at the end of a filename, .sfv (CRC checksum) or .md5 (MD5 checksum) files can also be used for the same purpose. Sometimes a group makes mistakes and wants to correct them by releasing a second version(v2), in this case the changed CRC makes it easy to differentiate between multiple versions.

You can use [Anime Checker](http://animechecker.sourceforge.net/) or [RapidCRC](https://www.ov2.eu/programs/rapidcrc-unicode) to verify or add CRC to your files.
