---
label: Terminology
visibility: hidden
---

# Terminology

## Torrents

**Peer** - Any person who is uploading/downloading a torrent.

**Swarm** - The entire set of peers.

**Seed** - A peer who has finished downloading the torrent and is now uploading it to other peers.

**Leech** - A peer who is still downloading or someone who downloads more than they upload (slang).

**Seeding** - Uploading to other peers in the swarm.

**Leeching** - Downloading from other peers in the swarm.

**Ratio** - The number of bytes of data you have seeded to others divided by the number of bytes of data you have downloaded from others. If you download a 2GB movie, and you have seeded 1.5GB, your ratio on that torrent is 0.75 (1.5/2.0).

**Torrent file** - A small file with a .torrent extension that you download to begin torrenting. Contains metadata about the torrent (checksum, list of files and their sizes, etc).

**Checksum/hash** - A fixed-length value that is calculated based on all the data in the complete torrent that is intended to be unique to the specific torrent. Think of it like the torrent's unique ID. Used to identify the torrent to trackers and DHT.

**Magnet link** - a link containing at minimum the checksum of the torrent. May also contain the torrent name and/or one or more trackers. Since the magnet link does not contain torrent metadata, the .torrent file must be downloaded from the swarm first before the actual torrent can begin. Without any trackers, a magnet link requires DHT to find initial peers and begin downloading the .torrent file.

**Tracker** - A server that clients talk to to get lists of peers from the swarm. Each torrent may have zero, one, or many trackers. Private trackers are websites that run their own tracker. All private torrents will use only that private tracker.

**Trackerless torrent** - A torrent that does not use any trackers. Peers share lists of peers with each other instead.

**DHT** - Distributed hash table, the basis of trackerless torrents. Basically it is a giant table that, as the name implies, is distributed among all torrent clients. This table basically functions like a giant distributed tracker of all torrents that use DHT.
    
**PEX** - Peer exchange. This protocol allows peers on the same torrent to exchange lists of peers with each other. It is not as powerful as DHT because it cannot be used to join a new trackerless torrent; you have to know which peers to ask before you can ask.

## Audio and Video

**Container** - A container is a file format that allows multiple data streams to be embedded into a single file. Examples - mp4, mkv, flac. Not to be confused with codec.

**Codec** - Audio/Video compression format. Examples - 
Video - HEVC(h265/x265), AVC(h264/x264)
Audio - lossless (FLAC, TrueHD, DTS-HDMA) and lossy (AAC, OPUS, MP3). 

**Bit depth** - [Color depth](https://en.wikipedia.org/wiki/Color_depth), usually 10-bit for anime encodes.

**Frame rate/FPS** - Frames per second, usually 23.976 FPS.

**Level/Profile** - Specifications within the h264/h265 standard which give an idea of compatibility and specify the maximum resolution and bitrate, for example h264 4.0 = 1080p 30fps 20 Mbps. Higher level/profile = lower compatibility = more processing power needed to decode.

## Release Types

**BDMV** - A complete copy of the bluray, often used as a source for making another release or encoding.

**BDRemux** - The BDMV is losslessly packaged into mkv files.

**BDRip** - An encode made from the BDMV/BDRemux.

**WEB-DL** - A file losslessly downloaded from streaming services.

**WEBRip** - Screen captured (re-encoded) rips from streaming services.

**Re-encode** - Encodes of a BDRip or WEB source.

**Mini encode** - Encodes created with a low file size in mind. Typically less than 500MB per episode.