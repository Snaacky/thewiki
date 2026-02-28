---
label: Plex
description: Modify the Plex Windows app to include an extended MPV version
author:
  name: "! ThaUnknown_#3951"
  link: http://github.com/ThaUnknown
  avatar: https://avatars.githubusercontent.com/u/6506529
---
# Plex MPV Mod

## Why Modify The Player?

You get:
- Less buffering
- Better quality
- Dolby Vision support without Plex pass
- HDR10+ support without Plex pass
- No transcoding

# How to Modify the MPV Player

Be sure to close the Plex App before applying the modifications.

- Go to https://sourceforge.net/projects/mpv-player-windows/files/libmpv/
  - Click the modified column to sort by newest.
  - Download `mpv-dev-x86_64-v3-{date}-git-{hash}.7z`, where `date` is the most recent date in `YYYYMMDD` format and `hash` is some random hash.
- Extract the zip to somewhere, and copy the `libmpv-2.dll` file.
- Go to plex install location (by default `C:\Program Files\Plex\Plex`, though it can vary).
  - If inside the Plex folder `libmpv-2.dll` exists paste the copied `libmpv-2.dll` and overwrite it. If it doesn't paste it and rename it to `mpv-2.dll`. It should be ~80MB in constrast to the original's 10MB.
- Go to `%localappdata%\plex` (by default `C:\Users\YourUserName\AppData\Local\Plex`, where `YourUserName` is your account name. Again, it can vary).
  - Create `mpv.conf` file
  - Paste this into the file:
```
profile=high-quality
tls-ca-file={Plex}\resources\cacert.pem
```
where `{Plex}` is your plex install path, so it'll most likely look something like `tls-ca-file=C:\Program Files\Plex\Plex\resources\cacert.pem` if you never changed it.

`C:\Users\YourUserName\AppData\Local\Plex` is the root folder of the MPV config so you can put your own MPV config in there, do note that Plex overwrites most keybinds tho. For more details on MPV configs [visit the tutorial](https://thewiki.moe/tutorials/mpv/).

- Open Plex and go to 🔧 Settings and change these settings:
  - Quality:
    - Internet Streaming > Video Quality: Maximum
    - Home Streaming > Video Quality: Maximum (You may need to uncheck Use recommended settings first)
  - Player: Show Advanced >
    - Normalize Multi-chanel Audio: ☐ Unchecked
    - Exclusive Audio: ☐ Unchecked
    - Video Playback Quality: Make My GPU Hurt
    - Use Hardware Decoding: ☑ Checked
    - Enable HDR Switching: ☑ Checked (This setting might not show up for you.)
  - Debug: Show Advanced >
    - Direct Play: ☑ Checked
    - Direct Stream: ☑ Checked

!!!
This works Identical for Plex and Plex HTPC, though it seems like HTPC has better HDR support than the normal Plex app. If you want to do this for the HTPC app just substitute all Plex file paths with Plex HTPC.
!!!

!!!warning
For full HDR support you might need target-colorspace-hint=yes inside the mpv.conf file depending on your HDR display. It also might impact the looks of non-HDR content; test yourself.
!!!
