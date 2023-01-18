---
label: Sourcing
visibility: hidden
---

# Sourcing

## Anime

**Note: The old sourcing guide is now at [/guides/quality](/guides/quality) and remains a strongly recommended reading to completely understand this guide.**

### Torrents

If you're in a rush to get something, try searching on nyaa with the recommended release from -

- [SeaDex Index](http://releases.moe)

- [A Certain Fansubber's Index](http://index.fansubcar.tel)

The first one is geared more towards video quality than the best subtitles. A lot of releases were compared by various people and compiled into these sheets to make it easier for you. However, these don't cover all the anime that exists. Subtitle preferences are subjective and the author's might not match with your own or the best video might not be compatible with your setup. It's always better to have an idea about these things yourself so you can always find what you need.

### Searching

The main source for finding torrents is [Nyaa](nyaa.si), you already have a general idea about the codecs, quality and release groups from the sections above. These 3 things are what we'll use to quickly find the best release for any anime. Nyaa search is simple and limited, but it's enough for finding anything, given that the uploader correctly tags the release.

First, change the All Categories option to `Anime - English Translated`. It can be set to just `Anime` to include english, raws and other languages. The two useful search operators are `-` and `|(OR)`, `AND` is already implicit in every search. For example -

`"Attack on Titan"|"Shingeki no Kyojin"` will return results that match either

`-` is useful when you want to exclude something. When I try to search for an anime titled just [`Monster`](https://myanimelist.net/anime/19/Monster,), the results are flooded by Pocket Monsters episodes. This can be solved by changing the search to `Monster -pocket`.

> When searching for a dubbed version of an anime, be sure to add the term `dual audio`.{.is-info}

Once you have the search results, they are sorted by date (newest) by default. Sorting by seeds or completed downloads might help in finding the best release.

**Example Search**

For a complete example, let's take [KonoSuba](https://myanimelist.net/anime/30831/Kono_Subarashii_Sekai_ni_Shukufuku_wo), the long title is complicated enough to make searching hard. Suppose I want to watch this on a TV, and a compatibility check reveals that I can't play x264 10-bit (hi10p) . For the video, I want to avoid hi10p and prefer hevc/x265 10-bit for potentially smaller sizes. And I want the superior video from a bluray, with fansubs, instead of a web release. For some reason, I also desire flac. So my search becomes

`Konosuba BD 1080 hevc flac -hi10`

Most releases are tagged both hevc and x265, explicitly mentioning only 265 like `Konosuba BD 265 flac` will also give you some results where the uploader forgot to tag hevc. If I don't want flac, it'll be `Konosuba BD hevc -flac` or `Konosuba BD hevc opus`. There's still a problem in this search, we used `konosuba` instead of the full name and some results are missing. Changing it to `Kono BD hevc flac` matches everything. Now we can sort the results and immediately see one by kawaiika-raws. Aware of the fact that they usually have the best video and often pick good fansubs, you can safely download it.

This was just to provide a better idea of the process, most actual searches are easy and rarely need anything beyond `Anime name BD` or `Anime name hevc` for smaller sizes.

### Trackers

Public:

- [Nyaa.si](https://nyaa.si)

- [Anidex](https://anidex.info/)

- [Rutracker](https://rutracker.org/) (Russian, use Google Translate)

- [Tokyotosho](https://www.tokyotosho.info/?cat=1)

- [AnimeTosho](https://animetosho.org/) - Scrapes other public trackers, useful for getting information, mediainfo or subtitles and attachments from a torrent.

Private:

- [AnimeBytes](https://animebytes.tv/)

- [BakaBT](http://bakabt.me/)

- [AnimeTorrents](https://animetorrents.me/)

- [U2](https://u2.dmhy.org)

- [Skyeysnow](https://skyeysnow.com/) (open signup)

### DDL

- XDCC - Downloads over IRC offered by many groups, useful to obtain content with dead torrents.

- Anichiraku (private) - Stuff from nyaa and other places mirrored to google drive for fast direct downloads.

- Animetosho - Mirrors most torrents posted on TokyoTosho, Anidex and Nyaa's English translated anime category onto various file hosting services, as well as usenet. Screenshots, mediainfo, subtitles are also extracted and posted.

## Advanced

### Typesetting

Anime usually has .ass subtitle styling, while the subs will show up on most TVs and players (plex, emby, jellyfin etc.), the typesetting or overlapping dialogue is sometimes broken. This is especially a problem if you're watching fansubs. **Kodi** is one of the few players which supports proper ass rendering. Note that the player is separate from the kodi media server and can even be used with plex.

### Ordered Chapters/mkv linking

On some releases(like coalgirls), the OP/ED are removed from the episodes and placed into separate files. These files are then linked to the appropriate position in the episodes where they are supposed to play. While this is a great idea to save space, unfortunately, it doesn't work on a lot of players and the OP/ED is never played. If you notice this while watching anime, OCs are the most probable cause.

[Explanation for ordered chapters](https://mod16.org/hurfdurf/?p=8)

Fix ordered chapters using [UnlinkMKV](https://github.com/gnoling/UnlinkMKV)

### Muxing

Matroska(mkv) is a very versatile container. It can contain multiple streams of video, audio, subtitles, and other attachments. The process of taking these streams, adding or removing some, and bundling them into a new mkv is called muxing. In general, you could be muxing any format, but for anime we'll mostly be dealing with mkv. It's useful when you want to use subtitles from a different release with what you already have downloaded, or to remove the extra english audio tracks to save space. Note that this is a lossless process different from encoding and takes only a few seconds.

[Mkvtoolnix](https://mkvtoolnix.download/) is the best tool for all kinds of muxing. The equivalent cli option is mkvmerge (installed with mkvtoolnix) or ffmpeg. The process can also be done in batch for a whole folder at once. Example (paste in cmd in that folder) -

`FOR /F "tokens=*" %G IN ( 'dir /b *.mkv ') DO ffmpeg -n -i "%G" -map 0:v:0 -map 0:a:1 -map 0:s:2 -map 0:t? -c copy "%~nG .mkv"`

This will copy the first video, second audio, third subtitle stream and all attachments, which are usually the japanese ones in dual audio releases. You can check the stream number with mediainfo. The index starts from zero so `-map 0:s:0` would select the first subtitle stream. `-map 0:t?` copies the attachments(fonts) if any exist. Understanding more about the [ffmpeg map option](https://trac.ffmpeg.org/wiki/Map) is helpful here as that is the only part you need to change.

### CRC32

This is used to verify that the files you have are the correct ones, or whether they match with the version released by the uploader. CRCs will change if any part of the file changes, even if you only change one letter in one tag in the file. This allows us to identify files that were changed from the original source or corrupted at some point. However, just renaming the file will not change it. Apart from the numbers included within [] at the end of a filename, .sfv (CRC checksum) or .md5 (MD5 checksum) files can also be used for the same purpose. Sometimes a group makes mistakes and wants to correct them by releasing a second version(v2), in this case the changed CRC makes it easy to differentiate between multiple versions.

You can use [Anime Checker](http://animechecker.sourceforge.net/) or [RapidCRC](https://www.ov2.eu/programs/rapidcrc-unicode) to verify or add CRC to your files.
