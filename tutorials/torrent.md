# Torrent

Before you make a torrent, there are a few things to keep in mind:
- Do not make a torrent of a directory containing a single file, instead make a torrent of the file itself.
- Hybrid and V2 torrents are not supported on nyaa and most other trackers.
- Smaller pieces make for more protocol overhead, bigger torrent files, longer hashing times and possibly higher load on the CPU and HDD.
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
- [torrenttools](https://github.com/fbdtemme/torrenttools) - Recommended, Cross Platform, CLI.
- [mktorrent](https://github.com/pobrn/mktorrent) - Linux only, CLI.
- [dottorrent-gui](https://github.com/kz26/dottorrent-gui) - Windows, GUI.
- [Qbittorrent](https://www.qbittorrent.org/) - Cross Platform, GUI, Slowest of the bunch.

## torrenttools
Refer to the torrenttools [docs](https://fbdtemme.github.io/torrenttools/index.html) for advanced features like [Named trackers](https://fbdtemme.github.io/torrenttools/configuration.html#named-trackers) and [Tracker groups](https://fbdtemme.github.io/torrenttools/configuration.html#tracker-groups).

Command to make a torrent of a single file or directory
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

Command to make a torrent for each subdirectory
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

Command to make a torrent for each file in a directory
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

[![3](https://files.catbox.moe/e9qczy.png "3")](https://files.catbox.moe/e9qczy.png "3")

## qbittorrent

Line breaks after each tracker URL are necessary when creating a torrent with Qbittorrent.

[![1](https://files.catbox.moe/rwmjc7.png "1")](https://files.catbox.moe/rwmjc7.png "1")

[![2](https://files.catbox.moe/ua77r3.png "2")](https://files.catbox.moe/ua77r3.png "2")
