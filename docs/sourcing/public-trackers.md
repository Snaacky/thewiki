---
label: Public Trackers
order: -2
---

# Public Trackers

- [Nyaa.si](https://nyaa.si)

- [Anidex](https://anidex.info/)

- [Rutracker](https://rutracker.org/) (Russian, use Google Translate)

- [Tokyotosho](https://www.tokyotosho.info/?cat=1)

- [AnimeTosho](https://animetosho.org/) - Scrapes other public trackers, useful for getting information, mediainfo or subtitles and attachments from a torrent.

# Using Nyaa

The main source for finding torrents is [Nyaa](nyaa.si), you already have a general idea about the codecs, quality and release groups from the [quality guide](/guides/quality). These 3 things are what we'll use to quickly find the best release for any anime. Nyaa search is simple and limited, but it's enough for finding anything, given that the uploader correctly tags the release.

First, change the All Categories option to `Anime - English Translated`. It can be set to just `Anime` to include english, raws and other languages. The two useful search operators are `-` and `|(OR)`, `AND` is already implicit in every search. For example -

`"Attack on Titan"|"Shingeki no Kyojin"` will return results that match either

`-` is useful when you want to exclude something. When I try to search for an anime titled just [`Monster`](https://myanimelist.net/anime/19/Monster,), the results are flooded by Pocket Monsters episodes. This can be solved by changing the search to `Monster -pocket`.

!!!
When searching for a dubbed version of an anime, be sure to add the term `dual audio`
!!!

Once you have the search results, they are sorted by date (newest) by default. Sorting by seeds or completed downloads might help in finding the best release.

**Example Search**

For a complete example, let's take [KonoSuba](https://myanimelist.net/anime/30831/Kono_Subarashii_Sekai_ni_Shukufuku_wo), the long title is complicated enough to make searching hard. Suppose I want to watch this on a TV, and a compatibility check reveals that I can't play x264 10-bit (hi10p) . For the video, I want to avoid hi10p and prefer hevc/x265 10-bit for potentially smaller sizes. And I want the superior video from a bluray, with fansubs, instead of a web release. For some reason, I also desire flac. So my search becomes

`Konosuba BD 1080 hevc flac -hi10`

Most releases are tagged both hevc and x265, explicitly mentioning only 265 like `Konosuba BD 265 flac` will also give you some results where the uploader forgot to tag hevc. If I don't want flac, it'll be `Konosuba BD hevc -flac` or `Konosuba BD hevc opus`. There's still a problem in this search, we used `konosuba` instead of the full name and some results are missing. Changing it to `Kono BD hevc flac` matches everything. Now we can sort the results and immediately see one by kawaiika-raws. Aware of the fact that they usually have the best video and often pick good fansubs, you can safely download it.

This was just to provide a better idea of the process, most actual searches are easy and rarely need anything beyond `Anime name BD` or `Anime name hevc` for smaller sizes.
