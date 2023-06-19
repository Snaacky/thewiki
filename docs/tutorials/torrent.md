---
label: Torrent
---

# Torrent

==- What is a .torrent file?
A `.torrent` file is a file that BitTorrent and other peer-to-peer (P2P) file-sharing programs use to download content, such as videos, music, games, or documents, over the Internet. It contains metadata that describes the to-be-downloaded content and tells BitTorrent where to download the content from. For example, `.torrent` files specify the name, file size, and folder structure of to-be-downloaded content.
===

Before you make a torrent, there are a few things to keep in mind:

- Newer clients and tools allow you to make three types of `.torrent` files which are `v1`, `Hybrid` and `v2`. Always use `v1` when making torrents because none of the public or private trackers currently support `Hybrid` or `v2` as these are [relatively new](https://blog.libtorrent.org/2020/09/bittorrent-v2/).
- Do not make a torrent of a directory containing a single file, instead make a torrent of the file itself.
- When making a torrent file, you'll have to select a "piece" size in which your data is sliced up so you can get small amounts of verifiable data from several peers at a time.
- Too small pieces make for more protocol overhead, bigger torrent files, longer hashing times and possibly higher load on the CPU and HDD.
- Too large pieces can slow down piece distribution.
- Older torrent clients do not support 16MiB+ piece sizes. When using 16MiB pieces, torrent size limits are only a concern above ~5TB.
- Most clients automatically pick too small piece sizes, except dottorrent-gui which aims for a reasonable 1000-1500 pieces.
- You can target ~1000 pieces with 16MiB being the upper limit. Here's a reference:

```
    2 to 4GB - 4MiB
    4 to 8GB  - 8MiB
    8 to ∞ GB - 16MiB
```

## Public Tracker URLs

```
http://nyaa.tracker.wf:7777/announce

http://anidex.moe:6969/announce

udp://open.stealth.si:80/announce

udp://tracker.opentrackr.org:1337/announce

udp://exodus.desync.com:6969/announce

udp://tracker.torrent.eu.org:451/announce
```

There are several ways to make a torrent:

- [torf-cli](https://github.com/rndusr/torf-cli) - Cross Platform, CLI.
- [torrenttools](https://github.com/fbdtemme/torrenttools) - Cross Platform, CLI.
- [mktorrent](https://github.com/pobrn/mktorrent) - Linux only, CLI.
- [dottorrent-gui](https://github.com/kz26/dottorrent-gui) - Windows, GUI.
- [Qbittorrent](https://www.qbittorrent.org/) - Cross Platform, GUI, Slowest of the bunch.

## torf-cli

Refer to the torf-cli [docs](https://rndusr.github.io/torf-cli/torf.1.html) for advanced features like [Profiles](https://rndusr.github.io/torf-cli/torf.1.html#:~:text=is%20not%20supported.-,Profiles,-A%20profile%20is).

Command to make a torrent of a single file or directory

+++ Windows
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
+++ Linux
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
+++

Command to make a torrent for each subdirectory

+++ Windows
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
+++ Linux
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
+++

Command to make a torrent for each file in a directory

+++ Windows
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
+++ Linux
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
+++

## torrenttools

Refer to the torrenttools [docs](https://fbdtemme.github.io/torrenttools/index.html) for advanced features like [Named trackers](https://fbdtemme.github.io/torrenttools/configuration.html#named-trackers) and [Tracker groups](https://fbdtemme.github.io/torrenttools/configuration.html#tracker-groups).

Command to make a torrent of a single file or directory

+++ Windows
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
+++ Linux
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
+++

Command to make a torrent for each subdirectory

+++ Windows
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
+++ Linux
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
+++

Command to make a torrent for each file in a directory

+++ Windows
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
+++ Linux
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
+++

## mktorrent

For mktorrent, piece size is passed as `-l n` where `n` is the power of `2`, making the piece size equal to `2^(n)` bytes.

```
n = 22 -> 4MiB
n = 23 -> 8MiB
n = 24 -> 16MiB
```

Command to make a torrent of a single file or directory

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

Command to make a torrent for each subdirectory

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

Command to make a torrent for each file in a directory

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

## dottorrent-gui

[![3](https://user-images.githubusercontent.com/78981416/226189863-40578262-dbd9-4bd2-a73c-3c6d7fe15451.png "3")](https://user-images.githubusercontent.com/78981416/226189863-40578262-dbd9-4bd2-a73c-3c6d7fe15451.png "3")

## qbittorrent

Line breaks after each tracker URL are necessary when creating a torrent with Qbittorrent.

[![1](https://user-images.githubusercontent.com/78981416/226189888-1fdf6f20-8232-4c37-aca6-2bd5e1d22e88.png "1")](https://user-images.githubusercontent.com/78981416/226189888-1fdf6f20-8232-4c37-aca6-2bd5e1d22e88.png "1")

[![2](https://user-images.githubusercontent.com/78981416/226189911-eeeb29ab-9a0b-4c40-8601-a5c106fa7f6c.png "2")](https://user-images.githubusercontent.com/78981416/226189911-eeeb29ab-9a0b-4c40-8601-a5c106fa7f6c.png "2")
