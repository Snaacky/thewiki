---
label: Comparison
description: Learn How to Compare Various Sources of a Video
image: https://user-images.githubusercontent.com/78981416/219719610-2ccd06e9-e3ec-4b50-aae3-a7881292887f.gif
---

# Comparison

## Vapoursynth-Preview

### Setup

1. Install [Python 3.10](https://www.python.org/downloads/release/python-3105/)
   - During installation tick `Add Python 3.10 to PATH`.
2. Install [Git](https://gitforwindows.org/)
   - Optionally type `git version` in your terminal to verify Git was installed.
3. Install [Vapoursynth](https://github.com/vapoursynth/vapoursynth/releases)

   - Select `Install for me only`.
   - If you have any issues here, check out the [official Installation Guide](https://www.vapoursynth.com/doc/installation.html).

4. Before we install vs-preview, we need to install the following dependencies:
   -  [`LibP2P`](https://github.com/DJATOM/LibP2P-Vapoursynth "LibP2P")
   -  [`LSMASHSource`](https://github.com/HomeOfAviSynthPlusEvolution/L-SMASH-Works "LSMASHSource")
   -  [`Subtext`](https://github.com/vapoursynth/subtext "Subtext")
   -  [`vs-placebo`](https://github.com/Lypheo/vs-placebo "vs-placebo")
   -  [`libdovi`](https://github.com/quietvoid/dovi_tool/releases/tag/libdovi-1.6.7 "libdovi")
   -  [`awsmfunc`](https://github.com/OpusGang/awsmfunc "awsmfunc")

   To install LSMASHSource, download [LSMASHSource](https://github.com/HomeOfAviSynthPlusEvolution/L-SMASH-Works/releases "LSMASHSource") and open it. Inside the zip, open the x64 folder, copy `LSMASHSource.dll` and paste it in `%appdata%\VapourSynth\plugins64`
   
   To install the rest of the plugins, run the following commands in your terminal:
   ```
   vsrepo.py install libp2p sub placebo dovi_library
   ```
   ```
   pip install git+https://github.com/OpusGang/awsmfunc.git
   ```
   !!!
   If `vsrepo.py` command doesn't work, make sure Windows is set to open `.py` files with Python.
   !!!

5. Install [vs-preview](https://github.com/Irrational-Encoding-Wizardry/vs-preview):

    ```
    pip install vspreview
    ```

6. Create a new file called `comp.vpy`.
7. Open `comp.vpy` with a text editor such as notepad++ and paste the following, edit as required and save it:

    ```py
    import vapoursynth as vs
    from vapoursynth import core
    from awsmfunc import FrameInfo

    ## Hold shift and right click your file, select copy as path and paste it here
    clip1 = core.lsmas.LWLibavSource(r"C:\Paste\File\Path\Here.mkv")
    clip2 = core.lsmas.LWLibavSource(r"C:\Paste\File\Path\Here.mkv")
    clip3 = core.lsmas.LWLibavSource(r"C:\Paste\File\Path\Here.mkv")

    ## Name of the source, uncomment as needed depending on how many sources you're comparing
    source1 = "FirstSourceName"
    source2 = "SecondSourceName"
    source3 = "ThirdSourceName"

    ## Convert clip to 16bit (Only for filters that need it such as tonemapping and gamma fixing)
    ##clip1 = core.resize.Bicubic(clip1, format=vs.YUV444P16)
    ##clip2 = core.resize.Bicubic(clip2, format=vs.YUV444P16)
    ##clip3 = core.resize.Bicubic(clip3, format=vs.YUV444P16)

    ## Uncomment and edit values as needed if clips need cropping (Black letterboxing) [16 BIT REQUIRED FOR ODD NUMBERS]
    ##clip1 = core.std.Crop(clip1, left=240, right=240, top=0, bottom=0)
    ##clip2 = core.std.Crop(clip2, left=0, right=0, top=276, bottom=276)
    ##clip3 = core.std.Crop(clip3, left=0, right=0, top=21, bottom=21)

    ## Uncomment and edit values as needed if you need to upscale your clips
    ## Always scale to the highest resolution
    ##clip1 = core.resize.Spline36(clip1, 1920, 1080)
    ##clip2 = core.resize.Spline36(clip2, 1920, 1080)
    ##clip3 = core.resize.Spline36(clip3, 3840, 2160)

    ## Uncomment and edit values as needed if you need to trim your clips (Calculate the frame difference and enter the number here)
    ##clip1 = clip1[0:]
    ##clip2 = clip2[24:]
    ##clip3 = clip3[0:]

    ## Tonemapping (For HDR/DV content only), src_csp=1 is for HDR, src_csp=3 is for DV [16 BIT REQUIRED]
    ##clip1 = core.placebo.Tonemap(clip1, dynamic_peak_detection=0, tone_mapping_function=3, src_csp=1, dst_csp=0)
    ##clip2 = core.placebo.Tonemap(clip2, dynamic_peak_detection=0, tone_mapping_function=3, src_csp=3, dst_csp=0)
    ##clip3 = core.placebo.Tonemap(clip3, dynamic_peak_detection=0, tone_mapping_function=3, src_csp=1, dst_csp=0)

    ##Set DV clips to limited after tonemapping so FrameInfo works
    ##clip1 = core.resize.Bicubic(clip1, format=vs.YUV444P16, range=0)
    ##clip2 = core.resize.Bicubic(clip2, format=vs.YUV444P16, range=0)
    ##clip2 = core.resize.Bicubic(clip3, format=vs.YUV444P16, range=0)

    ## Fix Gamma bug (When one source is much brighter than others) [16 BIT REQUIRED]
    ##clip1 = core.std.Levels(clip1, gamma=0.88, min_in=4096, max_in=60160, min_out=4096, max_out=60160, planes=0)
    ##clip2 = core.std.Levels(clip2, gamma=0.88, min_in=4096, max_in=60160, min_out=4096, max_out=60160, planes=0)
    ##clip3 = core.std.Levels(clip3, gamma=0.88, min_in=4096, max_in=60160, min_out=4096, max_out=60160, planes=0)

    ## Displays the frame number, type, and group name in the top left corner
    clip1= FrameInfo(clip1, source1)
    clip2= FrameInfo(clip2, source2)
    clip3= FrameInfo(clip3, source3)

    ## Slow.pics labelling for automatic comparisons
    clip1 = clip1.std.SetFrameProp('Name', data = source1)
    clip2 = clip2.std.SetFrameProp('Name', data = source2)
    clip3 = clip3.std.SetFrameProp('Name', data = source3)

    ## Comment/Uncomment as needed depending on how many clips you're comparing
    clip1.set_output(1)
    clip2.set_output(2)
    clip3.set_output(3)
    ```
8. That's it for the setup, now to use vspreview you just need to run this command in your terminal or paste it in a text file and save it as `comp.bat`:

    ```
    vspreview /path/to/comp.vpy
    ```

    Now, when making comps you just edit `comp.vpy` to include the necessary file paths, comment/uncomment lines as required, edit the crop, trim, upscale, etc values when needed and then run `comp.bat` or run `vspreview comp.vpy` directly from your terminal.


### Quality of Life Changes for Screenshotting Manually via VS-Preview (Recommended)

- **Use slow.pics friendly file naming, so you can drag all the images onto the site and have them automaically sorted**
   In VSPreview on the bottom bar select Misc, set file name template to `{frame}_{script_name}_({index})`.

- **Jump through frames quickly**
   In VSPreview on the bottom bar select Playback, directly right of the playback keys set the `1` value to `120`.

- **Take screenshots quicker by setting the save image button to enter**
   Open `%localappdata%\Programs\Python\Python310\Lib\site-packages\vspreview\toolbars\misc` and edit `toolbar.py` - Line 150: `Replace Qt.SHIFT + Qt.Key_S` with `Qt.Key_Return`. - If you can't spam fast enough, in vs-preview click settings and set PNG compression to a lower level (higher value).

- **Swap binds to save your pinky finger, so you no longer have to hold shift all the time**
   Open `%localappdata%\Programs\Python\Python310\Lib\site-packages\vspreview\toolbars\playback\` and edit `toolbar.py` - Line 164-165 Add `Qt.SHIFT +` before `Qt.Key_Left` and `Qt.Key_Right` - Line 166-167 Remove `Qt.SHIFT +` before `Qt.Key_Left` and `Qt.Key_Right`

### Making the comparison

- Press `Right Arrow` key to move forward a set amount of frames.
- `Number` keys to switch between video sources and compare quality.
- Double tap `Enter` key to take and save screenshots.
- Aim to screenshot a variety of scenes like light/dark, low/high motion, etc.
- Try and match frame type when screenshotting, e.g. all sources on a `B` frame, single frame jump comes in handy for when they don't match (`Shift + Arrow keys`).
  - *Note: If a source file does not have `B` frames for you to match, you should skip matching frame type entirely for that source. This is usually true for Crunchyroll WEB-DLs, which have no `B` frames.*

### Screenshotting Automatically

If you don't want to take screenshots and upload them manually, then you can simply use VS-Preview's automatic comparison function.

- Click the `comp` button in the toolbar.
- Set the `Picture Type` to `All`, or `B` if you want matching frame-types
- Name the collection appropriately for Slow.pics.
- Use a large amount of images to make the comparison as useful as possible, ideally at least 40 since you'll get a lot of futile results with it being automatic.
- Hit `Start Upload` and patiently wait while vspreview absolutely molests your battlestation.
  [![Comp](https://i.imgur.com/00m9QvB.png "Comp")](https://i.imgur.com/00m9QvB.png "Comp")

## Automatic Comparison Scripts

Prequesites:
1. Install [Python 3.10](https://www.python.org/downloads/release/python-3105/)
2. Install [Vapoursynth](https://github.com/vapoursynth/vapoursynth/releases)
   - Select `Install for me only`.
   - If you have any issues here, check out the [official Installation Guide](https://www.vapoursynth.com/doc/installation.html).
3. Install dependencies via running the following command in your terminal:
   ```
   pip install pathlib anitopy pyperclip requests requests_toolbelt
   ```
4. Download [LSMASHSource](https://github.com/HomeOfAviSynthPlusEvolution/L-SMASH-Works/releases "LSMASHSource") and open it. Inside the zip, open the x64 folder, copy `LSMASHSource.dll` and paste it in `%appdata%\VapourSynth\plugins64`
5. (Optional) Install [FFmpeg](https://ffmpeg.org/download.html) and add it to PATH.

Instructions:
1. Put `comp.py` into the same folder where the video files you want to compare are located.
2. Rename your files to have the typical `[Group] Show - EP.mkv` naming, since the script will try to parse the group and show name.
3. When there's no group, such as BDMV or WEB-DL, use the source/service as the group tag.
   For example:
   ```
   /Comparison/
   ├── [JPBD] Youjo Senki - 01.m2ts 
   ├── [Crunchyroll WEB-DL] Youjo Senki - 01.mkv
   ├── [Group] Youjo Senki - 01.mkv
   └── comp.py
   ```
   This will result in images named as JPBD.png, Crunchyroll WEB-DL.png, and Group.png from JPBD, Crunchyroll WEB-DL, and Group respectively.
4. Adjust the variables in the script accordingly.
5. Run the script via either double clicking it or by running `py comp.py` in your terminal.

- [Vodes' Script](https://gist.github.com/Vodes/300997ac1473ac2b9d08d62c169a26de)
- [McBaws' Script](https://gist.github.com/McBaws/a5dcbb244c91bcfdedababf3ee652609)

## Manually with MPV

- [Install MPV](/tutorials/mpv)
- Screenshots are to be taken using MPV.
- [Slow.pics](https://slow.pics/) must be used for making comparisons.
- Any other video-altering settings (different scalers, shaders, etc) are not allowed to be used.
- Hardware decoding must be disabled.
- Both screenshots are to be the exact same frame.
- Both screenshots must be taken at the same resolution, take scaled screenshots if necessary.
- Always scale to the highest resolution.
- Subtitles are to be disabled.
- Comparisons must be titled the name of the anime, and all encodes must be labelled accordingly.
- You must include source in the comparison, this will usually be the BDMV/Remux when BluRay encodes are being compared or WEB-DL if WEB encodes are being compared.

MPV config must be the one as shown below:

```
no-deband
profile=gpu-hq
screenshot-format=png
```

!!!
It's recommended to use the latest git builds of MPV.
!!!
