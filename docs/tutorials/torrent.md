---
label: Torrents
---

# Torrents

In [torrenting](/getting-started/torrenting), a `.torrent` is a file that BitTorrent and other peer-to-peer (P2P) file-sharing programs use to download content, such as videos, music, games, or documents, over the Internet. It contains metadata that describes the to-be-downloaded content and tells BitTorrent where to download the content from, specifying the name, file size, and folder structure of the content.

## Preface

There are a few things to keep in mind before creating a torrent:

- Newer clients and tools allow you to make three types of `.torrent` files:  `v1`, `Hybrid`, and `v2`. *You should always use `v1` when creating torrents, as most public/private trackers do not support `Hybrid` or `v2` as these are [relatively new](https://blog.libtorrent.org/2020/09/bittorrent-v2)*
- Do not create a torrent of a directory containing a single file. *Instead, make a torrent of the file itself*

### Pieces

When creating a torrent, the file is cut into several, smaller pieces, which are then put together by a peer's [torrent client](/getting-started/torrenting/#torrent-client) to create the final result. These pieces are low in size and are used to facilitate peer-to-peer sharing, allowing for downloading and uploading to be much faster and seamless. However, there are some things to keep note of when setting the pieces for your torrent:

- Creating too small pieces can cause more protocol overhead, bigger torrent files, longer hashing times, and possibly higher load on the CPU and drive
- Creating too large pieces can slow down piece distribution
- Older torrent clients do not support piece sizes greater than 16 MiB. *Greater piece sizes are only a concern for torrents above ~5 TB*
- Most clients automatically pick too small piece sizes. *[dottorrent-gui](#dottorrent-gui) is an exception, as it aims for 1000-1500 pieces during creation*

We recommend targeting 1000 pieces, with 16 MiB being the upper limit. A quick reference is below:

Torrent Size | Piece Size
-------------|-------------
2 to 4 GB    | 4 MiB
4 to 8 GB    | 8 MiB
8 to ∞ GB    | 16 MiB

### Trackers

If you plan on sharing your torrent to the public, you'll need to set tracker URLs in order to communicate with other peers. Below is a few popular [public trackers](/sourcing/public-trackers) to use when creating your torrent:

```text
http://nyaa.tracker.wf:7777/announce
http://anidex.moe:6969/announce
udp://open.stealth.si:80/announce
udp://tracker.opentrackr.org:1337/announce
udp://exodus.desync.com:6969/announce
udp://tracker.torrent.eu.org:451/announce
```

## Creating

There are several tools you can use to make a torrent:

- [torf-cli](https://github.com/rndusr/torf-cli) [!badge icon=":heart:" variant="primary" text="Recommended"] [!badge variant="secondary" text="Cross-Platform"] [!badge variant="secondary" text="CLI"]
- [dottorrent-gui](https://github.com/kz26/dottorrent-gui) [!badge variant="secondary" text="Windows"] [!badge variant="secondary" text="GUI"]
- [mktorrent](https://github.com/pobrn/mktorrent) [!badge variant="secondary" text="Linux"] [!badge variant="secondary" text="CLI"]
- [qBittorrent](https://www.qbittorrent.org) [!badge variant="secondary" text="Cross-Platform"] [!badge variant="secondary" text="GUI"]
- [torrenttools](https://github.com/fbdtemme/torrenttools) [!badge variant="secondary" text="Cross-Platform"] [!badge variant="secondary" text="CLI"]

### torf-cli

[torf-cli](https://github.com/rndusr/torf-cli) is a cross-platform CLI application for creating torrents.

!!!
*See the [torf-cli docs](https://rndusr.github.io/torf-cli/torf.1.html) for advanced features like [profiles](https://rndusr.github.io/torf-cli/torf.1.html#:~:text=is%20not%20supported.-,Profiles,-A%20profile%20is).*
!!!

+++ Windows

==- Torrent for a single file/directory

```batch
torf --max-piece-size 16 ^
-t http://nyaa.tracker.wf:7777/announce,^
http://anidex.moe:6969/announce,^
udp://open.stealth.si:80/announce,^
udp://tracker.opentrackr.org:1337/announce,^
udp://tracker.coppersurfer.tk:6969/announce,^
udp://exodus.desync.com:6969/announce ^
"D:\path\to\root\directory"
```

==- Torrent for each subdirectory

```batch
for /d %X in (*) do torf --max-piece-size 16 ^
-t http://nyaa.tracker.wf:7777/announce,^
http://anidex.moe:6969/announce,^
udp://open.stealth.si:80/announce,^
udp://tracker.opentrackr.org:1337/announce,^
udp://tracker.coppersurfer.tk:6969/announce,^
udp://exodus.desync.com:6969/announce ^
"%X"
```

==- Torrent for each file in a directory

```batch
for %X in (*.mkv) do torf --max-piece-size 16 ^
-t http://nyaa.tracker.wf:7777/announce,^
http://anidex.moe:6969/announce,^
udp://open.stealth.si:80/announce,^
udp://tracker.opentrackr.org:1337/announce,^
udp://tracker.coppersurfer.tk:6969/announce,^
udp://exodus.desync.com:6969/announce ^
"%X"
```

===

+++ Linux

==- Torrent for a single file/directory

```shell
torf --max-piece-size 16 \
-t http://nyaa.tracker.wf:7777/announce,\
http://anidex.moe:6969/announce,\
udp://open.stealth.si:80/announce,\
udp://tracker.opentrackr.org:1337/announce,\
udp://tracker.coppersurfer.tk:6969/announce,\
udp://exodus.desync.com:6969/announce \
"New Folder"
```

==- Torrent for each subdirectory

```shell
for dir in ./*/; do torf --max-piece-size 16 \
-t http://nyaa.tracker.wf:7777/announce,\
http://anidex.moe:6969/announce,\
udp://open.stealth.si:80/announce,\
udp://tracker.opentrackr.org:1337/announce,\
udp://tracker.coppersurfer.tk:6969/announce,\
udp://exodus.desync.com:6969/announce \
"$dir"; done
```

==- Torrent for each file in a directory

```shell
for file in *.mkv; do torf --max-piece-size 16 \
-t http://nyaa.tracker.wf:7777/announce,\
http://anidex.moe:6969/announce,\
udp://open.stealth.si:80/announce,\
udp://tracker.opentrackr.org:1337/announce,\
udp://tracker.coppersurfer.tk:6969/announce,\
udp://exodus.desync.com:6969/announce \
"$file"; done
```

===

+++

By default, torf heavily biases 8 MB piece size in situations where 16 MB would be more suitable. *We recommend performing the following edit to more aggressively target 1000 pieces:*

- Go to your torf install location (e.g. `%localappdata%\Programs\Python\Python311\Lib\site-packages\torf`)
- Open `_torrent.py` using your text editor and replace lines 723-728:

```py
        elif size <= 16 * 2**30: # 16 GiB / 1024 pieces = 16 MiB max
            pieces = size / 1024
        elif size <= 32 * 2**30: # 32 GiB / 2048 pieces = 16 MiB max
            pieces = size / 2048
        elif size <= 64 * 2**30: # 64 GiB / 2048 pieces = 32 MiB max
            pieces = size / 2048
```

### dottorrent-gui

[dottorrent-gui](https://github.com/kz26/dottorrent-gui) is a Windows GUI application for creating torrents.

==- Torrent for a single file/directory

- In dottorrent-gui, select the file type under **Input path** and locate the file/directory of your torrent. *Alternatively, you may drag-and-drop the desired file/directory into the box*
- Add your tracker URLs under **Seeding**. *There should be one [tracker URL](#trackers) per line*
- Under **Torrent options**, select the [piece size based on the size of your torrent](#pieces)

[!embed text="Creating a torrent for a directory"](/static/torrenting/dottorrent-gui/creating-torrents-single.mp4)

===

### mktorrent

[mktorrent](https://github.com/pobrn/mktorrent) is a Linux CLI application for creating torrents.

In mktorrent, piece size is passed as `-l n`, where `n` is a power of `2`, making the piece size equal to `2^(n)` bytes.

n    | Piece Size
-----|-------------
`22` | 4 MiB
`23` | 8 MiB
`24` | 16 MiB

==- Torrent for a single file/directory

```shell
mktorrent -v -l 24 \
-a http://nyaa.tracker.wf:7777/announce \
-a http://anidex.moe:6969/announce \
-a udp://open.stealth.si:80/announce \
-a udp://tracker.opentrackr.org:1337/announce \
-a udp://tracker.coppersurfer.tk:6969/announce \
-a udp://exodus.desync.com:6969/announce \
"D:\path\to\root\directory"
```

==- Torrent for each subdirectory

```shell
for dir in ./*/; do mktorrent -v -l 24 \
-a http://nyaa.tracker.wf:7777/announce \
-a http://anidex.moe:6969/announce \
-a udp://open.stealth.si:80/announce \
-a udp://tracker.opentrackr.org:1337/announce \
-a udp://tracker.coppersurfer.tk:6969/announce \
-a udp://exodus.desync.com:6969/announce \
"$dir"; done
```

==- Torrent for each file in a directory

```shell
for file in *.mkv; do mktorrent -v -l 24 \
-a http://nyaa.tracker.wf:7777/announce \
-a http://anidex.moe:6969/announce \
-a udp://open.stealth.si:80/announce \
-a udp://tracker.opentrackr.org:1337/announce \
-a udp://tracker.coppersurfer.tk:6969/announce \
-a udp://exodus.desync.com:6969/announce \
"$file"; done
```

===

### qBittorrent

[qBittorrent](https://www.qbittorrent.org) is a cross-platform GUI [torrent client](/getting-started/torrenting/#torrent-client) which can also create torrents.

==- Torrent for a single file/directory

- In qBittorrent, go to the top bar and click on **Tools** > **Torrent Creator**
- Under **Select file/folder to share** > **Path**, paste in the file/directory location of your torrent. *Alternatively, you can search for it using the **Select file**/**Select folder** buttons or dragging them into the **[Drag and drop area]***
- Under **Settings**, select the [piece size based on the size of your torrent](#pieces)
- Under **Fields** > **Tracker URLS**, paste in the trackers

!!!warning
Line breaks after each [tracker URL](#trackers) are necessary when creating the torrent.
!!!

[!embed text="Creating a torrent for a directory"](/static/torrenting/qbittorrent/creating-torrents-single.mp4)

===

### torrenttools

[torrenttools](https://github.com/fbdtemme/torrenttools) is a cross-platform CLI application for creating torrents.

!!!
*See the [torrenttools docs](https://fbdtemme.github.io/torrenttools/index.html) for advanced features like [named trackers](https://fbdtemme.github.io/torrenttools/configuration.html#named-trackers) and [tracker groups](https://fbdtemme.github.io/torrenttools/configuration.html#tracker-groups).*
!!!

+++ Windows

==- Torrent of a single file/directory

```batch
torrenttools create -l 16MiB ^
-a http://nyaa.tracker.wf:7777/announce ^
http://anidex.moe:6969/announce ^
udp://open.stealth.si:80/announce ^
udp://tracker.opentrackr.org:1337/announce ^
udp://tracker.coppersurfer.tk:6969/announce ^
udp://exodus.desync.com:6969/announce ^
"D:\path\to\root\directory"
```

==- Torrent for each subdirectory

```batch
for /d %X in (*) do torrenttools create -l 16MiB ^
-a http://nyaa.tracker.wf:7777/announce ^
http://anidex.moe:6969/announce ^
udp://open.stealth.si:80/announce ^
udp://tracker.opentrackr.org:1337/announce ^
udp://tracker.coppersurfer.tk:6969/announce ^
udp://exodus.desync.com:6969/announce ^
"%X"
```

==- Torrent for each file in a directory

```batch
for %X in (*.mkv) do torrenttools create -l 16MiB ^
-a http://nyaa.tracker.wf:7777/announce ^
http://anidex.moe:6969/announce ^
udp://open.stealth.si:80/announce ^
udp://tracker.opentrackr.org:1337/announce ^
udp://tracker.coppersurfer.tk:6969/announce ^
udp://exodus.desync.com:6969/announce ^
"%X"
```

==-

+++ Linux

==- Torrent of a single file/directory

```shell
torrenttools create -l 16MiB \
-a http://nyaa.tracker.wf:7777/announce \
http://anidex.moe:6969/announce \
udp://open.stealth.si:80/announce \
udp://tracker.opentrackr.org:1337/announce \
udp://tracker.coppersurfer.tk:6969/announce \
udp://exodus.desync.com:6969/announce \
"New Folder"
```

==- Torrent for each subdirectory

```shell
for dir in ./*/; do torrenttools create -l 16MiB \
-a http://nyaa.tracker.wf:7777/announce \
http://anidex.moe:6969/announce \
udp://open.stealth.si:80/announce \
udp://tracker.opentrackr.org:1337/announce \
udp://tracker.coppersurfer.tk:6969/announce \
udp://exodus.desync.com:6969/announce \
"$dir"; done
```

==- Torrent for each file in a directory

```shell
for file in *.mkv; do torrenttools create -l 16MiB \
-a http://nyaa.tracker.wf:7777/announce \
http://anidex.moe:6969/announce \
udp://open.stealth.si:80/announce \
udp://tracker.opentrackr.org:1337/announce \
udp://tracker.coppersurfer.tk:6969/announce \
udp://exodus.desync.com:6969/announce \
"$file"; done
```

==-

+++
