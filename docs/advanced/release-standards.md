---
label: Release Standards
order: -4
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

### Recommended Tools

- [eac3to](https://forum.doom9.org/showthread.php?t=125966) with [updated libraries](https://mega.nz/#!dFAmEC4Y!WMTQvzLkfTDHPfhTXURSLaFWbmDMVaq3dKfk4ucjYrI) for extracting and transcoding.
- [SoX](http://sox.sourceforge.net/) for resampling and bit depth reduction.
- [opus-tools](https://opus-codec.org/downloads/) for transcoding lossless audio to Opus.
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
for %I in (*.flac) do sox "%I" -G -b 16 "%~nI.flac" rate -v -L 48000 dither
```
+++ Linux
```shell
for file in *.flac; do sox -S "${file}" -R -G -b 16 "${file%.*}.flac" rate -v -L 48000 dither; done
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

### Recommended Tools

- [arch1t3cht/Aegisub](https://github.com/arch1t3cht/Aegisub) for all subtitle work.
- [SubKt](https://github.com/Myaamori/SubKt) is a highly configurable toolkit for fansubbing automation.
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

Once you have prepared the individual files, you'll have to put them together in a container, preferably [Matroska](https://www.matroska.org/index.html), commonly seen as files with the extension `.mkv`.

### Recommended Tools

- [MKVToolNix](https://mkvtoolnix.download/downloads.html)
- [SubKt](https://github.com/Myaamori/SubKt)
- [MKVToolNix-Sequential-Batch-Mapper](https://github.com/McBaws/MKVToolNix-Sequential-Batch-Mapper)
- [Inviska-MKV-Extract](https://www.videohelp.com/software/Inviska-MKV-Extract)

### Forced Subtitles

Forced here **DOES NOT** mean that these subs will be permanently on the screen, which is a common misconception.
Forced subtitles only provide subtitles when the characters speak a foreign or alien language, or a sign, flag, or other text in a scene is not translated in the localization and dubbing process. In our case, it's supposed to be displayed whenever the English dub has untranslated things like Japanese Signs and Songs like Opening/Ending or Inserts.

### Correct Tagging

Proper tagging enables a player to autoselect the correct language streams for audio and subtitles. Tags can be edited in the MKVToolNix Header Editor or mkvpropedit without remuxing.

- **Video track:**
  - Language tag is optional.
  - Mention the name of the `Encoder/Encode group` or simply region like `JPBD` or `ITABD` if it's untouched BluRay Remux.
- **Audio Tracks:**
  - Mention the Codec, Channels, and Bitrate in the `Name` field.
  - `Japanese` audio should always be tagged as `jpn` and marked as `Default`
  - `English` audio should always be tagged as `eng` but should not be marked as `Default` or `Forced`. This is to make sure auto-selection works well for both dub and sub watchers.
- **Subtitle Tracks:**
  - Language tag must be used appropriately reflecting the language of the subtitles.
  - Tag the best `Full Subtitles` as `eng` and `Default`
  - Tag the `Signs/Songs` track as `eng` and `forced`. This is to make sure auto-selection works well for both dub and sub watchers.
  - Tag the `Honorifics` track as `enm` but neither `Default` nor `Forced`.
  - Tag any and all additional subtitle tracks as neither `Default` nor `Forced`.

**Note:** Newer mkvtoolnix versions automatically set the default flag to `yes` on all streams. This is technically the correct use for the flag but all players do not have the intended results with this kind of tagging.

| Track        | Language | Name                      | Default | Forced |
| ------------ | -------- | ------------------------- | ------- | ------ |
| Video        | jpn      | Encode Group              | yes     | no     |
| Audio #1     | jpn      | FLAC 2.0                  | yes     | no     |
| Audio #2     | eng      | Opus 5.1 @ 320kb/s        | no      | no     |
| Subtitles #1 | eng      | Full Subtitles [Fansub]   | yes     | no     |
| Subtitles #2 | enm      | Honorifics [Fansub]       | no      | no     |
| Subtitles #3 | eng      | Signs/Songs [Fansub]      | no      | yes    |
| Subtitles #4 | eng      | Full Subtitles [Official] | no      | no     |

**Command to tag everything properly (assuming the order of tracks is correct):**
+++ Windows
```batch
for %X in (*.mkv) do mkvpropedit "%X" --add-track-statistics-tags --edit info --delete title ^
--edit track:v1 --set name="Encode Group"              --set language=jpn --set flag-default=1 ^
--edit track:a1 --set name="FLAC 2.0"                  --set language=jpn --set flag-default=1 ^
--edit track:a2 --set name="Opus 5.1 @ 320kb/s"        --set language=eng --set flag-default=0 ^
--edit track:s1 --set name="Full Subtitles [Fansub]"   --set language=eng --set flag-default=1 --set flag-forced=0 ^
--edit track:s2 --set name="Honorifics [Fansub]"       --set language=enm --set flag-default=0 --set flag-forced=0 ^
--edit track:s3 --set name="Signs/Songs [Fansub]"      --set language=eng --set flag-default=0 --set flag-forced=1 ^
--edit track:s4 --set name="Full Subtitles [Official]" --set language=eng --set flag-default=0 --set flag-forced=0
```
+++ Linux
```shell
for file in *.mkv; do
  mkvpropedit "${file}" --add-track-statistics-tags --edit info --delete title \
  --edit track:v1 --set name="Encode Group"              --set language=jpn --set flag-default=1 \
  --edit track:a1 --set name="FLAC 2.0"                  --set language=jpn --set flag-default=1 \
  --edit track:a2 --set name="Opus 5.1 @ 320kb/s"        --set language=eng --set flag-default=0 \
  --edit track:s1 --set name="Full Subtitles [Fansub]"   --set language=eng --set flag-default=1 --set flag-forced=0 \
  --edit track:s2 --set name="Honorifics [Fansub]"       --set language=enm --set flag-default=0 --set flag-forced=0 \
  --edit track:s3 --set name="Signs/Songs [Fansub]"      --set language=eng --set flag-default=0 --set flag-forced=1 \
  --edit track:s4 --set name="Full Subtitles [Official]" --set language=eng --set flag-default=0 --set flag-forced=0
done
```
+++

## Naming

This guide aims to somewhat standardize naming schemes used for Anime in an effort to make them work beyond File Explorer and work well with Usenet, XDCC, automation software, and media servers.
**Everything mentioned here aims to work with everything and if something isn't mentioned, it's very likely because it breaks support for one thing or another.** Adopting all of it will ensure compatibility with basically everything, for example, not only will your releases be snatched by auto-downloaders but will also be parsed and matched accurately and people can drop your releases in media servers without having to rename and break seeding.

### Recommended Tools

- [Advanced Renamer](https://www.advancedrenamer.com/)
- [Filebot-mod](https://github.com/barry-allen07/FB-Mod)
- [Bulk Rename Utility](https://www.bulkrenameutility.co.uk/)
- [tvnamer](https://github.com/dbr/tvnamer)

### General

- Must contain the name of the Anime.
- Must contain `(year)` when there are multiple versions, e.g, `Hunter x Hunter (1999)` and `Hunter x Hunter (2011)`. **Note:** _Including year is required for all movies, regardless of whether multiple versions exist or not._
- Must contain source information, i.e, `BD/BluRay`, `WEB/WEB-DL/WEBRip`, or `DVD`. Append the word `Remux` to `BD/BluRay` or `DVD` if it's a Remux.
- For `WEB/WEB-DL/WEBRip`, you must include source tags. `AMZN` for Amazon, `DSNP` for Disney+, and `NF` for Netflix.
- Must contain resolution, i.e, `1080`, `720`, `576` or `480` together with scan type, i.e, `p` if the content is fully Progressive or `i` if it’s Interlaced. So finally it looks like this: `1080p`, `720p`, `576p`, `480i`. Different aspect ratios do not change this resolution, e.g. `1440x1080` and `1920x800` are both still 1080p.
- Must mention the Video and Audio Codec.
- Always add a group tag to your release, ideally placing it at the end for both easier parsing and human readability.
- You should use `.` or `-` as a delimiter.

### Filename

- Must contain Season and Episode information in the format `SXXEYY` following [TVDB](https://thetvdb.com/).
- Absolute Episode Numbers may be used together with `SXXEYY`.
- Special Episodes must follow the format `S00EXX`.
- `Group` tag can be at the start or the end of the filename.

Examples:

```
[Group] Anime Name - S01E01 - (BD 1080p HEVC FLAC) [Dual Audio] [CRC32].mkv
Anime Name - S01E01 - (BD 1080p HEVC FLAC) [Dual Audio] [CRC32] [Group].mkv
Anime Name - S01E01 - (BD 1080p HEVC FLAC) [Dual Audio] [CRC32]-Group.mkv
Anime Name - S01E01 - (BD 1080p HEVC FLAC) [Dual Audio] [Group].mkv
Anime Name - S00E01 - (AMZN WEB-DL 1080p H.264 EAC3) [Dual Audio] [Group].mkv
Anime Name - 18.5 - S00E05 - (BD 1080p HEVC FLAC) [Dual Audio]-Group.mkv
Anime Name (2022) - S01E01 - (BD Remux 1080p HEVC FLAC) [Dual Audio]-Group.mkv
Anime.Name.S01E01.1080p.BluRay.Opus2.0.x264-Hi10P-Group.mkv
```

### Specials/OVA Filename

- These are wildly inconsistent across different databases which means you need to be more careful when naming these to avoid any confusion while also keeping it both human and machine readable. Databases like [thetvdb](https://thetvdb.com/) use `S00EXX` for specials while databases like [MAL](https://myanimelist.net/) simply use the episode name for specials.
- A common thing in anime releases is Specials/OVAs numbered as `0.5`, e.g. `Anime Name - 18.5`. This is not recommended at all.
- `0.5` Episodes are not supported by a single piece of software or database but they usually make it easier for the person watching to know where it chronologically lands in the show. You may or may not include this in the filename.
- `S00EXX` isn't very user friendly, because it doesn't really tell much on its own but it guarantees automatic parsing and correct identification. You must include this in the filename. Refer to [thetvdb](https://thetvdb.com/) to find this for your episode.
- Including the Episode title is highly recommended in this case as it makes identification significantly easier and understandable for anyone who downloads it. Neither `S00EXX` or `0.5` provides this information at a glance which is why the episode title is really helpful in this case. Refer to either [thetvdb](https://thetvdb.com/) or [MAL](https://myanimelist.net/) to find the episode name.
- You may include the absolute number of the episode as long as it's not a decimal episode that lands between any two existing ones. Optional and really just down to preference. It's much better than decimal episode numbering but still not ideal.
- This means we need a name that is easily readable, parsable, and identifiable. This is achieved by using a combo of the above.

```
Anime Name - S00E04 - Title of the Episode
```

- This is an example of a well named special episode. `Anime Name` tells us the name of the anime, `S00E04` tells us the episode number in the `SXXEXX` standard which makes it easily identifiable, and `Title of the Episode` makes it significantly more human readable.

```
Anime Name - 18.5 - S00E05 - Title of the Episode
```

- Another good example, this tells us the name of the anime, where it lands chronologically, what episode it is with reference to databases and the name of the episode to aid with identifying the episode without much effort.

#### More Examples

Bad, do not use:

```
[Group] Anime Name - S02OVA02.mkv
[Group] Anime Name - 18.5 [BD 1080p FLAC] [CRC].mkv
[Group] Anime Name - 06.5 (SP) [BD 1080p FLAC].mkv
[Group] Anime Name - Title of the Episode [CRC].mkv
[Group] Anime Name - 17.5 (OVA) [CRC].mkv
```

Good names:

```
Anime Name - 06.5 - S00E04 - Title of the Episode (BD 1080p HEVC FLAC) [Dual Audio] [CRC32] [Group].mkv
Anime Name - S00E01 - Title of the Episode (AMZN WEB-DL 1080p H.264 EAC3) [Dual Audio] [Group].mkv
Anime Name - 18.5 - S00E05 - Title of the Episode (BD 1080p HEVC FLAC) [Dual Audio]-Group.mkv
Anime Name (2022) - S00E01 - Title of the Episode (BD Remux 1080p HEVC FLAC) [Dual Audio]-Group.mkv
Anime.Name.S00E09.Title.of.the.Episode.1080p.BluRay.Opus2.0.x264-Hi10P-Group.mkv
```

### Folder

- Must contain Season information in the format: `S0X` or `Season X`.
- Make one folder for each Season following all the guidelines. This is due to the fact that most trackers do not allow multi-season batch torrents.
- Put `Group` tag at the end of the folder name to allow alphabetical sorting and easier parsing.
- **Everything** that isn't an episode, e.g Creditless Intro/Outro, PVs, etc **must** be put in a subfolder called `Extras` (not `Extra`). Do **not** use subfolders like `NC`, `Creditless`, etc.
- Special Episode/OVA must be put in the same folder if it's singular or a subfolder called `Specials` if there are multiple of them.
- Anime that are a single season split between two cours, e.g. Eighty-Six aired in two cours, [cour 1](https://anilist.co/anime/116589/) and [cour 2](https://anilist.co/anime/131586/), but is a [single season](https://thetvdb.com/series/eighty-six/seasons/official/1), must be named in a way that satisfies both which is achieved by adding the episode range after the season info (`S0X`). For example, `Eighty-Six S01 E01-E11` and `Eighty-Six S01 E12-E23`.

Examples:

```
Anime Name Season 2 (BD Remux 1080p H.264 FLAC) [Dual Audio] [Group]
Anime Name (2022) S01 (BD 1080p HEVC Opus) [Dual Audio]-Group
Anime.Name.2022.S01.1080p.AMZN.WEB-DL.DDP2.0.H.264-Group
Anime Name S02 E01-E24 (BD Remux 1080p x264 8-bit DTS-HD) [Dual-Audio] [Group]
Anime Name S01 E25-E48 (BD Remux 1080p x264 8-bit DTS-HD) [Dual-Audio] [Group]
Anime.Name.2022.S01.E01-E11.1080p.BluRay.FLAC2.0.H.265-Group
Anime.Name.2022.S01.E12-E23.1080p.BluRay.FLAC2.0.H.265-Group
```

### Nyaa

#### Title

- Same guidelines as Folder, simply add more aliases at the end for easier searching.
- You can also put the group tag before everything else in the Nyaa title.

Examples:

```
Anime Name Season 2 (BD 1080p Hi10 Opus) [Dual Audio] [Group] | Alternate Name
[Group] Anime Name Season 2 (BD Remux 1080p H.264 FLAC) [Dual Audio] | Alternate Name
Anime Name (2022) S01 (BD 1080p HEVC Opus) [Dual Audio]-Group | Alternate Name
Anime.Name.2022.S01.1080p.AMZN.WEB-DL.DDP2.0.H.264-Group | Alternate Name
[Group] Anime Name S02 E01-E24 (BD Remux 1080p x264 8-bit DTS-HD) [Dual-Audio] | Alternate Name
[Group] Anime Name S01 E25-E48 (BD Remux 1080p x264 8-bit DTS-HD) [Dual-Audio] | Alternate Name
Anime.Name.2022.S01.E01-E11.1080p.BluRay.FLAC2.0.H.265-Group | Alternate Name
Anime.Name.2022.S01.E12-E23.1080p.BluRay.FLAC2.0.H.265-Group | Alternate Name
```

#### Description

- Always include [MediaInfo](https://mediaarea.net/en/MediaInfo).
- Always include a link to a [comparison](/tutorials/comparison) of the video used in the release.

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

- For making patches, use [dan0v/xdelta3-cross-gui](https://github.com/dan0v/xdelta3-cross-gui).
- Append `v2` at the end of `SXXEYY`, e.g, `S01E01v2`.
- Do **NOT** alter episode files that don't need a revision.
- This will also change the `CRC32` value of the file.
- Re-upload the complete patched release while also including the patch separately in the description for people who have downloaded the previous version so that they don't have to redownload the whole thing again.
