---
label: Playback
order: -2
---

# Playback

## Video players

#### PC

- [MPV](https://mpv.io/installation/) - [Installation and configuration tutorial](/tutorials/mpv) (recommended)
- [MPC-HC](https://github.com/clsid2/mpc-hc/releases)/[Potplayer](https://potplayer.daum.net) - [Configuration guide with MadVR](https://kokomins.wordpress.com/2021/03/27/mpc-hc-and-madvr-setup-guide/)

VLC is not recommended because it often displays wrong colours, introduces visual artifacts, and breaks intensive subtitles. Here are some comparisons showing these issues -

https://slow.pics/c/XhbmrYgU (Spice and Wolf by MTBB)
https://slow.pics/c/vH560pvp

#### Android

- [mpv-android](https://play.google.com/store/apps/details?id=is.xyz.mpv&hl=lv&gl=US)
- [MX player](https://play.google.com/store/apps/details?id=com.mxtech.videoplayer.ad&hl=lv&gl=US)

#### iOS

- [MX player](https://apps.apple.com/in/app/mx-player/id1429703801)
- [Outplayer](https://apps.apple.com/us/app/outplayer/id1449923287)

#### TV

- [Kodi](https://kodi.tv), optionally through [LibreElec(Minimal OS)](https://libreelec.tv)

There are three ways of using Kodi on a TV -

**1. Kodi installed on a computer which stores the content and is directly connected to the TV.**

- No decoding problems with a powerful cpu.
- Possibility to use high quality shaders that utilize your gpu to improve upscaling with [an external player of your choice.](https://kodi.wiki/view/External_players)
- Control using a normal mouse+keyboard or through an android app like [Yatse.](https://yatse.tv/)

**2. Kodi on a TV/Android box with the content on a separate computer.**

- Works with a simple [SMB share](https://kodi.wiki/view/SMB) from your computer on the same network.

**3. Kodi on a TV/Android box with the content on a separate computer running a media server.**

- Transcoding support
- Remote streaming support outside your local network

Transcoding is only a last resort to deal with compatibility problems. Direct play is always prefereble to transcoding, which affects quality and uses cpu power on your server. More about transcoding on plex - https://support.plex.tv/articles/200250387-streaming-media-direct-play-and-direct-stream/

Here, Kodi acts as a client,increasing compatibility and replacing the default one that comes with Plex, Emby or Jellyfin. This is done through their respective add-ons for kodi.

- [PlexKodiConnect](https://github.com/croneter/PlexKodiConnect)
- [Emby for Kodi](https://github.com/MediaBrowser/plugin.video.emby)
- [Jellyfin for Kodi](https://github.com/jellyfin/jellyfin-kodi)

## Media Server

The setup consists of two parts - the server and the client. A client is simply a media player that will access the content from a server. The server runs on the device which stores your content. Both may be installed on the same system, but they're separate applications. Usually you'd have the server on a computer connected to other devices in your home network and a client installed on all of those devices. All three of the popular ones come with their own client/player as well as support for using kodi as a client (recommended for anime).

- [Plex](https://www.plex.tv/) - [Guide for Anime](https://docs.google.com/document/d/1sXKZDYzbBDDWS8eqJ3IcaxSWhYKIPDdtChm74CBJ6ig)
- [Emby](https://emby.media/)
- [Jellyfin](https://jellyfin.org/)

### Scanning anime without renaming

- https://kodi.wiki/view/Anime#Scanning_files_without_renaming_them
- [Absolute Series Scanner for Plex](https://github.com/ZeroQI/Hama.bundle)

## Scaling

1. Quality is not the same as resolution.
2. Whenever something with a different resolution than your display is playing on it in full screen mode, then something is scaling it to the resolution of the display.

This something is usually the video player. For example, playing a 1080p video on a 2160p monitor means that it has to be upscaled to fill the screen, otherwise it'd simply play in 1/4th of the screen. Upscaling is not a choice that you can enable or disable while in full screen, the only thing that can be changed is the method of scaling. Consequently, upscaling is not something you can use to "enhance" the video when playing back at the same resolution. It is not a way to somehow improve 1080p video playing on a 1080p display. The shaders activate only when the video resolution does not match the screen resolution and scaling is needed.

The default scaling on most players is bad. Mpv has a built in high quality profile called gpu-hq which enables better upscaling algorithms (scale=spline36, cscale=spline36, dscale=mitchell). This option is necessary even if you use external shaders to act as a fallback. Any scaling options explicitly specified after this will override it. For those with powerful gpus, even higher quality external shaders are available - FSRCNNX, NNEDI etc. The file has to be placed in %appdata%/mpv/shaders and the line `glsl-shader="~~/FSRCNNX_x2_8-0-4-1.glsl"` has to be added to the config. Press shift+I followed by 2 for confirmation that the shader is working. Dropped frames or high frame times (above 25ms) are a signal that your gpu isn't able to keep up and you should switch to a less demanding shader.

The shaders mentioned above are 2x scalers, which means that they always scale by 2x. For 720p to 1080p, the video will first be scaled to 1440p by FSRCNNX and then downscaled to 1080p by dscale=mitchell. For 720p to 2160p, FSRCNNX will do 720p to 1440p and then scale=spline36 will scale it the rest of the way to 2160p. When scaling below a certain threshold is required, FSRCNNX will not activate and mpv will fallback to spline36. There are also scalers like ravu-zoom can upscale to arbitrary ratios, at the cost of slow performance because of rendering to the target resolution directly.

Ravu and NNEDI - https://github.com/bjin/mpv-prescalers
FSRCNNX - https://github.com/igv/FSRCNN-TensorFlow/releases

## Filtering

The same kind of filtering that is used by encoders but in real time. It's not an alternative to a good encode, but rather a temporary fix for web sources. Debanding is the most commonly used and the only one necessary. Detail enhancement, noise reduction, sharpening etc. are not recommended. All these options are available with both mpv and madvr. More about quality, video artifacts and encoding in the [quality guide](/guides/quality).

## Smooth Playback

**The problem:** Almost all anime plays back at 23.976fps (technically 24000/1001) but we'll round up to 24fps for convenience. However, displays often run at refresh rates that do not match this frame rate, leading to an effect known as judder.
Judder is most commonly seen with laptops/desktops that have 60hz displays, because the refresh rate is not an integer multiple of the content frame rate. 60hz/24fps is 2.5, and since you can't refresh 2.5 times per frame, it has to be averaged out over time. In this case the first frame will be held for 2 refreshes, the second frame for 3 refreshes, the third for 2 and so on leading to the 2.5 average. Because of this, each frame is shown for a different amount of time (33.3ms for even frames, 50ms for odd) which leads to motion appearing stuttery/laggy, this is especially noticeable during panning scenes.
While almost all anime is 24fps, you also may watch other content such as live action (sometimes 25fps) and YouTube (mostly 30/60fps, sometimes 25/50fps) and if you do, it's important to be able to play those frame rates back properly.

**The solution:** Matching the display refresh rate to the content frame rate, of which there are many methods

**Adaptive sync:** G-Sync/G-Sync Compatible (Certified)/Freesync Premium is by far the best solution, however requires the correct GPU and monitor to utilise. The display will perfectly match the content regardless of the frame rate. Normal Freesync may work, however only if the monitor supports LFC.
Be sure to use a player that will activate adaptive sync, such as MPV.

**Automatic Refresh Rate Adjustment:** Many streaming devices (Apple TV, Shield, Fire stick etc) will have the ability to change a TVs refresh rate to match the content frame rate, either through the device settings itself or a setting in the playback software (Kodi, Plex etc). 24/30/60fps content should all work perfectly, however 25fps content requires 25/50hz support, which some TVs in [NTSC regions](https://upload.wikimedia.org/wikipedia/commons/thumb/0/0d/PAL-NTSC-SECAM.svg/2560px-PAL-NTSC-SECAM.svg.png) may not support.

**High refresh rate displays:** 120hz, 144hz, 240hz, and 360hz are all multiples of 24, and as such will match perfectly since each frame can be repeated for multiple refreshes. However 144hz will not display 30/60fps content properly, and none of the above will handle 25fps content correctly.

## Compatibility

Decoding is the process of playing the video file. There are two types - hardware and software. Everything can be decoded, the difference is in the amount of work needed to do that. When it's hardware accelerated, it doesn't put any strain on your device cpu.

Most PC players will decode everything with swdecode as a fallback option. The problem is when this decoding isn't fast enough to play your video in real time. That won't ever be a problem is hwdecode is supported for that codec.

If you're watching on a TV/+Android box, you'll see a list of supported codecs in their specifications. Plex and other media servers will recognize a format as not supported and start transcoding to h264 8 bit. Kodi player is the better option it'll fallback to software decoding like on a PC, giving you the option to always direct stream or run without a server. However, since the cpu on your tv/box is much weaker compared to a PC, there will be instances of horrible lag when trying to force swdecode. Note that software decoding utilizes your cpu, so the cause of lag isn't limited to decoding. It's anything that might be using your cpu at the time. Like when there's a lot of complicated .ass typesetting which needs to be rendered, that will lag with a weak cpu no matter what the video codec is. Generally -

- h264 8-bit works everywhere
- h264 10-bit rarely works without a decent cpu. Hardware decode support for this is non-existent.
- h265 8-bit and 10-bit work when hardware support exists. Most newer devices in the last 5 years support it.

### Typesetting

Anime usually has .ass subtitle styling, while the subs will show up on most TVs and players (plex, emby, jellyfin etc.), the typesetting or overlapping dialogue is sometimes broken. This is especially a problem if you're watching fansubs. **Kodi** is one of the few players which supports proper ass rendering. Note that the player is separate from the kodi media server and can even be used with plex.
