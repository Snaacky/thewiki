---
label: Comparison
description: Learn how to compare various sources of a video
image: /static/comparison/hyouka.gif
---

# Comparison

Quality comparisons are frequently used within the enthusiast community to compare the video quality offered by different sources/releases. [It serves as a great way to distinguish the differences between good and bad sources](/guides/quality/#types-of-releases), and can help you determine which one to download.

This guide goes through the process of setting up and effectively utilizing [VSPreview](https://github.com/Jaded-Encoding-Thaumaturgy/vs-preview), a previewer utility for [VapourSynth](https://github.com/vapoursynth/vapoursynth), to produce useful quality comparisons that will allow you to ascertain which release offers the best visual experience.

## Setup

### VapourSynth

VapourSynth is an open-source video processing framework. It handles all of your sources, playback, and filtering options in conjunction with VSPreview.

#### Dependencies

- [Python 3.12](https://www.python.org/downloads/) - *Select `Add python.exe to PATH` during installation*
- [Git](https://gitforwindows.org/)

#### Installation

Download and install `VapourSynth-x64-RXX.exe` from [VapourSynth](https://github.com/vapoursynth/vapoursynth/releases). *During installation, select `Install for me only`.*

![Vapoursynth release](/static/comparison/vs-download.png)

### VSPreview

VSPreview is a previewer application for scripts created in VapourSynth. It features a simple graphical interface to allow you to use VapourSynth's features (and create comparisons) with ease.

#### Dependencies

+++ General Setup

In order to create comparisons with VSPreview, you will need to install its necessary dependencies.

- [`LibP2P`](https://github.com/DJATOM/LibP2P-Vapoursynth), [`LSMASHSource`](https://github.com/AkarinVS/L-SMASH-Works), [`Subtext`](https://github.com/vapoursynth/subtext), and [`vs-placebo`](https://github.com/Lypheo/vs-placebo) can be installed using `vsrepo` from [VapourSynth](https://github.com/vapoursynth/vapoursynth/releases). In your terminal, run the following:

  ```powershell
  vsrepo.py install libp2p lsmas sub placebo
  ```

  !!!
  If `vsrepo.py` command doesn't work, make sure Windows is set to open `.py` files with Python. *[You may also need to add it to the `PATHEXT` environment variable.](/static/comparison/python-pathext.png)*
  !!!

- [`awsmfunc`](https://github.com/OpusGang/awsmfunc) can be installed using `pip`:

  ```powershell
  python -m pip install git+https://github.com/OpusGang/awsmfunc.git
  ```

+++ Dolby Vision

If you're working with Dolby Vision (DV) content, you will need to install additional dependencies.

- [`libdovi`](https://github.com/quietvoid/dovi_tool) can be installed using `vsrepo` from [VapourSynth](https://github.com/vapoursynth/vapoursynth/releases). In your terminal, run the following:

  ```powershell
  vsrepo.py install dovi_library
  ```

- You will need to install [`vA.5b`](https://github.com/AkarinVS/L-SMASH-Works/releases/tag/vA.5b) of `LSMASHSource`. By default, `vsrepo` installs `vA.3x` which does not support DV. `vA.5x` is an experimental version which is not available on `vsrepo` and requires manual installation:
  - Download and extract the correct version for your operating system. *For most users, this will be `release-x86_64-cachedir-cwd.zip`*
  - Copy `libvslsmashsource.dll` and paste it in `%appdata%\VapourSynth\plugins64\`
    - If you have an existing `libvslsmashsource.dll` in the `plugins64`, replace it with the newer `libvslsmashsource.dll`

+++

#### Installation

Download and install [`vs-jet`](https://github.com/Jaded-Encoding-Thaumaturgy/vs-jet) using `pip` in your terminal:

```powershell
python -m pip install vsjet
```

```powershell
python -m vsjet latest
```

!!!warning
Currently, installing [`vs-preview`](https://github.com/Jaded-Encoding-Thaumaturgy/vs-preview) directly from PyPI or git is broken. Instead, you have to install it using [`vs-jet`](https://github.com/Jaded-Encoding-Thaumaturgy/vs-jet), which will install all necessary JET packages.
!!!

## Usage

### Scripting

In order to create a comparison, you will need to create a VapourSynth script. This script outlines the parameters and files which VSPreview will use when generating your comparison.

Create a file called `comp.py`. Launch it in your favorite text editor and add sections as desired:

==- :icon-file-code: Initial script [!badge variant="danger" text="Required"]

The basic `comp.py` script to get started. This script includes the required dependencies and features to run and get the best experience out of VSPreview for creating your comparisons.

!!!
Make sure to comment (add `##` to the beginning of the line) and uncomment lines as needed.
!!!

```py
## Dependencies: Allows vspreview to run [required; do not remove]
import vstools
import vapoursynth as vs
from vapoursynth import core
from awsmfunc import FrameInfo
from vskernels import Hermite, EwaLanczos
from vspreview import set_output
from awsmfunc.types.placebo import PlaceboColorSpace as csp
from awsmfunc.types.placebo import PlaceboTonemapMode as TMmode
from awsmfunc.types.placebo import PlaceboTonemapFunction as TMfunc
from awsmfunc.types.placebo import PlaceboGamutMode as GMTmode
from awsmfunc.types.placebo import PlaceboTonemapOpts as TMopts

## <Additional dependencies>
## Place any additional dependencies you want to use in your comp here
## <End of additional dependencies>

## File paths: Hold shift and right-click your file, select copy as path, and paste it here
clip1 = core.lsmas.LWLibavSource(r"C:\Paste\File\Path\Here.mkv")
clip2 = core.lsmas.LWLibavSource(r"C:\Paste\File\Path\Here.mkv")
clip3 = core.lsmas.LWLibavSource(r"C:\Paste\File\Path\Here.mkv")

## Source: Name of the source
source1 = "FirstSourceName"
source2 = "SecondSourceName"
source3 = "ThirdSourceName"

## <Additional comp settings>
## Place any additional settings you want to use in your comp here
## <End of additional comp settings>

## Frameinfo: Displays the frame number, type, and group name in the top left corner (no modification required; add/remove lines as needed)
clip1 = FrameInfo(clip1, source1)
clip2 = FrameInfo(clip2, source2)
clip3 = FrameInfo(clip3, source3)

## FrameProp: Slowpoke Pics/file-name labeling (no modification required; add/remove lines as needed)
clip1 = clip1.std.SetFrameProp('Name', data = source1)
clip2 = clip2.std.SetFrameProp('Name', data = source2)
clip3 = clip3.std.SetFrameProp('Name', data = source3)

## Output: Comment/uncomment as needed depending on how many clips you're comparing
set_output(clip1, name=source1)
set_output(clip2, name=source2)
set_output(clip3, name=source3)
```

{.compact}
Section             | Description
--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
**Dependencies**    | Dependencies required to create comparisons in VSPreview
**File paths**      | The location of your source file
**Source**          | The name of each source. [We recommend following the naming scheme here.](#recommended-source-naming) *If you plan to use [Slowpoke Pics](#slowpoke-pics), this will be the name that will be displayed in comparisons*
**FrameInfo**       | Lists the frame number, type, and source name in the top left of the videos
**FrameProp**       | Sets the source name entered under **Source** for correct labeling on [Slowpoke Pics](#slowpoke-pics)
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
If you are cropping with odd numbers, you will need to change the color depth to 16-bit (see [Color & contrast](#color-contrast-depth-tonemapping-range-gamma-matrix-drc) -> *Depth*).
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
  ## Upscaling: Increases the resolution of clips to match the highest resolution using EwaLanczos (equivalent scaling to mpv's high-quality profile); recommended
  clip1 = EwaLanczos.scale(clip1, 1920, 1080, sigmoid=True)
  clip2 = EwaLanczos.scale(clip2, 1920, 1080, sigmoid=True)
  clip3 = EwaLanczos.scale(clip3, 3840, 2160, sigmoid=True)
  ```

- For downscaling (e.g. 2160p/4K -> 1080p), use `Hermite`:

  ```py
  ## Downscaling: Decreases the resolution of clips to match the lowest resolution using Hermite (equivalent scaling to mpv's high-quality profile); not recommended
  clip1 = Hermite.scale(clip1, 1920, 1080, linear=True)
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
clip3 = clip3[0:]
```

!!!secondary
For more advanced trimming such as chaining, splicing, and looping, see [Vapoursynth's docs](https://www.vapoursynth.com/doc/pythonreference.html#slicing-and-other-syntactic-sugar).
!!!

==- :icon-paintbrush: Color & contrast (depth, tonemapping, range, gamma, matrix, DRC)

#### Depth

Converts clips to 16-bit depth with 4:4:4 chroma subsampling. *Required for filters such as cropping (with odd numbers) or tonemapping.*

```py
## Depth: Convert clips to 16-bit 4:4:4 [required for cropping with odd numbers or tonemapping]
clip1 = core.resize.Bicubic(clip1, format=vs.YUV444P16)
clip2 = core.resize.Bicubic(clip2, format=vs.YUV444P16)
clip3 = core.resize.Bicubic(clip3, format=vs.YUV444P16)
```

#### Tonemapping

Converts the dynamic range of the source (i.e. HDR/DV -> SDR).

- For converting HDR (washed out colors) -> SDR, set `source_colorspace=csp.HDR10`
- For converting DV (green/purple hue) -> SDR, set `source_colorspace=csp.DOVI`

!!!warning
If you want to use tonemapping, you will need to change the color depth to 16-bit (see [above](#depth)).
!!!

```py
## Tonemapping: Converts the dynamic range of the source [16-bit required]
## Specify the arguments based on your sources; play with different values when comparing against an SDR source to best match it
clip1args = TMopts(source_colorspace=csp.DOVI, target_colorspace=csp.SDR, tone_map_mode=TMmode.RGB, tone_map_function=TMfunc.ST2094_40, gamut_mode=GMTmode.Clip, peak_detect=True, use_dovi=True)
clip2args = TMopts(source_colorspace=csp.HDR10, target_colorspace=csp.SDR, tone_map_mode=TMmode.RGB, tone_map_function=TMfunc.ST2094_40, gamut_mode=GMTmode.Clip, peak_detect=True, use_dovi=True)
clip3args = TMopts(source_colorspace=csp.HDR10, target_colorspace=csp.SDR, tone_map_mode=TMmode.Hybrid, tone_map_function=TMfunc.Spline, gamut_mode=GMTmode.Darken, peak_detect=True, use_dovi=True, dst_max=120)
## Apply tonemapping
clip1 = core.placebo.Tonemap(clip1, **clip1args.vsplacebo_dict())
clip2 = core.placebo.Tonemap(clip2, **clip2args.vsplacebo_dict())
clip3 = core.placebo.Tonemap(clip3, **clip3args.vsplacebo_dict())
## Retag video to 709 after tonemapping [required]
clip1 = core.std.SetFrameProps(clip1, _Matrix=1, _Transfer=1, _Primaries=1)
clip2 = core.std.SetFrameProps(clip2, _Matrix=1, _Transfer=1, _Primaries=1)
clip3 = core.std.SetFrameProps(clip3, _Matrix=1, _Transfer=1, _Primaries=1)
```

!!!
`dst_max` forces a set brightness, which can be used to make HDR/DV clips appear brighter for easier comparison to SDR.
!!!

#### Range

Sets the color range of the clip as limited (`0`) or full (`1`). *This should be used on sources containing incorrect metadata or after tonemapping DV content (set it to limited).*

```py
## Color range: Marks the clip's range as limited (0) or full (1); DV clips will need to be set to limited (0) after tonemapping
clip1 = core.resize.Bicubic(clip1, format=vs.YUV444P16, range=0)
clip2 = core.resize.Bicubic(clip2, format=vs.YUV444P16, range=0)
clip3 = core.resize.Bicubic(clip3, format=vs.YUV444P16, range=1)
```

#### Gamma

Adjusts the gamma level of the video. *This should only be used to fix the QuickTime gamma bug or similar where one source will appear much brighter than the rest.*

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

#### Matrix

Sets the color gamut to fix the colors of your sources. This is most commonly used on sources you're upscaling or 4K SDR content. *This should be used on sources with incorrect/missing metadata or colors that are off, particularly reds and greens.*

Generally:

{.compact}
Type                    | Gamut       | Parameter
------------------------|-------------|-------------
SDR: BD/WEB (720p - 4K) | BT.709      | `intval=1`
SDR: DVD                | BT.601      | `intval=5`
HDR/DV                  | BT.2020 NCL | `intval=9`

```py
## Matrix: Repairs sources with incorrect/missing metadata; typically used for 4K SDR and upscaled/downscaled content (colors will be off, particularly reds, greens, and blues)
clip1 = core.std.SetFrameProp(clip1, prop="_Matrix", intval=1)
clip2 = core.std.SetFrameProp(clip2, prop="_Matrix", intval=6)
clip3 = core.std.SetFrameProp(clip3, prop="_Matrix", intval=5)
```

If you are unable to correct the source's colors with the initial matrix command, the source is likely flawed rather than an issue with the metadata. If this is the case, you should use the filters below:

```py
## Correct matrix: If the colors cannot be corrected with the initial matrix command
clip1 = core.resize.Point(clip1, matrix=5)
clip1 = core.std.SetFrameProp(clip1, prop="_Matrix", intval=1)
clip2 = core.resize.Point(clip2, matrix=6)
clip2 = core.std.SetFrameProp(clip2, prop="_Matrix", intval=1)
clip3 = core.resize.Point(clip3, matrix=4)
clip3 = core.std.SetFrameProp(clip3, prop="_Matrix", intval=1)
```

#### Double-Range Compression (DRC)

Fixes washed out colors on selected sources.

```py
## Fix DRC: Repairs sources with very washed out colors
clip1 = core.resize.Point(clip1, range_in=0, range=1, dither_type="error_diffusion")
clip1 = core.std.SetFrameProp(clip1, prop="_ColorRange", intval=1)
```

==-

==- :icon-file-code: Full/example script

The complete `comp.py` script. This script includes the [initial script](#initial-script-badge-variant-danger-text-required) and all of the additional filters mentioned above.

!!!warning
This script serves as an example for you to reference. We recommend creating your own script and copying only the things you need from the above sections for your comparisons.
!!!

```py
## Dependencies: Allows vspreview to run [required; do not remove]
import vstools
import vapoursynth as vs
from vapoursynth import core
from awsmfunc import FrameInfo
from vskernels import Hermite, EwaLanczos
from vspreview import set_output
from awsmfunc.types.placebo import PlaceboColorSpace as csp
from awsmfunc.types.placebo import PlaceboTonemapMode as TMmode
from awsmfunc.types.placebo import PlaceboTonemapFunction as TMfunc
from awsmfunc.types.placebo import PlaceboGamutMode as GMTmode
from awsmfunc.types.placebo import PlaceboTonemapOpts as TMopts

## <Additional dependencies>
## Place any additional dependencies you want to use in your comp here
## <End of additional dependencies>

## File paths: Hold shift and right-click your file, select copy as path, and paste it here
clip1 = core.lsmas.LWLibavSource(r"C:\Paste\File\Path\Here.mkv")
clip2 = core.lsmas.LWLibavSource(r"C:\Paste\File\Path\Here.mkv")
clip3 = core.lsmas.LWLibavSource(r"C:\Paste\File\Path\Here.mkv")

## Source: Name of the source
source1 = "FirstSourceName"
source2 = "SecondSourceName"
source3 = "ThirdSourceName"

## <Additional comp settings>

## Frame rate: Change fps to match other sources (needed for when the previewer is unable to automatically keep them in sync)
##clip1 = core.std.AssumeFPS(clip1, fpsnum=24000, fpsden=1001)
##clip2 = core.std.AssumeFPS(clip2, fpsnum=25000, fpsden=1000)
##clip3 = core.std.AssumeFPS(clip3, fpsnum=24000, fpsden=1000)

## FieldBased: Tags the content as progressive (0); used for progressive content tagged as interlaced
##clip1 = core.std.SetFieldBased(clip1, 0)
##clip2 = core.std.SetFieldBased(clip2, 0)
##clip3 = core.std.SetFieldBased(clip3, 0)

## Inverse telecine: Fixes telecined video
##clip1 = core.vivtc.VFM(clip1, 1)
##clip1 = core.vivtc.VDecimate(clip1)

## Cropping: Removes letterboxing (black bars) [16-bit required for odd numbers]
##clip1 = core.std.Crop(clip1, left=240, right=240, top=0, bottom=0)
##clip2 = core.std.Crop(clip2, left=0, right=0, top=276, bottom=276)
##clip3 = core.std.Crop(clip3, left=0, right=0, top=21, bottom=21)

## Upscaling: Increases the resolution of clips to match the highest resolution using EwaLanczos (equivalent scaling to mpv's high-quality profile); recommended
##clip1 = EwaLanczos.scale(clip1, 1920, 1080, sigmoid=True)
##clip2 = EwaLanczos.scale(clip2, 1920, 1080, sigmoid=True)
##clip3 = EwaLanczos.scale(clip3, 3840, 2160, sigmoid=True)

## Downscaling: Decreases the resolution of clips to match the lowest resolution using Hermite (equivalent scaling to mpv's high-quality profile); not recommended
##clip1 = Hermite.scale(clip1, 1920, 1080, linear=True)
##clip2 = Hermite.scale(clip2, 1920, 1080, linear=True)
##clip3 = Hermite.scale(clip3, 3840, 2160, linear=True)

## Trimming: Trim frames to match clips (calculate the frame difference and enter the number here)
##clip1 = clip1[0:]
##clip2 = clip2[24:]
##clip3 = clip3[0:]

## Depth: Convert clips to 16-bit 4:4:4 [required for cropping with odd numbers or tonemapping]
##clip1 = core.resize.Bicubic(clip1, format=vs.YUV444P16)
##clip2 = core.resize.Bicubic(clip2, format=vs.YUV444P16)
##clip3 = core.resize.Bicubic(clip3, format=vs.YUV444P16)

## Tonemapping: Converts the dynamic range of the source [16-bit required]
## Specify the arguments based on your sources; play with different values when comparing against an SDR source to best match it
##clip1args = TMopts(source_colorspace=csp.DOVI, target_colorspace=csp.SDR, tone_map_mode=TMmode.RGB, tone_map_function=TMfunc.ST2094_40, gamut_mode=GMTmode.Clip, peak_detect=True, use_dovi=True)
##clip2args = TMopts(source_colorspace=csp.HDR10, target_colorspace=csp.SDR, tone_map_mode=TMmode.RGB, tone_map_function=TMfunc.ST2094_40, gamut_mode=GMTmode.Clip, peak_detect=True, use_dovi=True)
##clip3args = TMopts(source_colorspace=csp.HDR10, target_colorspace=csp.SDR, tone_map_mode=TMmode.Hybrid, tone_map_function=TMfunc.Spline, gamut_mode=GMTmode.Darken, peak_detect=True, use_dovi=True, dst_max=120)
## Apply tonemapping
##clip1 = core.placebo.Tonemap(clip1, **clip1args.vsplacebo_dict())
##clip2 = core.placebo.Tonemap(clip2, **clip2args.vsplacebo_dict())
##clip3 = core.placebo.Tonemap(clip3, **clip3args.vsplacebo_dict())
## Retag video to 709 after tonemapping [required]
##clip1 = core.std.SetFrameProps(clip1, _Matrix=1, _Transfer=1, _Primaries=1)
##clip2 = core.std.SetFrameProps(clip2, _Matrix=1, _Transfer=1, _Primaries=1)
##clip3 = core.std.SetFrameProps(clip3, _Matrix=1, _Transfer=1, _Primaries=1)

## Color range: Marks the clip's range as limited (0) or full (1); DV clips will need to be set to limited (0) after tonemapping
##clip1 = core.resize.Bicubic(clip1, format=vs.YUV444P16, range=0)
##clip2 = core.resize.Bicubic(clip2, format=vs.YUV444P16, range=0)
##clip3 = core.resize.Bicubic(clip3, format=vs.YUV444P16, range=1)

## Gamma: Fixes gamma bug (i.e. one source is significantly brighter than the others) [32-bit required]
## Convert clips to 32-bit [required for gamma fix]
##clip1 = vstools.depth(clip1, 32)
##clip2 = vstools.depth(clip2, 32)
##clip3 = vstools.depth(clip3, 32)
## Apply fix
##clip1 = core.std.Levels(clip1, gamma=0.88, planes=0)
##clip2 = core.std.Levels(clip2, gamma=0.88, planes=0)
##clip3 = core.std.Levels(clip3, gamma=0.88, planes=0)

## Matrix: Repairs sources with incorrect/missing metadata; typically used for 4K SDR and upscaled/downscaled content (colors will be off, particularly reds, greens, and blues)
##clip1 = core.std.SetFrameProp(clip1, prop="_Matrix", intval=1)
##clip2 = core.std.SetFrameProp(clip2, prop="_Matrix", intval=6)
##clip3 = core.std.SetFrameProp(clip3, prop="_Matrix", intval=5)

## Correct matrix: If the colors cannot be corrected with the initial matrix command
##clip1 = core.resize.Point(clip1, matrix=5)
##clip1 = core.std.SetFrameProp(clip1, prop="_Matrix", intval=1)
##clip2 = core.resize.Point(clip2, matrix=6)
##clip2 = core.std.SetFrameProp(clip2, prop="_Matrix", intval=1)
##clip3 = core.resize.Point(clip3, matrix=4)
##clip3 = core.std.SetFrameProp(clip3, prop="_Matrix", intval=1)

## Fix DRC: Repairs sources with very washed out colors
##clip1 = core.resize.Point(clip1, range_in=0, range=1, dither_type="error_diffusion")
##clip1 = core.std.SetFrameProp(clip1, prop="_ColorRange", intval=1)

## <End of additional comp settings>

## Frameinfo: Displays the frame number, type, and group name in the top left corner (no modification required; add/remove lines as needed)
clip1 = FrameInfo(clip1, source1)
clip2 = FrameInfo(clip2, source2)
clip3 = FrameInfo(clip3, source3)

## FrameProp: Slowpoke Pics/file-name labeling (no modification required; add/remove lines as needed)
clip1 = clip1.std.SetFrameProp('Name', data = source1)
clip2 = clip2.std.SetFrameProp('Name', data = source2)
clip3 = clip3.std.SetFrameProp('Name', data = source3)

## Output: Comment/uncomment as needed depending on how many clips you're comparing
set_output(clip1, name=source1)
set_output(clip2, name=source2)
set_output(clip3, name=source3)
```

==-

### Running

To run your comparison script, launch a terminal window in your working directory and run the following:

```powershell
vspreview comp.py
```

Alternatively, you can create a create a `comp.bat` file, replacing `C:\path\to\comp.py` with the exact file path to your script:

```powershell
vspreview "C:\path\to\comp.py"
```

!!!
Want to jump straight to making comparisons? *See [Comparing](#comparing).*
!!!

## Interface

==- :icon-table: Basic Overview

VSPreview's user-friendly interface allows you to perform many actions while being able to compare sources with ease:

![VSPreview interface](/static/comparison/vsp-interface.png)

Key                                                        | Section           | Description
-----------------------------------------------------------|-------------------|---------------------------------------------------------------------------
![#ed8796](https://placehold.co/14x14/ed8796/ed8796.png) 1 | Preview           | The video preview window for viewing frames in your sources
![#f5a97f](https://placehold.co/14x14/f5a97f/f5a97f.png) 2 | Timeline          | The video timeline, source/clip selection, settings, and section toggles
![#eed49f](https://placehold.co/14x14/eed49f/eed49f.png) 3 | Playback          | Buttons for controlling playback and scrolling through the timeline
![#a6da95](https://placehold.co/14x14/a6da95/a6da95.png) 4 | Scening           | Tools for creating and modifying scene markers
![#91d7e3](https://placehold.co/14x14/91d7e3/91d7e3.png) 5 | Pipette           | The color information for the current frame
![#8aadf4](https://placehold.co/14x14/8aadf4/8aadf4.png) 6 | Benchmark         | The benchmark tool for testing device performance
![#c6a0f6](https://placehold.co/14x14/c6a0f6/c6a0f6.png) 7 | [Misc](#misc)     | Miscellaneous settings for VSPreview
![#f5bde6](https://placehold.co/14x14/f5bde6/f5bde6.png) 8 | [Comp](#comp)     | The comparison creator
![#f4dbd6](https://placehold.co/14x14/f4dbd6/f4dbd6.png) 9 | [Status](#status) | Various information about the current source

==-

### Misc

The misc bar contains additional settings for VSPreview.

![VSPreview misc bar](/static/comparison/vsp-misc.png)

==- 1 - Cropper

A simple VSPreview cropping tool. To enable the cropper, set the toggle from *OFF* to *ON*.

#### Relative cropping

Relative cropping removes lines of pixels from the side you select. For example, if you set *Left* to `500` in a `1920x1080` source, VSPreview will remove 500 vertical lines starting from the *left-hand side* of the video stream, creating a cropped resolution of `1420x1080`.

#### Absolute cropping

Absolute cropping removes lines of pixels starting from the right side (horizontal) or the top/bottom side (vertical). For example, if you set *Width* to `1420` in a `1920x1080` source, VSPreview will remove 500 vertical lines starting from the *right-hand side* of the video stream, creating a cropped resolution of `1420x1080`.

==- 2 - Frame saving

Functions for saving frames.

#### File name

By default, VSPreview uses the format `{script_name}_{frame}` when saving frames.

!!!
We recommend changing the file naming scheme to `{frame}_{index}_{Name}`, which is more friendly.
!!!

!!!warning
The `{Name}` placeholder may include extraneous data in newer versions of VSPreview (e.g. `b'sourceName'` instead of `sourceName`).
!!!

Below is a table of placeholders VSPreview uses:

Placeholder      | Description                                      | Example
-----------------|--------------------------------------------------|---------------------------------
`{script_name}`  | The name of your script file                     | `comp`
`{index}`        | The number of the source, from `0` to `n - 1`    | `4`
`{Name}`         | The name of the current source                   | `OZR (1080p, H.265, 3.10 GiB)`
`{total_frames}` | The total number of frames in the current source | `34072`
`{frame}`        | The current frame number                         | `20798`
`{fps_num}`      | The frame rate numerator                         | `24000`
`{fps_den}`      | The frame rate denominator                       | `1001`
`{width}`        | The width of the current frame in pixels         | `1920`
`{height}`       | The height of the current frame in pixels        | `1080`
`{format}`       | The video format                                 | `YUV420P10`

==-

### Comp

The comp bar is primarily used for creating [automatic](#automatic-badge-variant-secondary-text-fastest-option)/[semi-automatic](#semi-automatic-badge-icon-variant-primary-text-recommended) comparisons to [Slowpoke Pics](https://slow.pics):

![VSPreview comp bar](/static/comparison/vsp-comp.png)

==- 1 - Comparison title

The title of your comparison.

#### Naming scheme

When creating your comparison, we recommend naming it with the show title and sources used. This makes it easier for the user to understand what you are comparing. Some examples are:

- By video source: `The Eminence in Shadow - BD vs. WEB`
- By release type: `Watashi no Oshi wa Akuyaku Reijou. - Minis vs. Streams`
- By release group: `Miss Kobayashi's Dragon Maid - Okay-Subs vs. Beatrice-Raws`

==- 2 - Framing parameters

#### Random

The *Random* parameter sets the number of frames to randomly capture during screenshotting. *This is a setting used when creating [automatic](#automatic-badge-variant-secondary-text-fastest-option)/[semi-automatic](#semi-automatic-badge-icon-variant-primary-text-recommended) comparisons.*

For example, a value of `48` means VSPreview will take 48 randomly selected frames from each source when generating the comparison.

==- 3 - Picture type

Sets the frame type to capture. *This is a setting used when creating [automatic](#automatic-badge-variant-secondary-text-fastest-option)/[semi-automatic](#semi-automatic-badge-icon-variant-primary-text-recommended) comparisons.*

Only checked frames are captured. For instance, if `I` is unchecked, then VSPreview will only capture `P` or `B` frames.

==- 4 - TheMovieDB info

The TMDB ID found at [TheMovieDB](https://www.themoviedb.org).

- For TV shows, set the box to `TV`
- For movies, set the box to `Movie`

==- 5 - Collection tags

Tags for Slowpoke Pics collections.

==- 6 - Slowpoke Pics parameters

Parameter      | Description
---------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
`Public`       | Sets whether the comparison link is publicly visible on the front page and search on [Slowpoke Pics](https://slow.pics)
`NSFW`         | Sets whether the comparison displays an NSFW warning before showing images
`Delete After` | Sets the number of days for when the comparison will expire and be deleted from [Slowpoke Pics](https://slow.pics). *By default, comparisons are deleted after two years of inactivity*

==-

### Status

The status bar displays several information about the current source:

![VSPreview status bar](/static/comparison/vsp-status.png)

1 - Total number of frames
:   The total number of frames in the video stream.

2 - Video stream length
:   The playback length of the video stream.

3 - Video stream resolution
:   The content resolution of the video stream.

4 - Video format
:   The chroma subsampling type and bit depth.

5 - Source frame rate
:   The frame rate of the video stream, represented by a fraction and approximate decimal.

6 - Picture type of current frame
:   The picture type of the current frame.

## Comparing

### Tips

- *[Label your sources clearly to the user](#recommended-source-naming)*
- Try to capture a large variety of scenes (e.g. low/high detail, bright/dark, low/high motion)
- Try to capture frames of the same type
  - We recommend taking `P` or `B` type frames when possible
  - This may be harder to accomplish with particular sources, such as streams or WEB-DLs
  - *[See how to set a specific frame type](#3-picture-type)*

### Basic Keybinds

Key                | Action
-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
`Left arrow` (<-)  | Move back *n* frames based on [config](#change-frame-increment) (default: *n = 1*)
`Right arrow` (->) | Move forward *n* frames based on [config](#change-frame-increment) (default: *n = 1*)
Number keys        | Switches to source *n* (e.g. `2` switches to `clip2`)
`Shift` + `S`      | Take and save screenshot of the current frame
`Ctrl` + `Space`   | Mark current frame number for [semi-automatic](#semi-automatic-badge-icon-variant-primary-text-recommended) comparisons

### Process

VSPreview offers three methods for creating comparisons:

==- :icon-cpu: Automatic [!badge variant="secondary" text="Fastest Option"]

Automatic comparisons are created completely without any additional user input. VSPreview will automatically select, capture, and upload frames for you. *This is the fastest method for creating comparisons.*

#### Capturing

1. In VSPreview, navigate to the bottom bar and toggle the *Comp* section

2. Set your parameters. The following parameters should be filled:
   ![VSPreview comp bar](/static/comparison/vsp-comp.png)

   Key | Description
   ----|-----------------------------------------------------------------------------------------------------
   1   | The title of your comparison/show
   2   | The random frame interval to capture. *This should be set to a value higher or equal to 40 frames*
   3   | The picture type
   4   | The [TMDB ID](https://www.themoviedb.org) for the show

   - *See [Comp](#comp) for more detail on what each one is used for*

3. Hit the *Start Upload* button under *Comp* to begin creating your comparison

==- :icon-gear: Semi-automatic [!badge icon=":heart:" variant="primary" text="Recommended"]

Semi-automatic comparisons are created with minor user input. VSPreview will automatically capture and upload frame manually marked by the user. *This is the recommended method for creating comparisons.*

#### Setup

1. Locate the frame(s) you want to compare
   - By default, `Left arrow` and `Right arrow` navigates between frames
   - *See how to [skip frames faster](#change-frame-increment)*
   - *See [tips](#tips) on what you should look for*
  
2. Once you find a frame, mark the current frame
   - Default keybind: `Ctrl` + `Space`

#### Capturing

1. In VSPreview, navigate to the bottom bar and toggle the *Comp* section

2. Set your parameters. The following parameters should be filled:
   ![VSPreview comp bar](/static/comparison/vsp-comp.png)

   Key | Description
   ----|---------------------------------------------------------
   1   | The title of your comparison/show
   4   | The [TMDB ID](https://www.themoviedb.org) for the show

   - *See [Comp](#comp) for more detail on what each one is used for*

3. Hit the *Start Upload* button under *Comp* to begin creating your comparison

==- :icon-tools: Manual

Manual comparisons are created completely by the user. VSPreview displays and handles frame capture, while the main actions are performed by the user through the previewer.

#### Capturing

1. Locate the frame(s) you want to compare
   - By default, `Left arrow` and `Right arrow` navigates between frames
   - *See how to [skip frames faster](#change-frame-increment)*
   - *See [tips](#tips) on what you should look for*

2. Once you find a frame, take a screenshot of the current frame
   - Default keybind: `Shift` + `S`

!!!
If you want to use automatic [Slowpoke Pics](#slowpoke-pics) sorting, make sure your file naming scheme is set to [`{frame}_{index}_{Name}`](#2-frame-saving).
!!!

3. Switch to the other sources and take screenshots of their current frame
   - Press the number keys to change sources (e.g. `1` for `clip1`, `2` for `clip2`)
   - *You may need to finely readjust the position using the arrow keys*

4. Repeat process for the next frames in your comparison

!!!
By default, all frames are stored within your working directory unless manually changed to a different destination.
!!!

!!!secondary
*See [Post-processing](#post-processing) for additional scripts to apply after creating your comparison.*
!!!

==-

## QoL Changes

### VSPreview

==- :icon-project-roadmap: Recommended source naming

You can name your sources in any way you'd like. However, we recommend naming your sources in a way that makes it easy to understand:

![Example source names](/static/comparison/example-sources.png)

Key                                                        | Meaning
-----------------------------------------------------------|---------------------------------------------
![#ed8796](https://placehold.co/14x14/ed8796/ed8796.png) 1 | The name of the source/release group
![#f5a97f](https://placehold.co/14x14/f5a97f/f5a97f.png) 2 | The video resolution
![#eed49f](https://placehold.co/14x14/eed49f/eed49f.png) 3 | The video codec
![#a6da95](https://placehold.co/14x14/a6da95/a6da95.png) 4 | The file size
![#91d7e3](https://placehold.co/14x14/91d7e3/91d7e3.png) 5 | Additional tags, such as scaling or HDR/DV

How you should name your sources will depend on the content you're comparing. For example, if you are trying to compare HDR and SDR sources, you should include the type in the source name. *Generally, the first three will cover most comparisons you make, but you are free to include more as needed.*

==- :icon-versions: Change frame increment

- In VSPreview, navigate to the bottom bar and toggle the *Playback* section
- In *Playback*, set the number of frames to skip (*n*) when scrubbing by adjusting the first field:

![Adjust frames in the playback bar](/static/comparison/vsp-playback-frames.png)

- To skip *n* frames backward/forward, press `Shift` + `Left arrow`/`Right arrow`

==- :icon-file-media: Changing the screenshot key

The following guide changes the screenshot key from `Shift` + `S` to `Enter`:

- Launch *File Explorer* and go to `%localappdata%\Programs\Python\Python312\Lib\site-packages\vspreview\toolbars\misc`
- In a text editor, open `toolbar.py`
- Locate the lines below (approximately line 158):
  - *Use the Find command to locate text (`Ctrl` + `F`)*

  ```py
        self.main.add_shortcut(
            QKeyCombination(Qt.Modifier.SHIFT, Qt.Key.Key_S).toCombined(), self.save_frame_as_button.click
        )
  ```

- Replace the lines with the following:

  ```py
        self.main.add_shortcut(
            (Qt.Key.Key_Return), self.save_frame_as_button.click
        )
  ```

==- :icon-copy: Swap binds for seeking frames

The following guide switches the binds for `Left arrow`/`Right arrow` and `Shift` + `Left arrow`/`Shift` + `Right arrow`:

- Launch *File Explorer* and go to `%localappdata%\Programs\Python\Python312\Lib\site-packages\vspreview\toolbars\playback`
- In a text editor, open `toolbar.py`
- Locate the lines below (approximately line 180):
  - *Use the Find command to locate text (`Ctrl` + `F`)*

  ```py
        self.main.add_shortcut(Qt.Key.Key_Left, self.seek_to_prev_button.click)
        self.main.add_shortcut(Qt.Key.Key_Right, self.seek_to_next_button.click)
        self.main.add_shortcut(
            QKeyCombination(Qt.Modifier.SHIFT, Qt.Key.Key_Left).toCombined(), self.seek_n_frames_b_button.click
        )
        self.main.add_shortcut(
            QKeyCombination(Qt.Modifier.SHIFT, Qt.Key.Key_Right).toCombined(), self.seek_n_frames_f_button.click
        )
  ```

- Replace the lines with the following:

  ```py
        self.main.add_shortcut(
            QKeyCombination(Qt.Modifier.SHIFT, Qt.Key.Key_Left), self.seek_to_prev_button.click
        )
        self.main.add_shortcut(
            QKeyCombination(Qt.Modifier.SHIFT, Qt.Key.Key_Right), self.seek_to_next_button.click
        )
        self.main.add_shortcut(Qt.Key.Key_Left, self.seek_n_frames_b_button.click)
        self.main.add_shortcut(Qt.Key.Key_Right, self.seek_n_frames_f_button.click)
  ```

==-

### Slowpoke Pics

#### Fetching Account Tokens

If you plan on uploading to [Slowpoke Pics](https://slow.pics) (slow.pics) under your account, you will need to provide VSPreview with your account tokens.

+++ Chrome

- Visit [Slowpoke Pics](https://slow.pics) in your browser and log into your account
- Open your browser's **Developer Tools**. You will need to get two values:
  - To get your `browserId`, go to **Application** -> **Storage** -> **Local Storage** -> `https://slow.pics`. Copy the key listed there
  - To get your `sessionId`, go to **Network**. Refresh the page, then find `slow.pics`. In the right-hand section, go to **Cookies** and copy the **Value** listed under `SLP-SESSION`
- In VSPreview, go to **Settings** -> **Comp**
- Paste the two values in the boxes provided

+++ Firefox

- Visit [Slowpoke Pics](https://slow.pics) in your browser and log into your account
- Open your browser's **Developer Tools**. You will need to get two values:
  - To get your `browserId`, go to **Storage** -> **Local Storage** -> `https://slow.pics`. Copy the key listed there
  - To get your `sessionId`, go to **Storage** -> **Cookies** -> `https://slow.pics`. Copy the key listed under `SLP-SESSION`
- In VSPreview, go to **Settings** -> **Comp**
- Paste the two values in the boxes provided

+++

### Post-processing

!!!
The following scripts are best used with [manual comparisons](#manual).
!!!

==- :icon-file-zip: Compressing

Compresses all `.png` image files in the current directory with Oxipng compression (level 1). Runs fast (typically less than a minute to iterate over hundreds of images).

```py
import os
import glob
import subprocess

folder_path = os.getcwd()  # Get the current working directory

os.chdir(folder_path)  # Change the working directory to the specified folder

image_files = glob.glob("*.png")  # Get a list of PNG files in the folder

processes = []

# Divide the image files into chunks of four
image_chunks = [image_files[i:i+4] for i in range(0, len(image_files), 4)]

for images in image_chunks:
    command = ["oxipng"] + images + ["-o", "1", "-s", "-a", "-t", "8"]
    process = subprocess.Popen(command)
    processes.append(process)

# Wait for all processes to finish
for process in processes:
    process.wait()
```

==- :icon-file-media: Padding

Zero pads frame numbers on images in the current directory and removes extraneous data from file names. *This allows for automatic numerical sorting on [Slowpoke Pics](#slowpoke-pics) and filling of comparison names based on `comp.py`.*

```py
import os

def zero_pad_and_clean_file_names():
    current_directory = os.getcwd()
    for filename in os.listdir(current_directory):
        base_name, extension = os.path.splitext(filename)
        parts = base_name.split('_')

        if len(parts) > 2 and parts[0].isdigit():
            parts[0] = parts[0].zfill(5)
            parts[2] = parts[2].replace("b'", "").replace("'", "")
            new_filename = '_'.join(parts) + extension
            os.rename(os.path.join(current_directory, filename), os.path.join(current_directory, new_filename))

# Usage example
zero_pad_and_clean_file_names()
```

==- :icon-upload: Uploading to ptpimg

Uploads all `.png` image files in the current directory to [ptpimg](https://ptpimg.me). *Requires your ptpimg API key in `%USERPROFILE%\.ptpimg.key`.*

```py
#!/usr/bin/python
#Windows wildcard support fork - use at your own peril
import sys
import requests
import os
import glob
argc=len(sys.argv)
noconf=0
debug=0
configfile=os.path.join(os.path.expanduser("~"), ".ptpimg.key")
if argc==1:
    print("""Usage: ptpimgup.py file1 file2 file3...
    Uploads images to ptpimg and spits out http urls of said images
    config file %s should contain api_key eg:
    01234567-89ab-cdef-0123-456789abcdef
    contact oddbondboris with bugs"""%(configfile))
    sys.exit()
try:
    configfile=os.path.expanduser(configfile)
    cfgfile=open(configfile,'r')
    apikey=cfgfile.read()[:36]
    if len(apikey.split("-")) != 5:
        print("bad api key %s, file should only contain the api key"%(apikey))
    else:
        if debug==1:
            print(apikey)
except:
    print("broken configi %s"%(configfile))
    raise
    sys.exit()
finally:
    try:
        cfgfile.close()
    except:
        pass
links=[]
if '*' in sys.argv[-1]:
    sys.argv[-1:] = glob.glob(sys.argv[-1])
for fname in sys.argv[1:]:
    #print fname
    try:
        curimg=open(os.path.expanduser(fname),'rb')
        r=requests.post("https://ptpimg.me/upload.php",files={('file-upload[0]',('lol.png',curimg, 'image/jpg'))},data={'api_key':apikey})
        if r.status_code==200:
            print("https://ptpimg.me/%s.%s"%(r.json()[0]['code'],r.json()[0]['ext']))
            links.append(("https://ptpimg.me/%s.%s"%(r.json()[0]['code'],r.json()[0]['ext']),fname))
        else:
            print("error uploading file %s http %s"%(fname,r.status_code))
    except IOError as e:
        print("error on file %s : %s"%(fname,e.strerror))
    finally:
        try:
            curimg.close()
        except:
            pass
for link in links:
    print("[img]%s[/img]"%(link[0]))
for link in links:
    print("%s %s"%(link))
```

==-

## Additional Scripts

### Automation

If VSPreview is too complicated to setup, you can run a completed script to automatically generate the comparisons for you.

- [McBaws Script](https://github.com/McBaws/comp)
