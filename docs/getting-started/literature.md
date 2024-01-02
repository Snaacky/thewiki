---
label: Literature
order: -3
description: Learn How and Where to Read Your Favorite Stuff
image: https://user-images.githubusercontent.com/78981416/215575718-9d206b3c-4377-4bb4-baea-72516953c85f.gif
---

# Literature

## Manga

### Sourcing

+++ Downloads

[Nyaa](https://nyaa.si) [!badge icon=":heart:" variant="primary" text="Recommended"]
:   Public torrent tracker with the most scanlations and official digital rips

+++ Aggregators

[MangaDex](https://mangadex.org)
:   Browser aggregator with the most convenient scanlations

[MangaLife](https://manga4life.com)
:   Browser aggregator with the most convenient official rips

[MangaSee](https://mangasee123.com)
:   Browser aggregator with the most convenient official rips

+++

### Reading

+++ PC

[mpv](https://mpv.io/installation/) [!badge icon=":heart:" variant="primary" text="Best Quality"]
:   Standalone reader with the [highest quality scaling]() on PC

    Should be used with the [mpv-manga-reader script](https://github.com/Dudemanguy/mpv-manga-reader) to enable basic manga reader functionality, simply drag manga-reader.lua into your mpv scripts folder.

    ==- Best scaling settings
    To use scalers best suited to manga, add the following to your mpv.conf. This will not affect any settings related to video playback.
    ```
    [Manga]
    profile-desc="Read Manga"
    profile-cond=filename and filename:match('%.cbz$') or filename:match('%.cbr$') or filename:match('%.zip$') or filename:match('%.rar$') ~= nil
    profile=high-quality
    dscale=mitchell
    deband=no
    ```
    ==-
    ==- Fixing black levels
    mpv allows you to manually fix black levels with the brightness and contrast controls, add the following to your input.conf and customise the values to your liking
    ```
    B no-osd add contrast -4; no-osd add brightness 4; show-text "Black Level: ${brightness}"
    N no-osd add contrast 4; no-osd add brightness -4; show-text "Black Level: ${brightness}"
    ```
    This should only be used on sources where [obvious blacks appear gray](https://user-images.githubusercontent.com/78981416/233718453-c358222b-384e-45f5-9ff5-151aad32c94f.png).
    The Black Level value should be decreased [until they become black](https://user-images.githubusercontent.com/78981416/233718539-a670d966-9ab7-4f23-8abd-a1a8f2cb93f4.png). Generally -16 works for most sources.
    ==-
    ==- Touchscreen support
    Since mpv has extremely poor touchscreen functionality, it's recommended to use the third party program [GestureSign](gesturesign.win) which allows you to convert gestures to keypresses.
    It should be setup somewhat like [this example](https://i.imgur.com/kLFtLdr.png) to remove the need for a keyboard.  
    You will also want to add the following into your mpv.conf to avoid constantly triggering the OSD when swiping.
    ```
    no-osc
    no-window-dragging
    ```
    ==-

[CDisplayEx](https://www.cdisplayex.com) [!badge variant="secondary" text="Middle Ground"]
:   Standalone reader with the [second highest quality scaling](https://slow.pics/c/y737QBlP) on PC

    To get the best quality, set the [*Resizing Algorithm to Lanczos*](https://user-images.githubusercontent.com/78981416/233718226-57d36d09-fe1d-40bb-b1d6-415c66272d74.png) and the [*Lanczos quality slider to level 2*](https://user-images.githubusercontent.com/78981416/233718286-67f29d7f-53bf-47df-a41a-24fcd96a66f7.png).

    CDisplayEx features an [*Auto Colors*](https://user-images.githubusercontent.com/78981416/233718417-d994059b-b18d-4fa1-92cf-e2f7a51bd072.png) setting, which allows you to fix black levels. *This should only be used on sources where [obvious blacks appear gray](https://user-images.githubusercontent.com/78981416/233718453-c358222b-384e-45f5-9ff5-151aad32c94f.png). White level sensitivity should be increased slightly [until they become black](https://user-images.githubusercontent.com/78981416/233718539-a670d966-9ab7-4f23-8abd-a1a8f2cb93f4.png).*
    !!!warning
    For laptops or high-resolution displays, you may need to [disable Windows DPI scaling](https://user-images.githubusercontent.com/78981416/233718346-e66ff623-abcf-4c3a-8541-23fb840e65c9.png). Go to `C:\Program Files\CDisplayEx`, right-click on `CDisplayEx.exe` and select *Properties* -> *Compatibility* -> *Change high DPI settings.*
    !!!

[Tachidesk](https://github.com/Suwayomi/Tachidesk-Server) [!badge variant="secondary" text="Most Convenient"]
:   All-in-one manga reader with support for external sources and manga trackers. *Fork of [Tachiyomi](https://tachiyomi.org) for PC*

+++ Android

[Perfect Viewer](https://play.google.com/store/apps/details?id=com.rookiestudio.perfectviewer) [!badge icon=":heart:" variant="primary" text="Best Quality"]
:   Standalone reader with the [highest quality scaling](https://slow.pics/c/y737QBlP) on Android

    !!!primary
    To get the best quality, set the [*Image smooth filter to *Lanczos3*](https://user-images.githubusercontent.com/78981416/233718601-dbdd3303-d96a-474e-aed5-7d4c22a8e8da.png).
    !!!

[Tachiyomi](https://tachiyomi.org) [!badge variant="secondary" text="Most Convenient"] [!badge icon="link-external" variant="info" text="Forks"](https://tachiyomi.org/forks)
:   All-in-one manga reader with support for external sources and manga trackers

    !!!secondary
    We recommend enabling 32-bit color mode to [avoid banding](https://slow.pics/c/eOC7j5nI). This can be found under *Settings* -> *Reader*.
    !!!

+++ iOS

[Sidebooks](https://apps.apple.com/app/id409777225) [!badge icon=":heart:" variant="primary" text="Best Quality"]
:   Standalone reader with the [highest quality scaling](https://slow.pics/c/gUsyOomL) on iOS

[Aidoku](https://aidoku.app) [!badge variant="secondary" text="Most Convenient"]
:   All-in-one manga reader with support for external sources and manga trackers. *Requires TestFlight or sideloading*

[Paperback](https://paperback.moe)
:   All-in-one manga reader with support for external sources

+++ Media Servers

[Kavita](https://www.kavitareader.com)
:   Free and open-source manga, comic, and book server. *Includes an integrated web reader ([demo](https://wiki.kavitareader.com/en/kavita-demo)) and support for OPDS (e.g. [CDisplayEx](https://www.cdisplayex.com), [Perfect Viewer](https://play.google.com/store/apps/details?id=com.rookiestudio.perfectviewer))*

[Komga](https://komga.org)
:   Free and open-source manga server. *Includes an integrated web reader support for OPDS (e.g. [CDisplayEx](https://www.cdisplayex.com), [Perfect Viewer](https://play.google.com/store/apps/details?id=com.rookiestudio.perfectviewer))*

    !!!secondary
    See [Readers](https://komga.org/docs/category/readers) on Komga docs to set it up with your existing reading client.
    !!!

+++

## Light Novels

### Sourcing

+++ Downloads

[Nyaa](https://nyaa.si) [!badge icon=":heart:" variant="primary" text="Recommended"]
:   Public torrent tracker with the most light novel rips

+++

### Reading

+++ PC

[Calibre](https://calibre-ebook.com)
:   Standalone reader with support for various file types and includes additional e-book management features

[Sumatra PDF](https://www.sumatrapdfreader.org)
:   Standalone reader with support for various file types

+++ Android

[Moon+ Reader](https://play.google.com/store/apps/details?id=com.flyersoft.moonreader)
:   Standalone reader with support for various file types

[LNReader](https://github.com/LNReader/lnreader)
:   All-in-one light novel reader with support for external sources

+++
