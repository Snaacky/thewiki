---
label: Comparison
description: Learn how to compare various sources of a video
image: /static/comparison/hyouka.gif
---

# Comparison

Quality comparisons are frequently used within the enthusiast community to compare the video quality offered by different sources/releases. This guide goes through the process of setting up and effectively utilizing [VSPreview](https://github.com/Irrational-Encoding-Wizardry/vs-preview) to produce useful comparisons that will allow you to ascertain which release offers the best visual experience.

## Setup

### VapourSynth

#### Dependencies

- [Python 3.11](https://www.python.org/downloads/) - *Select `Add python.exe to PATH` during installation*
- [Git](https://gitforwindows.org/)

#### Installation

Download and install `VapourSynth-x64-RXX.exe` from [VapourSynth](https://github.com/vapoursynth/vapoursynth/releases). During installation, select `Install for me only`.

![Vapoursynth release](/static/comparison/vs-download.png)

### VSPreview

#### Dependencies

+++ General Setup

[VSPreview](https://github.com/Irrational-Encoding-Wizardry/vs-preview) requires the following dependencies on top of [VapourSynth](#vapoursynth):

- [awsmfunc](https://github.com/OpusGang/awsmfunc)
- [LibP2P](https://github.com/DJATOM/LibP2P-Vapoursynth)
- [LSMASHSource](https://github.com/AkarinVS/L-SMASH-Works)
- [Subtext](https://github.com/vapoursynth/subtext)
- [vs-placebo](https://github.com/Lypheo/vs-placebo)

==- :icon-gear: Installation

You can install the following dependencies using `vsrepo.py` from [VapourSynth](https://github.com/vapoursynth/vapoursynth/releases):

```powershell
vsrepo.py install lsmas libp2p sub placebo
```

!!!
If `vsrepo.py` command doesn't work, make sure Windows is set to open `.py` files with Python.
!!!

```powershell
pip install git+https://github.com/OpusGang/awsmfunc.git
```

==-

+++ Dolby Vision

If you're working with Dolby Vision (DV) content, you will also need to install the following dependencies:

- [libdovi](https://github.com/quietvoid/dovi_tool)
  ==- :icon-gear: Installation

  ```powershell
  vsrepo.py install dovi_library
  ```

  ==-

- [lsmas vA.5b](https://github.com/AkarinVS/L-SMASH-Works/releases/tag/vA.5b)
  - `vsrepo` installs `A.3x` by default which does not support DV. *`A.5x` is experimental and is not available on `vsrepo`, so you will have to install it manually*
  ==- :icon-gear: Installation

  - Download and extract the correct version for your operating system. *For most users, this will be `release-x86_64-cachedir-cwd.zip`*
  - Copy `libvslsmashsource.dll` and paste it in `%appdata%\VapourSynth\plugins64\`
    - If you have an existing `libvslsmashsource.dll` in the `plugins64`, replace it with the newer `libvslsmashsource.dll`

  ==-

+++

#### Installation

Install [VSPreview](https://github.com/Irrational-Encoding-Wizardry/vs-preview) using `pip`:

```powershell
pip install vspreview
```

## Usage

### Scripting

In order to create a comparison, you will need to create a `.vpy` script. This script outlines the parameters and files which [VSPreview](https://github.com/Irrational-Encoding-Wizardry/vs-preview) will use when generating your comparison.

Create a file called `comp.vpy`. Launch it in your text editor and add to the script based on what you need:

==- :icon-file-code: Initial script [!badge variant="danger" text="Required"]

The basic `comp.vpy` script to get started. This script includes the required dependencies and features to run and get the best experience out of VSPreview.

```py
## Dependencies: Allows vspreview to run (required; do not remove)
import vapoursynth as vs
from vapoursynth import core
from awsmfunc import FrameInfo
from vspreview import set_output

## File paths: Hold shift and right-click your file, select copy as path, and paste it here
clip1 = core.lsmas.LWLibavSource(r"C:\Paste\File\Path\Here.mkv")
clip2 = core.lsmas.LWLibavSource(r"C:\Paste\File\Path\Here.mkv")
clip3 = core.lsmas.LWLibavSource(r"C:\Paste\File\Path\Here.mkv")

## Source: Name of the source
source1 = "FirstSourceName"
source2 = "SecondSourceName"
source3 = "ThirdSourceName"

## Additional comp settings
## Place any additional parameters you want to use in your comp here

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

Section             | Description
--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
**Dependencies**    | Dependencies required to run [VSPreview](https://github.com/Irrational-Encoding-Wizardry/vs-preview)
**File paths**      | The location of your source file
**Source**          | The name of each source. [We recommend following the naming scheme here.](#recommended-source-naming) *If you plan to use [Slowpoke Pics](#slowpoke-pics), this will be the name that will be displayed in comparisons*
**FrameInfo**       | Lists the frame number, type, and source name in the top left of the videos
**FrameProp**       | Sets the source name entered under **Source** for correct labelling on [Slowpoke Pics](#slowpoke-pics)
**Output**          | Parameter to allow clips to appear in [VSPreview](https://github.com/Irrational-Encoding-Wizardry/vs-preview)

==- :icon-play: Playback (frame rate, FieldBased)

#### Frame rate

Sets the source frame rate (fps) based on fractions (`fpsnum`/`fpsden`). For example, `fpsnum=24000` and `fpsden=1000` forces the clip frame rate to 24.000 fps. *This should be used for sources that have different frame rates.*

```py
## Frame rate: Change fps to match other sources (needed for when the previewer is unable to automatically keep them in sync)
clip1 = core.std.AssumeFPS(clip1, fpsnum=24000, fpsden=1001)
clip2 = core.std.AssumeFPS(clip2, fpsnum=25000, fpsden=1000)
clip3 = core.std.AssumeFPS(clip3, fpsnum=24000, fpsden=1000)
```

#### FieldBased

Sets the content as progressive (`0`) or interlaced (`1`/`2`). *This should be used for incorrectly flagged sources.*

```py
## FieldBased: Sets the content as either progressive (0) or interlaced (1/2); required for progressive content tagged as interlaced
clip1 = core.std.SetFieldBased(clip1, 0)
clip2 = core.std.SetFieldBased(clip2, 1)
clip3 = core.std.SetFieldBased(clip3, 2)
```

==- :icon-file-media: Framing (cropping, scaling, trimming)

#### Cropping

Crops the source video by *n* pixels from the selected direction. For example, `left=20` will remove 20 horizonal pixels starting from the left side. *This should be used for sources that use letterboxing or other form of crop.*

!!!warning
16-bit is required for odd numbers. [Make sure you are using the 16-bit color depth.](#convert)
!!!

```py
## Cropping: Removes letterboxing (black bars) [16-bit required for odd numbers]
clip1 = core.std.Crop(clip1, left=240, right=240, top=0, bottom=0)
clip2 = core.std.Crop(clip2, left=0, right=0, top=276, bottom=276)
clip3 = core.std.Crop(clip3, left=0, right=0, top=21, bottom=21)
```

#### Scaling

Downscales or upscales the video. *This should be used to match multiple sources that have differing resolutions.*

```py
## Scaling: Upscale/downscale clips to match; recommended to scale to the highest resolution
clip1 = core.resize.Spline36(clip1, 1920, 1080)
clip2 = core.resize.Spline36(clip2, 1920, 1080)
clip3 = core.resize.Spline36(clip3, 3840, 2160)
```

#### Trimming

Removes the first *n* frames from the source. For example, `[24:]` will skip the first 24 frames and start the source at frame 24. *This should be used when a source is off-sync.*

```py
## Trimming: Trim frames to match clips (calculate the frame difference and enter the number here)
clip1 = clip1[0:]
clip2 = clip2[24:]
clip3 = clip3[0:]
```

==- :icon-paintbrush: Color & contrast (depth, tonemapping, range, gamma, matrix, DRC)

#### Depth

Converts the video to a different color depth (16-bit 444) for better precision. *This should only be used with filters that require it, such as [tonemapping](#tonemapping) or [gamma](#gamma).*

```py
## Depth: Convert clip to 16-bit 444 (only for filters that need it such as tonemapping and gamma fixing)
clip1 = core.resize.Bicubic(clip1, format=vs.YUV444P16)
clip2 = core.resize.Bicubic(clip2, format=vs.YUV444P16)
clip3 = core.resize.Bicubic(clip3, format=vs.YUV444P16)
```

#### Tonemapping

Sets the source to a different tone map (i.e. HDR/DV -> SDR).

- For washed-out colors on a high-dynamic range (HDR) source, use `src_csp=1`
- For overly green/purple colors on a Dolby Vision (DV) source, use `src_csp=3`

!!!warning
16-bit is required. [Make sure you are using the 16-bit color depth.](#convert)
!!!

!!!
`dst_max` forces a certain brightness, which can be used to make HDR/DV clips appear brighter for easier comparison to SDR.
!!!

```py
## Tonemapping: For HDR/DV content only; src_csp=1 is for HDR/DV Profile 8), src_csp=3 is for DV Profile 5 [16-bit required]
clip1 = core.placebo.Tonemap(clip1, dynamic_peak_detection=1, tone_mapping_function=2, tone_mapping_mode=3, src_csp=1, dst_csp=0, gamut_mode=2, intent=0, use_dovi=1)
clip2 = core.placebo.Tonemap(clip2, dynamic_peak_detection=1, tone_mapping_function=2, tone_mapping_mode=3, src_csp=3, dst_csp=0, gamut_mode=2, intent=0, use_dovi=1)
clip3 = core.placebo.Tonemap(clip3, dynamic_peak_detection=1, tone_mapping_function=2, tone_mapping_mode=3, src_csp=1, dst_csp=0, gamut_mode=2, intent=0, use_dovi=1, dst_max=120)
```

```py
## Fix tonemapping: Retags the video to 709 after tonemapping to resolve blue images
clip1 = core.std.SetFrameProps(clip1, _Matrix=1, _Transfer=1, _Primaries=1)
clip2 = core.std.SetFrameProps(clip2, _Matrix=1, _Transfer=1, _Primaries=1)
clip3 = core.std.SetFrameProps(clip3, _Matrix=1, _Transfer=1, _Primaries=1)
```

#### Range

Sets the color range of the clip as limited (`0`) or full (`1`). *This should be used for sources with incorrect metadata or DV tonemapping (set to limited).*

```py
## Color range: Marks the clip's range as limited (0) or full (1); DV clips will need to be set to limited (0) after tonemapping.
clip1 = core.resize.Bicubic(clip1, format=vs.YUV444P16, range=0)
clip2 = core.resize.Bicubic(clip2, format=vs.YUV444P16, range=0)
clip3 = core.resize.Bicubic(clip3, format=vs.YUV444P16, range=1)
```

#### Gamma

Adjusts the brightness level of the video.

!!!warning
16-bit is required. [Make sure you are using the 16-bit color depth.](#convert)
!!!

```py
## Gamma: Fixes gamma bug (i.e. one source is significantly brighter than the others) [16-bit required]
clip1 = core.std.Levels(clip1, gamma=0.88, min_in=4096, max_in=60160, min_out=4096, max_out=60160, planes=0)
clip2 = core.std.Levels(clip2, gamma=0.88, min_in=4096, max_in=60160, min_out=4096, max_out=60160, planes=0)
clip3 = core.std.Levels(clip3, gamma=0.88, min_in=4096, max_in=60160, min_out=4096, max_out=60160, planes=0)
```

#### Matrix

Sets the color gamut to fix the colors of your sources. This is most commonly used on upscaled sources or 4K SDR content. *This should be used on sources with incorrect/missing metadata or colors that are off.*

Generally:

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

If you are unable to correct the colors with the initial matrix command, apply the additional filters below:

!!!warning
16-bit is required. [Make sure you are using the 16-bit color depth.](#convert)
!!!

```py
## Correct matrix: If the colors cannot be corrected with the matrix command [16-bit required]
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

==- :icon-file-code: Full script

```py
## Dependencies: Allows vspreview to run (required; do not remove)
import vapoursynth as vs
from vapoursynth import core
from awsmfunc import FrameInfo
from vspreview import set_output

## File paths: Hold shift and right-click your file, select copy as path, and paste it here
clip1 = core.lsmas.LWLibavSource(r"C:\Paste\File\Path\Here.mkv")
clip2 = core.lsmas.LWLibavSource(r"C:\Paste\File\Path\Here.mkv")
clip3 = core.lsmas.LWLibavSource(r"C:\Paste\File\Path\Here.mkv")

## Source: Name of the source
source1 = "FirstSourceName"
source2 = "SecondSourceName"
source3 = "ThirdSourceName"

## Frame rate: Change fps to match other sources (needed for when the previewer is unable to automatically keep them in sync)
##clip1 = core.std.AssumeFPS(clip1, fpsnum=24000, fpsden=1001)
##clip2 = core.std.AssumeFPS(clip2, fpsnum=25000, fpsden=1000)
##clip3 = core.std.AssumeFPS(clip3, fpsnum=24000, fpsden=1000)

## FieldBased: Sets the content as either progressive (0) or interlaced (1/2); required for progressive content tagged as interlaced
##clip1 = core.std.SetFieldBased(clip1, 0)
##clip2 = core.std.SetFieldBased(clip2, 1)
##clip3 = core.std.SetFieldBased(clip3, 2)

## Cropping: Removes letterboxing (black bars) [16-bit required for odd numbers]
##clip1 = core.std.Crop(clip1, left=240, right=240, top=0, bottom=0)
##clip2 = core.std.Crop(clip2, left=0, right=0, top=276, bottom=276)
##clip3 = core.std.Crop(clip3, left=0, right=0, top=21, bottom=21)

## Scaling: Upscale/downscale clips to match; recommended to scale to the highest resolution
##clip1 = core.resize.Spline36(clip1, 1920, 1080)
##clip2 = core.resize.Spline36(clip2, 1920, 1080)
##clip3 = core.resize.Spline36(clip3, 3840, 2160)

## Trimming: Trim frames to match clips (calculate the frame difference and enter the number here)
##clip1 = clip1[0:]
##clip2 = clip2[24:]
##clip3 = clip3[0:]

## Depth: Convert clip to 16-bit 444 (only for filters that need it such as tonemapping and gamma fixing)
##clip1 = core.resize.Bicubic(clip1, format=vs.YUV444P16)
##clip2 = core.resize.Bicubic(clip2, format=vs.YUV444P16)
##clip3 = core.resize.Bicubic(clip3, format=vs.YUV444P16)

## Tonemapping: For HDR/DV content only; src_csp=1 is for HDR/DV Profile 8), src_csp=3 is for DV Profile 5 [16-bit required]
##clip1 = core.placebo.Tonemap(clip1, dynamic_peak_detection=1, tone_mapping_function=2, tone_mapping_mode=3, src_csp=1, dst_csp=0, gamut_mode=2, intent=0, use_dovi=1)
##clip2 = core.placebo.Tonemap(clip2, dynamic_peak_detection=1, tone_mapping_function=2, tone_mapping_mode=3, src_csp=3, dst_csp=0, gamut_mode=2, intent=0, use_dovi=1)
##clip3 = core.placebo.Tonemap(clip3, dynamic_peak_detection=1, tone_mapping_function=2, tone_mapping_mode=3, src_csp=1, dst_csp=0, gamut_mode=2, intent=0, use_dovi=1, dst_max=120)

## Fix tonemapping: Retags the video to 709 after tonemapping to resolve blue images
##clip1 = core.std.SetFrameProps(clip1, _Matrix=1, _Transfer=1, _Primaries=1)
##clip2 = core.std.SetFrameProps(clip2, _Matrix=1, _Transfer=1, _Primaries=1)
##clip3 = core.std.SetFrameProps(clip3, _Matrix=1, _Transfer=1, _Primaries=1)

## Color range: Marks the clip's range as limited (0) or full (1); DV clips will need to be set to limited (0) after tonemapping.
##clip1 = core.resize.Bicubic(clip1, format=vs.YUV444P16, range=0)
##clip2 = core.resize.Bicubic(clip2, format=vs.YUV444P16, range=0)
##clip3 = core.resize.Bicubic(clip3, format=vs.YUV444P16, range=1)

## Gamma: Fixes gamma bug (i.e. one source is significantly brighter than the others) [16-bit required]
##clip1 = core.std.Levels(clip1, gamma=0.88, min_in=4096, max_in=60160, min_out=4096, max_out=60160, planes=0)
##clip2 = core.std.Levels(clip2, gamma=0.88, min_in=4096, max_in=60160, min_out=4096, max_out=60160, planes=0)
##clip3 = core.std.Levels(clip3, gamma=0.88, min_in=4096, max_in=60160, min_out=4096, max_out=60160, planes=0)

## Matrix: Repairs sources with incorrect/missing metadata; typically used for 4K SDR and upscaled/downscaled content (colors will be off, particularly reds, greens, and blues)
##clip1 = core.std.SetFrameProp(clip1, prop="_Matrix", intval=1)
##clip2 = core.std.SetFrameProp(clip2, prop="_Matrix", intval=6)
##clip3 = core.std.SetFrameProp(clip3, prop="_Matrix", intval=5)

## Correct matrix: If the colors cannot be corrected with the matrix command [16-bit required]
##clip1 = core.resize.Point(clip1, matrix=5)
##clip1 = core.std.SetFrameProp(clip1, prop="_Matrix", intval=1)
##clip2 = core.resize.Point(clip2, matrix=6)
##clip2 = core.std.SetFrameProp(clip2, prop="_Matrix", intval=1)
##clip3 = core.resize.Point(clip3, matrix=4)
##clip3 = core.std.SetFrameProp(clip3, prop="_Matrix", intval=1)

## Fix DRC: Repairs sources with very washed out colors
##clip1 = core.resize.Point(clip1, range_in=0, range=1, dither_type="error_diffusion")
##clip1 = core.std.SetFrameProp(clip1, prop="_ColorRange", intval=1)

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
vspreview comp.vpy
```

Alternatively, you can create a create a `comp.bat` file, replacing `C:\path\to\comp.vpy` with the exact file path to your script:

```powershell
vspreview "C:\path\to\comp.vpy"
```

## Interface

VSPreview's user-friendly interface allows you to perform many actions while being able to compare sources with ease:

![VSPreview interface](/static/comparison/vsp-interface.png)

Key                                                        | Section           | Description
-----------------------------------------------------------|-------------------|----------------------------------------------------------------------
![#ed8796](https://placehold.co/14x14/ed8796/ed8796.png) 1 | Preview           | The video preview window for viewing frames in your sources
![#f5a97f](https://placehold.co/14x14/f5a97f/f5a97f.png) 2 | Timeline          | The video timeline and source/clip selection
![#eed49f](https://placehold.co/14x14/eed49f/eed49f.png) 3 | Playback          | Buttons for controlling playback and scrolling through the timeline
![#a6da95](https://placehold.co/14x14/a6da95/a6da95.png) 4 | Scening           | Tools for creating and modifying scene markers
![#91d7e3](https://placehold.co/14x14/91d7e3/91d7e3.png) 5 | Pipette           | The color information for the current frame
![#8aadf4](https://placehold.co/14x14/8aadf4/8aadf4.png) 6 | Benchmark         | The benchmark tool for testing device performance
![#c6a0f6](https://placehold.co/14x14/c6a0f6/c6a0f6.png) 7 | Misc              | Miscellaneous settings for VSPreview
![#f5bde6](https://placehold.co/14x14/f5bde6/f5bde6.png) 8 | Comp              | The comparison creator
![#f4dbd6](https://placehold.co/14x14/f4dbd6/f4dbd6.png) 9 | [Status](#status) | Various information about the current source

### Status

The status bar displays several information about the current source:

![VSPreview status bar](/static/comparison/vsp-status.png)

Key | Description
----|----------------------------------------
1   | Total number of frames
2   | Video stream length
3   | Video stream resolution
4   | Chroma subsampling type and bit depth
5   | Source frame rate
6   | Picture type of current frame

## Comparing

### Tips

- *[Name your sources clearly to the user](#recommended-source-naming)*
- Try to capture a large variety of scenes (e.g. low/high detail, bright/dark, low/high motion)
- Try to capture frames of the same type
  - We recommend taking `P` or `B` type frames
  - This may be harder to accomplish with particular sources, such as streams or WEB-DLs
  - *[See how to set a specific frame type](#setting-specific-frame-type)*

### Basic Keybinds

Key              | Action
-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
`Left arrow`     | Move back *n* frames based on [config](#change-frame-increment) (default: *n = 1*)
`Right arrow`    | Move forward *n* frames based on [config](#change-frame-increment) (default: *n = 1*)
Number keys      | Switches to source *n* (e.g. `2` switches to `clip2`)
`Shift` + `S`    | Take and save screenshot of the current frame
`Ctrl` + `Space` | Mark current frame number for [automatic](#automatic-badge-variant-secondary-text-fastest-option)/[semi-automatic](#semi-automatic-badge-icon-variant-primary-text-recommended) comparisons

### Process

VSPreview offers three methods for creating comparisons:

==- Automatic [!badge variant="secondary" text="Fastest Option"]

Automatic comparisons are created completely without any additional user input. VSPreview will automatically select, capture, and upload frames for you. *This is the fastest method for creating comparisons.*

==- Semi-automatic [!badge icon=":heart:" variant="primary" text="Recommended"]

Semi-automatic comparisons are created with minor user input. VSPreview will automatically capture and upload frame manually marked by the user. *This is the recommended method for creating comparisons.*

#### Setup

1. Locate the frame(s) you want to compare
   - By default, `Left arrow` and `Right arrow` navigates between frames
   - *See [tips](#tips) on what you should look for*
  
2. Once you find a frame, mark the current frame
   - Default keybind: `Ctrl` + `Space`

==- Manual

Manual comparison are created completely by the user. VSPreview displays and handles frame capture, while the main actions are performed manually by the user through the previewer.

#### Capturing

1. Locate the frame(s) you want to compare
   - By default, `Left arrow` and `Right arrow` navigates between frames
   - *See [tips](#tips) on what you should look for*

2. Once you find a frame, take a screenshot of the current frame
   - Default keybind: `Shift` + `S`

3. Switch to the other sources and take screenshots of their current frame
   - Press the number keys to change sources (e.g. `1` for `clip1`, `2` for `clip2`) and press the screenshot keybind
   - *You may need to readjust the position using the arrow keys*

4. Repeat process for the next frames in your comparison

!!!
By default, all frames are stored within your working directory unless manually changed to a different destination.
!!!

==-

## QoL Changes

### VSPreview

==- Recommended source naming

You can name your sources in any way you'd like. However, we recommend trying to follow any of the naming schemes below for better clarity:

![Example source names](/static/comparison/example-sources.png)

Key                                                        | Meaning
-----------------------------------------------------------|---------------------------------------------
![#ed8796](https://placehold.co/14x14/ed8796/ed8796.png) 1 | The name of the source/release group
![#f5a97f](https://placehold.co/14x14/f5a97f/f5a97f.png) 2 | The video resolution
![#eed49f](https://placehold.co/14x14/eed49f/eed49f.png) 3 | The video codec
![#a6da95](https://placehold.co/14x14/a6da95/a6da95.png) 4 | The file size
![#91d7e3](https://placehold.co/14x14/91d7e3/91d7e3.png) 5 | Additional tags, such as scaling or HDR/DV

Depending on what you are trying to compare will depend on how you should name your sources. For example, if you are trying to compare HDR vs. SDR sources, you should include the type in the source name. *Generally, the first three will cover most comparisons you make, and you are free to include more as needed.*

==- Change frame increment

- In VSPreview, navigate to the bottom bar and toggle the *Playback* section
- In *Playback*, set the number of frames to skip when scrubbing by adjusting the first field:

![Adjust frames in the playback bar](/static/comparison/vsp-playback-frames.png)

==- Changing the screenshot key

The following guide changes the screenshot key from `Shift` + `S` to `Enter`:

- Launch *File Explorer* and go to `%localappdata%\Programs\Python\Python311\Lib\site-packages\vspreview\toolbars\misc`
- In a text editor, open `toolbar.py`
- Locate the lines below (approximately line 143):
  - *Use the Find command to locate text (`Ctrl` + `F`)*

  ```py
        self.main.add_shortcut(
            QKeyCombination(Qt.Modifier.SHIFT, Qt.Key.Key_S).toCombined(), self.save_frame_as_button.click
        )
  ``

- Replace the lines with the following:

  ```py
        self.main.add_shortcut(
            (Qt.Key_Return), self.save_frame_as_button.click
        )
  ```

==- Friendly file naming

- In VSPreview, navigate to the bottom bar and toggle the *Misc* section
- In *Misc*, set file name template to `{frame}_{index}_{Name}`

==- Setting specific frame type

- In VSPreview, navigate to the bottom bar and toggle the *Comp* section
- In *Comp*, go to *Picture types* and uncheck/check the available boxes depending on the frame type(s) you want to use

==- Swap binds for seeking frames

The following guide switches the binds for `Left arrow`/`Right arrow` and `Shift` + `Left arrow`/`Shift` + `Right arrow`:

- Launch *File Explorer* and go to `%localappdata%\Programs\Python\Python311\Lib\site-packages\vspreview\toolbars\playback`
- In a text editor, open `toolbar.py`
- Locate the lines below (approximately line 178):
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
            QKeyCombination(Qt.SHIFT, Qt.Key.Key_Left), self.seek_to_prev_button.click
        )
        self.main.add_shortcut(
            QKeyCombination(Qt.SHIFT, Qt.Key.Key_Right), self.seek_to_next_button.click
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
  - To get your `sessionId`, go to **Network** -> slow.pics. Copy the key listed under `SLPSESSION`
- In VSPreview, go to **Settings** -> **Comp**
- Paste the two values in the boxes provided

+++ Firefox

- Visit [Slowpoke Pics](https://slow.pics) in your browser and log into your account
- Open your browser's **Developer Tools**. You will need to get two values:
  - To get your `browserId`, go to **Storage** -> **Local Storage** -> `https://slow.pics`. Copy the key listed there
  - To get your `sessionId`, go to **Storage** -> **Cookies** -> `https://slow.pics`. Copy the key listed under `SLPSESSION`
- In VSPreview, go to **Settings** -> **Comp**
- Paste the two values in the boxes provided

+++

## Additional Scripts

### Automation

If VSPreview is too complicated to setup, you can run a completed script to automatically generate the comparisons for you.

- [McBaws Script](https://github.com/McBaws/comp)

### Post-processing

!!!
The following scripts are best used with [manual comparisons](#manual).
!!!

==- Compressing

Compresses all `.png` image files in the current directory with Oxipng compression (level 1). Runs fast (typically less than a minute to iterate hundreds of images)

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

==- Uploading to ptpimg

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

==- Zero padding

Removes padding from frames in the current directory and removes extraneous data from the file name. This allows for automatic numerical sorting on [Slowpoke Pics](#slowpoke-pics) and filling of comparison names based on `comp.vpy`.

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

==-
