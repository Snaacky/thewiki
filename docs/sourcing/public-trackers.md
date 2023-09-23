---
label: Public Trackers
order: -2
---

# Public Trackers

Public trackers are BitTorrent trackers that allow anyone to access and download torrents without requiring user registration, unlike [private trackers](/sourcing/private-trackers).

If you're new to torrenting, see the [torrenting guide](/getting-started/torrenting) on how to get started.

!!!warning
We suggest you use a reliable content blocker such as [uBlock Origin](https://ublockorigin.com).
!!!

- [Nyaa](https://nyaa.si) [!badge icon=":heart:" variant="primary" text="Recommended"]
- [AcgnX](https://share.acgnx.se) [!badge variant="secondary" text="Chinese"]
- [ACG.RIP](https://acg.rip) [!badge variant="secondary" text="Chinese"]
- [AniDex](https://anidex.info)
- [AnimeTosho](https://animetosho.org) [!badge variant="danger" icon="search" text="Scraper"]
- [DMHY](https://dmhy.org) [!badge variant="secondary" text="Chinese"]
- [RuTracker](https://rutracker.org) [!badge variant="secondary" text="Russian"]
- [Tokyo Toshokan](https://www.tokyotosho.info/?cat=1)

## Nyaa

[Nyaa](https://nyaa.si) is a public BitTorrent tracker for anime, manga, light novels, etc.

### Searching

#### Categories

Torrents on [Nyaa](https://nyaa.si) are sorted by category. These can be changed using the second dropdown menu on the search bar.

To search for releases in English, use the `- English-translated` option under the various categories.

#### Regex

Below is a list of useful regex that can help narrow down your search when looking for torrents:

Regex | Meaning                                          | Example Usage
------|--------------------------------------------------|----------------------------------------------------------------
`"`   | Only return results that match exact word/phrase | `"Gochuumon wa Usagi desu ka??"`
`|`   | Return results that match either word/phrase; or | `"Is the Order a Rabbit?"|"Gochuumon wa Usagi desu ka?"`
`-`   | Exclude word/phrase from search                  | `Gochuumon wa Usagi desu ka? BD Hi10P -BLOOM -"Sing For You"`

*See [their help section](https://nyaa.si/help#using-search) for more information.*

### Custom RSS

[Nyaa](https://nyaa.si) supports custom RSS feed creation. This is useful if you plan on automatically downloading releases that do not have existing RSS feeds, such as fansubs for airing shows.

- Search and go to the user's profile you want to create a feed for
- On the top bar, click on **RSS**. *Copy this URL and add it to your torrent client*

==- Video tutorial

[!embed text="Creating an RSS feed for Yameii"](/static/torrenting/nyaa/custom-rss.mp4)

==-

!!!warning
You should always make searches from the profile page, as global searches carry the risk of potentially downloading torrents from imposters using the group's name, which may contain malware or other undesirable content.
!!!
