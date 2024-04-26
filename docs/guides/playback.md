---
label: Playback
order: -2
description: Learn how to play your anime
image: /static/tohsaka.gif
---

# Playback

## Media Players

+++ :desktop_computer: PC

- [mpv](https://mpv.io/installation/) [!badge icon=":heart:" variant="primary" text="Recommended"] [!badge icon="sliders" variant="info" text="Setup Guide"](/tutorials/mpv)

!!!warning
VLC is not recommended as it introduces visual artifacts, displays wrong colors, and breaks intensive subtitles. We suggest using alternative media players.
*See the example comparisons to mpv: [Spice and Wolf](https://slow.pics/c/XhbmrYgU), [One Piece](https://slow.pics/c/FW2nBwKP)*
!!!

+++ :robot_face: Android

- [Kodi](https://play.google.com/store/apps/details?id=org.xbmc.kodi) [!badge icon="video" variant="info" text="Hi10P Support"]
- [mpv-android](https://play.google.com/store/apps/details?id=is.xyz.mpv)
- [VLC for Android](https://play.google.com/store/apps/details?id=org.videolan.vlc)

+++ :green_apple: iOS

- [Outplayer](https://apps.apple.com/app/outplayer/id1449923287)
- [VLC media player](https://apps.apple.com/app/vlc-media-player/id650377962)

+++ :tv: TV/Media Servers

- [Kodi](https://kodi.tv) [!badge icon=":heart:" variant="primary" text="Recommended"] [!badge icon="link-external" variant="info" text="Available on LibreELEC OS"](https://libreelec.tv)
- [Plex](https://www.plex.tv) [!badge icon=":heart:" variant="primary" text="Recommended"]
- [Emby](https://emby.media)
- [Jellyfin](https://jellyfin.org)

+++

## Media Servers

The setup consists of two parts: the **server** and the **client**. *Both may be installed on the same system, but they are separate applications:*

- The **client** is a media player that will access the content stored on the server
- The **server** runs on the device which hosts your content

In a typical setup, the server is installed on a computer hosted on your home network, with the client being installed on all your devices. Most media players will also come with their own client, as well as including support for using [Kodi](https://kodi.tv) as a client (recommended for anime).

[!button variant="secondary" text="Emby" margin="0 8 0 0"](https://emby.media)
[!button variant="secondary" text="Plex" margin="0 8 0 0"](https://www.plex.tv)
[!button variant="secondary" text="Jellyfin"](https://jellyfin.org)

!!!
Running a media server requires a rigid folder structure and a specific file naming scheme.
*See the guides for your server: [Plex](https://support.plex.tv/articles/naming-and-organizing-your-movie-media-files/), [Jellyfin](https://jellyfin.org/docs/general/server/media/shows)*
!!!

### Kodi

#### Installing Kodi

There are three ways of installing and configuring Kodi for your TV:

- **Basic:** Kodi client on your computer, with content also stored on your computer
- **Intermediate:** Kodi client on a TV/Android Box, with content stored on your computer
- **Advanced:** Kodi client on a TV/Android Box, with content stored on a media server

+++ Basic

- No decoding problems with a powerful CPU
- The ability to use high-quality shaders that utilize your GPU to improve upscaling with [an external player of your choice](https://kodi.wiki/view/External_players)
- Control using a normal keyboard and mouse or through an Android app like [Yatse](https://yatse.tv)
- Your files will be scanned by Kodi. *See [Kodi's documentation on scanning files without renaming them](https://kodi.wiki/view/Anime#Scanning_files_without_renaming_them)*

+++ Intermediate

- Works with a simple [SMB share](https://kodi.wiki/view/SMB) from your computer on the same network
- Your files will be scanned by Kodi. *See [Kodi's documentation on scanning files without renaming them](https://kodi.wiki/view/Anime#Scanning_files_without_renaming_them)*

+++ Advanced

- Transcoding support for videos
- The ability to remotely stream content from devices outside your local network
- Your files will be scanned by your media server, abiding by your server's folder structure and naming scheme

Transcoding is used as a last resort to deal with compatibility problems. *Direct play is always preferable to transcoding, which affects quality and uses CPU power on your server. See [Plex's article about transcoding](https://support.plex.tv/articles/200250387-streaming-media-direct-play-and-direct-stream).*

Here, Kodi acts as the client, increasing compatibility and replacing the default one that comes with Emby, Plex, or Jellyfin. This is done through their respective add-ons:

- [Emby for Kodi](https://github.com/MediaBrowser/plugin.video.emby)
- [PlexKodiConnect](https://github.com/croneter/PlexKodiConnect)
- [Jellyfin for Kodi](https://github.com/jellyfin/jellyfin-kodi)

+++

## Settings

### Scaling

Scaling is the process of taking content that does not match your screen resolution and resizing it to fit your display.

- Downscaling takes a *large* video and scales *downwards* (e.g. 1080p video to 720p display)
- Upscaling takes a *small* video and scales *upwards* (e.g. 1080p video to 2160p/4K display)

For displays that match the content resolution you will only see chroma scaling, as in most video the chroma resolution is half the video resolution. *These shaders only activate when these resolutions don't match. Scaling is not an enhancement and cannot be enabled manually.*

If you use [mpv](https://mpv.io), see the [setup guide](/tutorials/mpv/#scaling) on setting up scalers.

### Filtering

Filters act as a temporary fix to amend issues that may appear when watching anime. These can be applied using the options available in [mpv](https://mpv.io) and [madVR](https://madvr.com).

Debanding is the most commonly used filter, which helps to fix issues with [color banding](/tutorials/mpv/#debanding) in your video.

!!!warning
We do not recommend using detail enhancement, noise reduction, or sharpening filters, as it can negatively affect the quality of your content.
!!!

*See the [quality guide](/guides/quality) for more information.*

### Smooth Playback

Most modern anime will play close to 24 fps (24000/1001 fps). However, most displays often run at refresh rates that do not match this frame rate, leading to an effect known as **judder**.

Judder is most commonly seen with devices that have 60Hz displays, *as the refresh rate is not an integer multiple of the content frame rate.*

==- :icon-info: Explaining judder
A 60Hz monitor can refresh up to 60 frames every second. Thus, if you were to play a video or game locked at:

- 60 fps: 1 frame is shown for *every 1 refresh* (next frame in 16.67 ms)
- 30 fps: 1 frame is shown for *every 2 refreshes* (next frame in 33.33 ms)
- 15 fps: 1 frame is shown for *every 4 refreshes* (next frame in 66.67 ms)

![Perfect frame rates](/static/playback/smooth_playback.png)

These frame rates work because they divide evenly into the monitor's 60Hz refresh rate. Every frame shows for the same amount of time; the next frame in a 60 fps video *always* appears 16.67 ms later than the first on a 60Hz monitor.

When content plays at exactly 24 fps, your monitor will need to show 1 frame for every 2.5 refreshes (60Hz divided by 24 fps):

![Imperfect frame rate](/static/playback/smooth_playback_2.png)

However, refreshes cannot be divided. A single refresh is limited to a single frame, therefore, you cannot have half of a frame in a single refresh. Instead, your monitor will try to refresh 2.5 times like this:

- Frame 1 is shown for *2 refreshes* (next frame in 33.33 ms)
- Frame 2 is shown for *3 refreshes* (next frame in 50 ms)
- Frame 3 is shown for *2 refreshes* (next frame in 33.33 ms)
- Frame 4 is shown for *3 refreshes* (next frame in 50 ms)

![Frame rate compensation](/static/playback/smooth_playback_3.png)

Every odd frame appears for *2 refreshes*, and every even frame appears for *3 refreshes*.

This process is repeated for the entire video. Because each frame is displayed for a different number of refreshes (i.e. the time between changing frames is not the same), *it leads to motion appearing stuttery or laggy. This is especially noticeable during panning scenes.*

===

To avoid judder, it is best to try and match your display's refresh rate with the content frame rate via the following methods:

+++ High Refresh Rate
Displays that run at 120Hz, 144Hz, 240Hz, or 360Hz will match with each frame, as they are all multiples of 24. Technically, they should be set at 119.88Hz, 143.86Hz, 239.76Hz, or 359.64Hz to closely match with the majority of content at around 23.976 fps (24000/1001 fps).

*144Hz displays will not display 30/60 fps content properly. Additionally, none of the above will handle 25 fps content correctly.*

+++ Adaptive Sync
Adaptive sync, also known as variable refresh rate, is a technology that dynamically adjusts your display's refresh rate based on the frame rate of the content on your screen.

Most modern GPUs will have adaptive sync functionality, such as through AMD FreeSync or NVIDIA G-SYNC.

The best solution is to use G-SYNC/G-SYNC Compatible or FreeSync Premium. *Normal FreeSync may work if your monitor supports [Low Framerate Compensation (LFC)](https://www.amd.com/en/technologies/free-sync-faq#faq-What-is-Low-Framerate-Compensation?). You will also need to use a media player that supports adaptive sync, such as [mpv](https://mpv.io).*

Additionally, you may need to force exclusive fullscreen to activate adaptive sync. In mpv, this can be done by adding `ontop` & `fullscreen` to your `mpv.conf` file. You can tell adaptive sync is active when your cursor feels laggy, as this means your display has dropped its refresh rate to match the content.

+++ Automatic Refresh Rate Adjustment
Many modern streaming devices (e.g. Amazon Fire TV, Apple TV, NVIDIA SHIELD, etc.) will have the option to change the TV refresh rate to match the content frame rate, either through the device's settings or a setting in the playback software (Kodi, Plex, etc.)

24/30/60 fps content should all work perfectly. 25 fps content requires 25/50Hz support, *which some TVs in [NTSC regions](https://upload.wikimedia.org/wikipedia/commons/thumb/0/0d/PAL-NTSC-SECAM.svg/2560px-PAL-NTSC-SECAM.svg.png) do not support.*

![NTSC Regions](/static/ntsc-regions.png)

+++

## Compatibility

### Codecs

Decoding is the process of deciphering the encoded video into a format that your device can display.

There are two ways to handle video decoding:

- **Software decoding** uses your CPU, allowing your system to decode any format, and is only limited by how powerful your chip is
- **Hardware decoding** uses dedicated decoders on your GPU, making it vastly more efficient and resulting in better performance, lower power usage, and less strain on your CPU. *However, some older devices will not support this option for newer codecs*

For most users, this isn't an issue. Generally:

- H.264 8-bit (AVC) works everywhere
- H.264 10-bit (Hi10P) works on some hardware. *Recommended to use a decent CPU; hardware decoding support is suboptimal*
- H.265 8-bit/10-bit (HEVC) works with most modern hardware

Most TVs and boxes will display a list of supported codecs on their specification page.

[Plex](https://www.plex.tv) and other media servers will recognize unsupported formats and start transcoding to a supported format, such as H.264.

[Kodi](https://kodi.tv) can fallback to software decoding like on PC media players, giving you the option to directly stream from your server or run without it. *However, since the CPU on your TV/Android box is much weaker, there may be instances of lag when forcing software decoding. On top of the strain from decoding, any additional CPU activity may further affect your experience, such as rendering complicated `.ass` typesetting.*

### Typesetting

Almost all subbed anime will use `.ass` subtitles, as they allow for extensive styling and typesetting. This comes at the cost of additional system resources, though most modern computers, phones, and media servers will not have an issue.

While subtitles will display on most TVs/media players, the typesetting or overlapping dialogue can sometimes be broken when using certain applications. *This is especially a problem with fansubs, where typesetting is used extensively in various areas (e.g. text on a sign, moving scenes).*

!!!
Most TVs will have difficulty showing `.ass` subs. You should use an external box, such as Amazon Fire TV, Apple TV, or NVIDIA SHIELD.
!!!
