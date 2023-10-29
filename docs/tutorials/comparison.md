---
label: Comparison
description: Learn how to compare various sources of a video
image: /static/comparison/hyouka.gif
---

# Comparison

Comparisons are frequently used within the enthusiast community to compare the video quality offered by different sources, be that official releases or user made encodes. This guide goes through the process of setting up and effectively utilising Vapoursynth-Preview to produce useful comparisons that will allow you to ascertain which release offers the best visual experience.

## Initial setup

### Installing dependencies

1. Install [Python 3.11](https://www.python.org/downloads/). During installation tick `Add python.exe to PATH`
2. Install [Git](https://gitforwindows.org/)
3. Install [Vapoursynth](https://github.com/vapoursynth/vapoursynth/releases). Select `Install for me only`
4. Before we install vs-preview, we need to install the following dependencies:

    [`libP2P`](https://github.com/DJATOM/LibP2P-Vapoursynth "LibP2P") [`lsmas`](https://github.com/AkarinVS/L-SMASH-Works "LSMASHSource") [`subtext`](https://github.com/vapoursynth/subtext "Subtext") [`vs-placebo`](https://github.com/Lypheo/vs-placebo "vs-placebo") [`awsmfunc`](https://github.com/OpusGang/awsmfunc "awsmfunc")

    You can install these with the following commands:

    ```powershell
    vsrepo.py install lsmas libp2p sub placebo
    ```

    ```powershell
    pip install git+https://github.com/OpusGang/awsmfunc.git
    ```

    !!!
    If `vsrepo.py` command doesn't work, make sure Windows is set to open `.py` files with Python
    !!!

5. (Optional) If you're working with Dolby Vision, you'll need to install the following additional dependencies:

==- :large_purple_square:Dolby Vision:large_green_square: Specific Installation

- [`libdovi`](https://github.com/quietvoid/dovi_tool "libdovi")

```powershell
vsrepo.py install dovi_library
```

- [`lsmas vA.5b`](https://github.com/AkarinVS/L-SMASH-Works/releases/tag/vA.5b) instead of [`lsmas vA.3j`](https://github.com/AkarinVS/L-SMASH-Works/releases/tag/vA.3j). Currently, `vsrepo` installs `A.3x` by default which does not support DV. `A.5x` is experimental and not available on `vsrepo`, so you will have to install it manually.

- Download `release-x86_64-cachedir-cwd.zip` and open it
- Copy `libvslsmashsource.dll` and paste it in `%appdata%\VapourSynth\plugins64`
- If asked, overwite the old `libvslsmashsource.dll` with the new version
===

Install [vs-preview](https://github.com/Setsugennoao/vs-preview):

```powershell
pip install vspreview
```

### The comparison script

1. Create a new file called `comp.vpy`.
2. Open `comp.vpy` with a text editor such as Notepad++ and paste the following, edit as required, and save it:
==- :icon-file-code: comp.vpy

```python
## Boring stuff: Ignore me
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

## FieldBased: Sets the content as either progressive (0) or interlaced (1/2), required for progressive content tagged as interlaced
##clip1 = core.std.SetFieldBased(clip1, 0)
##clip2 = core.std.SetFieldBased(clip2, 1)
##clip3 = core.std.SetFieldBased(clip3, 2)

## Convert: Convert clip to 16bit 444 (Only for filters that need it such as tonemapping and gamma fixing)
##clip1 = core.resize.Bicubic(clip1, format=vs.YUV444P16)
##clip2 = core.resize.Bicubic(clip2, format=vs.YUV444P16)
##clip3 = core.resize.Bicubic(clip3, format=vs.YUV444P16)

## Cropping: Removes letterboxing (Black bars) [16 BIT REQUIRED FOR ODD NUMBERS]
##clip1 = core.std.Crop(clip1, left=240, right=240, top=0, bottom=0)
##clip2 = core.std.Crop(clip2, left=0, right=0, top=276, bottom=276)
##clip3 = core.std.Crop(clip3, left=0, right=0, top=21, bottom=21)

## Scaling: Up/downscale clips to match, recommended to scale to the highest resolution
##clip1 = core.resize.Spline36(clip1, 1920, 1080)
##clip2 = core.resize.Spline36(clip2, 1920, 1080)
##clip3 = core.resize.Spline36(clip3, 3840, 2160)

## Trimming: Trim frames to match clips (Calculate the frame difference and enter the number here)
##clip1 = clip1[0:]
##clip2 = clip2[24:]
##clip3 = clip3[0:]

## Set FPS: Change fps to match other sources, only needed when the previewer doesn't automatically keep them in sync
##clip1 = core.std.AssumeFPS(clip1, fpsnum=24000, fpsden=1001)
##clip2 = core.std.AssumeFPS(clip2, fpsnum=25000, fpsden=1000)
##clip3 = core.std.AssumeFPS(clip3, fpsnum=24000, fpsden=1000)

## Tonemapping: For HDR/DV content only, src_csp=1 is for HDR (and DV Profile 8), src_csp=3 is for DV (Profile 5) [16 BIT REQUIRED]
##clip1 = core.placebo.Tonemap(clip1, dynamic_peak_detection=1, tone_mapping_function=2, tone_mapping_mode=3, src_csp=1, dst_csp=0, gamut_mode=2, intent=0, use_dovi=1)
##clip2 = core.placebo.Tonemap(clip2, dynamic_peak_detection=1, tone_mapping_function=2, tone_mapping_mode=3, src_csp=3, dst_csp=0, gamut_mode=2, intent=0, use_dovi=1)
##clip3 = core.placebo.Tonemap(clip3, dynamic_peak_detection=1, tone_mapping_function=2, tone_mapping_mode=3, src_csp=1, dst_csp=0, gamut_mode=2, intent=0, use_dovi=1, dst_max=120)

## Retagging video to 709 after tonemapping (Otherwise you'll get horrible blue images)
##clip1 = core.std.SetFrameProps(clip1, _Matrix=1, _Transfer=1, _Primaries=1)
##clip2 = core.std.SetFrameProps(clip2, _Matrix=1, _Transfer=1, _Primaries=1)
##clip3 = core.std.SetFrameProps(clip3, _Matrix=1, _Transfer=1, _Primaries=1)

##Set Colour Range: Marks the clip as limited (0) or full (1) range, DV clips will need to be set to limited after tonemapping.
##clip1 = core.resize.Bicubic(clip1, format=vs.YUV444P16, range=0)
##clip2 = core.resize.Bicubic(clip2, format=vs.YUV444P16, range=0)
##clip3 = core.resize.Bicubic(clip3, format=vs.YUV444P16, range=1)

## Gamma: Fix Gamma bug (When one source is much brighter than others) [16 BIT REQUIRED]
##clip1 = core.std.Levels(clip1, gamma=0.88, min_in=4096, max_in=60160, min_out=4096, max_out=60160, planes=0)
##clip2 = core.std.Levels(clip2, gamma=0.88, min_in=4096, max_in=60160, min_out=4096, max_out=60160, planes=0)
##clip3 = core.std.Levels(clip3, gamma=0.88, min_in=4096, max_in=60160, min_out=4096, max_out=60160, planes=0)

## Set Matrix: If source has incorrect/missing metadata, usually applies to 4K SDR and content you're up/downscaling. (Colours will be off, particularly reds, greens, and blues)
##clip1 = core.std.SetFrameProp(clip1, prop="_Matrix", intval=1)
##clip2 = core.std.SetFrameProp(clip2, prop="_Matrix", intval=6)
##clip3 = core.std.SetFrameProp(clip3, prop="_Matrix", intval=5)

## Correct Matrix: If the colours cannot be corrected with the above command [16 BIT REQUIRED]
##clip1 = core.resize.Point(clip1, matrix=5)
##clip1 = core.std.SetFrameProp(clip1, prop="_Matrix", intval=1)
##clip2 = core.resize.Point(clip2, matrix=6)
##clip2 = core.std.SetFrameProp(clip2, prop="_Matrix", intval=1)
##clip3 = core.resize.Point(clip3, matrix=4)
##clip3 = core.std.SetFrameProp(clip3, prop="_Matrix", intval=1)

## Fix Double range compression: When one source looks very washed out
##clip1 = core.resize.Point(clip1, range_in=0, range=1, dither_type="error_diffusion")
##clip1 = core.std.SetFrameProp(clip1, prop="_ColorRange", intval=1)

## Frameinfo: Displays the frame number, type, and group name in the top left corner (Leave these lines untouched, it reads the source from the top)
clip1= FrameInfo(clip1, source1)
clip2= FrameInfo(clip2, source2)
clip3= FrameInfo(clip3, source3)

## FrameProp: Slow.pics/file name labelling (Leave these lines untouched, it reads the source from the top)
clip1 = clip1.std.SetFrameProp('Name', data = source1)
clip2 = clip2.std.SetFrameProp('Name', data = source2)
clip3 = clip3.std.SetFrameProp('Name', data = source3)

## Output: Comment/Uncomment as needed depending on how many clips you're comparing
set_output(clip1, name=source1)
set_output(clip2, name=source2)
set_output(clip3, name=source3)
```

==-
3. Now to use VS-Preview you just need to run this command in your terminal, or paste it into a text file and save it as `comp.bat`

    ```powershell
    vspreview "C:\Path\To\comp.vpy"
    ```

- To get the path, shift and right-click the comp.py file you just made, and select `Copy as path`

Now, when making comps you just edit `comp.vpy` to include the necessary file paths, comment/uncomment lines as required, edit the crop, trim, upscale, etc values when needed, and then run `comp.bat` or run `vspreview comp.vpy` directly from your terminal.

#### Understanding the comparison script

Unsure of how to go about editing the comp script? The panel below will explain how each section works.

==- :icon-info: Understanding comp.vpy
At first, the comparison script may seem daunting, especially since it's a giant wall of text with a lot of technical jargon. It has basic explanations of each section, however, this guide will go over each part in more detail and describe how to properly edit the values.

When comparing, you will want to go through the different sections, uncommenting/commenting the parts you need to use (removing/adding the ## in front of them, which determines whether the line will be used or not), modifying the values to suit your needs and applying them to the correct clip numbers.

**Boring stuff**: Just the requirements to make the script work, can be safely ignored.

**File paths**: The location of the video files you want to compare, you just need to hold shift and right-click the video file, select copy as path, then paste it into this section.

**Source:** The name for each video, this will be shown on the slowpics page, frameinfo in the top left, and file name if you save manually. Recommended to set as the encoder name (not necessarily the release group) in the case of encodes, the region and type in the case of discs/remuxes, or the streaming service in the case of WEB-DLs. Some examples for each respective category would be Beatrice-Raws for one of their encodes, JPN BD for a Japanese Blu-ray or Remux, and DSNP for a WEB-DL from Disney+. If your source is HDR/DV it's best to specify that too.

**FieldBased:** Only required when the content is flagged as interlaced, it may actually be progressive which would require you to set a value of 0 to tell VS-Preview that it's progressive, otherwise the quality will look worse than it is.

**Convert:** Converts the video from 8/10 bit to 16 bit and full chroma, which allows for better precision with certain filters. Should only be enabled when you're using a filter that requires it, such as tonemapping, gamma fixing, or cropping an odd number of pixels.

**Cropping:** Crops pixels from any side of the video, should be used on any content that has letterboxing (black borders) on the top/bottom or sides. Generally encodes do this already, so it will only apply to Blurays/Remuxes and WEB-DLs. To calculate the value you need to enter, you can use the crop feature in the Misc section at the bottom right of VS-Preview, keep on increasing the value until the black bars are completely gone, turn it off then copy the numbers into comp.vpy. Alternatively, you can quickly count the pixels with a tool such as ShareX.

**Scaling:** When you have multiple clips of different resolutions, you need to scale them to match each other. Generally, this means getting the resolution of the highest res video and upscaling the rest to match it. In VS-Preview at the bottom left you can see the video resolution, so for example, if you're comparing a 1920x1080 video to a 1280x720 video, you would set the 1280x720 video to 1920, 1080.

**Trimming:** Often your videos will not be in sync, so you need to cut a certain amount of frames from one of the clips in order for them to match. To easily calculate the value required, navigate to a unique frame such as a scene change in one source, note the frame number, then navigate to the exact same unique frame in your other source and note that frame number, then simply work out the difference between the two numbers, and delay the further ahead video by that amount. Usually, the difference is just 24 black frames at the start. After reloading VS-Preview (Control+R) they should both be perfectly in sync, however, it's possible that they may de-sync later on due to small differences in cuts. In those situations, you can either figure out the new delay as you go along (Bolt-Action) or find all the delays first and set the whole clip to be in sync (Auto/Semi-auto). An example of chaining together trims would be `clip4 = clip4[0:8705]+clip4[8706:25894]+clip4[25895:]`

**Set FPS:** Sometimes you will find that different videos have different frame rates, visible at the bottom left of VS-Preview. This framerate difference usually causes your sources to slowly become more and more out of sync, so you'll need to set the FPS of the outlier to make it match the rest. In rarer circumstances VS-Preview will keep the sources in sync despite there being a difference in frame rate, in those scenarios you can ignore this section, but do note that this means that one of the sources either has dropped or duplicated frames, which should be noted when posting the comp.

**Tonemapping:** For HDR or DV content, by default they won't display correctly so they need to be tonemapped to SDR to look normal. This filter requires you to uncomment the Convert section mentioned earlier. If the content appears as washed out, it's HDR and you should use src_csp=1, if it appears green/purple then it's DV and you need to use src_csp=3. Most other values can remain unchanged unless you wish to play around with different tone mapping algorithms. dst_max forces a certain brightness, it can be used to make HDR/DV clips appear brighter for easier comparison to SDR.

**Retagging video to 709 after tonemapping** vs-placebo, which is used for tonemapping, doesn't set the right props after tonemapping, so we need to manually set them here so VS-Preview doesn't still assume it's an HDR video and display the incorrect colors.

**Set Colour Range:** When content has the wrong metadata or you're tonemapping a DV clip (needs to be set to limited)

**Gamma:** Allows you to adjust the gamma of the videos, most commonly used to correct what's known as the "Gamma bug" that you'll sometimes come across, where one source is much brighter than the rest.

**Set Matrix:** If you notice that certain colours are slightly off between sources (Most notably reds/greens/blues) then your source likely has incorrect or missing matrix metadata. Most commonly you encounter this when upscaling content (eg DVDs to 1080p, BDs to 4K) and 4K SDR WEB releases. Generally speaking, HD SDR content (BDs, 720p-4K WEB) should be set as intval=1 (BT.709), SD content (DVDs) should be set as 5 (BT.601), HDR/DV content should be set as intval=9 (BT.2020 NCL). If this does not fix your issue, see below.

**Correct Matrix:** Sometimes simply setting the correct Matrix does not work, and this is because the video has undergone an incorrect conversion which you need to reverse.

**Fix Double range compression:** Some sources will have undergone range compression twice, and will appear as very washed out. To fix this simply uncomment the lines and enter the correct clip number.

**Frameinfo:** Lists the frame number, type, and source name in the top left of the videos.

**FrameProp:** Sets the source name you entered earlier, for correct labelling on slow.pics.

**Output:** Simply makes the clips appear in VS-Preview.
==-

## Screenshotting

There are 3 methods of screenshotting:

**Full-auto:** Allowing VS-Preview to do everything for you, by far the fastest option.

**Semi-auto:** Manually marking the frame numbers as you go through the clips, then letting VS-Preview automatically extract, compress, and upload them to slow.pics. Generally the recommended option.

**Bolt-action:** Saving the images locally as you go through the clips, then uploading them manually later. Mainly useful for uploading to other sites and keeping the comparison archived locally, but also has other benefits like faster and more efficient compression.

+++ Full-auto

- Click the `comp` button in the bottom right of the toolbar.
- Untick `I` so only `P` and `B` are selected, or only select `B` if you want matching frame-types.
- Tick the `public` box to make the comparison available on the slow.pics homepage.
- Name the collection appropriately for Slow.pics, get the TMDB ID from <https://www.themoviedb.org/> and paste it in the TMDB box, with it correctly set to Movie/TV depending on the content.
- Set the tag to the same name as listed on TMDB.
- Set "Delete After" to your desired value, by default comps expire after 2 years of not being viewed.
- Under `random` enter a frame count amount, you should use a large number of images to make the comparison as effective as possible, ideally at least 40 since not all frames will be useful.
- Make sure your PNG compression is set to 0 (Max) under Settings -> Main*
- Hit `Start Upload` and patiently wait while VS-Preview collects the frames and uploads the comparison.

**If you wish to upload to a slow.pics account for comp history and ability to edit comps, do the following:**

- Make sure you're logged into slow.pics
- Open the developer menu, go to Application at the top, then Storage on the left, open Local Storage and select slow.pics. Copy the browserId.
- Still in the developer menu, go to Network, refresh the page to populate the list, select the slow.pics entry and scroll until you see SLPSESSION and copy the ID.
- Back in VS-Preview, go to settings, Comp, and paste the two IDs in the relevant boxes.

+++ Semi-auto

### Marking the frames

- Press `Shift` & `Right Arrow` keys to move forward a set amount of frames (configurable in playback section of the toolbar)
- `Number` keys to switch between video sources and compare quality.
- Press `Control` & `Space` keys to mark the frame numbers. (Configurable in cfg files)
- Aim to screenshot a variety of scenes, light/dark, low/high motion, etc.
- Try and match frame type when screenshotting, e.g. all sources on a `B` frame, single frame jump comes in handy for when they don't match (`Arrow keys`).
- If a source file does not have `B` frames for you to match, you should skip matching frame type entirely for that source. This is usually true for Crunchyroll WEB-DLs, which have no `B` frames.

### Uploading the comp

- Once you are done marking frames, click the `comp` button in the bottom right of the toolbar.
- Tick the `public` box to make the comparison available on the slow.pics homepage.
- Name the collection appropriately for Slow.pics, get the TMDB ID from <https://www.themoviedb.org/> and paste it in the TMDB box, with it correctly set to Movie/TV depending on the content.
- Set the tag to the same name as listed on TMDB.
- Set "Delete After" to your desired value, by default comps expire after 2 years of not being viewed.
- Under `random` you may specify an additional frame amount for VS-Preview to pick at random, recommended to help fill the comparison with more frames.
- Untick `Current frame` as you likely don't want that included.
- Make sure your PNG compression is set to 0 (Max) under Settings -> Main

**If you wish to upload to a slow.pics account for comp history and the ability to edit comps do the following:**

- Make sure you're logged into slow.pics
- Open the developer menu, go to Application at the top, then Storage on the left, open Local Storage and select slow.pics. Copy the browserId.
- Still in the developer menu, go to Network, refresh the page to populate the list, select the slow.pics entry and scroll until you see SLPSESSION and copy the ID.
- Back in VS-Preview, go to settings, Comp, and paste the two IDs in the relevant boxes.

### QOL Changes

- **Jump through frames quickly**

   In VS-Preview on the bottom bar select Playback, directly right of the playback keys set the `1` value to `120`.

- **Mark screenshots quicker by setting the mark frame button to enter**

    Open `%localappdata%\Programs\Python\Python311\Lib\site-packages\vspreview\toolbars\comp` and edit `toolbar.py`

    - Line 552: Replace `QKeyCombination(Qt.Modifier.CTRL, Qt.Key.Key_Space).toCombined(), self.add_current_frame_to_comp` with `(Qt.Key_Return), self.add_current_frame_to_comp`

- **Swap binds to save your pinky finger, so you no longer have to hold shift all the time**

   Open `%localappdata%\Programs\Python\Python311\Lib\site-packages\vspreview\toolbars\playback\` and edit `toolbar.py`
    - Select lines 180-187, delete them, and paste

    ```python
    self.main.add_shortcut(QKeyCombination(Qt.SHIFT, Qt.Key.Key_Left), self.seek_to_prev_button.click)
    self.main.add_shortcut(QKeyCombination(Qt.SHIFT, Qt.Key.Key_Right), self.seek_to_next_button.click)
    self.main.add_shortcut(Qt.Key.Key_Left, self.seek_n_frames_b_button.click)
    self.main.add_shortcut(Qt.Key.Key_Right, self.seek_n_frames_f_button.click)
    ```

+++ Bolt-action

- Press `Shift` & `Right Arrow` keys to move forward a set amount of frames (configurable in playback section of the toolbar)
- `Number` keys to switch between video sources and compare quality.
- Press `Shift` & `S` keys to take and save screenshots. (Configurable in cfg files)
- *Note: Make sure your PNG compression is enabled under Settings -> Main (0-50 depending on speed impact), or disabled (100) if you plan to compress with a tool such as Oxipng later*
- Aim to screenshot a variety of scenes, light/dark, low/high motion, etc.
- Try and match frame type when screenshotting, e.g. all sources on a `B` frame, single frame jump comes in handy for when they don't match (`Arrow keys`).
- *Note: If a source file does not have `B` frames for you to match, you should skip matching frame type entirely for that source. This is usually true for Crunchyroll WEB-DLs, which have no `B` frames.*

#### QOL Improvements

- **Use slow.pics friendly file naming, so you can drag all the images onto the site and have them automatically sorted, with comparison names intact.**

   In VS-Preview on the bottom bar select Misc, and set file name template to `{frame}_{index}_{Name}`.

- **Jump through frames quickly**

   In VS-Preview on the bottom bar select Playback, directly right of the playback keys set the `1` value to `120`.

- **Take screenshots quicker by setting the save image button to enter**

   Open `%localappdata%\Programs\Python\Python311\Lib\site-packages\vspreview\toolbars\misc` and edit `toolbar.py`
    - Line 166: Replace `QKeyCombination(Qt.Modifier.SHIFT, Qt.Key.Key_S).toCombined(), self.save_frame_as_button.click` with `(Qt.Key_Return), self.save_frame_as_button.click`.
    - If you can't spam fast enough, in vs-preview click settings and set PNG compression to a lower level (higher value).

- **Swap binds to save your pinky finger, so you no longer have to hold shift all the time**

    Open `%localappdata%\Programs\Python\Python311\Lib\site-packages\vspreview\toolbars\playback\` and edit `toolbar.py`

    - Select lines 180-187, delete them, and paste the following, making sure the indentation of the lines match (using spaces).

    ```python
    self.main.add_shortcut(QKeyCombination(Qt.SHIFT, Qt.Key.Key_Left), self.seek_to_prev_button.click)
    self.main.add_shortcut(QKeyCombination(Qt.SHIFT, Qt.Key.Key_Right), self.seek_to_next_button.click)
    self.main.add_shortcut(Qt.Key.Key_Left, self.seek_n_frames_b_button.click)
    self.main.add_shortcut(Qt.Key.Key_Right, self.seek_n_frames_f_button.click)
    ```

#### Post-processing

The Bolt-action method is best paired with some scripts to run after you've saved all the frames. The following scripts are generated by chatgpt as I cannot code 👍
Generally, I just run all 3 with a .bat file, or just the first 2 if I'm not uploading to ptpimg.
==- Zero padding

Zero pads all frames in the current directory and removes some of the garbage from the name. Allows for automatic numerical sorting on slow.pics and automatic filling of comparison names based on comp.vpy

```python
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

==- Compressing
Compresses all png files in the current directory with Oxipng compression level 1, very fast and should take less than a minute to iterate over hundreds of images.

```python
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

==-
==- Uploading to ptpimg
Uploads all png files in the current directory to ptpimg. Requires your ptpimg API key in `%USERPROFILE%\.ptpimg.key`.

```python
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
+++

## Automatic Comp Scripts

If VS-Preview is too scary to setup, you can simply run a script to generate the comparisons for you.
- [McBaws' Script](https://github.com/McBaws/comp)
