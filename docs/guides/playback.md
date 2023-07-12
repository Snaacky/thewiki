---
label: Playback
order: -2
description: Learn how to play your anime
image: https://user-images.githubusercontent.com/78981416/215166522-1d7358e8-bec2-4a54-a9ec-71deab646e56.gif
---

# Playback

## Media Players

+++ PC :desktop_computer:

- [mpv](https://mpv.io/installation/) [!badge Recommended] - [Installation and configuration tutorial](/tutorials/mpv)
- [MPC-HC](https://github.com/clsid2/mpc-hc/releases) - [Configuration guide with madVR](https://kokomins.wordpress.com/2021/03/27/mpc-hc-and-madvr-setup-guide/)
- [Potplayer](https://potplayer.daum.net)

+++ Android :robot_face:

- [mpv-android](https://play.google.com/store/apps/details?id=is.xyz.mpv) [!badge Recommended]
- [VLC for Android](https://play.google.com/store/apps/details?id=org.videolan.vlc)

+++ iOS :green_apple:

- [Outplayer](https://apps.apple.com/app/outplayer/id1449923287)
- [VLC media player](https://apps.apple.com/app/vlc-media-player/id650377962)

+++ TV/Media Servers :tv:

- [Kodi](https://kodi.tv) [!badge Recommended] - *Can be installed optionally through [LibreELEC OS](https://libreelec.tv).*
- [Plex](https://www.plex.tv) [!badge Recommended]
- [Emby](https://emby.media)
- [Jellyfin](https://jellyfin.org)

+++

!!!warning
VLC is not recommended as it introduces visual artifacts, displays wrong colors, and breaks intensive subtitles. We suggest using alternative media players.
*See the example comparisons to mpv: [Spice and Wolf](https://slow.pics/c/XhbmrYgU), [One Piece](https://slow.pics/c/FW2nBwKP)*
!!!

## Media Servers

The setup consists of two parts: the **server** and the **client**. *Both may be installed on the same system, but they are separate applications.*

- The **client** is a media player that will access the content stored on the server.
- The **server** runs on the device which hosts your content.

In a typical setup, the server is installed on a computer hosted on your home network, with the client being installed on all your devices. Most media players will also come with their own client, as well as including support for using [Kodi](https://kodi.tv) as a client (recommended for anime).

[!button variant="info" text="Emby" margin="0 8 0 0"](https://emby.media)
[!button variant="info" text="Plex" margin="0 8 0 0"](https://www.plex.tv)
[!button variant="info" text="Jellyfin"](https://jellyfin.org)

!!!
Running a media server requires a rigid folder structure and a specific file naming scheme.
*See the guides for your server: [Plex](https://support.plex.tv/articles/naming-and-organizing-your-movie-media-files/), [Jellyfin](https://jellyfin.org/docs/general/server/media/shows)*
!!!

### Kodi

#### Installing Kodi

There are three ways of installing and configuring Kodi for your TV:

- **Basic:** Kodi client on your computer, with content also stored on your computer.
- **Intermediate:** Kodi client on a TV/Android Box, with content stored on your computer.
- Com**Plex:** Kodi client on a TV/Android Box, with content stored on a media server.

+++ Basic

- No decoding problems with a powerful CPU.
- The ability to use high-quality shaders that utilize your GPU to improve upscaling with [an external player of your choice](https://kodi.wiki/view/External_players).
- Control using a normal keyboard and mouse or through an Android app like [Yatse](https://yatse.tv).

+++ Advanced

- Works with a simple [SMB share](https://kodi.wiki/view/SMB) from your computer on the same network.

+++ Com**Plex**

- Transcoding support for videos.
- The ability to remotely stream content from devices outside your local network.

Transcoding is used as a last resort to deal with compatibility problems. *Direct play is always preferable to transcoding, which affects quality and uses CPU power on your server. See [Plex's article about transcoding](https://support.plex.tv/articles/200250387-streaming-media-direct-play-and-direct-stream).*

Here, Kodi acts as the client, increasing compatibility and replacing the default one that comes with Emby, Plex, or Jellyfin. This is done through their respective add-ons:

- [Emby for Kodi](https://github.com/MediaBrowser/plugin.video.emby)
- [PlexKodiConnect](https://github.com/croneter/PlexKodiConnect)
- [Jellyfin for Kodi](https://github.com/jellyfin/jellyfin-kodi)

+++

#### Scanning Files

*See [Kodi's documentation on scanning files without renaming them](https://kodi.wiki/view/Anime#Scanning_files_without_renaming_them).*

## Settings

### Scaling

Scaling is the process of taking content that does not match your screen resolution and scaling it to fit your display.

- Downscaling takes a *large* video and scales *downwards* (e.g. 1080p video to 720p display)
- Upscaling takes a *small* video and scales *upwards* (e.g. 1080p video to 2160p/4K display)

For displays that match the content resolution you will only see chroma scaling, as in most video the chroma resolution is half the video resolution. *These shaders only activate when these resolutions don't match. Scaling is not an enhancement and cannot be enabled manually.*

[mpv](https://mpv.io) has a built-in high-quality profile called `gpu-hq` which enables better upscaling algorithms (`scale=spline36`, `cscale=spline36`, `dscale=mitchell`). By default, mpv uses `spline36`. *This option is necessary to enable even if you use an external shader, as it can act as a fallback.*

!!!
`gpu-hq` enables debanding by default, which is not recommended for high-quality sources. It should be followed by `deband=no`.
!!!

#### Recommended Shaders

Below is a brief list of recommended shaders for [mpv](https://mpv.io):

+++ High-End PCs
For those with high-end hardware, we recommend using [nnedi3-nns256-win8x4](https://github.com/bjin/mpv-prescalers/blob/master/nnedi3-nns256-win8x4.hook).

+++ Mid-Range PCs
For those with mid-range hardware, we recommend using [nnedi3-nns128-win8x4](https://github.com/bjin/mpv-prescalers/blob/master/nnedi3-nns128-win8x4.hook).

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

```properties
glsl-shader="~~/shaders/<name>"
```

- Confirm your shader is working by pressing *Shift* + *I*, followed by *2*.
- Watch for dropped frames or high frame times, as they can be a sign that your GPU is unable to keep up with your shader. *If this applies to you, we suggest switching to a less-demanding shader.*

===

### Filtering

Filters act as a temporary fix to amend issues that may appear when watching anime. These can be applied using the options available in [mpv](https://mpv.io) and [madVR](https://madvr.com).

Debanding is the most commonly used filter, which helps to fix issues with [color banding](/tutorials/mpv/#debanding) in your video.

!!!warning
We do not recommend detail enhancement, noise reduction, or sharpening filters, as it will negatively affect the quality of your content.
!!!

*See the [Quality Guide](/guides/quality) for more information.*

### Smooth Playback

Most modern anime will play close to 24 fps (24000/1001 fps). However, most displays often run at refresh rates that do not match this frame rate, leading to an effect known as **judder**.

Judder is most commonly seen with devices that have 60Hz displays, *as the refresh rate is not an integer multiple of the content frame rate.*

==- Explaining Judder
A 60Hz monitor can refresh up to 60 frames every second. Thus, if you were to play a video or game locked at:

- 60 fps: 1 frame is shown for *every 1 refresh* (next frame in 16.67 ms).
- 30 fps: 1 frame is shown for *every 2 refreshes* (next frame in 33.33 ms).
- 15 fps: 1 frame is shown for *every 4 refreshes* (next frame in 66.67 ms).

These frame rates work because they divide evenly into the monitor's 60Hz refresh rate. With 60 fps video, for instance, each frame is shown per refresh on a 60Hz monitor, with the next frame always appearing 16.67 ms later than the first.

When content plays at 24 fps, your monitor will need to show 1 frame for every 2.5 refreshes (60Hz divided by 24 fps). *However, refreshes cannot be divided, and you cannot have "part" of a refresh show on your monitor without screen tearing.*

Instead, your monitor will try to refresh 2.5 times like this:

- Frame 1 is shown for *2 refreshes* (next frame in 33.33 ms).
- Frame 2 is shown for *3 refreshes* (next frame in 50 ms).
- Frame 3 is shown for *2 refreshes* (next frame in 33.33 ms).
- Frame 4 is shown for *3 refreshes* (next frame in 50 ms).

Every odd frame appears for *2 refreshes*, and every even frame appears for *3 refreshes*.

This process is repeated for the entire video. Because each frame is displayed for a different number of refreshes, plus the time between changing frames is not the same, *it leads to motion appearing stuttery or laggy. This is especially noticeable during panning scenes.*

===

To avoid judder, it is best to try and match your display's refresh rate with the content frame rate via the following methods:

+++ High Refresh Rate
Displays that run at 120Hz, 144Hz, 240Hz, or 360Hz will match with each frame, as they are all multiples of 24. Technically, they should be set at 119.88Hz, 143.86Hz, 239.76Hz, or 359.64Hz to perfectly match with the majority of content at 23.976 fps (24000/1001 fps).

*144Hz displays will not display 30/60 fps content properly. Additionally, none of the above will handle 25 fps content correctly.*

+++ Adaptive Sync
Adaptive sync, also known as variable refresh rate, is a technology that dynamically adjusts your display's refresh rate based on the frame rate of the content on your screen.

Most modern GPUs will have adaptive sync functionality, such as through AMD FreeSync or NVIDIA G-SYNC.

The best solution is to use G-SYNC/G-SYNC Compatible or FreeSync Premium. *Normal FreeSync may work if your monitor supports [Low Framerate Compensation (LFC)](https://www.amd.com/en/technologies/free-sync-faq#faq-What-is-Low-Framerate-Compensation?). You will also need to use a media player that supports adaptive sync, such as [mpv](https://mpv.io).*

Additionally, you may need to force exclusive fullscreen to activate adaptive sync. In mpv, this can be done by adding `ontop` & `fullscreen` to your `mpv.conf` file. You can tell adaptive sync is active when your cursor feels laggy, as this means your display has dropped its refresh rate to match the content.

+++ Automatic Refresh Rate Adjustment
Many modern streaming devices (e.g. Amazon Fire TV, Apple TV, NVIDIA SHIELD, etc.) will have the option to change the TV refresh rate to match the content frame rate, either through the device's settings or a setting in the playback software (Kodi, Plex, etc.)

24/30/60 fps content should all work perfectly. 25 fps content requires 25/50Hz support, *which some TVs in [NTSC regions](https://upload.wikimedia.org/wikipedia/commons/thumb/0/0d/PAL-NTSC-SECAM.svg/2560px-PAL-NTSC-SECAM.svg.png) do not support.*

![NTSC Regions](https://i.imgur.com/Xb0Frlb.png)

+++

## Compatibility

### Codecs

Decoding is the process of deciphering the encoded video into a format that your device can display.

There are two ways to handle video decoding:

- **Software decoding** uses your CPU, allowing your system to decode any format, and is only limited by how powerful your chip is.
- **Hardware decoding** uses dedicated decoders on your GPU, making it vastly more efficient and resulting in better performance, lower power usage, and less strain on your CPU. *However, some older devices will not support this option for newer codecs.*

For most users, this isn't an issue. Generally:

- H.264 8-bit (AVC) works everywhere.
- H.264 10-bit (Hi10P) works on some hardware. *Recommended to use a decent CPU. Hardware decoding support is suboptimal.*
- H.265 8-bit/10-bit (HEVC) works with most modern hardware.

Most TVs and boxes will display a list of supported codecs on their specification page.

[Plex](https://www.plex.tv) and other media servers will recognize unsupported formats and start transcoding to a supported format, such as H.264.

[Kodi](https://kodi.tv) can fallback to software decoding like on PC media players, giving you the option to directly stream from your server or run without it. *However, since the CPU on your TV/Android box is much weaker, there may be instances of lag when forcing software decoding. On top of the strain from decoding, any additional CPU activity may further affect your experience, such as rendering complicated `.ass` typesetting.*

### Typesetting

Almost all subbed anime will use `.ass` subtitles, as they allow for extensive styling and typesetting. This comes at the cost of additional system resources, though most modern computers, phones, and media servers will not have an issue.

While subtitles will display on most TVs/media players, the typesetting or overlapping dialogue can sometimes be broken when using certain applications. *This is especially a problem with fansubs, where typesetting is used extensively in various areas (e.g. text on a sign, moving scenes).*

!!!
Most TVs will have difficulty showing `.ass` subs. You should use an external box, such as Amazon Fire TV, Apple TV, or NVIDIA SHIELD.
!!!
