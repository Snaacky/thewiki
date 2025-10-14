---
label: Comparison
description: Learn how to compare various sources of a video
image: /static/comparison/hyouka.gif
---

# Comparison

Quality comparisons are frequently used within the enthusiast community to compare the video quality offered by different sources/releases. [It serves as a great way to distinguish the differences between good and bad sources](/guides/quality/#types-of-releases), and can help you identify video issues and determine which one to keep.

This guide goes through the process of setting up and effectively utilizing [VSPreview](https://github.com/Jaded-Encoding-Thaumaturgy/vs-preview), a previewer utility for [VapourSynth](https://github.com/vapoursynth/vapoursynth), to produce useful quality comparisons that will allow you to ascertain which video offers the best visual experience.

## Setup

### VapourSynth

VapourSynth is an open-source video processing framework. It handles all of your sources, playback, and filtering options in conjunction with VSPreview.

#### Dependencies

- [Python 3.13.x](https://www.python.org/downloads/) - *Download the latest 3.13.x version at the bottom and select `Add python.exe to PATH` during installation*
- [Git](https://gitforwindows.org/) - *Repeatedly click next during installation*

#### Installation

Download and install the latest `VapourSynth-x64-RXX.exe` from [VapourSynth](https://github.com/vapoursynth/vapoursynth/releases). *During installation, select `Install for me only`.*

![Vapoursynth release](/static/comparison/vs-download.png)

### VSPreview

VSPreview is a previewer application for scripts created in VapourSynth. It features a simple graphical interface to allow you to use VapourSynth's features (and create comparisons) with ease.

#### Dependencies

In order to create comparisons with VSPreview, you will need a few necessary dependencies:

- [`ffms2`](https://github.com/FFMS/ffms2), [`fpng`](https://github.com/richgel999/fpng), [`LibP2P`](https://github.com/DJATOM/LibP2P-Vapoursynth), [`LSMASHSource`](https://github.com/HomeOfAviSynthPlusEvolution/L-SMASH-Works), [`vs-placebo`](https://github.com/sgt0/vs-placebo), [`resize2`](https://github.com/Jaded-Encoding-Thaumaturgy/vapoursynth-resize2), and [`akarin`](https://github.com/Jaded-Encoding-Thaumaturgy/akarin-vapoursynth-plugin) can be installed using `vsrepo` from [VapourSynth](https://github.com/vapoursynth/vapoursynth/releases). In your terminal, run the following:

  ```powershell
  vsrepo.py install ffms2 fpng libp2p lsmas placebo resize2 akarin
  ```

  !!!
  If `vsrepo.py` command doesn't work, make sure Windows is set to open `.py` files with Python. *[You may also need to add it to the `PATHEXT` environment variable.](/static/comparison/python-pathext.png)*
  !!!

- [`awsmfunc`](https://github.com/OpusGang/awsmfunc) can be installed using `pip`:

  ```powershell
  python -m pip install git+https://github.com/OpusGang/awsmfunc.git
  ```

#### Installation

!!!warning
If you have the previous vs-jet packages installed, please follow the instructions on [`vs-jet`](https://github.com/Jaded-Encoding-Thaumaturgy/vs-jet) page to uninstall them. You need to do this before installing the newer vs-jetpack package.
!!!

Download and install [`vs-jetpack`](https://github.com/Jaded-Encoding-Thaumaturgy/vs-jetpack) and [`vs-preview`](https://github.com/Jaded-Encoding-Thaumaturgy/vs-preview) using `pip` in your terminal:

```powershell
python -m pip install vsjetpack vspreview
```

## Usage

### Scripting

In order to create a comparison, you will need to create a VapourSynth script. This script outlines the parameters and files which VSPreview will use when generating your comparison.

Create a file called `comp.py`. Launch it in your favorite text editor and add sections as desired:

==- :icon-file-code: Initial script [!badge variant="danger" text="Required"]

The following `comp.py` script loads your sources into the previewer.

!!!
Make sure to comment (add `##` to the beginning of the line) and uncomment lines as needed.
!!!

```py
## Dependencies: Allows vspreview to run [required; do not remove]
import vstools
from vstools import vs, core
from vskernels import Hermite, EwaLanczos
from vspreview import set_output

## <Additional dependencies>
## Place any additional dependencies you want to use in your comp here
## <End of additional dependencies>

## File paths: Hold Shift and Right-click your file, select copy as path, and paste it here. For NF WEB-DLs change core.lsmas.LWLibavSource to core.ffms2.Source
clip1 = core.lsmas.LWLibavSource(r"C:\Paste\File\Path\Here.mkv", rap_verification=False)
clip2 = core.lsmas.LWLibavSource(r"C:\Paste\File\Path\Here.mkv", rap_verification=False)
clip3 = core.lsmas.LWLibavSource(r"C:\Paste\File\Path\Here.mkv", rap_verification=False)

## Source: Name of the source
source1 = "FirstSourceName"
source2 = "SecondSourceName"
source3 = "ThirdSourceName"

## <Additional comp settings>
## Place any additional settings you want to use in your comp here
## <End of additional comp settings>

## Output: Comment/uncomment as needed depending on how many clips you're comparing
set_output(clip1, name=source1)
set_output(clip2, name=source2)
set_output(clip3, name=source3)
```

{.compact}
Section             | Description
--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------
**Dependencies**    | Dependencies required to create comparisons in VSPreview
**File paths**      | The location of your source file
**Source**          | The source name. This should be the name of the encoder (*not* muxer) or the specific source used for untouched releases (e.g. Beatrice-Raws, JPN BD, DSNP)
**Output**          | Parameter that allows clips to appear in VSPreview

==- :icon-play: Playback (frame rate, FieldBased, inverse telecine)

#### Frame Rate

Sets the source frame rate (fps) based on fractional input (`fpsnum`/`fpsden`). For example, `fpsnum=24000` and `fpsden=1001` forces the clip frame rate to 23.976 fps. *This should be used on sources that have different frame rates that don't automatically stay in sync.*

!!!
If a clip stays in sync without changing during scrubbing, you should note that the specific source has dropped or duplicate frames.
!!!

```py
## Frame rate: Change fps to match other sources (needed for when the previewer is unable to automatically keep them in sync)
clip1 = core.std.AssumeFPS(clip1, fpsnum=24000, fpsden=1001)
clip2 = core.std.AssumeFPS(clip2, fpsnum=25000, fpsden=1000)
clip3 = core.std.AssumeFPS(clip3, fpsnum=24000, fpsden=1000)
```

#### FieldBased

Sets interlaced flagged content that may be progressive as progressive for higher quality chroma upscaling.

```py
## FieldBased: Tags the content as progressive (0); used for progressive content tagged as interlaced
clip1 = core.std.SetFieldBased(clip1, 0)
clip2 = core.std.SetFieldBased(clip2, 0)
clip3 = core.std.SetFieldBased(clip3, 0)
```

#### Inverse Telecine

Quick inverse telecine filter for converting telecined clips (usually 30 fps interlaced video) back to the original framerate (24 fps progressive).

!!!warning
[`vivtc`](https://github.com/vapoursynth/vivtc) needs to be installed in order for the following snippet to work. You can install it with `vsrepo.py install vivtc`.
!!!

```py
## Inverse telecine: Fixes telecined video
clip1 = core.vivtc.VFM(clip1, 1)
clip1 = core.vivtc.VDecimate(clip1)
```

==- :icon-file-media: Framing (cropping, scaling, trimming)

#### Cropping

Crops the source video by *n* pixels from the selected side. For example, `left=20` will remove 20 horizontal pixels starting from the left side. *This should be used on sources that use letterboxing or other form of borders.*

!!!warning
If you are cropping with odd numbers, you will need to convert your clip to 16-bit depth with 4:4:4 chroma subsampling. (see [Color & contrast](#color--contrast-depth-debanding-tonemapping-gamma-color-spaces-drc) -> *Depth*).
!!!

```py
## Cropping: Removes letterboxing (black bars) [16-bit required for odd numbers]
clip1 = core.std.Crop(clip1, left=240, right=240, top=0, bottom=0)
clip2 = core.std.Crop(clip2, left=0, right=0, top=276, bottom=276)
clip3 = core.std.Crop(clip3, left=0, right=0, top=21, bottom=21)
```

!!!
Make sure to check for variable aspect ratios throughout the file and only crop the smallest border. We recommend using [ShareX](https://getsharex.com) for quickly calculating border size.
!!!

#### Scaling

Downscales or upscales the video. *This should be used to match sources that have differing resolutions.*

- For upscaling (e.g. 720p -> 1080p), use `EwaLanczos`:

  ```py
  ## Upscaling: Increases the resolution of clips to match the highest resolution using EwaLanczos (equivalent scaling to mpv's high-quality profile)
  clip1 = EwaLanczos.scale(clip1, 1440, 1080, sigmoid=True)
  clip2 = EwaLanczos.scale(clip2, 1920, 1080, sigmoid=True)
  clip3 = EwaLanczos.scale(clip3, 3840, 2160, sigmoid=True)
  ```

!!!warning
If the colors of your sources do not match after upscaling, see the Color Spaces section below
!!!

- For downscaling (e.g. 2160p/4K -> 1080p), use `Hermite`:

  ```py
  ## Downscaling: Decreases the resolution of clips to match the lowest resolution using Hermite (equivalent scaling to mpv's high-quality profile)
  clip1 = Hermite.scale(clip1, 1280, 720, linear=True)
  clip2 = Hermite.scale(clip2, 1920, 1080, linear=True)
  clip3 = Hermite.scale(clip3, 3840, 2160, linear=True)
  ```

!!!warning
Downscaling is generally not recommended. We suggest upscaling your sources to match the highest resolution unless you have a specific reason (e.g. comparing how a higher resolution file would look on a lower resolution display).
!!!

#### Trimming

Removes the first *n* frames from the source. For example, `[24:]` will skip the first 24 frames and start the source at frame 25. *This should be used on sources that are out of sync.*

To get the frame difference, find a unique frame (e.g. scene changes) in the correct and incorrect source. Note the frame numbers each one begin at, then set the difference of the two for the incorrect source.

```py
## Trimming: Trim frames to match clips (calculate the frame difference and enter the number here)
clip1 = clip1[0:]
clip2 = clip2[24:]
clip3 = clip3[144:]
```

!!!secondary
For more advanced trimming such as chaining, splicing, and looping, see [Vapoursynth's docs](https://www.vapoursynth.com/doc/pythonreference.html#slicing-and-other-syntactic-sugar).
!!!

==- :icon-paintbrush: Color & contrast (depth, debanding, tonemapping, gamma, color spaces, DRC)

#### Depth

Converts clips to 16-bit depth with 4:4:4 chroma subsampling. *Required for filters such as cropping (with odd numbers) or tonemapping.*

```py
## Depth: Convert clips to 16-bit 4:4:4 [required for cropping with odd numbers or tonemapping]
clip1 = core.resize.Lanczos(clip1, format=vs.YUV444P16)
clip2 = core.resize.Lanczos(clip2, format=vs.YUV444P16)
clip3 = core.resize.Lanczos(clip3, format=vs.YUV444P16)
```

#### Debanding

Applies a debanding filter to the selected clip(s).

!!!
Otherwise competitive sources with obvious banding should be debanded to see how they'd fare with [mpv's](/tutorials/mpv/) built-in deband filter. The debanded clip should *never* replace the original. Instead, it should be added as an additional node.
!!!

```py
clip1 = vstools.depth(clip1, 32)
clip1 = core.placebo.Deband(clip1, planes=7, iterations=4, threshold=3.44, radius=16.0, grain=0.5)
clip1 = vstools.depth(clip1, 8)
```

#### Tonemapping

Converts the dynamic range of the source (i.e. HDR/DV -> SDR).

- For converting HDR (washed out colors) -> SDR, set `source_colorspace=ColorSpace.HDR10`
- For converting DV (green/purple hue) -> SDR, set `source_colorspace=ColorSpace.DOVI`

!!!warning
If you want to use tonemapping, you will need to change the color depth to 16-bit (see [above](#depth)).
!!!

```py
## Additional imports [Paste these at the very top of your script]
from awsmfunc.types.placebo import PlaceboColorSpace as ColorSpace
from awsmfunc.types.placebo import PlaceboTonemapFunction as Tonemap
from awsmfunc.types.placebo import PlaceboGamutMapping as Gamut
from awsmfunc.types.placebo import PlaceboTonemapOpts

## Tonemapping: Converts the dynamic range of the source [16-bit required]
## Specify the arguments based on your sources; play with different values when comparing against an SDR source to best match it
clip1args = PlaceboTonemapOpts(source_colorspace=ColorSpace.DOVI, target_colorspace=ColorSpace.SDR, tone_map_function=Tonemap.ST2094_40, gamut_mapping=Gamut.Clip, peak_detect=True, use_dovi=True, contrast_recovery=0.3)
clip2args = PlaceboTonemapOpts(source_colorspace=ColorSpace.HDR10, target_colorspace=ColorSpace.SDR, tone_map_function=Tonemap.ST2094_40, gamut_mapping=Gamut.Clip, peak_detect=True, use_dovi=False, contrast_recovery=0.3)
clip3args = PlaceboTonemapOpts(source_colorspace=ColorSpace.HDR10, target_colorspace=ColorSpace.SDR, tone_map_function=Tonemap.Spline, gamut_mapping=Gamut.Darken, peak_detect=True, use_dovi=False, contrast_recovery=0.3, dst_max=120)

## Apply tonemapping
clip1 = core.placebo.Tonemap(clip1, **clip1args.vsplacebo_dict())
clip2 = core.placebo.Tonemap(clip2, **clip2args.vsplacebo_dict())
clip3 = core.placebo.Tonemap(clip3, **clip3args.vsplacebo_dict())

## Retag video to 709 after tonemapping [required]
clip1 = core.std.SetFrameProps(clip1, _Matrix=vs.MATRIX_BT709, _Transfer=vs.TRANSFER_BT709, _Primaries=vs.PRIMARIES_BT709)
clip2 = core.std.SetFrameProps(clip2, _Matrix=vs.MATRIX_BT709, _Transfer=vs.TRANSFER_BT709, _Primaries=vs.PRIMARIES_BT709)
clip3 = core.std.SetFrameProps(clip3, _Matrix=vs.MATRIX_BT709, _Transfer=vs.TRANSFER_BT709, _Primaries=vs.PRIMARIES_BT709)
```

#### Clipping
Sometimes the source will be SDR in an HDR container, often seen with anime on Netflix. In these cases you can clip the source to get an exact match to SDR, unlike with traditional tonemapping.

```py
## Clip HDR source to SDR
clip1 = core.resize.Bicubic(clip1, matrix=1, transfer=1, primaries=1)

## Clip DV source to SDR
clip2args = PlaceboTonemapOpts(source_colorspace=ColorSpace.DOVI, target_colorspace=ColorSpace.HDR10, use_dovi=True)
clip2 = core.placebo.Tonemap(clip2, **clip2args.vsplacebo_dict())
clip2 = core.std.SetFrameProps(clip2, _Matrix=vs.MATRIX_BT2020_NCL, _Transfer=vs.TRANSFER_ST2084, _Primaries=vs.PRIMARIES_BT2020)
clip2 = core.resize.Bicubic(clip2, matrix=1, transfer=1, primaries=1)
```


!!!
Refer to the [libplacebo](https://libplacebo.org/options/) and [vs-placebo](https://github.com/sgt0/vs-placebo?tab=readme-ov-file#tonemap) docs to gain a better understanding of what each parameter does.
!!!

#### Gamma

Adjusts the gamma level of the video. *This should only be used to fix the QuickTime gamma bug or similar issues where one source will appear much brighter than the rest.*

```py
## Gamma: Fixes gamma bug (i.e. one source is significantly brighter than the others) [32-bit required]
## Convert clips to 32-bit [required for gamma fix]
clip1 = vstools.depth(clip1, 32)
clip2 = vstools.depth(clip2, 32)
clip3 = vstools.depth(clip3, 32)
## Apply fix
clip1 = core.std.Levels(clip1, gamma=0.88, planes=0)
clip2 = core.std.Levels(clip2, gamma=0.88, planes=0)
clip3 = core.std.Levels(clip3, gamma=0.88, planes=0)
```

#### Color Spaces

Sets the correct color information, should be used on sources with incorrect/missing metadata, particularly when upscaling.

```py
## Color Spaces: Tag sources with incorrect/missing metadata; typically used for 4K SDR and upscaled/downscaled content (colors will be off, particularly reds, greens, and blues)

# HD BD/WEB
clip1 = core.std.SetFrameProps(clip1, _Matrix=vs.MATRIX_BT709, _Transfer=vs.TRANSFER_BT709, _Primaries=vs.PRIMARIES_BT709)

# PAL DVD
clip2 = core.std.SetFrameProps(clip2, _Matrix=vs.MATRIX_BT470_BG, _Transfer=vs.TRANSFER_BT470_BG, _Primaries=vs.PRIMARIES_BT470_BG)

# NTSC DVD
clip3 = core.std.SetFrameProps(clip3, _Matrix=vs.MATRIX_ST170_M, _Transfer=vs.TRANSFER_BT601, _Primaries=vs.PRIMARIES_ST170_M)

# HD BD/WEB with incorrectly tagged Matrix
clip4 = core.std.SetFrameProps(clip4, _Matrix=vs.MATRIX_ST170_M), _Transfer=vs.TRANSFER_BT709, _Primaries=vs.PRIMARIES_BT709)

# HD BD/WEB with incorrectly converted Matrix
clip5 = core.resize.Point(clip5, _Matrix=vs.MATRIX_ST170_M)
clip5 = core.std.SetFrameProp(clip5, _Matrix=vs.MATRIX_BT709)
```

#### Double-Range Compression (DRC)

Fixes washed out/blown out colors on selected sources.

```py
## Fix DRC: Repairs sources with very washed out colors
clip1 = core.resize.Point(clip1, range_in=0, range=1, dither_type="error_diffusion")
clip1 = core.std.SetFrameProp(clip1, prop="_ColorRange", intval=1)
```

```py
## Fix Reverse DRC: Repairs sources with extremely crushed blacks and blown out highlights
clip1 = core.std.SetFrameProp(clip1, prop="_ColorRange", intval=1)
```

==-

### Running

To run your comparison script, launch a terminal window in your working directory and run the following:

```powershell
vspreview comp.py
```

Alternatively, you can create a `comp.bat` file, replacing `C:\path\to\comp.py` with the exact file path to your script:

```powershell
vspreview "C:\path\to\comp.py"
```

## First-time Setup

1. Drag the *Plugins* menu from the right-side of the VSPreview window and open the *SlowPics Comps* tab. Alternatively, you can access the *Plugins* menu using `Ctrl+P`

2. Under *Settings*, set the following:
   - Set *Collection Name Template* to `{tmdb_title} ({tmdb_year}) - S01E01 - {video_nodes}`
   - Set *Compression Type* to `Slow`
   - Enter your [slow.pics](https://slow.pics) username and password (optional)
   - Tick the *Public Flag*

3. Click the *Settings* button on the bottom tab. Under *Main*, change *Save Plugins Bar Position* to `Global`

4. Click the *Playback* button on the bottom tab. Set the bottom-left value to an odd number (e.g `109`)

Once complete, close and relaunch VSPreview to apply these changes.

## Comparing

### Basic Keybinds

For the purpose of making comparisons, you will only need the following binds:

Key                        | Action
---------------------------|------------------------------------------------------------------
`PgDown` or `Shift`+`Left` | Moves back *n* frames (default: *n = 1*)
`PgUp` or `Shift`+`Right`  | Moves forward *n* frames (default: *n = 1*)
`Number keys`              | Switches to source *n* (e.g. `2` switches to the second source)
`Ctrl`+`Space`             | Marks the current frame number
`Ctrl`+`P`                 | Opens the plugins window

!!!
If you wish to manually set your keybinds, you can find them under *Settings* -> *Shortcuts*.
!!!

### Capturing

1. Open the *Plugins* window and select the *SlowPics Comps* tab

2. Set the TMDB type and ID from [The Movie Database](https://www.themoviedb.org) (e.g. `94605` for [Arcane](https://www.themoviedb.org/tv/94605))

3. Before creating a comparison, skim through all your sources and ensure that they are displaying correctly. Most issues can be fixed using [filters](#scripting) in your comparison script

4. Select the frames to be uploaded. There are multiple methods to doing so:
   - **Manual:** Scrub through the video and mark each frame using `Ctrl`+`Space`
   - **Automatic:** Specify an amount of random frames in the *Plugin* menu. You may choose to use this feature alongside capturing frames manually
   - Frames should ideally show a variety of scenarios (e.g. light/dark, static/high-motion, flat/grainy, etc.). Particularly, you may want to capture scenes with on-screen text and bright reds (if possible).

5. Hit the *Start Upload* button. VSPreview will automatically screenshot all selected frames and upload a comparison to [slow.pics](https://slow.pics)

!!!warning
We don't recommend using the dark/light frames feature, as it currently has many issues.
!!!

## Choosing Sources (Anime)
**The following will help make your comparison as effective as possible**
- Include every available BD, you generally will need to check U2 (Private tracker)
- If 2 sources have identical video, only include 1 and label it as such (eg JPN/USA BD)
- We generally recommend picking episode 2 to ensure both the OP and ED are shown
- Include at least 1 web source when available
- Include all relevant encodes, see below

==- Source Hierarchy
+++ Tier 1
**These sources should always be included**
- **Any BD/WEB fansub release**
- Akatomba-Raws
- Beatrice-Raws
- H-Enc
- HQR
- Kagura
- Kawaiika-Raws
- km
- Kuroi-raws
- mottoj
- NanoAlchemist
- neko-raws
- Raws-Maji/KnK
- Salender-Raws
- SCY
- Seicher
- sergey_krs
- SOFCJ-Raws
- UQW
- Urotsuki
- VCB-Studio
- YURASUKA
- Yurasyk/Chyrka
- Yousei-raws
- Zagzad
- `=^_^=`/frost

+++ Tier 2
**These sources should be included when the previous tier is not available, or when the only existing muxes use them**
- Almighty
- ANK-Raws
- CBM
- DmonHiro
- IrizaRaws
- iAHD
- jsum
- Koten_Gars
- kuchikirukia
- Lowpower-Raws
- moscowgolem
- Moozzi2
- philosophy-raws
- ReinForce
- SEV
- Snow-Raws
- UCCUSS

+++ Tier 3
**These sources should never be included as they provide no value**
- **Any mini-encode**
- **Any re-encode**
- 7³ACG
- AI-RAWS
- Centaurea-Raws
- DarkDream
- DBD-Raws
- Deadmau- RAWS
- FY-Raws
- GHOST
- Salieri
- Shiniori-Raws

+++ Web
**Generally you will only need 1 web source in the comparison, which should be picked in the following order**
1. Crunchyroll (CR) - Should be the latest 8Mbps stream, if not publicly available [check this list](https://bin.theindex.moe/?c7a22bd18ce6d196#9Uxu6xHeZbYTV1wkM3FWhebLiPdUx37XkBQzF7CBvhHG)
1. Disney+ (DSNP)
1. Netflix (NF)
1. Animation Digital Network (ADN)
1. Amazon (AMZN)
1. HIDIVE (HIDI)

==-

## Additional Scripts

### Automation

If VSPreview is too complicated to set up, you can use [McBaws' script](https://github.com/McBaws/comp) to automatically generate the comparisons for you. However, this script is more limited compared to VSPreview.
