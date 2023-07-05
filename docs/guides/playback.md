---
label: Playback
order: -2
description: Learn how to play your anime
image: https://user-images.githubusercontent.com/78981416/215166522-1d7358e8-bec2-4a54-a9ec-71deab646e56.gif
---

# Playback

## Media Players

==- PC
- [MPC-HC](https://github.com/clsid2/mpc-hc/releases) - [Configuration guide with madVR](https://kokomins.wordpress.com/2021/03/27/mpc-hc-and-madvr-setup-guide/)
- [mpv](https://mpv.io/installation/) - [Installation and configuration tutorial](/tutorials/mpv) (recommended)
- [Potplayer](https://potplayer.daum.net)

==- Android
- [mpv-android](https://play.google.com/store/apps/details?id=is.xyz.mpv)
- [VLC for Android](https://play.google.com/store/apps/details?id=org.videolan.vlc)

==- iOS
- [Outplayer](https://apps.apple.com/app/outplayer/id1449923287)
- [VLC media player](https://apps.apple.com/app/vlc-media-player/id650377962)

==- TV/Media Servers
- [Emby](https://emby.media)
- [Jellyfin](https://jellyfin.org)
- [Kodi](https://kodi.tv) - *Can be installed optionally through [LibreELEC OS](https://libreelec.tv).*
- [Plex](https://www.plex.tv)

===

!!!warning
VLC is not recommended as it introduces visual artifacts, displays wrong colors, and breaks intensive subtitles. *We suggest using alternative media players.*

*See the example comparisons to [mpv](https://mpv.io): [Spice and Wolf](https://slow.pics/c/XhbmrYgU), [One Piece](https://slow.pics/c/FW2nBwKP)*
!!!

## Media Servers

The setup consists of two parts: the **server** and the **client**. *Both may be installed on the same system, but they are separate applications.*
- The **client** is a media player that will access the content stored on the server.
- The **server** runs on the device which hosts your content.

In a typical setup, the server is installed on a computer hosted on your home network, with the client being installed on all your devices. *Most media players will also come with their own client, as well as including support for using [Kodi](https://kodi.tv) as a client (recommended for anime).*

- [Emby](https://emby.media)
- [Plex](https://www.plex.tv)
- [Jellyfin](https://jellyfin.org)

!!!
Running a media server requires a rigid folder structure and a set file naming scheme.

*See the guides for your server: [Jellyfin](https://jellyfin.org/docs/general/server/media/shows), [Plex](https://support.plex.tv/articles/naming-and-organizing-your-movie-media-files/)*
!!!

### Kodi

#### Installing Kodi

There are three ways of installing and configuring Kodi for your TV:

==- Client on PC with content stored on the computer
- No decoding problems with a powerful CPU.
- The ability to use high-quality shaders that utilize your GPU to improve upscaling with [an external player of your choice](https://kodi.wiki/view/External_players).
- Control using a normal keyboard and mouse or through an Android app like [Yatse](https://yatse.tv).

==- Client on TV/Android box with content stored on a separate computer
- Works with a simple [SMB share](https://kodi.wiki/view/SMB) from your computer on the same network.

==- Client on TV/Android box with content stored on a media server
- Transcoding support for videos.
- The ability to remotely stream content from devices outside your local network.

Transcoding is used as a last resort to deal with compatibility problems. *Direct play is always preferable to transcoding, which affects quality and uses CPU power on your server. See [Plex's article about transcoding](https://support.plex.tv/articles/200250387-streaming-media-direct-play-and-direct-stream).*

Here, Kodi acts as the client, increasing compatibility and replacing the default one that comes with Emby, Plex, or Jellyfin. This is done through their respective add-ons:
- [Emby for Kodi](https://github.com/MediaBrowser/plugin.video.emby)
- [PlexKodiConnect](https://github.com/croneter/PlexKodiConnect)
- [Jellyfin for Kodi](https://github.com/jellyfin/jellyfin-kodi)

===

#### Scanning Files

*See [Kodi's documentation on scanning files without renaming them](https://kodi.wiki/view/Anime#Scanning_files_without_renaming_them).*

## Settings

### Scaling

Scaling is the process of taking content that does not match your screen resolution and scaling it to fit your display.
- Downscaling takes a *large* video and scales *downwards* (e.g. 1080p video to 720p display)
- Upscaling takes a *small* video and scales *upwards* (e.g. 1080p video to 2160p/4K display)

For displays that match the content resolution, scaling isn't necessary. *These shaders only activate when these resolutions don't match. Scaling is not an enhancement and cannot be enabled manually.*

[mpv](https://mpv.io) has a built-in high-quality profile called `gpu-hq` which enables better upscaling algorithms (`scale=spline36`, `cscale=spline36`, `dscale=mitchell`). By default, mpv uses `spline36`. *This option is necessary to enable even if you use external shaders, as it can act as a fallback.*

#### External Shaders

Below is a brief list of recommended external shaders for [mpv](https://mpv.io):

+++ High-End PCs
For those with high-end hardware, we recommend using [nnedi3-nns256-win8x4](https://github.com/bjin/mpv-prescalers/blob/master/nnedi3-nns256-win8x4.hook).

+++ Mid-End PCs
For those with mid-end hardware, we recommend using [nnedi3-nns128-win8x4](https://github.com/bjin/mpv-prescalers/blob/master/nnedi3-nns128-win8x4.hook).

+++ Low-End PCs
For those with low-end hardware, we recommend sticking to mpv's built-in scalers.

In your `mpv.conf`, add the following:
```properties
vo=gpu
scale=ewa_lanczos
dscale=mitchell
cscale=ewa_lanczos
```

+++

==- Installing External Shaders in mpv
- Head to your shader folder (`%appdata%/mpv/shaders`). *You may need to create one if it doesn't exist.*
- Place your downloaded external shaders in the directory.
- Add the following line to your `mpv.conf`, replacing `<name>` with the file name of your shader:
```
glsl-shader="~~/shaders/<name>"
```
- Confirm your shader is working by pressing *Shift* + *I*, followed by *2*.
   - Watch for dropped frames or high frame times, as they can be a sign that your GPU is unable to keep up with your shader. *If this applies to you, we suggest switching to a less-demanding shader.*

===

### Filtering

Filters act as a temporary fix to amend issues that may appear when watching anime. These can be applied using the options available in [mpv](https://mpv.io) and [madVR](https://madvr.com).

Debanding is the most commonly used filter, which helps to fix issues with [color banding](/tutorials/mpv/#debanding) in your video.

!!!warning
We do not recommend using detail enhancement, noise reduction, or sharpening filters with your media player, *as it can negatively affect the quality of your content.*
!!!

*See the [Quality Guide](/guides/quality) for more information.*

### Smooth Playback

Most modern anime will play close to 24 fps (24000/1001 = 23.976 fps). *However, most displays often run at refresh rates that do not match this frame rate, leading to an effect known as **judder**.*

Judder is most commonly seen with devices that have 60Hz displays, *as the refresh rate is not an integer multiple of the content frame rate.*

==- Explaining Judder
A 60Hz monitor can refresh up to 60 frames every second. Thus, if you were to play a video/game locked at:
- 60 fps: 1 frame is shown for *every 1 refresh* (next frame in 16.67 ms).
- 30 fps: 1 frame is shown for *every 2 refreshes* (next frame in 33.33 ms).
- 15 fps: 1 frame is shown for *every 4 refreshes* (next frame in 66.67 ms).

These frame rates work because they divide evenly into the monitor's 60Hz refresh rate. *With 60 fps video, for instance, each frame is shown for 1 refresh on a 60Hz monitor, with the next frame always appearing 16.67 ms later than the first.*

If anime plays at 24 fps, your monitor will need to show 1 frame for every 2.5 refreshes (60Hz divided by 24 fps). *However, refreshes cannot be divided, and you cannot have "part" of a refresh show on your monitor.*

Instead, your monitor will try to refresh 2.5 times like this:
- Frame 1 is shown for *2 refreshes* (next frame in 33.33 ms).
- Frame 2 is shown for *3 refreshes* (next frame in 50 ms).
- Frame 3 is shown for *2 refreshes* (next frame in 33.33 ms).
- Frame 4 is shown for *3 refreshes* (next frame in 50 ms).

This process is repeated for the entire video. Because each frame is shown for a different number of refreshes, plus the time between changing frames is not the same, *it leads to motion appearing stuttery/laggy. This is especially noticeable when panning scenes.* 

===

To avoid judder, *it is best to try and match your display's refresh rate with the content frame rate:*

==- Using a High Refresh Rate
Displays that run at 120Hz, 144Hz, 240Hz, or 360Hz will match perfectly with each frame, as they are all multiples of 24.

*144Hz displays will not display 30/60 fps content properly. Additionally, none of the above will handle 25 fps content correctly.*

==- Using Adaptive Sync
Adaptive sync is a technology that dynamically adjusts your display's refresh rate based on the frame rate of the content on your screen.

Most modern GPUs will have adaptive sync functionality, such as through AMD FreeSync or NVIDIA G-Sync.

The best solution is to use G-Sync/G-Sync Compatible or FreeSync Premium. *Normal FreeSync may work if your monitor supports [Low Framerate Compensation (LFC)](https://www.amd.com/en/technologies/free-sync-faq#faq-What-is-Low-Framerate-Compensation?). You will also need to use a media player that supports adaptive sync, such as [mpv](https://mpv.io).*

==- Using Automatic Refresh Rate Adjustment
Many modern streaming devices (e.g. Apple TV, NVIDIA Shield, Amazon Fire Stick, etc.) will have the option to change the TV refresh rate to match the content frame rate, either through the device's settings or a setting in the playback software (e.g. Kodi, Plex, etc.)

24/30/60 fps content should all work perfectly. *25 fps content requires 25/50Hz support, which some TVs in [NTSC regions](https://upload.wikimedia.org/wikipedia/commons/thumb/0/0d/PAL-NTSC-SECAM.svg/2560px-PAL-NTSC-SECAM.svg.png) do not support.*

![](https://upload.wikimedia.org/wikipedia/commons/thumb/0/0d/PAL-NTSC-SECAM.svg/2560px-PAL-NTSC-SECAM.svg.png)

===

## Compatibility

### Codecs

Decoding is the process of playing the video file.

There are two ways to handle video decoding: hardware and software. *Hardware decoding removes additional strain from your CPU as it uses your video hardware. By default, most PC players will decode everything using software as a fallback.*

For most users, this isn't an issue. Generally:
- H.264 8-bit (AVC) works everywhere.
- H.264 10-bit (AVC) works on some hardware. *Recommended to use a decent CPU. Additionally, hardware decoding support is suboptimal.*
- H.265 8-bit/10-bit (HEVC) works with most modern hardware.

Most TVs and boxes will display a list of supported codecs in their specifications.

[Plex](https://www.plex.tv) and other media servers will recognize unsupported formats and start transcoding to a supported format, such as H.264.

[Kodi](https://kodi.tv) can fallback to software decoding like on PC media players, giving you the option to directly stream from your server or run without it. *However, since the CPU on your TV/box is much weaker, there may be instances of lag when forcing software decoding. On top of the strain from decoding, any additional CPU activity may further affect your experience, such as rendering complicated `.ass` typesettings.*

### Typesetting

Most subbed anime will use `.ass` subtitles, allowing for extensive styling and typesetting. This comes at the cost of additional system resources, *though most modern computers, phones, and media servers will not have an issue.*

While subtitles will show up on most TVs/media players, the typesetting or overlapping dialogue can sometimes be broken when using certain applications. *This is especially a problem with fansubs, where typesetting is used extensively in various areas (e.g. text on a sign, moving scenes).*

!!!
Most TVs will have difficulty showing `.ass` subtitles. *You should use an external box, such as Apple TV, Fire TV, or NVIDIA Shield.*
!!!