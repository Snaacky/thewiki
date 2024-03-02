---
label: Literature
order: -3
description: Learn how and where to read your favorite stuff
image: /static/literature/kujou.gif
---

# Literature

## Manga

### Sourcing

+++ Downloads

[Nyaa](https://nyaa.si) [!badge icon=":heart:" variant="primary" text="Recommended"]
:   Public torrent tracker with the most high-quality official digital rips

    Scanlations can also be found here, though significantly less than aggregator sites.

    ==- :icon-search: Searching tips
    - **Official Digital Rips:** Search for the English title and add `Digital` to your search (e.g. `Attack on Titan Digital`). When searching, prioritize newer releases, as they're likely from better sources compared to older releases.
    - **Scanlation Rips:** Search for both the English and Japanese/native title by [adding the `|` operator](/sourcing/public-trackers/#search-operators) in between (e.g. `Attack on Titan|Shingeki no Kyojin`) and sort by file size. Although these can be significantly lower in quality, note that not all manga get official digital releases and scanlations may be your only option.
    ==-

[AnimeBytes](https://animebytes.tv) [!badge icon="lock" variant="danger" text="Private"]
:   Private torrent tracker with better organisation, retention, and access to older rips.

+++ Aggregators

[ComicK](https://comick.cc/)
:   Browser aggregator with the least compressed official rips

[MangaDex](https://mangadex.org)
:   Browser aggregator with the most convenient scanlations

[MangaLife](https://manga4life.com)/[MangaSee](https://mangasee123.com)
:   Browser aggregator with the most convenient official rips

!!!
Aggregators use downscaled and compressed images, resulting in lower image quality than digital rips.
!!!

+++

### Reading

+++ PC

[mpv](https://mpv.io/installation/) [!badge icon=":heart:" variant="primary" text="Best Quality"]
:   Standalone reader with the highest quality scaling on PC

    This should be used with the [mpv-manga-reader script](https://github.com/Dudemanguy/mpv-manga-reader) in order to enable basic manga reader functionality. *See [how to install custom scripts in mpv](/tutorials/mpv/#custom-scripts).*

    ==- :icon-file-media: Recommended scaling settings
    We recommend using the following scaling config for reading manga. This config will not affect standard video playback. To use it, add the following to your `mpv.conf`:
    ```properties
    [Manga]
    profile-desc="Read Manga"
    profile-cond=filename and filename:match('%.cbz$') or filename:match('%.cbr$') or filename:match('%.zip$') or filename:match('%.rar$') ~= nil
    profile=high-quality
    dscale=mitchell
    deband=no
    ```
    ==-
    ==- :icon-sliders: Fixing black levels
    mpv allows you to manually fix black levels with the brightness and contrast controls. To use them, add the following to your `input.conf` and customise the values to your liking (default keybinds are `B` and `N`):
    ```properties
    B no-osd add contrast -4; no-osd add brightness 4; show-text "Black Level: ${brightness}"
    N no-osd add contrast 4; no-osd add brightness -4; show-text "Black Level: ${brightness}"
    ```
    This should only be used on sources where [obvious blacks appear gray](/static/literature/incorrect-black-levels.png). The black level value should be decreased [until the grays become black](/static/literature/correct-black-levels.png). *Generally, `-16` works for most sources.*
    ==-
    ==- :icon-sparkle-fill: Touchscreen support
    mpv's bundled touchscreen controls are suboptimal. We recommend using third-party programs for adding touchscreen functionality to mpv, such as [GestureSign](https://gesturesign.win), a tool which allows you to convert gestures to keypresses ([example setup](/static/literature/gesturesign-example.png)).  
    You should also add the following into your `mpv.conf` to avoid triggering the OSD when swiping:
    ```properties
    no-osc
    no-window-dragging
    ```
    ==-

[Suwayomi](https://github.com/Suwayomi/Suwayomi-Server) [!badge variant="secondary" text="Most Convenient"]
:   All-in-one manga reader server with support for external sources and manga trackers

[CDisplayEx](https://www.cdisplayex.com)
:   Standalone reader with the [second highest quality scaling](https://slow.pics/c/y737QBlP) on PC

    ==- :icon-file-media: Recommended scaling settings
    To get the best quality, set the [*Resizing Algorithm to Lanczos*](/static/literature/cdisplayex-scaling.png) and the [*Lanczos quality slider to level 2*](/static/literature/cdisplayex-scaling2.png).
    ==-
    ==- :icon-sliders: Fixing black levels
    CDisplayEx features an [*Auto Colors*](/static/literature/cdisplayex-auto-colors.png) setting, which allows you to fix black levels. This should only be used on sources where [obvious blacks appear gray](/static/literature/incorrect-black-levels.png). White level sensitivity should be increased slightly [until they become black](/static/literature/correct-black-levels.png).
    ==-
    !!!warning
    For laptops or high-resolution displays, you may need to [disable Windows DPI scaling](/static/literature/cdisplayex-dpi.png). Go to `C:\Program Files\CDisplayEx`, right-click on `CDisplayEx.exe` and select *Properties* -> *Compatibility* -> *Change high DPI settings.*
    !!!

+++ Android

[Perfect Viewer](https://play.google.com/store/apps/details?id=com.rookiestudio.perfectviewer) [!badge icon=":heart:" variant="primary" text="Best Quality"]
:   Standalone reader with the [highest quality scaling](https://slow.pics/c/y737QBlP) on Android

    !!!
    To get the best quality, set the [Image smooth filter to *Lanczos3*](/static/literature/perfect-viewer-scaling.png).
    !!!

[Mihon](https://github.com/mihonapp/mihon) [!badge variant="secondary" text="Most Convenient"]
:   All-in-one manga reader with support for external sources and manga trackers

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
:   Public torrent tracker with the most high-quality light novel rips

[AnimeBytes](https://animebytes.tv) [!badge icon="lock" variant="danger" text="Private"]
:   Private torrent tracker with a more organized layout and access to old and rare rips

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
