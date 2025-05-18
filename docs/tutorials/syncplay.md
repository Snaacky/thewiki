---
label: SyncPlay
description: Configuring SyncPlay for Group Watch Events
image: /static/tutorials/syncplay.png
author:
  name: "guyman"
  avatar: https://github.com/Snaacky/thewiki/assets/78981416/2045b3c7-90a5-40ce-9d81-a81baa421227
---

# SyncPlay for Group Watch Events (Windows)

Quick tutorial to get SyncPlay set up for Snackbox Group Watch Events

## Required

1. [SyncPlay](https://syncplay.pl/syncplay-1-7-4/) - download the Windows Installer
2. [Visual C++ Redist](https://aka.ms/vs/17/release/vc_redist.x86.exe) - you need the x86 version, not the x64 version
3. [mpv media player](https://thewiki.moe/tutorials/mpv/) - option 4 is a quick method
4. `.mkv` of the release being group watched - a different encode of the same source should work

## Steps

1. Download SyncPlay Windows Installer and install it.
2. Download and install Visual C++ Redist.
3. Download and unzip mpv to a known location. You may skip this step if mpv is already installed.
4. Run SyncPlay from the Start Menu.

![Example](/static/tutorials/syncplay_example.png)

This is what you should see on your first startup.

 - In box 1, click the dropdown arrow on the far right. This will usually be announced alongside the name of the room.
 - In box 2, input a username. 
 - In box 3, input the room name. This will usually be announced alongside the server to select.
 - In box 4, you need to click browse and select your copy of `mpv.exe`. This is wherever you the extracted the `.zip`. 

!!!
If you installed mpv using scoop, `mpv.exe` is located in `%USERPROFILE%\scoop\apps\mpv\current\mpv.exe`
!!!

5. Click the button labeled 5 ("Store configuration and run Syncplay") to connect to the server.
6. Drag the `.mkv` file from File Explorer into the newly created mpv window. Closing either window will disconnect you from the session.

**Boxes 1 and 3 are subject to change per event, keep an eye on the announcements.**

