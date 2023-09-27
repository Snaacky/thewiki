---
label: Comparison
description: Learn how to compare various sources of a video
image: /static/comparison/hyouka.gif
---

# Comparison

Quality comparisons are frequently used within the enthusiast community to compare the video quality offered by different sources/releases. This guide goes through the process of setting up and effectively utilising [VSPreview](https://github.com/Irrational-Encoding-Wizardry/vs-preview) to produce useful comparisons that will allow you to ascertain which release offers the best visual experience.

## Setup

### VapourSynth

Download and install [VapourSynth](https://github.com/vapoursynth/vapoursynth/releases). You will also need its dependencies:

- [Python](https://www.python.org/downloads/) - *Select `Add python.exe to PATH` during installation*
- [Git](https://gitforwindows.org/)

### VSPreview

+++ Basic Setup

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

If you're working with Dolby Vision content, you will also need to install the following dependencies:

- [libdovi](https://github.com/quietvoid/dovi_tool)
  ==- :icon-gear: Installation

  ```powershell
  vsrepo.py install dovi_library
  ```

  ==-

- [lsmas vA.5b](https://github.com/AkarinVS/L-SMASH-Works/releases/tag/vA.5b)
  - *`vsrepo` installs `A.3x` by default which does not support DV. `A.5x` is experimental and not available on `vsrepo`, so you will have to install it manually*
  ==- :icon-gear: Installation

  - Download and extract the correct version for your operating system. *For most users, this will be `release-x86_64-cachedir-cwd.zip`*
  - Copy `libvslsmashsource.dll` and paste it in `%appdata%\VapourSynth\plugins64\`
    - If you currently have an older `libvslsmashsource.dll` in the `plugins64`, replace it with the newer `libvslsmashsource.dll`

  ==-

+++

### Installation

Install [VSPreview](https://github.com/Irrational-Encoding-Wizardry/vs-preview) using `pip`:

```powershell
pip install vspreview
```

## Usage

### Scripting

In order to create a comparison, you will need to create a `.vpy` script. This script outlines the parameters and files which [VSPreview](https://github.com/Irrational-Encoding-Wizardry/vs-preview) will use when generating your comparison.

Create a file called `comp.vpy`. Launch it in your text editor and input the example script below:

==- :icon-file-code: Sample `comp.vpy`

```python
## Dependencies
import vapoursynth as vs
from vapoursynth import core
from awsmfunc import FrameInfo
from vspreview import set_output

## File paths: Hold shift + right-click your file and select copy as path, then paste it here
clip1 = core.lsmas.LWLibavSource(r"C:\Paste\File\Path\Here.mkv")
clip2 = core.lsmas.LWLibavSource(r"C:\Paste\File\Path\Here.mkv")
clip3 = core.lsmas.LWLibavSource(r"C:\Paste\File\Path\Here.mkv")

## Source: Name of the source
source1 = "FirstSourceName"
source2 = "SecondSourceName"
source3 = "ThirdSourceName"

## FieldBased: Sets the content as either progressive (0) or interlaced (1/2); required for progressive content tagged as interlaced
##clip1 = core.std.SetFieldBased(clip1, 0)
##clip2 = core.std.SetFieldBased(clip2, 1)
##clip3 = core.std.SetFieldBased(clip3, 2)

## Convert: Convert clip to 16-bit 444 (used only for filters that need it such as tonemapping and gamma fixing)
##clip1 = core.resize.Bicubic(clip1, format=vs.YUV444P16)
##clip2 = core.resize.Bicubic(clip2, format=vs.YUV444P16)
##clip3 = core.resize.Bicubic(clip3, format=vs.YUV444P16)

## Cropping: Removes letterboxing/black bars [16-bit required for odd numbers]
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

## FPS: Change fps to match other sources (needed when the previewer doesn't automatically keep them in sync)
##clip1 = core.std.AssumeFPS(clip1, fpsnum=24000, fpsden=1001)
##clip2 = core.std.AssumeFPS(clip2, fpsnum=25000, fpsden=1000)
##clip3 = core.std.AssumeFPS(clip3, fpsnum=24000, fpsden=1000)

## Tonemapping: For HDR/DV content only, src_csp=1 is for HDR (and DV Profile 8), src_csp=3 is for DV (Profile 5) [16-bit required]
##clip1 = core.placebo.Tonemap(clip1, dynamic_peak_detection=1, tone_mapping_function=2, tone_mapping_mode=3, src_csp=1, dst_csp=0, gamut_mode=2, intent=0, use_dovi=1)
##clip2 = core.placebo.Tonemap(clip2, dynamic_peak_detection=1, tone_mapping_function=2, tone_mapping_mode=3, src_csp=3, dst_csp=0, gamut_mode=2, intent=0, use_dovi=1)
##clip3 = core.placebo.Tonemap(clip3, dynamic_peak_detection=1, tone_mapping_function=2, tone_mapping_mode=3, src_csp=1, dst_csp=0, gamut_mode=2, intent=0, use_dovi=1, dst_max=120)

## Color Range: Marks the clip as limited (0) or full (1) range; DV clips will need to be set to limited after tonemapping
##clip1 = core.resize.Bicubic(clip1, format=vs.YUV444P16, range=0)
##clip2 = core.resize.Bicubic(clip2, format=vs.YUV444P16, range=0)
##clip3 = core.resize.Bicubic(clip3, format=vs.YUV444P16, range=1)

## Gamma: Fix gamma bug (when one source is much brighter than others) [16-bit required]
##clip1 = core.std.Levels(clip1, gamma=0.88, min_in=4096, max_in=60160, min_out=4096, max_out=60160, planes=0)
##clip2 = core.std.Levels(clip2, gamma=0.88, min_in=4096, max_in=60160, min_out=4096, max_out=60160, planes=0)
##clip3 = core.std.Levels(clip3, gamma=0.88, min_in=4096, max_in=60160, min_out=4096, max_out=60160, planes=0)

## Matrix: If source has incorrect/missing metadata, usually applies to 4K SDR and content you're up/downscaling. (Colours will be off, particularly reds, greens, and blues)
##clip1 = core.std.SetFrameProp(clip1, prop="_Matrix", intval=1)
##clip2 = core.std.SetFrameProp(clip2, prop="_Matrix", intval=6)
##clip3 = core.std.SetFrameProp(clip3, prop="_Matrix", intval=5)

## Correct Matrix: Extra parameters if colors are not corrected with Matrix [16-bit required]
##clip1 = core.resize.Point(clip1, matrix=5)
##clip1 = core.std.SetFrameProp(clip1, prop="_Matrix", intval=1)
##clip2 = core.resize.Point(clip2, matrix=6)
##clip2 = core.std.SetFrameProp(clip2, prop="_Matrix", intval=1)
##clip3 = core.resize.Point(clip3, matrix=4)
##clip3 = core.std.SetFrameProp(clip3, prop="_Matrix", intval=1)

## Fix DRC: Used when one source looks very washed out
##clip1 = core.resize.Point(clip1, range_in=0, range=1, dither_type="error_diffusion")
##clip1 = core.std.SetFrameProp(clip1, prop="_ColorRange", intval=1)

## FrameInfo: Displays the frame number, type, and group name in the top-left corner (leave these lines untouched, it reads the source from the top)
clip1 = FrameInfo(clip1, source1)
clip2 = FrameInfo(clip2, source2)
clip3 = FrameInfo(clip3, source3)

## FrameProp: slow.pics/file name labeling (leave these lines untouched, it reads the source from the top)
clip1 = clip1.std.SetFrameProp('Name', data = source1)
clip2 = clip2.std.SetFrameProp('Name', data = source2)
clip3 = clip3.std.SetFrameProp('Name', data = source3)

## Output: Comment/uncomment as needed, depending on how many sources you're comparing
set_output(clip1, name = source1)
set_output(clip2, name = source2)
set_output(clip3, name = source3)
```

==- :icon-info: Understanding the script

Section            | Description
-------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
**Dependencies**   | Dependencies required to run [VSPreview](https://github.com/Irrational-Encoding-Wizardry/vs-preview)
**File paths**     | The location of your source file
**Source**         | The name of each source. [We recommend following the naming scheme here.](/static/comparison/source-name.png) *If you use slow.pics, this will be the name that will be displayed in comparisons*
**FieldBased**     | Sets the content as progressive or interlaced. *This should be used for incorrectly flagged sources*
**Convert**        | Converts the video to a different color space for better precision. *This should only be used with a filter that requires it, such as tonemapping*
**Cropping**       | Crops certain pixels from any side of the video. *This should be used for sources that use letterboxing or other form of crop*
**Scaling**        | Downscales or upscales the video. *This should be used when using multiple sources that have differing resolutions*
**Trimming**       | Removes a set number of frames from the source. *This should be used when the source is off-sync*
**FPS**            | Sets the source frame rate. *This should be used for sources that have different frame rates*
**Tonemapping**    | Sets the source to a different tonemap (e.g. HDR/DV -> SDR). Requires **Convert** parameters to be enabled. *For washed-out colors on HDR, use `src_csp=1.` For green/purple colors in DV, use `src_csp=3`*
**Color Range**    | Sets the color range for the source. *This should be used for sources with incorrect metadata or DV tonemapping (set to limited)*
**Gamma**          | Adjusts the gamma of the video
**Matrix**         | Sets the metadata to fix colors for your sources. [See the guide here.](/static/comparison/matrix.png) *This should be used for sources with colors that are slightly off*
**Correct Matrix** | Extra parameters to fix **Matrix**
**Fix DRC**        | Fixes double range compression washed out sources
**FrameInfo**      | Lists the frame number, type, and source name in the top left of the videos
**FrameProp**      | Sets the source name entered under **Source** for correct labelling on slow.pics
**Output**         | Parameter to allow clips to appear in [VSPreview](https://github.com/Irrational-Encoding-Wizardry/vs-preview)

==-

### Running

To run your comparison script, launch a terminal window in your working directory and run the following:

```powershell
vspreview comp.vpy
```

Alternatively, you can create a create a `comp.bat` file, replacing `C:\Path\To\comp.vpy` with the file path to your script:

```powershell
vspreview "C:\Path\To\comp.vpy"
```

## Usage

VSPreview offers three methods for creating comparisons:

- **Automatic** - VSPreview will automatically select, capture, and upload frames for you; *fastest option*
- **Semi-automatic** - VSPreview will capture and upload manually marked frames; *recommended*
- **Manual** - All actions are performed manually by the user through VSPreview

+++ Automatic

- Click the `comp` button in the bottom right of the toolbar
- Untick `I` so only `P` and `B` are selected, or only select `B` if you want matching frame-types
- Tick the `public` box to make the comparison available on the slow.pics homepage
- Name the collection appropriately for Slow.pics, get the TMDB ID from <https://www.themoviedb.org/> and paste it in the TMDB box, with it correctly set to Movie/TV depending on the content
- Set the tag to the same name as listed on TMDB
- Set "Delete After" to your desired value, by default comps expire after 2 years of not being viewed
- Under `random` enter a frame count amount, you should use a large number of images to make the comparison as effective as possible, ideally at least 40 since not all frames will be useful
- Make sure your PNG compression is set to 0 (Max) under Settings -> Main*
- Hit `Start Upload` and patiently wait while VSPreview collects the frames and uploads the comparison

+++ Semi-automatic

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
- Under `random` you may specify an additional frame amount for VSPreview to pick at random, recommended to help fill the comparison with more frames.
- Untick `Current frame` as you likely don't want that included.
- Make sure your PNG compression is set to 0 (Max) under Settings -> Main

**If you wish to upload to a slow.pics account for comp history and the ability to edit comps do the following:**

- Make sure you're logged into slow.pics
- Open the developer menu, go to Application at the top, then Storage on the left, open Local Storage and select slow.pics. Copy the browserId.
- Still in the developer menu, go to Network, refresh the page to populate the list, select the slow.pics entry and scroll until you see SLPSESSION and copy the ID.
- Back in VSPreview, go to settings, Comp, and paste the two IDs in the relevant boxes.

### QOL Changes

- **Jump through frames quickly**

   In VSPreview on the bottom bar select Playback, directly right of the playback keys set the `1` value to `120`.

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

+++ Manual

- Press `Shift` & `Right Arrow` keys to move forward a set amount of frames (configurable in playback section of the toolbar)
- `Number` keys to switch between video sources and compare quality.
- Press `Shift` & `S` keys to take and save screenshots. (Configurable in cfg files)
- *Note: Make sure your PNG compression is enabled under Settings -> Main (0-50 depending on speed impact), or disabled (100) if you plan to compress with a tool such as Oxipng later*
- Aim to screenshot a variety of scenes, light/dark, low/high motion, etc.
- Try and match frame type when screenshotting, e.g. all sources on a `B` frame, single frame jump comes in handy for when they don't match (`Arrow keys`).
- *Note: If a source file does not have `B` frames for you to match, you should skip matching frame type entirely for that source. This is usually true for Crunchyroll WEB-DLs, which have no `B` frames.*

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

## QoL Changes

### Slowpoke Pics

#### Fetching Account Tokens

If you plan on uploading to [Slowpoke Pics](https://slow.pics) (slow.pics) under your account, you will need to provide VSPreview with your account tokens.

+++ Chrome

- Visit [Slowpoke Pics](https://slow.pics) in your browser and log into your account
- Open your browser's **Developer Tools**
  - To get your `browserId`, go to the **Application** tab and locate **Storage** -> **Local Storage** -> `https://slow.pics`. Copy the key listed there
  - To get your `sessionId`, go to the **Network** tab and locate the slow.pics entry. Scroll until you find `SLPSESSION` and copy the key stored there
- Paste the two values in VSPreview under **Settings** -> **Comp**

+++ Firefox

- Visit [Slowpoke Pics](https://slow.pics) in your browser and log into your account
- Open your browser's **Developer Tools**
  - To get your `browserId`, go to the **Storage** tab and locate **Local Storage** -> `https://slow.pics`. Copy the key listed there
  - To get your `sessionId`, go to the **Storage** tab and locate **Cookies** -> `https://slow.pics`. Copy the `SLPSESSION` value
- Paste the two values in VSPreview under **Settings** -> **Comp**

+++

#### Friendly File Naming

In VSPreview on the bottom bar select Misc, and set file name template to `{frame}_{index}_{Name}`.

- **Jump through frames quickly**

   In VSPreview on the bottom bar select Playback, directly right of the playback keys set the `1` value to `120`.

- **Take screenshots quicker by setting the save image button to enter**

   Open `%localappdata%\Programs\Python\Python311\Lib\site-packages\vspreview\toolbars\misc` and edit `toolbar.py`
    - Line 166: Replace `QKeyCombination(Qt.Modifier.SHIFT, Qt.Key.Key_S).toCombined(), self.save_frame_as_button.click` with `(Qt.Key_Return), self.save_frame_as_button.click`.
    - If you can't spam fast enough, in VSPreview click settings and set PNG compression to a lower level (higher value).

- **Swap binds to save your pinky finger, so you no longer have to hold shift all the time**

    Open `%localappdata%\Programs\Python\Python311\Lib\site-packages\vspreview\toolbars\playback\` and edit `toolbar.py`

    - Select lines 180-187, delete them, and paste the following, making sure the indentation of the lines match (using spaces).

    ```python
    self.main.add_shortcut(QKeyCombination(Qt.SHIFT, Qt.Key.Key_Left), self.seek_to_prev_button.click)
    self.main.add_shortcut(QKeyCombination(Qt.SHIFT, Qt.Key.Key_Right), self.seek_to_next_button.click)
    self.main.add_shortcut(Qt.Key.Key_Left, self.seek_n_frames_b_button.click)
    self.main.add_shortcut(Qt.Key.Key_Right, self.seek_n_frames_f_button.click)
    ```

### Automation

If VSPreview is too complicated to setup, you can run a completed script to generate the comparisons for you.

- [McBaws Script](https://github.com/McBaws/comp)
