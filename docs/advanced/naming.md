---
label: Naming
description: Learn how to correctly name your files
image: /static/tohsaka.gif
order: -2
---


# Naming

This guide aims to somewhat standardize naming schemes used for anime
in an effort to make them not just File Explorer-friendly,
but also work well with automation software and media servers.

**The goal of every recommendation given here is to ensure it works with both automation software _and_ file explorers. 
If something isn't mentioned, it's very likely because it breaks support for one or the other.**

Adopting all of these suggestions will ensure compatibility with basically everything.
For example, not only will auto-downloaders snatch your releases,
but they will also be parsed and matched accurately in media servers
without the need to rename files or otherwise break seeding.

## Why should you bother?

1. Commonly used anime naming looks like gibberish to both humans and machines alike.

    Example 1: `[SubsPlease] Hikari no Ou - 14 (1080p) [48F1910E].mkv`
    - First look tells me it's a show called Hikari no Ou by SubsPlease and it's 1080p.
    - But where is this from? BluRay? DVD? WEB-DL? WEBRip? Oh web-dl? Which one?
    - Is it an encode or a remux?
    - What audio/video codec is in this? My TV can only play XYZ.

    Example 2: `[GJM] Mobile Suit Gundam - The Witch from Mercury - 24 [AEB8BE46].mkv`
    - First look tells me it's a show called Mobile Suit Gundam - The Witch from Mercury by GJM.
    - What resolution is this? I only want 1080p!
    - But where is this from? BluRay? DVD? WEB-DL? WEBRip? This upload is after the BluRay released so maybe it's BluRay. (WRONG, it's WEB)
    - Is it an encode or a remux?
    - What audio/video codec is in this? My TV can only play XYZ.

    Example 3: `[SubsPlease] Boku no Hero Academia - 90 (1080p) [4B8B1261].mkv`
    - First look tells me it's a show called Boku no Hero Academia by SubsPlease.
    - This show has 6 seasons which one is this?
    - But where is this from? BluRay? DVD? WEB-DL? WEBRip? Oh web-dl? Which one?
    - Is it an encode or a remux?
    - What audio/video codec is in this? My TV can only play XYZ.

    As a thought excercise, here's what it looks like to both humans and machines: `[SomeGroup] Some Show - 17 [CRC].mkv`

    The example is a real release which I've simply replaced the names to simulate how it looks to someone who's not acquainted with the whole anime scene. Now tell me:

    - What source is it?
    - What resolution is it?
    - What codecs are in it?
    - Is this season 1 with 24 episodes or is it some other season with absolute numbering?
    - Is it an encode or a remux?

    Now let's try the same thought excercise on something that's actually named properly:

    ```
    Some.Show.S03E04.1080p.CR.WEB-DL.Dual-Audio.AAC2.0.H.264-SomeGroup.mkv
    [SomeGroup] Some Show - S03E04 (BD 1080p HEVC Opus) [Dual Audio] [CF1029D9].mkv
    Some.Show.S03E04.1080p.BluRay.Remux.Dual-Audio.FLAC2.0.H.264-SomeGroup.mkv
    ```
    Here, from just one look, I know exactly:
    - What show they are, that is, `Some Show`
    - Which episode of which season they are, that is, `S03E04`
    - What sources they are. First is a Crunchyroll WEB-DL, second is a BD encode, and the third one is a remux.
    - What resolution they are, that is, 1080p and that's what I want.
    - What codecs have been used, so I can judge it's compatibility with my device before grabbing it.

2. You lose downloads from automatic snatchers because they simply fail to parse stupidly named releases.
3. People usually have to rename said stupidly named releases to make them work in their media servers, which means they can no longer seed it.
4. Switching to a more compatible naming scheme has no disadvantages. It won't harm you or the quality of your release, it'll only serve to widen your audience and improve convenience. It should be a no-brainer considering that it only benefits those who are interested in the release.

### Recommended Tools

