---
label: Torrent FAQ
order: -2
---

# Torrent FAQ

## How do I torrent?

Check out our [detailed torrenting guide](/guides/torrenting), it's very useful along with [Google](https://www.google.com/).

## What torrent clients are recommended?

* Windows: [qBittorrent](https://www.qbittorrent.org/), [Deluge](https://deluge-torrent.org/)

* macOS: [rTorrent](https://github.com/rakshasa/rtorrent), [Transmission](https://transmissionbt.com), [qBittorrent](https://www.qbittorrent.org), [Deluge](https://deluge-torrent.org/)

* *nix: [rTorrent](https://github.com/rakshasa/rtorrent), [Transmission](https://transmissionbt.com), [qBittorrent](https://www.qbittorrent.org), [Deluge](https://deluge-torrent.org/)

* Android: [Flud](https://play.google.com/store/apps/details?id=com.delphicoder.flud&hl=en_US&gl=US), [LibreTorrent](https://play.google.com/store/apps/details?id=org.proninyaroslav.libretorrent&hl=en_US&gl=US)

!!!
Do your own research on the clients you use but these are generally the most widely accepted clients. Keep in mind that some clients (such as newer versions of uTorrent and BitTorrent) have been known to be bundled with crypto miners and adware.
!!!


## Is torrenting illegal?

Torrenting itself isn't illegal. However, sharing and downloading copyrighted material may be illegal depending on your local laws. Copyright organizations can send takedown notices to the internet service provider (ISP) of the file transfer participants and start lawsuits against them for infringement.

## What's the difference between torrents and streaming?

|Torrenting|Streaming|
:--|:--|
|Wide range of quality (audio and video-wise)|Often heavily compressed (to reduce storage/bandwidth costs) |
|High quality fansubs|Official subs may be bad|
|Downloaded files are kept locally permanently|Accessibility relies on internet connection|
|Guaranteed ad-free experience|Not always ad-free|
|Stable internet connection is not required|Requires a stable internet connection to avoid buffering while streaming|
|VPN is needed in some countries|VPN is not required|
|Takes up local storage|No storage is used|

## Do I need a VPN to torrent?
If you live in a developed country, you most likely need a VPN or seedbox for torrenting and if you live in a developing country, you probably don't need to care about VPNs. Obviously this is a generalization, so make sure to do further research on the topic.

## Where can I find anime torrents?

- [Nyaa](https://nyaa.si)

- [AniDex](https://anidex.info/)

- [TokyoTosho](https://www.tokyotosho.info/?cat=1)

## What group should I get releases from?

P2P groups `SubsPlease` and `Erai-raws` and scene groups `SUGOI` and `SENPAI` upload currently airing anime. `HorribleSubs` formerly uploaded airing anime but stopped releasing in October 2020.

However, they're not the best source for older anime or anime that has been released to Blu-ray. For the absolute best video quality or the best fansubs, try searching for the releases mentioned in:

- [SeaSmoke's Anime Index](https://releases.moe)


- [A Certain Fansubber's Index](https://docs.google.com/spreadsheets/d/1PJYwhjzLNPXV2X1np-S4rdZE4fb7pxp-QbHY1O0jH6Q/htmlview)

Check out the [sourcing guide](/guides/sourcing) for more detailed information on finding the best release for any particular anime.

## How large should a typical high quality 1080p anime episode be?

For airing anime ripped from official streams, the size is always 1.3-1.4 GB. Mini encodes or the versions on streaming sites are encoded from this and are in the range of 200-400 MB. None of those can be called "high quality". A good encode using the blu-ray as a source is usually between 1 and 2 GB, and as high as 4-5 GB where it's needed. 

Size can be used to judge web sources, but it's not the best measure of quality for BDRips. Even though the raw BDMV is larger, a good encode will give you the better experience. Check out [Sourcing - Quality](/en/guides/sourcing#quality) for more.

## What does it mean for a torrent to be "stalled"?

The files you download from torrents are downloaded from [**seeders**](/guides/torrenting), who have already finished downloading the files and are uploading to leechers/peers, who may also upload to each other. In case there aren't enough seeders, or if you are unable to [connect](/guides/torrenting#connectability) to a seeder, you will be unable to download 100% of a torrent. 

This may also be because the initial seeder (the uploader) hasn't uploaded the entirety of the file to the **swarm** yet. Usually, you can wait and you should eventually connect to a seeder, but in case there are no seeders, you can ask someone who has the file to re-seed it so that you can download it. You can also try **reannouncing** to the [tracker](/guides/torrenting#glossary) (manually requesting the tracker for a refresh on peers and seeders) to try and get more seeders.

## What do I do if some files are corrupt/unplayable?

There is a **recheck** option in most torrent clients. This will recheck the hashes of the pieces and will re-download the missing or corrupted pieces, and fix the files. However, if there are no seeders or only partial seeders, you will be unable to download those pieces.

## Where can I download smaller sized episodes?

For those low on data or storage, HEVC mini encodes on nyaa are the best option. In terms of quality, they'll be equal to or often better than streaming sites. Simply search `anime name hevc` in the category `Anime - English Translated` on nyaa and sort by seeds. Some release groups doing these are - Judas, Akihito, Ember, DB, and Cleo.

## Where can I download subtitles separately?

[Animetosho](https://animetosho.org/) has subtitles and attachments for most torrents on nyaa. They can be downloaded by clicking the `all attachments` button at the bottom of any page. Remember that the included fonts are important for any fansub to display correctly. They can be installed, placed in the font directory for your player or muxed into the mkv.

## How can I add/remove subtitles or audio?

[MKVToolNix](https://mkvtoolnix.download/) is the tool for all kinds of muxing, simply drag and drop the files and you'll be able to select the streams to keep in the final output. This is a lossless process different from encoding and takes only a few seconds.

## What do the things in brackets mean at the end of a torrent?

| Keyword         | What it means                                                     |
|-----------------|-------------------------------------------------------------------|
| BD              | Video is taken from BluRay (typically JPBD)                       |
| Remux           | Untouched Video taken from BluRay                                 |
| x265/HEVC/H.265 | Codec used to encode the video                                    |
| x264/AVC/H.264  | Codec used to encode the video                                    |
| AV1             | Codec used to encode the video                                    |
| 1080p           | Resolution is 1920x1080p, `p` stands for progressive video.       |
| 8-bit           | Bit-Depth of the video                                            |
| 10-bit          | Bit-Depth of the video                                            |
| FLAC            | Codec used to encode the audio                                    |
| Opus            | Codec used to encode the audio                                    |
| AAC             | Codec used to encode the audio                                    |
| E-AC-3/EAC3/DDP | Codec used to encode the audio, DDP stands for Dobly Digital Plus |
| Dual Audio      | Has two audio tracks. Typically English and Japanese              |

For more details, check out the sourcing guide - [basics](/en/guides/sourcing#basics) and [CRC32.](/en/guides/sourcing#crc32)
