---
label: MPV
description: Installation, Setup and Configuration for MPV
image: https://user-images.githubusercontent.com/78981416/215125796-08b99128-fe50-4d0c-b0dd-49f8828af0dc.png
---

# MPV

## Windows Rezising

1. Grab the topmost filee from [here](https://sourceforge.net/projects/mpv-player-windows/files/64bit/).
   - _Note: If you don't want to setup MPV yourself, you can grab this pre-configured [MPV portable build](https://mega.nz/folder/11QCTZgR#sdsjUYkIieGjVR09mnpYSw) with the same settings as described in this guide. All you need to do is download it and follow these installation instructions._
2. Extract it into a folder.
   - _Note: This folder cannot be changed after installation. If you want to change it after installation, you'll have to uninstall and then reinstall it in the new location._
3. Navigate to the `installer` folder.
4. Inside the `installer` folder you'll find `mpv-install.bat`, run this as an administrator.
   - _If you want to read about what `mpv-install.bat` does, visit [here](https://github.com/rossy/mpv-install)._
5. After it's done, you'll get a prompt to open Control Panel and set MPV as the default player.

That's it for the basic installation of MPV. You can use it without doing anything else.

### Adding MPV to PATH

1. Open the Start Search, type in `env`, and choose `Edit the system environment variables`.
2. Click the `Environment Variables…` button.
3. Under the `System Variables` section (the lower half), find the row with `Path` in the first column, and click `edit`.
4. The `Edit environment variable` UI will appear. Here, you can click `New` and add the path of the directory containing `mpv.exe`.
5. Dismiss all of the dialogs by choosing `OK`. Your changes are saved.

## MacOS

Grab MacOS builds from [here](https://laboratory.stolendata.net/~djinn/mpv_osx/)

### Homebrew

You will need [homebrew](https://brew.sh/) installed for this and then run the following command in your terminal.

```
brew install mpv --HEAD
```

## Linux

```
apt install mpv
```

# Additional files/folders

The default path for mpv config is `%APPDATA%/mpv/` but a folder named `portable_config` next to the `mpv.exe` overrides this. Here's a brief overview of files/folders you may want inside either of these folders -

- `mpv.conf` - MPV user settings.
- `input.conf` - custom keybind settings. You can see the default key bindings [here](https://i.imgur.com/G6Rx74P.png) and syntax [here](https://mpv.io/manual/master/#input-conf).
- `fonts` - Font files in this directory are used by mpv/libass for subtitles.
- `scripts` - This directory is used to load custom scripts, usually `.lua` files. You can find scripts [here](https://github.com/mpv-player/mpv/wiki/User-Scripts).
- `shaders` - This directory is used to load custom shaders like NNEDI3, Ravu, FSRCNNX, etc.
- `script-opts` - A complementary folder to `scripts`, required by some `.lua` scripts to store user settings.

For more details, visit [here](https://mpv.io/manual/master/#files)

# Custom config

MPV is a great player out-of-the-box but you can customize it further to make it even better.

## Basic Config

The comments are a brief explaination of what each line does.

```
#### General
profile=gpu-hq                    #Allows for higher quality playback on MPV. Uses scaling methods that are significantly better than Default MPV, VLC and MPC.
vo=gpu                            #General purpose, customizable, GPU-accelerated video output driver. It supports extended scaling methods, dithering, color management, custom shaders, HDR, and more.
priority=high                     #Makes PC prioritize MPV for allocating resources.
keep-open=yes
save-position-on-quit
script-opts=ytdl_hook-ytdl_path=yt-dlp.exe
cursor-autohide-fs-only
cursor-autohide=100
fullscreen=yes                   #Set to no if you don't want to start in fullscreen

#### Shaders
scale=spline36                    #if lag occurs change spline36 to bicubic_fast
dscale=mitchell                   #if lag occurs change mitchell to bicubic_fast
cscale=spline36                   #if lag occurs change spline36 to bicubic_fast

#### Screenshots
screenshot-format=png
screenshot-high-bit-depth=no
screenshot-png-compression=9       #if it takes too long taking screenshots then delete this line

#### Subtitle Options
demuxer-mkv-subtitle-preroll=yes   #try harder to show embedded soft subtitles when seeking somewhere
sub-ass-vsfilter-blur-compat=no    #Scale \blur tags by video resolution instead of script resolution (enabled by default)
sub-fix-timing=no                  #Adjust subtitle timing is to remove minor gaps or overlaps between subtitles
sub-auto=fuzzy                     #Load all subs containing the media filename.

#### Language Priority
#Sub
slang=jpn,eng,en                   #Add enm before eng for honorifics
alang=jpn,ja,jpn

#Dub                               #uncomment this section to prefer English Dub with subtitles for English Dub.
#alang=eng,en
#slang=zxx,auto,eng
#subs-with-matching-audio=no

```

## Profiles for automatic debanding

Banding is a visual artifact, visual artifacts should never be in a video.
Example of banding:
![Banding (bad) vs No Banding (good)](https://user-images.githubusercontent.com/78981416/214381256-e5722886-57d7-4cd4-834e-edbd30b432e0.png "Banding (bad) vs No Banding (good)")
Debanding is the process of removing said banding.
6 minute explanation of what causes banding: https://youtu.be/h9j89L8eQQk

```
#Banding is a visual artifact, visual artifacts should never be in a video.
#Example of banding: https://imgur.com/32d77H0
#Debanding is the process of removing said banding.
#6 minute explanation of what causes banding: https://youtu.be/h9j89L8eQQk

[Web]
    profile-cond=string.match(p.filename, "HorribleSubs")~=nil or string.match(p.filename, "Erai%-raws")~=nil or string.match(p.filename, "SubsPlease")~=nil
    deband=yes

[Mini-Encode #1]
    profile-cond=string.match(p.filename, "ASW")~=nil or string.match(p.filename, "DKB")~=nil or string.match(p.filename, "Judas")~=nil
    deband=yes

[Mini-Encode #2]
    profile-cond=string.match(p.filename, "Cleo")~=nil or profile-cond=string.match(p.filename, "Cerberus")~=nil or string.match(p.filename, "Reaktor")~=nil
    deband=yes

[Mini-Encode #3]
    profile-cond=string.match(p.filename, "Ember")~=nil or string.match(p.filename, "Nep%_Blanc")~=nil or string.match(p.filename, "Akihito")~=nil
    deband=yes

```

## Profiles for upscaling

These are for a 2160p display, simply adjust the values accordingly for other resolutions.
[!ref Read about scaling in details](/guides/playback/#scaling)

```
[2x_upscaling]
    profile-desc='Profile for 1080: 2*1080=2160'
    profile-cond=(height == 1080 and estimated_vf_fps <= 31 and string.match(path, 'http') == nil)
    glsl-shaders='~~/shaders/compute/nnedi3-nns16-win8x4.hook'
    scaler-resizes-only=no  # yes with FSRCNNX_x2_16-0-4-1.glsl

[3x_upscaling]
    profile-desc='Profile for 720: 3*720=2160'
    profile-cond=(height == 720 and string.match(path, 'http') == nil)
    glsl-shaders='~~/shaders/vulkan/compute/ravu-3x-r3.hook'
    scaler-resizes-only=no

[4x_upscaling]
    profile-desc='Profile for 540: 2*2*540=2160'
    profile-cond=(height == 540 and string.match(path, 'http') == nil)
    glsl-shaders-append='~~/shaders/vulkan/compute/ravu-r3.hook'
    glsl-shaders-append='~~/shaders/vulkan/compute/ravu-r3.hook'
    scaler-resizes-only=no
```
![An Example Showing some of the Scalers. Do note this does not cover everything.](https://user-images.githubusercontent.com/78981416/214379891-5caaf01d-8c24-4e3c-addf-ed8eee02305e.png)

!!!
Scalers only work when the resolution of your video does not match with your display. If the resolution of the video you're playing matches with your display's resolution, scalers will remain unused.
!!!

For more options, check out the following links:

- [MPV Docs](https://mpv.io/manual/master/#options)
- [Configuration guide 1](https://iamscum.wordpress.com/guides/videoplayback-guide/)
- [Configuration guide 2](https://kokomins.wordpress.com/2019/10/14/mpv-config-guide/)
- [Sample config 1](https://github.com/DeadNews/mpv-config/blob/main/mpv/mpv.conf)
- [Sample config 2](https://github.com/LightArrowsEXE/dotfiles/blob/master/mpv/.config/mpv/mpv.conf)

!!!warning
Some of the above guides are quite advanced, do not blindly copy paste things from them without understanding what they do.
!!!

## Scripts

- https://github.com/ekisu/mpv-webm
- https://github.com/jonniek/mpv-playlistmanager
- https://github.com/mpv-player/mpv/blob/master/TOOLS/lua/autoload.lua
- https://github.com/mpv-player/mpv/blob/master/TOOLS/lua/pause-when-minimize.lua
- https://github.com/po5/thumbfast
- https://github.com/po5/trackselect

## DV/HDR Tonemapping

For watching Dobly Vision or HDR content on an SDR screen you need to tonemap it. To do this change the `vo=gpu` to `vo=gpu-next` and add `tone-mapping=bt.2446a` to your config.

```
vo=gpu-next
tone-mapping=bt.2446a
```

!!!
This will only affect content that needs tonemapping, SDR content will be unaffected. `gpu-next` is still an experimental output driver so you may experience unexpected behavior. You can read more about `gpu-next` [here](https://github.com/mpv-player/mpv/wiki/GPU-Next-vs-GPU).
!!!
