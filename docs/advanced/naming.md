---
label: Naming
description: Learn how to properly name your files
image: /static/tohsaka.gif
order: -2
---

# Naming

When naming anime files, it's important to note as much information as possible (though not *everything*). This allows any viewer to understand what the file contains, making it easy for viewers to differentiate it between other files and allow for compatibility with various tools, such as [media servers](/guides/playback/#media-servers), [Usenet](/sourcing/usenet), [XDCC](/sorucing/ddl), etc.

This page serves as a guide on the most optimal naming schemes that provide all the necessary details to users while ensuring the best compatibility with most automated setups.

## File Names

In general, a good file name should include:

- The full **anime show/series** or **movie title**
  - If there are multiple versions of the series, you must include the year within the title (e.g. [Hunter x Hunter (1999)](https://thetvdb.com/series/hunter-x-hunter) vs. [Hunter x Hunter (2011)](https://thetvdb.com/series/hunter-x-hunter-2011)). *Note that you must include the year for all movies*
  - We recommend using names found in databases such as [TheTVDB](https://www.thetvdb.com) (TV series) and [The Movie Database (TMDB)](https://www.themoviedb.org/?language=en-US) (movies)
- The **season/episode number** (for TV series)
  - For specials, use `S00EXX` for guaranteed automatic parsing and correct identification. *However, it is suggested to add an additional identifier such as including `OVA` or the episode title (i.e. `S00E01 - OVA Title`) to easily differentiate between episodes*
- The **source type** (i.e. `BD`, `DVD`, `WEB`)
  - Append `Remux` if your file uses a Blu-ray/DVD remux (i.e. `BD Remux` or `DVD Remux`)
  - For `WEB` releases, you may also include the source tag (e.g. `AMZN` for Amazon, `CR` for Crunchyroll, `DSNP` for Disney+, `NF` for Netflix)
- The **video resolution** and **scan type**
  - Use `p` for progressive video (e.g. `1080p`, `720p`)
  - Use `i` for interlaced video (e.g. `1080i`, `720i`)
- The **video and audio codec**
  - If your file includes multiple audio tracks in different codecs, only list the default track's codec
- The **releaser/group tag**

*See [Recommended Schemes](#recommended-schemes) for various formats to follow for your files.*

### Examples of Bad Names

A bad file name often leaves out important information that may be useful to the viewer. The following shows examples of badly named releases:

![Releases using bad naming schemes](/static/advanced/naming/releases-bad-naming.png)

These naming schemes are **bad** as they:

Do not include the [type of release](/guides/quality/#types-of-releases) and [where it is sourced from](/guides/quality/#blu-ray-vs-web)
:   - It can be harder to determine which release is the better one
    - File 2 does not list the video codec/resolution, audio codec, "Blu-ray/WEB" source, dual audio, etc.

May be too long in length
:   - Many programs do not support file paths longer than 256 characters
    - File 2 includes episode chapters in the file name, totaling 159 characters alone

Use an inefficient/invalid naming scheme
:   - File 1 uses absolute numbering for episodes. *We recommend using the `SXXEXX` format instead*
    - File 3 uses brackets to hold the season and episode number (`[S02,E00]`). *As a result, some programs may incorrectly parse this as a release group*

Alter or uses an incorrect title
:   - Some programs may fetch incorrect title metadata
    - File 3 uses the incorrect order for the show title (`From Me to You Kimi Ni Todoke` instead of `Kimi ni Todoke - From Me to You`)
    - File 4 does not include the show year. *As a result, media servers may fetch incorrect metadata ([JoJo's Bizarre Adventure](https://thetvdb.com/series/jojos-bizarre-adventure) is detected instead of [JoJo's Bizarre Adventure (2012)](https://thetvdb.com/series/jojos-bizarre-adventure-2012))*

Using good naming schemes, it could look like the following:

![Releases using good naming schemes](/static/advanced/naming/releases-good-naming.png)

### Recommended Schemes

A good file name allows a viewer to easily understand what the file is and what it contains. See the recommended variants below:

+++ :icon-file-code: Variant 1

![Example file name using variant 1](/static/advanced/naming/variant-1.png)

!!!warning
Spaces and special characters (i.e. apostrophes `'`, commas `,`) should be replaced with periods `.` instead.
!!!

![#ed8796](https://placehold.co/14x14/ed8796/ed8796.png) Show title
:   The full show/series or movie title. *We recommend using names found in databases such as [TheTVDB](https://www.thetvdb.com) (TV series) and [The Movie Database (TMDB)](https://www.themoviedb.org/?language=en-US) (movies). This can be in English or romanized Japanese*

    !!!warning
    If there are multiple versions of the series, you must include the year within the title (e.g. [`Hunter.x.Hunter.1999`](https://thetvdb.com/series/hunter-x-hunter) vs. [`Hunter.x.Hunter.2011`](https://thetvdb.com/series/hunter-x-hunter-2011)). *For movies, this should always be included*
    !!!

![#f5a97f](https://placehold.co/14x14/f5a97f/f5a97f.png) Season/episode number
:   The season and episode number

    !!!info
    For specials, use `S00EXX` for guaranteed automatic parsing and correct identification. *However, it is suggested to add an additional identifier such as including `OVA` or the episode title (i.e. `S00E01.OVA.Title`) to easily differentiate between episodes*
    !!!

![#eed49f](https://placehold.co/14x14/eed49f/eed49f.png) Episode title [!badge variant="success" text="Optional"]
:   The title of the episode. *We recommend using names found in databases such as [TheTVDB](https://www.thetvdb.com) (TV series) and [The Movie Database (TMDB)](https://www.themoviedb.org/?language=en-US) (movies)*

    !!!warning
    This should generally be skipped to avoid creating file paths larger than 256 characters, which may cause problems with various programs.
    !!!

![#a6da95](https://placehold.co/14x14/a6da95/a6da95.png) Version number [!badge variant="warning" text="Conditional"]
:   The version number for patched releases in `REPACKX` format, where `X` is the version number

![#81c8be](https://placehold.co/14x14/81c8be/81c8be.png) Video resolution
:   The vertical resolution of the file's video. *We recommend using [MediaInfo](https://mediaarea.net/MediaInfo) to find this information*

![#91d7e3](https://placehold.co/14x14/91d7e3/91d7e3.png) Source type
:   The source where the video/audio are taken from

    !!!info
    For Blu-ray/DVD remuxes, add `Remux` to the end (e.g. `BluRay.Remux` or `DVD.Remux`)
    !!!

    !!!info
    For `WEB` releases, you may also include the source tag (e.g. `AMZN` for Amazon, `CR` for Crunchyroll, `DSNP` for Disney+, `NF` for Netflix)
    !!!

![#8aadf4](https://placehold.co/14x14/8aadf4/8aadf4.png) Dual audio marker [!badge variant="warning" text="Conditional"]
:   The marker for file using multiple audio languages/tracks

![#c6a0f6](https://placehold.co/14x14/c6a0f6/c6a0f6.png) Audio format(s)
:   The codec of the file's audio. *We recommend using [MediaInfo](https://mediaarea.net/MediaInfo) to find this information*

    !!!info
    If your file includes multiple audio tracks in different codecs, only list the default track's codec
    !!!

![#f5bde6](https://placehold.co/14x14/f5bde6/f5bde6.png) Video format
:   The codec of the file's video. *We recommend using [MediaInfo](https://mediaarea.net/MediaInfo) to find this information*

![#f0c6c6](https://placehold.co/14x14/f0c6c6/f0c6c6.png) Releaser
:   The name of the releaser or release group

+++ :icon-file-code: Variant 2

![Example file name using variant 2](/static/advanced/naming/variant-2.png)

![#ed8796](https://placehold.co/14x14/ed8796/ed8796.png) Releaser
:   The name of the releaser or release group

![#f5a97f](https://placehold.co/14x14/f5a97f/f5a97f.png) Show title
:   The full show/series or movie title. *We recommend using names found in databases such as [TheTVDB](https://www.thetvdb.com) (TV series) and [The Movie Database (TMDB)](https://www.themoviedb.org/?language=en-US) (movies). This can be in English or romanized Japanese*

    !!!warning
    If there are multiple versions of the series, you must include the year within the title (e.g. [`Hunter x Hunter (1999)`](https://thetvdb.com/series/hunter-x-hunter) vs. [`Hunter x Hunter (2011)`](https://thetvdb.com/series/hunter-x-hunter-2011)). *For movies, this should always be included*
    !!!

![#eed49f](https://placehold.co/14x14/eed49f/eed49f.png) Season/episode number
:   The season and episode number

    !!!info
    For specials, use `S00EXX` for guaranteed automatic parsing and correct identification. *However, it is suggested to add an additional identifier such as including `OVA` or the episode title (i.e. `S00E01 - OVA Title`) to easily differentiate between episodes*
    !!!

![#a6da95](https://placehold.co/14x14/a6da95/a6da95.png) Version number [!badge variant="warning" text="Conditional"]
:   The version number for patched releases in `vX` format, where `X` is the version number

![#81c8be](https://placehold.co/14x14/81c8be/81c8be.png) Episode title [!badge variant="success" text="Optional"]
:   The title of the episode. *We recommend using names found in databases such as [TheTVDB](https://www.thetvdb.com) (TV series) and [The Movie Database (TMDB)](https://www.themoviedb.org/?language=en-US) (movies)*

    !!!warning
    This should generally be skipped to avoid creating file paths larger than 256 characters, which may cause problems with various programs.
    !!!

![#91d7e3](https://placehold.co/14x14/91d7e3/91d7e3.png) Source type
:   The source where the video/audio are taken from

    !!!info
    For Blu-ray/DVD remuxes, add `Remux` to the end (e.g. `BD Remux` or `DVD Remux`)
    !!!

![#8aadf4](https://placehold.co/14x14/8aadf4/8aadf4.png) Video resolution
:   The vertical resolution of the file's video. *We recommend using [MediaInfo](https://mediaarea.net/MediaInfo) to find this information*

![#c6a0f6](https://placehold.co/14x14/c6a0f6/c6a0f6.png) Video format
:   The codec of the file's video. *We recommend using [MediaInfo](https://mediaarea.net/MediaInfo) to find this information*

![#f5bde6](https://placehold.co/14x14/f5bde6/f5bde6.png) Audio format(s)
:   The codec of the file's audio. *We recommend using [MediaInfo](https://mediaarea.net/MediaInfo) to find this information*

    !!!info
    If your file includes multiple audio tracks in different codecs, only list the default track's codec
    !!!

![#f0c6c6](https://placehold.co/14x14/f0c6c6/f0c6c6.png) Dual audio marker [!badge variant="warning" text="Conditional"]
:   The marker for file using multiple audio languages/tracks

![#f4dbd6](https://placehold.co/14x14/f4dbd6/f4dbd6.png) Checksum [!badge variant="success" text="Optional"]
:   The CRC32 checksum for file verification. *We recommend using [RapidCRC](https://www.ov2.eu/programs/rapidcrc-unicode) for generating checksums*

+++

## Folders

In general, a good folder should:

- List the **season number(s)** (for TV series)
  - If you are organizing multiple seasons, these should be separated and named by season (e.g. `Season 1`, `Season 2`, ...)
  - If a season is split into two cours but stored in a single season (e.g. [86: Eighty Six](https://thetvdb.com/series/eighty-six/seasons/official/1)), include the episode range after the season info (e.g. `86: Eighty Six - S01E01-E11)` and `86: Eighty Six - S01E12-E23)`)
- Store **specials/OVA episodes** in a separate folder named `Specials`
- Store **additional content** (creditless OPs/EDs, PVs) in a separate folder named `Extras`
  - `Extras` is a catch-all folder that automation/media servers completely ignore
  - Do not use folders such as `NC`, `Creditless`, etc.
- Include the **releaser/group tag**

Examples:

```text
Anime Name (2022) S01 (BD 1080p HEVC Opus) [Dual Audio] [Group]
Anime.Name.2022.S01.1080p.AMZN.WEB-DL.DDP2.0.H.264-Group
Anime Name - S01E01-E24 (BD Remux 1080p x264 8-bit DTS-HD) [Dual Audio] [Group]
Anime.Name.2022.S01.E12-E23.1080p.BluRay.FLAC2.0.H.265-Group
```

!!!info
For organization, it is recommended to put the group name at the end of folder names to allow for alphabetical sorting on the user's end.
!!!

## Nyaa

In general, a good Nyaa release should:

- Follow the same guidelines for [folders](#folders)
- **Not include mutliple seasons.** These are not supported by automation and cannot be cross-seeded
  - If you are including a few specials, or the special is important (e.g. finale), you may want to include it in the same torrent. *Otherwise, you should create a separate torrent*
- Additional aliases at the end of the title for better searching (e.g. alternate show titles, `Season X (SXX)`)
- A raw link to [MediaInfo](https://mediaarea.net/en/MediaInfo) information
  - For season releases, we recommend using the first episode
- A link to a [quality comparison](/tutorials/comparison) including the video used in the release
- A description of what was done or used to create the release
  - e.g. Video/audio source, typesetting/translation changes, subtitle retiming, artifact fixes, tracks from other releases

Example titles:

```text
[Group] Anime Name (2022) - Season 1 (S01) (BD 1080p HEVC Opus) [Dual Audio] | Alias 1 | Alias 2
Anime.Name.2022.S01.1080p.AMZN.WEB-DL.DUAL.DDP2.0.H.264-Group | Alias 1 | Alias 2
[Group] Anime Name - S01E01-E24 (BD Remux 1080p x264 8-bit DTS-HD) [Dual Audio] | Alias 1 | Alias 2
Anime.Name.2022.S01.1080p.BluRay.REMUX.AVC.FLAC.2.0-Group | Alias 1 | Alias 2
```

## Recommended Tools

The following tools can help speed up the renaming process:

- [Advanced Renamer](https://www.advancedrenamer.com)
- [Bulk Rename Utility](https://www.bulkrenameutility.co.uk)
- [Filebot-mod](https://github.com/barry-allen07/FB-Mod)
- [tvnamer](https://github.com/dbr/tvnamer)
