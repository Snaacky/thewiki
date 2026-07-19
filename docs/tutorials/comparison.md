---
label: Comparison
description: Learn how to compare different video sources
---

# Comparison

Quality comparisons allow you to evaluate different video sources/releases and identify differences in visual quality, helping determine which source provides the best experience.  
This guide covers how to set up and use [VSView](https://jaded-encoding-thaumaturgy.github.io/vs-view/) with [VapourSynth](https://github.com/vapoursynth/vapoursynth) to create accurate comparisons.

!!! VSPreview is deprecated

As of May 13th 2026, VSPreview has been archived and will no longer receive updates. Fresh installations require manual modifications to function correctly, and compatibility with third-party services such as slow.pics is not guaranteed indefinitely.  
As such, this guide now exclusively covers VSView.
!!!

## Installing

+++ uv [!badge icon=":heart:" variant="info"]

- Install [`uv`](https://docs.astral.sh/uv/getting-started/installation/)

- Make a folder that will contain all your comping related files

- Open your terminal in said folder and type `uv venv` and hit enter. 

- Paste the command below and hit enter again to install all the dependencies.
```py
uv pip install vsview[recommended] vsjetpack[full] awsmfunc vsfpng
```

+++ pip

- Install [`Python`](https://www.python.org/downloads/)

- Open your terminal, paste the command below, and hit enter to install all the dependencies.
```py
pip install vsview[recommended] vsjetpack[full] awsmfunc vsfpng
```
+++


## Scripting

In order to create a comparison, you will need to create a VapourSynth script.

Create a file called `comp.py`. Launch it in your favorite text editor and add sections as desired:

!!! Coming from VSPreview?

VSPreview scripts need a few modifications to function with VSView.

==- Migration steps
#### 1. Update the `vstools` import

Remove `set_output` from the `vstools` import:

```diff
- from vstools import vs, core, depth, set_output, PropEnum, Matrix, Transfer, Primaries, ColorRange, FieldBased
+ from vstools import vs, core, depth, PropEnum, Matrix, Transfer, Primaries, ColorRange, FieldBased
```

#### 2. Add the `vsview` import

Add the following line:

```diff
+ from vsview import set_output
```

#### 3. Update `set_output()` usage

Remove the `name=` argument:

```diff
- set_output(clip1, name=source1)
+ set_output(clip1, source1)
```
After these changes, your VSPreview script should be compatible with VSView.
==- 
!!!

==- :icon-file-code: Initial script [!badge variant="danger" text="Required"]

The following `comp.py` script loads your sources into the previewer.

!!!
Make sure to comment (add `##` to the beginning of the line) and uncomment lines as needed.
!!!

```py
## Dependencies: Allows VSView to run [required; do not remove]
from vstools import vs, core, depth, PropEnum, Matrix, Transfer, Primaries, ColorRange, FieldBased, ChromaLocation
from vssource import FFMS2, LSMAS
from vskernels import Point, EwaLanczosSharp, Hermite
from vsdeinterlace import vfm, vdecimate
from awsmfunc.types.placebo import PlaceboColorSpace as ColorSpace
from awsmfunc.types.placebo import PlaceboTonemapFunction as Tonemap
from awsmfunc.types.placebo import PlaceboGamutMapping as Gamut
from awsmfunc.types.placebo import PlaceboTonemapOpts
from vsview import set_output

## File paths: Hold Shift and Right-click your file, select copy as path, and paste it here. Change LSMAS to FFMS2 if you encounter any issues
clip1 = LSMAS.source(r"C:\Paste\File\Path\Here.mkv")
clip2 = LSMAS.source(r"C:\Paste\File\Path\Here.mkv")
clip3 = LSMAS.source(r"C:\Paste\File\Path\Here.mkv")

## Source: Name of the source
source1 = "FirstSourceName"
source2 = "SecondSourceName"
source3 = "ThirdSourceName"

## <Additional comp settings>
## Place any additional settings you want to use in your comp here
## <End of additional comp settings>

## Output: Comment/uncomment as needed depending on how many clips you're comparing
set_output(clip1, source1)
set_output(clip2, source2)
set_output(clip3, source3)
```

{.compact}
Section             | Description
--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------
**Dependencies**    | Dependencies required to create comparisons in VSView
**File paths**      | The location of your source file
**Source**          | The source name. This should be the name of the encoder (*not* muxer) or the specific source used for untouched releases (e.g. Beatrice-Raws, JPN BD, DSNP)
**Output**          | Parameter that allows clips to appear in VSView
==-

## Filters

!!!warning
Note the order of the following filters. All 3 *(Required)* options **must** be used, and the filters under each *(Required)* section **must** be placed in that order in your script.
!!!

### Inverse Telecine

Quick inverse telecine filter for converting telecined clips (usually 30 fps interlaced video) back to the original framerate (24 fps progressive).

```py
clip1 = vdecimate(vfm(clip1))
```

### FieldBased

Properly tags progressive content in interlaced container as progressive for correct chroma upscaling.

```py
clip1 = FieldBased.PROGRESSIVE.apply(clip1)
```

## Subsampling [!badge variant="danger" text="(Required)"]

Converts clips to 16-bit depth with 4:4:4 chroma subsampling.

```py
clip1 = EwaLanczosSharp().scale(depth(clip1, 16), format=vs.YUV444P16, antiring=0.6)
```

!!!warning
This is currently broken on Nvidia with more than 2 sources unless you manually upgrade vs-placebo

==- Installation instructions

1. Make sure you're logged into GitHub, then download and extract the fixed [libvs_placebo.dll](https://github.com/sgt0/vs-placebo/actions/runs/24158358466/artifacts/6337091717)

   If you cannot access GitHub, use [this mirror](https://mega.nz/file/eJAxDZxC#cChjkDlCfqAydnG23zV3ZX5toigG8mBpwXL6bPmUSk8) instead

2. Locate your existing `libvs_placebo.dll`.

   Example locations:

   ```text
   C:\Comp\.venv\Lib\site-packages\vapoursynth\plugins\libvs_placebo.dll
   ```
   ```text
   C:\Users\USER\AppData\Local\Programs\Python\Python312\Lib\site-packages\vapoursynth\plugins\libvs_placebo.dll
   ```

3. Replace the existing DLL with the newly downloaded version.

==-
!!!

### Frame Rate

Sets the source frame rate (fps) based on fractional input (`fpsnum`/`fpsden`). For example, `fpsnum=24000` and `fpsden=1001` forces the clip frame rate to 23.976 fps. *This should be used on sources that have different frame rates that don't automatically stay in sync. If they do stay in sync, note that the source has dropped or duplicate frames.*

```py
clip1 = core.std.AssumeFPS(clip1, fpsnum=24000, fpsden=1001)
```

### Trimming

Removes the first *n* frames from the source. For example, `[24:]` will skip the first 24 frames and start the source at frame 25. *This should be used on sources that are out of sync.*
To get the frame difference, find a unique frame (e.g. scene changes) in the correct and incorrect source. Note the frame numbers each one begin at, then set the difference of the two for the incorrect source.

```py
clip1 = clip1[24:]
```

!!!secondary
For more advanced trimming such as chaining, splicing, and looping, see [Vapoursynth's docs](https://www.vapoursynth.com/doc/pythonreference.html#slicing-and-other-syntactic-sugar).
!!!

### Cropping

Crops the source video by *n* pixels from the selected side. For example, `left=20` will remove 20 horizontal pixels starting from the left side. *This should be used on sources that use letterboxing or other form of borders.* Make sure to check for variable aspect ratios throughout the file and only crop the smallest border.

```py
clip1 = core.std.Crop(clip1, left=240, right=240, top=0, bottom=0)
```


### Color Spaces

Sets the correct color information, should be used on sources with incorrect/missing metadata.

```py
clip1 = PropEnum.ensure_presences(clip1, (Matrix.BT709, Transfer.BT709, Primaries.BT709, ColorRange.LIMITED))
```
==- Example adjustments

HD BD/WEB
```py
clip1 = PropEnum.ensure_presences(clip1, (Matrix.BT709, Transfer.BT709, Primaries.BT709, ColorRange.LIMITED))
```

NTSC DVD
```py
clip1 = PropEnum.ensure_presences(clip1, (Matrix.ST170_M, Transfer.BT601, Primaries.ST170_M, ColorRange.LIMITED))
```

PAL DVD
```py
clip1 = PropEnum.ensure_presences(clip1, (Matrix.BT470_BG, Transfer.BT601, Primaries.BT470_BG, ColorRange.LIMITED))
```

HDR
```py
clip1 = PropEnum.ensure_presences(clip1, (Matrix.BT2020_NCL, Transfer.ST2084, Primaries.BT2020, ColorRange.LIMITED))
```

HD BD/WEB with incorrectly tagged matrix (Reds/green will look off)
```py
clip1 = PropEnum.ensure_presences(clip1, (Matrix.ST170_M, Transfer.BT709, Primaries.BT709, ColorRange.LIMITED))
```

HD BD/WEB with incorrectly converted matrix (Inverse of the above, use this if that method makes colors look worse)
```py
clip1 = Point().resample(clip1, matrix=Matrix.ST170_M, format=vs.YUV444P16)
clip1 = PropEnum.ensure_presences(clip1, (Matrix.ST170_M, ColorRange.LIMITED))
```
===

### Double-Range Compression

Reverses Double-Range Compression to fix washed out colors.

```py
clip1 = depth(clip1, range_in=ColorRange.LIMITED, range_out=ColorRange.FULL)
clip1 = ColorRange.LIMITED.apply(clip1)
```
Sets the range as full for incorrectly tagged sources to fix blown out highlights and crushed blacks.
```py
clip1 = ColorRange.FULL.apply(clip1)
```

### Clipping
Sometimes an HDR source will be mostly or even entirely within the SDR range, often seen with anime on Netflix. In these cases you can clip the source to get an exact match to SDR, unlike with traditional tonemapping.
Clipping should always be attempted first before resorting to tonemapping, even if some highlights get blown out. You may also need to apply a 0.92  [gamma adjustment](#gamma) afterwards.

Clip HDR source to SDR
```py
clip1 = Point().resample(clip1, matrix=Matrix.BT709, transfer=Transfer.BT709, primaries=Primaries.BT709, format=vs.YUV444P16)
```
Clip DV (Profile 5) source to SDR
```py
clip1args = PlaceboTonemapOpts(source_colorspace=ColorSpace.DOVI, target_colorspace=ColorSpace.HDR10, use_dovi=True)
clip1 = core.placebo.Tonemap(clip1, **clip1args.vsplacebo_dict())
clip1 = PropEnum.ensure_presences(clip1, (Matrix.BT2020_NCL, Transfer.ST2084, Primaries.BT2020))
clip1 = Point().resample(clip1, matrix=Matrix.BT709, transfer=Transfer.BT709, primaries=Primaries.BT709, format=vs.YUV444P16)
```

### Tonemapping

Converts the dynamic range of the source (i.e. HDR/DV -> SDR).

- For converting HDR (washed out colors) -> SDR, set `ColorSpace.HDR10`
- For converting DV (green/purple hue) -> SDR, set `ColorSpace.DOVI`

```py
clip1args = PlaceboTonemapOpts(source_colorspace=ColorSpace.HDR10, target_colorspace=ColorSpace.SDR, tone_map_function=Tonemap.Spline, gamut_mapping=Gamut.Perceptual, peak_detect=True, use_dovi=True, contrast_recovery=0.3 ,dst_max=100)
clip1 = core.placebo.Tonemap(clip1, **clip1args.vsplacebo_dict())
clip1 = core.std.SetFrameProps(clip1, _Matrix=vs.MATRIX_BT709, _Transfer=vs.TRANSFER_BT709, _Primaries=vs.PRIMARIES_BT709)
```

## Depth [!badge variant="danger" text="(Required)"]
Converts the clip to 32 bit depth for accurate output
```py
clip1 = depth(clip1, 32)
```

### Gamma

Adjusts the gamma level of the video. *Should be used to fix the QuickTime gamma bug (0.88) or similar where one source is brighter than others.*

```py
clip1 = core.std.Levels(clip1, gamma=0.88, planes=0)
```

## Pixel format [!badge variant="danger" text="(Required)"]
Converts to RGBS which is required for debanding and scaling
```py
clip1 = Point().resample(clip1, format=vs.RGBS)
```

### Debanding

Otherwise competitive sources with obvious banding should be debanded to see how they'd fare with [mpv's](/tutorials/mpv/) built-in deband filter. The debanded clip should *never* replace the original. Instead, it should be added as an additional node.

```py
clip1 = core.placebo.Deband(clip1, planes=1|2|4, threshold=48 / 16.384, grain=32 / 8.192)
```

### Scaling

Upscales lower resolution sources to match the highest resolution source. **(Recommended)**
  ```py
  clip1 = EwaLanczosSharp().scale(clip1, 1920, 1080, sigmoid=True, antiring=0.6)
  ```

Downscales higher resolution sources to match a lower resolution source. **Only use this for demonstrative purposes**
  ```py
  clip1 = Hermite().scale(clip1, 1920, 1080, linear=True)
  ```

## Running

+++ uv

To run your comparison script, launch a terminal window in your comp folder and run the following:

```powershell
uv run vsview comp.py
```

Alternatively, you can create a `comp.bat` file, replacing `C:\path\to\comp.py` with the exact file path to your script:

```powershell
uv run vsview "C:\path\to\comp.py"
```
+++ pip

To run your comparison script, launch a terminal window in your comp folder and run the following:

```powershell
vsview comp.py
```

Alternatively, you can create a `comp.bat` file, replacing `C:\path\to\comp.py` with the exact file path to your script:

```powershell
vsview "C:\path\to\comp.py"
```
+++

## First-time Setup

1. Open the settings menu in the top left

2. Under *Plugin - Comparison*, set the following:
   - Set *Collection Name Template* to `{name} ({year}) - S01E01 - {vs_names}`
   - Enter your [slow.pics](https://slow.pics) credentials (optional)

==- Optional QoL Changes

These are some subjective changes which making skimming around for manual frame selecting easier

1. Open the settings menu in the top left

2. Under *View*, set the following:
    - Untick enable zoom animation

3. Under Shortcuts, set the following:
    - Play/Pause: Ctrl+Space
    - Seek Previous Frame: Shift+Left
    - Seek Next Frame: Shift+Right
    - Seek N Frames Back: Left
    - Seek N Frames Forward: Right
    - Add Current Frame: Space

4. Right click the *Play* button in the bottom left. Set seek step to a random odd number (e.g `97`)
==-

Once complete, close and relaunch VSView to apply these changes.

## Comparing

### Basic Keybinds

For the purpose of making comparisons, you will only need the following binds:

Key                        | Action
---------------------------|------------------------------------------------------------------
`Left` | Moves back 1 frame
`Right`  | Moves forward 1 frame
`Shift`+`Left` | Moves back *n* frames (default: *n = 24*)
`Shift`+`Right`  | Moves forward *n* frames (default: *n = 24*)
`Number keys`              | Switches to source *n* (e.g. `2` switches to the second source)
`Ctrl`+`Space`             | Marks the current frame number
`Ctrl`+`P`                 | Opens the plugins window

!!!
If you wish to manually set your keybinds, you can find them under *Settings* -> *Shortcuts*.
!!!

### Capturing

1. Open the *Plugins* window and select the *Comparisons* tab

2. Under *TMDB Name* search for the show/movie and select it, change/remove the episode number if needed

3. Before creating a comparison, skim through all your sources and ensure that they are displaying correctly and stay in sync throughout the entire runtime. Most issues can be fixed using [filters](#scripting) in your comparison script

4. Select the frames to be uploaded. There are multiple methods to doing so:
   - **Manual:** Scrub through the video and mark each frame using `Ctrl`+`Space`
   - **Automatic:** Specify an amount of random frames in the *Plugin* menu and click *Select frames*
   - Frames should ideally show a variety of scenarios (e.g. light/dark, static/high-motion, flat/grainy, etc.). Make sure to include scenes with on-screen text and bright reds when possible
   - An average of 50 frames is recommended, which is the maximum allowed for automatic comparisons. You can still add more manual frames on top of this if needed

5. Hit the *Extract & Upload* button. VSView will automatically screenshot all selected frames and upload a comparison to [slow.pics](https://slow.pics)

## Choosing Sources (Anime)
**The following will help make your comparison as effective as possible**
- Include every available Blu-ray, you generally will need to check U2 (Private tracker)
- If 2 sources have identical video, only include 1 and label it as such (eg JPN/USA BD)
- We generally recommend choosing episode 2 to ensure both the OP and ED are shown
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
**These sources should generally never be included unless the only existing muxes use them**
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
1. Amazon (AMZN)
1. Animation Digital Network (ADN)
1. HIDIVE (HIDI)

==-