- [Advanced Renamer](https://www.advancedrenamer.com/)
- [Filebot-mod](https://github.com/barry-allen07/FB-Mod)
- [Bulk Rename Utility](https://www.bulkrenameutility.co.uk/)
- [tvnamer](https://github.com/dbr/tvnamer)

### General

- Must contain the name of the Anime.
- Must contain `(year)` when there are multiple versions, e.g, `Hunter x Hunter (1999)` and `Hunter x Hunter (2011)`. **Note:** _Including year is required for all movies, regardless of whether multiple versions exist or not._
- Must contain source information, i.e, `BD/BluRay`, `WEB/WEB-DL/WEBRip`, or `DVD`. Append the word `Remux` to `BD/BluRay` or `DVD` if it's a Remux.
- For `WEB/WEB-DL/WEBRip`, you must include source tags. `AMZN` for Amazon, `DSNP` for Disney+, and `NF` for Netflix.
- Must contain resolution, i.e, `1080`, `720`, `576` or `480` together with scan type, i.e, `p` if the content is fully Progressive or `i` if it’s Interlaced. So finally it looks like this: `1080p`, `720p`, `576p`, `480i`. Different aspect ratios do not change this resolution, e.g. `1440x1080` and `1920x800` are both still 1080p.
- Must mention the Video and Audio Codec. There are cases where you have two audio tracks with two different codecs, in which case you must only put the codec of the original/primary/default audio track in the filename.
- Always add a group tag to your release, ideally placing it at the end for both easier parsing and human readability.
- You should use a `.`, `-`, or space (` `) as a delimiter.
- Limit the use of special characters or characters that need to be escaped. Ideally, don't use any special characters and limit yourself to `a-z`,`A-Z`, and `0-9`. E.g, write `MASH` instead of `M*A*S*H`.

### Filename

- Must contain Season and Episode information in the format `SXXEYY` following [TVDB](https://thetvdb.com/).
- Absolute Episode Numbers may be used together with `SXXEYY`.
- Special Episodes must follow the format `S00EXX`.
- `Group` tag can be at the start or the end of the filename.

Examples:

```
[Group] Anime Name - S01E01 - (BD 1080p HEVC FLAC) [Dual Audio] [CRC32].mkv
Anime Name - S01E01 - (BD 1080p HEVC FLAC) [Dual Audio] [CRC32]-Group.mkv
Anime Name - 18.5 - S00E05 - (BD 1080p HEVC FLAC) [Dual Audio]-Group.mkv
Anime Name (2022) - S01E01 - (BD Remux 1080p HEVC FLAC) [Dual Audio]-Group.mkv
Anime.Name.S01E01.1080p.BluRay.Opus2.0.x264-Hi10P-Group.mkv
```

### Specials/OVA Filename

- These are wildly inconsistent across different databases which means you need to be more careful when naming these to avoid any confusion while also keeping it both human and machine readable. Databases like [thetvdb](https://thetvdb.com/) use `S00EXX` for specials while databases like [MAL](https://myanimelist.net/) simply use the episode name for specials.
- A common thing in anime releases is Specials/OVAs numbered as `0.5`, e.g. `Anime Name - 18.5`. This is not recommended at all.
- `0.5` Episodes are not supported by a single piece of software or database but they usually make it easier for the person watching to know where it chronologically lands in the show. You may or may not include this in the filename.
- `S00EXX` isn't very user friendly, because it doesn't really tell much on its own but it guarantees automatic parsing and correct identification. You must include this in the filename. Refer to [thetvdb](https://thetvdb.com/) to find this for your episode.
- Including the Episode title is highly recommended in this case as it makes identification significantly easier and understandable for anyone who downloads it. Neither `S00EXX` or `0.5` provides this information at a glance which is why the episode title is really helpful in this case. Refer to either [thetvdb](https://thetvdb.com/) or [MAL](https://myanimelist.net/) to find the episode name.
- You may include the absolute number of the episode as long as it's not a decimal episode that lands between any two existing ones. Optional and really just down to preference. It's much better than decimal episode numbering but still not ideal.
- This means we need a name that is easily readable, parsable, and identifiable. This is achieved by using a combo of the above.

```
Anime Name - S00E04 - Title of the Episode
```

- This is an example of a well named special episode. `Anime Name` tells us the name of the anime, `S00E04` tells us the episode number in the `SXXEXX` standard which makes it easily identifiable, and `Title of the Episode` makes it significantly more human readable.

```
Anime Name - 18.5 - S00E05 - Title of the Episode
```

- Another good example, this tells us the name of the anime, where it lands chronologically, what episode it is with reference to databases and the name of the episode to aid with identifying the episode without much effort.

#### More Examples

Bad, do not use:

```
[Group] Anime Name - S02OVA02.mkv
[Group] Anime Name - 18.5 [BD 1080p FLAC] [CRC].mkv
[Group] Anime Name - 06.5 (SP) [BD 1080p FLAC].mkv
[Group] Anime Name - Title of the Episode [CRC].mkv
[Group] Anime Name - 17.5 (OVA) [CRC].mkv
```

Good names:

```
Anime Name - 06.5 - S00E04 - Title of the Episode (BD 1080p HEVC FLAC) [Dual Audio] [CRC32] [Group].mkv
Anime Name - S00E01 - Title of the Episode (AMZN WEB-DL 1080p H.264 EAC3) [Dual Audio] [Group].mkv
Anime Name - 18.5 - S00E05 - Title of the Episode (BD 1080p HEVC FLAC) [Dual Audio]-Group.mkv
Anime Name (2022) - S00E01 - Title of the Episode (BD Remux 1080p HEVC FLAC) [Dual Audio]-Group.mkv
Anime.Name.S00E09.Title.of.the.Episode.1080p.BluRay.Opus2.0.x264-Hi10P-Group.mkv
```

### Folder

- Must contain Season information in the format: `S0X` or `Season X`.
- Make one folder for each Season following all the guidelines. This is due to the fact that most trackers do not allow multi-season batch torrents.
- Put `Group` tag at the end of the folder name to allow alphabetical sorting and easier parsing.
- **Everything** that isn't an episode, e.g Creditless Intro/Outro, PVs, etc **must** be put in a subfolder called `Extras` (not `Extra`). Do **not** use subfolders like `NC`, `Creditless`, etc.
- Ideally, you should upload specials/OVAs separately if they have their own entry on AnimeBytes/AniDB. If you still insist on grouping them with the main entry, you must put them in a subfolder called `Specials`. This is because `S00` is alphabetically above any other season, so a naive user might click the first file and end up watching the wrong episode. Furthermore, grouping them this way will lead to unnecessary complications when someone (even you) cross-posts it to a tracker that enforces proper boundaries between entries (which includes virtually every private tracker).
- Anime that are a single season split between two cours, e.g. Eighty-Six aired in two cours, [cour 1](https://anilist.co/anime/116589/) and [cour 2](https://anilist.co/anime/131586/), but is a [single season](https://thetvdb.com/series/eighty-six/seasons/official/1), must be named in a way that satisfies both which is achieved by adding the episode range after the season info (`S0X`). For example, `Eighty-Six S01 E01-E11` and `Eighty-Six S01 E12-E23`.

Examples:

```
Anime Name Season 2 (BD Remux 1080p H.264 FLAC) [Dual Audio] [Group]
Anime Name (2022) S01 (BD 1080p HEVC Opus) [Dual Audio]-Group
Anime.Name.2022.S01.1080p.AMZN.WEB-DL.DDP2.0.H.264-Group
Anime Name S02 E01-E24 (BD Remux 1080p x264 8-bit DTS-HD) [Dual-Audio] [Group]
Anime Name S01 E25-E48 (BD Remux 1080p x264 8-bit DTS-HD) [Dual-Audio] [Group]
Anime.Name.2022.S01.E01-E11.1080p.BluRay.FLAC2.0.H.265-Group
Anime.Name.2022.S01.E12-E23.1080p.BluRay.FLAC2.0.H.265-Group
```

### Nyaa

#### Title

- Same guidelines as Folder, simply add more aliases at the end for easier searching.
- You can also put the group tag before everything else in the Nyaa title.

Examples:

```
Anime Name Season 2 (BD 1080p Hi10 Opus) [Dual Audio] [Group] | Alternate Name
[Group] Anime Name Season 2 (BD Remux 1080p H.264 FLAC) [Dual Audio] | Alternate Name
Anime Name (2022) S01 (BD 1080p HEVC Opus) [Dual Audio]-Group | Alternate Name
Anime.Name.2022.S01.1080p.AMZN.WEB-DL.DDP2.0.H.264-Group | Alternate Name
[Group] Anime Name S02 E01-E24 (BD Remux 1080p x264 8-bit DTS-HD) [Dual-Audio] | Alternate Name
[Group] Anime Name S01 E25-E48 (BD Remux 1080p x264 8-bit DTS-HD) [Dual-Audio] | Alternate Name
Anime.Name.2022.S01.E01-E11.1080p.BluRay.FLAC2.0.H.265-Group | Alternate Name
Anime.Name.2022.S01.E12-E23.1080p.BluRay.FLAC2.0.H.265-Group | Alternate Name
```

#### Description

- Always include [MediaInfo](https://mediaarea.net/en/MediaInfo).
- Always include a link to a [comparison](/tutorials/comparison) of the video used in the release.
