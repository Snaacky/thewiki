---
label: Muxing
description: Muxing files with MKVToolNix
image: /static/muxing/muxingembed.gif
order: -2
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

   [![](/static/muxing/mkvtoolnix.png)](/static/muxing/mkvtoolnix.png)

2. You can either right-click anywhere in the top "Source Files" box or drag and drop your file in it.

   - Once you add your source file, all the tracks in your file will show up in the box below, along with other relevant things in each tab. 
   - Tracks will be randomly assigned a color to indicate what source they belong to.
   - Checking or unchecking a track will decide if it'll be kept or removed in the output file.
   - By default, anything you won't explicitly uncheck will be copied over to the new output file.
   - Clicking on any track allows you to modify several properties in the window on the right. It's important you get this right and it's covered in more details [below](#correct-tagging).

   [![](/static/muxing/mkvtoolnix2.png)](/static/muxing/mkvtoolnix2.png)

3. Once you're done making your changes, assign a [name](/advanced/release-standards/#naming) to your file and hit `Start multiplexing`. Make sure to check all 3 tabs to ensure what's being copied over.

   [![](/static/muxing/mkvtoolnix3.png)](/static/muxing/mkvtoolnix3.png)

   !!!
   MKVToolNix also allows you to [generate a commandline](/static/muxing/mkvtoolnix4.png) with all the changes you made in the GUI.
   !!!

### Correct Tagging

The `Properties` tab allows you tag each track with various flags. Tagging a track correctly is very important and must be done correctly because proper tagging enables a player to autoselect the correct language streams for audio and subtitles. Tags can be edited in the MKVToolNix or mkvpropedit without remuxing.

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

!!!info
The [`Forced display`](https://user-images.githubusercontent.com/78981416/242083337-174f6edf-8ac7-4bbf-a4ea-1dd46f66d3d7.png) flag here DOES NOT mean that these subs will be permanently on the screen, which is a common misconception. Forced subtitles only provide subtitles when the characters speak a foreign or alien language, or a sign, flag, or other text in a scene is not translated in the localization and dubbing process. In our case, it's supposed to be displayed whenever the English dub has untranslated things like Japanese Signs and Songs like Opening/Ending or Inserts.
!!!

Here's an example of what a properly tagged file would look like:

| Track        | Language | Name                      | Default | Forced |
| ------------ | -------- | ------------------------- | ------- | ------ |
| Video        | jpn      | Encode Group              | yes     | no     |
| Audio #1     | jpn      | FLAC 2.0                  | yes     | no     |
| Audio #2     | eng      | Opus 5.1 @ 320kb/s        | no      | no     |
| Subtitles #1 | eng      | Full Subtitles [Fansub]   | yes     | no     |
| Subtitles #2 | enm      | Honorifics [Fansub]       | no      | no     |
| Subtitles #3 | eng      | Signs/Songs [Fansub]      | no      | yes    |
| Subtitles #4 | eng      | Full Subtitles [Official] | no      | no     |

!!!warning
Newer mkvtoolnix versions automatically set the default flag to `yes` on all streams. This is technically the correct use for the flag but all players do not have the intended results with this kind of tagging.
!!!

==- :icon-terminal: Command to tag everything properly for an existing file without remuxing (assuming the order of tracks is correct)
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

===

### Fonts

[![Font attachments](/static/muxing/mkvtoolnix6.png)](/static/muxing/mkvtoolnix6.png)

Fonts used by the `.ass` subtitles must be attached to the `.mkv` file for displaying subtitles accurately on the user's end. This can be easily done by dragging the fonts used by the `.ass` file into the `attachments` tab. You can usually source the correct fonts from the same fansub release you got the subtitles from. You can also get the fonts used by the `.ass` file with [Aegisub](https://github.com/arch1t3cht/Aegisub) or [FontCollector](https://github.com/moi15moi/FontCollector) if you have them on your system already.

### MKV Cropping

MKVToolNix allows you to set crop values for the video stream. This is quite helpful for cropping black bars without having to encode. Players that support MKV crop values, such as mpv, will display the video as intended (without black bars), while players that do not support these values will continue to play it normally (with black bars) without any adverse effects.

Advantages of cropping and why you should do it:

- Cropping black bars allows the video to fill the entire screen. A couple of common examples would be letterboxed content on ultrawide displays or pillarboxed content on 4:3 displays, where black bars would otherwise prevent the video from filling the entire screen
- On 16:9 displays where cropping would result in black bars regardless, it's still beneficial because it avoids [dirty lines](https://silentaperture.gitlab.io/mdbook-guide/filtering/dirty_lines.html) caused by scaling

Cropping is quite simple via the MKVToolNix GUI, simply click on the video stream, go to the `Properties` tab on the right, and then navigate to the `Video Properties`.

[![The crop values are ordered as follows: `LEFT`, `TOP`, `RIGHT`, `BOTTOM`](/static/muxing/mkvcropping1.png)](/static/muxing/mkvcropping1.png)

Alternatively, you can use the `Header Editor` in the left pane to add the crop values and then press `CTRL+S` to save without the need to remux the file.

[![MKVToolNix GUI Header Editor](/static/muxing/mkvtoolnix_header_editor.png)](/static/muxing/mkvtoolnix_header_editor.png)

Here's an example of said feature in action:

[![Uncropped](/static/muxing/mkvcropping2.png)](/static/muxing/mkvcropping2.png)

[![MKV Cropped](/static/muxing/mkvcropping3.png)](/static/muxing/mkvcropping3.png)

### Container Delays

[![Container Delay](/static/muxing/mkvtoolnix5.png)](/static/muxing/mkvtoolnix5.png)

MKVToolNix allows you to set positive or negative delays on each track in order to synchronize them with the designated video track. This is usually the easiest way to sync different tracks together, but you should be careful with it. Avoid delays exceeding 1000ms, as they may lead to playback problems on different media players. Instead, consider syncing each track using their specific tools and only utilize container delays for smaller adjustments as a final resort.
