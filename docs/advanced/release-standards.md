---
label: Release Standards
order: -5
---

# Release Standards

## Video

Ideally, the video should be an encode that's an improvement over the source video but realistically that isn't always the case since sometimes no good encode exists. This guide won't be tackling the topic of encoding since there's already one [here](https://guide.encode.moe/encoding/preparation.html).

### General

- WEB-DLs are usually the best source for shows which haven't gotten a BluRay (usually airing or recently ended).
- An encode of the BluRay that's better than the source will trump the source.
- BluRay Remux trumps WEB-DL/WEBRip and Bad Encodes.
- Avoid mini encodes as these are targeted to save size by sacrificing quality. Some popular mini encode groups to avoid are:

```
Anime Time, AnimeKaizoku, AnimeRG, BakedFish, Bonkai, CBB/CherryBomB, Cleo, DB, DeadFish,
Edge, EMBER, Hakata Ramen, Hi10, iPUNISHER, Judas, Kanjouteki, M@nI, MiniFreeza,
MiniTheatre, Mr.Deadpool, NemDiggers, NoobSubs, project-gxs, SSA, youshikibi
```

### Video Source

Unless you are making your own encode, you'll have to stick to the available options. To get started:

- Download the different raws/encodes you find along with the source (BDMV/Remux) and make a [comparison](/tutorials/comparison).
- Look at the comparison to decide the best source and if you are having trouble picking one, feel free to join the [SeaDex Discord](https://discord.com/invite/jPeeZewWRn) where pixel peepers will check it out and help you decide.

## Audio

### Useful Tools

- [eac3to](https://forum.doom9.org/showthread.php?t=125966) with [updated libraries](https://mega.nz/#!dFAmEC4Y!WMTQvzLkfTDHPfhTXURSLaFWbmDMVaq3dKfk4ucjYrI) for extracting and transcoding.
- [SoX](http://sox.sourceforge.net/) for resampling and bit depth reduction.
- [opus-tools](https://opus-codec.org/downloads/) for transcoding lossless audio to Opus.
- [muxtools](https://github.com/Jaded-Encoding-Thaumaturgy/muxtools) is an automation package for everything related to encoding and subbing.
- [acsuite](https://github.com/OrangeChannel/acsuite) for frame-based cutting/trimming/splicing of audio files using VapourSynth clip information.
- [sync-audio-tracks](https://github.com/alopatindev/sync-audio-tracks) calculates a delay between two audios and produces a shifted audio.
- [downsampler-threaded](https://gitlab.com/beep_street/downsampler-threaded) is a multi-process sox frontend for automatically resampling FLAC files.

### General

Either lossless or lossy audio can be used for a good release but with a few things to keep in mind:

- Avoid including lossless audio tracks like PCM, DTS-HD, DTS-HD MA, and TrueHD from BDMVs/Remuxes. These tracks should always be transcoded to either FLAC (lossless compression) or a lossy codec with an appropriate bitrate.
  - **Note**: _Do not transcode object-based codecs like DTS:X or Dolby Atmos to FLAC as Dolby Atmos is an extension to TrueHD and DTS:X is an extension to DTS-HD MA. Their metadata will be ignored or stripped out during conversion._
- Audio tracks should either be converted to 16-bit FLAC because 24-bit FLAC is bloated with no benefits or to a lossy audio codec like Opus with an appropriate bitrate like 128-192Kb/s for Stereo tracks and 256-320Kb/s for multi-channel tracks to achieve transparency.
- Never downmix multichannel audio to stereo.
- Never transcode a lossy track.

!!!warning
If you're making a remux, you must not do any lossy conversions. Remuxes are meant to be lossless copies of the source and only lossless conversions should be done (e.g, PCM to FLAC). For example, converting 24bit FLAC to 16bit FLAC is a lossy process and should not be done for a remux.
!!!

### eac3to

Once you have eac3to with updated libraries, you can start with this simple command:

```batch
eac3to ExampleAnime.mkv
```

Or, if it's a BDMV

```batch
eac3to /path/to/BDMV/
```

This will return all the tracks in the file.

==- Example
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
===

Now you can select, extract and convert them all at once by using the numbers corresponding to each track.

+++ eac3to
```batch
eac3to ExampleAnime.mkv 2: "ENG.5.1.flac" 3: "ENG.5.1.*" 4: "JPN.5.1.flac"
```
+++ Windows
```batch
for %I in (*.mkv) do eac3to "%I" 2: "%~nI.ENG.5.1.flac" 3: "%~nI.ENG.5.1.*" 4: "%~nI.JPN.5.1.flac"
```
+++ Linux
```shell
for file in *.mkv; do eac3to "${file}" 2: "${file%.*}.ENG.5.1.flac" 3: "${file%.*}.ENG.5.1.*" 4: "${file%.*}.JPN.5.1.flac"; done
```
+++

This will select the second and fourth tracks and transcode both of them losslessly to FLAC but the third lossy track will be extracted without any transcoding because no codec was specified.

==- eac3to CLI example
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
===

Here the second and fourth lossless tracks get transcoded to FLAC while the lossy AC3 track remains the same.

### SoX

If you're planning to include lossless audio from the source, downconvert and dither 24-bit lossless tracks to 16-bit FLAC for the sake of reducing bloat as 24-bit is significantly larger with no benefits over 16-bit FLAC.

Here's the recommended command:

+++ sox
```batch
sox input.flac -R -G -b 16 output.flac rate -v -L 48000 dither
```
+++ Windows
```batch
for %I in (*.flac) do sox "%I" -G -b 16 "%~nI.16bit.flac" rate -v -L 48000 dither
```
+++ Linux
```shell
for file in *.flac; do sox -S "${file}" -R -G -b 16 "${file%.*}.16bit.flac" rate -v -L 48000 dither; done
```
+++

### opus-tools

If you're planning to include lossy audio, it's recommended to use Opus with a bitrate of 128-192Kb/s for Stereo tracks and 256-320Kb/s for multi-channel tracks to achieve transparency.

Here are the recommended commands:

+++ opusenc
```batch
opusenc --bitrate 192 Stereo_track.flac Stereo_track.opus
```
```batch
opusenc --bitrate 320 multichannel_track.flac multichannel_track.opus
```
+++ Windows
```batch
for %I in (*.flac) do opusenc --bitrate 192 "%I" "%~nI.opus"
```
+++ Linux
```shell
for file in *.flac; do opusenc --bitrate 192 "${file}" "${file%.*}.opus"; done
```
+++

## Subtitles

### Useful Tools

- [arch1t3cht/Aegisub](https://github.com/arch1t3cht/Aegisub) for all subtitle work.
- [muxtools](https://github.com/Jaded-Encoding-Thaumaturgy/muxtools) is an automation package for everything related to encoding and subbing.
- [Myaamori-Aegisub-Scripts](https://github.com/TypesettingTools/Myaamori-Aegisub-Scripts) includes several helpful scripts for processing `.ASS` subtitles.
- [Prass](https://github.com/tp7/Prass) is another Console processor for `.ASS` subtitles.
- [tp7/Sushi](https://github.com/tp7/Sushi) is an automatic subtitle shifter based on audio.
- [FichteFoll/Sushi](https://github.com/FichteFoll/Sushi) is the python 3 fork of [tp7/Sushi](https://github.com/tp7/Sushi).

### General

- Multiple Subtitle tracks take negligible space and provide more options so there's no reason not to include them.
- Always include the official subtitle track.
- Always include fansubs if they exist and are of good quality. Good Quality is subjective so you have to decide some things yourself or ask around for opinions but for starters you can go with popular fansub groups or judge them yourself based on factors like timing, grammar, typesetting, extreme localization, or lack there of.
- `Honorifics` track must be tagged as `enm`.
- Must include a `Signs and Songs` track for English dub.
- It's recommended to check the subtitle track for obvious grammatical errors, punctuation errors, typos, etc.
- Check the subtitle tracks for scene bleeds, i.e, when a line extends farther than it should/past a keyframe/scene change.
- Pick the default subtitle track by diffing the different options and finding the best one. This can be done via sites like [Diffchecker](https://www.diffchecker.com/) or [evadiff](https://meitantei.org/public/evadiff/).
- Here's a [subdigest](https://github.com/TypesettingTools/Myaamori-Aegisub-Scripts#sub-digest) script to get clean text file output of .ASS subtitles for diff checking:

+++ Windows
```batch
for %I in (*.ass) do python -m subdigest -i "%I" --selection-set "text" "\\p" --remove-selected --selection-set style "Sign|OP|ED|op|ed|kfx|Karaoke|Eyecatch|Signs" --remove-selected --remove-all-tags --remove-comments --get-field text -o "%~nI.txt"
```
+++ Linux
```shell
for file in *.ass do; python -m subdigest -i "${file}" --selection-set "text" "\\p" --remove-selected --selection-set style "Sign|OP|ED|op|ed|kfx|Karaoke|Eyecatch|Signs" --remove-selected --remove-all-tags --remove-comments --get-field text -o "${file%.*}.txt"; done
```
+++

### Styling

- The dialogue style must be readable without being distracting.
- Feel free to restyle bad styling. Here's a reference for some good [dialogue fonts](https://user-images.githubusercontent.com/78981416/233718844-5d05ae4e-228a-4bfc-b9fb-e7209b5ac037.png).
- You can also copy styling from existing groups like [GJM](https://nyaa.si/user/GoodJobMedia), [Kaleido](https://nyaa.si/user/Kaleido-subs), or [DDY](https://nyaa.si/user/DameDesuYo).
- Try to match the styling of previous seasons of the same show to maintain consistency. Although this isn't mandatory and should be avoided if previous season releases had bad styling.

## Muxing and Tagging

Refer to the [Muxing](/advanced/muxing) page.

## Naming

Refer to the [Naming](/advanced/naming) page.

## QC

- Check the MediaInfo for any missing tracks, tracks in wrong order or wrong tags.
- Check for any missing fonts. You can use [fontvalidator](https://github.com/TypesettingTools/Myaamori-Aegisub-Scripts#font-validator) for this.
- Watch your finished release for any errors you may have missed before going forward.

## CRC

- [RapidCRC](https://www.ov2.eu/programs/rapidcrc-unicode)
- CRC32 is an error-detecting function that uses a CRC32 algorithm to detect changes between source and target data.
- It's commonly seen as a random string at the end of the file name, e.g, `[BE8625C3]`.
- It's intended to verify that the files you have are the correct ones, or whether they match with the version released by the uploader in case there have been any revisions.
- Renaming a file does not change its CRC32 value.
- CRC32 is redundant for torrents since torrents already have an infohash, an SHA-1 hash calculated over the contents of the info dictionary in bencode form. You can easily check their integrity via your client's `Recheck` feature.
- CRC32 is helpful when the file has been obtained from sources like Drive, Usenet, or XDCC.

## Torrent

- To create a torrent, simply follow the guide [here](/tutorials/torrent).

## Patches/Revisions

- For making patches, see the [xDelta guide](/tutorials/xdelta).
- Append `v2` at the end of `SXXEYY`, e.g, `S01E01v2`.
- Do **NOT** alter episode files that don't need a revision.
- This will also change the `CRC32` value of the file.
- Re-upload the complete patched release while also including the patch separately in the description for people who have downloaded the previous version so that they don't have to redownload the whole thing again.
