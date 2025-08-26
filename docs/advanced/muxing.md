---
label: Muxing
description: Muxing files with MKVToolNix
image: /static/advanced/muxing/muxingembed.gif
order: -1
---

# Muxing

[Matroska](https://www.matroska.org/) is a very versatile container. Matroska is usually found as `.mkv` files (Matroska video), .`mka` files (Matroska audio), `.mks` files (subtitles). It can contain multiple streams of video, audio, subtitles, and other attachments. The process of taking these streams, adding or removing some, and bundling them into a new file is called muxing or multiplexing. Muxing is a lossless process and takes only a few seconds. In general, you could be muxing any format, but for anime we'll mostly be dealing with mkv. It's useful when you want to use subtitles from a different release with what you already have downloaded, or to remove the extra english audio tracks to save space.

## MKVToolNix

[MKVToolNix](https://mkvtoolnix.download/downloads.html) is a set of tools to create, alter and inspect Matroska & WebM files under Windows, macOS, Linux and other Unices. It is the de-facto reference implementation of a Matroska multiplexer.

MKVToolNix consists of the following command-line tools:

- `mkvmerge` is a tool to create Matroska & WebM files from other formats.
- `mkvinfo` allows one to get information about the tracks in Matroska & WebM files.
- `mkvextract` can extract tracks from Matroska & WebM files to other formats.
- `mkvpropedit` can edit properties such as header and chapter information or attachments without remuxing.

On top of them sits `MKVToolNix GUI`, an easy-to-use program making the functionality of those command-line tools available as a GUI.

### Getting started

1. After installing MKVToolNix, you're pretty much ready to go. Once you launch it, you'll be greeted with this.
   You'll see 3 tabs here:
   - `Input` tab for adding your files, modifying existing tracks, adding/removing tracks and more.
   - `Output` tab for adding metadata like file title or chapters.
   - `Attachments` tab for additional attachments, commonly used for adding fonts required by the subtitle tracks and things like cover.

    ==- :icon-file-media: Screenshot
    [![](/static/advanced/muxing/mkvtoolnix.png)](/static/advanced/muxing/mkvtoolnix.png)
    ===

2. When you first add a file in MKVToolNix, you'll get a pop to select it's behavior.
    - Select `Add as new source files to the current multiplex settings`
    - Check `Always use the action selected above and don't ask again`
    - Click `OK`

    ==- :icon-file-media: Screenshot
    [![](/static/advanced/muxing/mkvtoolnix-gui_first_time_popup.png)](/static/advanced/muxing/mkvtoolnix-gui_first_time_popup.png)
    ===

3. Now, You can either right-click anywhere in the top "Source Files" box or drag and drop your file in it.

   - Once you add your source file, all the tracks in your file will show up in the box below, along with other relevant things in each tab. 
   - Tracks will be randomly assigned a color to indicate what source they belong to.
   - Checking or unchecking a track will decide if it'll be kept or removed in the output file.
   - By default, anything you won't explicitly uncheck will be copied over to the new output file.
   - Clicking on any track allows you to modify several properties in the window on the right. It's important you get this right and it's covered in more details [below](#correct-tagging).
  
    ==- :icon-file-media: Screenshot
    [![](/static/advanced/muxing/mkvtoolnix2.png)](/static/advanced/muxing/mkvtoolnix2.png)
    ===

4. Once you're done making your changes, assign a [name](/advanced/release-standards/#naming) to your file and hit `Start multiplexing`. Make sure to check all 3 tabs to ensure what's being copied over.

   ==- :icon-file-media: Screenshot
   [![](/static/advanced/muxing/mkvtoolnix3.png)](/static/advanced/muxing/mkvtoolnix3.png)
   ===

   !!!
   MKVToolNix also allows you to [generate a commandline](/static/advanced/muxing/mkvtoolnix4.png) with all the changes you made in the GUI.
   !!!

### Correct Tagging

The `Properties` tab allows you tag each track with various flags. Tagging a track correctly is very important and must be done correctly because proper tagging enables a player to autoselect the correct language streams for audio and subtitles. Tags can be edited in the MKVToolNix or mkvpropedit without remuxing.

==- :icon-play: Video track

  - You can leave the language tag as `und`. There's not much documentation about this but the official examples leave it as `und` and I haven't found any issues with or without the language tag on a video track. Feel free to correct me if you find otherwise.
  - Mark it as `Default`.
  - Mention the name of the `Encoder/Group` or simply region like `JPNBD` or `ITABD` if it's untouched BluRay Remux.

==- :icon-unmute: Audio Tracks

  - Mention the codec, channels, and quality in the `Name` field.
  - Language tag must be used appropriately reflecting the language of the audio.
  - Audio tracks must not be marked as `Forced`.
  - Regular `Japanese` audio must be tagged as `jpn` and marked as `Default` and `Original language`.
  - Regular `English` audio must be tagged as `eng` and marked as `Default`.
  - Any and all other regular audio tracks (e.g Spanish dub, German dub, etc) must be tagged with their respective language tag and marked as `Default`.
  - If you have multiple dialects of the same language, they must be tagged with the dialect and mentioned in the `Name` field. For example, Castilian Spanish must be tagged as `es-ES` while Latin American Spanish must be tagged as `es-419`.
  - Any and all non-Regular/Specialized tracks (e.g `commentary`, `descriptive audio`, etc) must be tagged with their respective flags and language but must not be marked as `Default`

==- :icon-log: Subtitle Tracks

  - Language tag must be used appropriately reflecting the language of the subtitles.
  - Regular subtitle tracks (i.e, `Full Subtitles`) must be tagged with the appropriate language and marked as `Default`.
  - The forced track (i.e, `Signs/Songs`) must be tagged with the appropriate language and marked as `Forced`. This track should not be marked as `Default` because it is not a "regular" track; instead, it is a specialized one meant to be used with dubs. The language tag must be identical to the language tag of the audio track it is intended to be used with.
  - There should only be one `Forced` track per language.
  - `Honorifics` track must be tagged as `enm` and `Default` but not `Forced`. *Note: This isn't a Matroska standard but a widely accepted convention in the anime community. Commercial software like [Plex](https://www.plex.tv/) also support this convention.*
  - If you have multiple dialects of the same language, they must be tagged with the dialect and mentioned in the `Name` field. For example, Castilian Spanish must be tagged as `es-ES` while Latin American Spanish must be tagged as `es-419`.
  - Any and all non-regular/specialized tracks (e.g `commentary`, `SDH`, etc) must be tagged with their respective flags and language but must not be marked as `Default`.

==- :icon-list-ordered: Track Order
  - The track order also plays an important part in the automatic selection of tracks.
  - Tracks should be grouped by language, with the regular track being the first within its language group.
  - Recommended track order is:
      1. Video
      2. Original audio group
          1. Original regular audio
          2. Original Specialized audio (commentary, descriptive, etc)
      3. Dub audio group
          1. Regular dub audio
          2. Specialized dub audio (commentary, descriptive, etc)
      4. Subtitle group related to Original audio
          1. Regular subtitle tracks for regular original audio
          2. Specialized subtitle track for audio tracks in the original audio group
      5. Subtitle group related to dub audio
          1. Regular subtitle tracks for the regular dub audio
          2. Specialized subtitle track for audio tracks in the dub audio group (forced, commentary, sdh, etc)
  - When you have two regular tracks of the same language, let's say a `Japanese 2.0` track and a `Japanese 5.1` track, and you have correctly tagged them, you'll notice that these end up being tagged identically. In this case, players will fall back to using track order to select the audio track. Now, it's pretty much up to your personal preference as to which one goes first and ends up being the real default for the end user.

  !!!
  Refer to the [Practical Example](#practical-example-basic) to better understand the track ordering
  !!!

===

==- :icon-checklist: Practical Example - Basic

  | Track       | Language | Name                    | Default            | Forced             | Additional Flags  |
  |-------------|----------|-------------------------|--------------------|--------------------|-------------------|
  | Video       | und      | Group                   | :white_check_mark: | :x:                | None              |
  | Audio #1    | jpn      | FLAC 2.0                | :white_check_mark: | :x:                | Original language |
  | Audio #2    | eng      | Opus 5.1 @ 320kb/s      | :white_check_mark: | :x:                | None              |
  | Subtitle #1 | eng      | Full Subtitles [Fansub] | :white_check_mark: | :x:                | None              |
  | Subtitle #2 | eng      | Signs/Songs [Fansub]    | :x:                | :white_check_mark: | None              |

==- :icon-checklist: Practical Example - Advanced

  | Track       | Language | Name                                    | Default            | Forced             | Additional Flags              |
  |-------------|----------|-----------------------------------------|--------------------|--------------------|-------------------------------|
  | Video       | und      | Group                                   | :white_check_mark: | :x:                | None                          |
  | Audio #1    | jpn      | FLAC 5.1                                | :white_check_mark: | :x:                | Original language             |
  | Audio #2    | jpn      | FLAC 2.0                                | :white_check_mark: | :x:                | Original language             |
  | Audio #3    | jpn      | FLAC 2.0 - Japanese Commentary          | :x:                | :x:                | Commentary, Original language |
  | Audio #4    | eng      | Opus 5.1 @ 320kb/s                      | :white_check_mark: | :x:                | None                          |
  | Audio #5    | eng      | Opus 2.0 @ 192kb/s - Commentary         | :x:                | :x:                | Commentary                    |
  | Audio #6    | eng      | Opus 2.0 @ 192kb/s - Descriptive        | :x:                | :x:                | Visual-impaired               |
  | Subtitle #1 | eng      | Full Subtitles [Fansub]                 | :white_check_mark: | :x:                | None                          |
  | Subtitle #2 | enm      | Honorifics [Fansub]                     | :white_check_mark: | :x:                | None                          |
  | Subtitle #3 | eng      | Japanese Commentary [USABD]             | :x:                | :x:                | Commentary                    |
  | Subtitle #4 | eng      | Signs/Songs [Fansub]                    | :x:                | :white_check_mark: | None                          |
  | Subtitle #5 | eng      | SDH [USABD]                             | :x:                | :x:                | Hearing-impaired              |
  | Subtitle #6 | eng      | English Commentary [USABD]              | :x:                | :x:                | Commentary                    |
  | Subtitle #7 | es-ES    | Full Subtitles (Castilian) [SPABD]      | :white_check_mark: | :x:                | None                          |
  | Subtitle #8 | es-419   | Full Subtitles (Latin American) [SPABD] | :white_check_mark: | :x:                | None                          |
  | Subtitle #9 | de       | Full Subtitles [GERBD]                  | :white_check_mark: | :x:                | None                          |

===

#### Related Info

==- :icon-info: Forced Flag

  ![Forced Flag](/static/advanced/muxing/forced_track_flag.png)

  This flag DOES NOT mean that a subtitle track will be permanently on the screen, which is a common misconception. Forced subtitles only provide subtitles when the characters speak a foreign or alien language, or a sign, flag, or other text in a scene is not translated in the localization and dubbing process. In our case, it's supposed to be displayed whenever the English dub has untranslated things like Japanese Signs and Songs like Opening/Ending or Inserts.

==- :icon-info: Default Flag 

  ![Default Flag](/static/advanced/muxing/default_track_flag.png)

  This flag DOES NOT mean that a track is going to be the default choice upon playback, instead it's a hint for the player indicating that a given track SHOULD be eligible to be automatically selected as the default track for a given language. If no tracks in a given language have the default track flag set, then all tracks in that language are eligible for automatic selection. This can be used to indicate that a track provides “regular service” suitable for users with default settings, as opposed to specialized services, such as commentary, hearing-impaired captions, or descriptive audio.

==- :icon-info: Player Support 

  | Player                                             | Respects Matroska Tags | Additional Notes                                                                                                                                                 |
  |----------------------------------------------------|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
  | [mpv](https://mpv.io/)                             | Yes                    |                                                                                                                                                                  |
  | [VLC](https://www.videolan.org/vlc/)               | Mostly                 | Doesn't respect the `Forced` flag.                                                                                                                               |
  | [Plex](https://www.plex.tv/)                       | Mostly                 | Plex respects Matroska tags (such as Forced, SDH, language, etc) with the exception of the `Default` flag. Track order takes precedence over the `Default` flag. |
  | [Jellyfin](https://jellyfin.org/)                  | Yes                    |                                                                                                                                                                  |
  | [Kodi](https://kodi.tv/)                           | Yes                    |                                                                                                                                                                  |
  | [clsid2/MPC-HC](https://github.com/clsid2/mpc-hc/) | Mostly                 | Doesn't respect the `Forced` flag.                                                                                                                               |

  !!!
  The latest version of each player was tested on 2024-01-09
  !!!

==- :icon-info: Full spec
  Everything I've covered in this section is a simplified version of the full spec, which you can read [here](https://www.matroska.org/technical/notes.html) if you want to.

===

### Adding LayoutRes to ASS Files

When working with ASS subtitle files, it's a good idea to explicitly add the `LayoutResX` and `LayoutResY` headers to [ensure your subtitles display correctly](https://slow.pics/c/EHRGN6GH). Here are three common cases to consider:

- Typically, you'll want `LayoutResX` and `LayoutResY` to match `PlayResX` and `PlayResY` for newly authored subtitles.

- If `LayoutResX` and `LayoutResY` are already set and they differ from `PlayResX` and `PlayResY`, it's best to keep the difference as-is. However, avoid creating new files with differing values.

- For older files that don't have `LayoutResX` and `LayoutResY`, you should make an educated guess. One approach is to set these values to match `PlayResX` and `PlayResY` or the storage size of the video, depending on which seems more appropriate for the situation. If you cannot make an educated guess, it's better to skip setting `LayoutResX` and `LayoutResY` to avoid potential layout issues.

To set `LayoutResX` and `LayoutResY` in Aegisub, follow these steps:

1. Open your subtitle file in Aegisub.
2. Go to the menu bar and click on `File`.
3. From the drop-down menu, select `Properties`.
4. In the `Script Properties` window, [you will see fields for `Script` (PlayRes) and `Layout` (LayoutRes)](/static/advanced/muxing/aegisub_script_properties.png).
5. Set the appropriate values and click `OK`.

!!!
As of writing this, you need the [git master build of arch1t3cht/Aegisub](https://github.com/arch1t3cht/Aegisub) to edit `LayoutRes`.
!!!


### Fonts

[![Font attachments](/static/advanced/muxing/mkvtoolnix6.png)](/static/advanced/muxing/mkvtoolnix6.png)

Fonts used by the `.ass` subtitles must be attached to the `.mkv` file for displaying subtitles accurately on the user's end. This can be easily done by dragging the fonts used by the `.ass` file into the `attachments` tab. You can usually source the correct fonts from the same fansub release you got the subtitles from. Alternatively, dropping the original fansub release in MKVToolNix will automatically carry over all the fonts in it. You can also get the fonts used by the `.ass` file with [Aegisub](https://github.com/arch1t3cht/Aegisub) or [FontCollector](https://github.com/moi15moi/FontCollector) if you have them on your system already.


### MKV Cropping

MKVToolNix allows you to set crop values for the video stream. This is quite helpful for cropping black bars without re-encoding. Players that support MKV crop values, such as mpv, will display the video as intended (without black bars), while players that do not support these values will continue to play it normally (with black bars) without any adverse effects.

Advantages of cropping and why you should do it:

- Cropping black bars allows the video to fill the entire screen. A couple of common examples would be letterboxed content on ultrawide displays or pillarboxed content on 4:3 displays, where black bars would otherwise prevent the video from filling the entire screen
- On 16:9 displays where cropping would result in black bars regardless, it's still beneficial because it avoids [dirty lines](https://silentaperture.gitlab.io/mdbook-guide/filtering/dirty_lines.html) caused by scaling

!!!warning
You must check that the aspect ratio is consistent throughout the entire file and only crop the smallest value. It's possible that the video might switch aspect ratio during its runtime, in which case a careless crop can result in the unintended removal of content. Thoroughly check for such changes before making any adjustments.
!!!

#### Two ways to crop

==- :icon-copy: Remuxing
  - Click on the video stream in MKVToolNix GUI
  - Go to the `Properties` tab on the right, and then navigate to the `Video Properties`
  - Enter your crop values in the format `LEFT, TOP, RIGHT, BOTTOM`, i.e, `0,180,0,180` to crop `180` pixels from top and bottom.

  [![The crop values are ordered as follows: `LEFT`, `TOP`, `RIGHT`, `BOTTOM`](/static/advanced/muxing/mkvcropping1.png)](/static/advanced/muxing/mkvcropping1.png)

==- :icon-pencil: Without Remuxing

  - Open `Header Editor` in the left pane
  - Add your file
  - Navigate to the Video track's properties
  - Adjust them as needed and then press `CTRL+S` to save without remuxing the file

  [![MKVToolNix GUI Header Editor](/static/advanced/muxing/mkvtoolnix_header_editor.png)](/static/advanced/muxing/mkvtoolnix_header_editor.png)

==- :icon-play: Result
  **Uncropped**
  ![Uncropped](/static/advanced/muxing/mkvcropping2.png)

  **MKV Cropped**
  ![MKV Cropped](/static/advanced/muxing/mkvcropping3.png)

===

### Container Delays

[![Container Delay](/static/advanced/muxing/mkvtoolnix5.png)](/static/advanced/muxing/mkvtoolnix5.png)

MKVToolNix allows you to set positive or negative delays on each track in order to synchronize them with the designated video track. This is usually the easiest way to sync different tracks together, but you should be careful with it. Avoid delays exceeding 1001ms, as they may lead to playback problems on different media players. Instead, consider syncing each track using their specific tools and only utilize container delays for smaller adjustments as a final resort.

!!!
The only exception to this is `TrueHD` audio, where container delay is your only option. Multi-channel `TrueHD` streams contain alternative presentations (downmixes): 2.0, 5.1 and 7.1. These are embeded either as discrete channels or derived through custom coefficients.
These alternative presentations are lost once decoded, even if they were to be encoded/transcoded back to TrueHD.
!!!

### QoL stuff

==- :icon-terminal: mkvpropedit command to wipe potentially identifiable info and generate track statistics
  
  This is commonly seen in WEB-DL releases.

  The `--add-track-statistics-tags` option calculates statistics for all tracks in a file and adds new statistics tags for them. If the file already contains such tags then they'll be updated. The other options are pretty self-explanatory.

  +++ Single File
    mkvpropedit "file.mkv" --tags all: --add-track-statistics-tags --edit info --delete date --set muxing-application="" --set writing-application=""
  +++ Batch
    for %X IN (*.mkv) do mkvpropedit "%X" --tags all: --add-track-statistics-tags --edit info --delete date --set muxing-application="" --set writing-application=""
  +++ Shell
    for file in *.mkv; do mkvpropedit "$file" --tags all: --add-track-statistics-tags --edit info --delete date --set muxing-application="" --set writing-application=""; done
  +++

==- :icon-terminal: mkvpropedit command to retag an `mkv` file without remuxing

  !!!
  This is an example, you'll have to edit the command further to apply to your specific file
  !!!

  +++ Windows

  ```batch
  for %X in (*.mkv) do mkvpropedit "%X" --add-track-statistics-tags --edit info --delete title ^
  --edit track:v1 --set name="Encode Group"              --set language=jpn --set flag-default=1 ^
  --edit track:a1 --set name="FLAC 2.0"                  --set language=jpn --set flag-default=1 ^
  --edit track:a2 --set name="Opus 5.1 @ 320kb/s"        --set language=eng --set flag-default=1 ^
  --edit track:s1 --set name="Full Subtitles [Fansub]"   --set language=eng --set flag-default=1 --set flag-forced=0 ^
  --edit track:s2 --set name="Honorifics [Fansub]"       --set language=enm --set flag-default=1 --set flag-forced=0 ^
  --edit track:s3 --set name="Signs/Songs [Fansub]"      --set language=eng --set flag-default=0 --set flag-forced=1 ^
  --edit track:s4 --set name="Full Subtitles [Official]" --set language=spa --set flag-default=1 --set flag-forced=0
  ```

  +++ Linux

  ```shell
  for file in *.mkv; do
    mkvpropedit "${file}" --add-track-statistics-tags --edit info --delete title \
    --edit track:v1 --set name="Encode Group"              --set language=jpn --set flag-default=1 \
    --edit track:a1 --set name="FLAC 2.0"                  --set language=jpn --set flag-default=1 \
    --edit track:a2 --set name="Opus 5.1 @ 320kb/s"        --set language=eng --set flag-default=1 \
    --edit track:s1 --set name="Full Subtitles [Fansub]"   --set language=eng --set flag-default=1 --set flag-forced=0 \
    --edit track:s2 --set name="Honorifics [Fansub]"       --set language=enm --set flag-default=1 --set flag-forced=0 \
    --edit track:s3 --set name="Signs/Songs [Fansub]"      --set language=eng --set flag-default=0 --set flag-forced=1 \
    --edit track:s4 --set name="Full Subtitles [Official]" --set language=spa --set flag-default=1 --set flag-forced=0
  done
  ```

  +++

===
