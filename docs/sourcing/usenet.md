---
label: Usenet
order: -5
---

# Usenet

## Getting started

You will need 3 things at a basic level before you can download anything:
- A usenet provider, which will provide servers to download content from. Many offer in-bulk deals (yearly) which can average out to as low as 3-4 USD a month.
- An NZB indexer, this is where you will get your `.nzb` file. An `.nzb` file is akin to a `.torrent` file in that it contains a map pointing to the location of the content that you want to download. These can be paid, free, public or private. Just like torrents, private ones are better than the public ones.
- [SABnzbd](https://sabnzbd.org/), which is a download client, into which you will feed your `.nzb` files in order to begin downloading your desired content. Make sure to put your SABnzbd behind a strong password.

!!!warning Warning
[NZBGet](https://github.com/nzbget/nzbget) has been deprecated by its developers. It will not receive any more updates.
!!!

## Anime Tosho

[Anime Tosho](https://animetosho.org/) is a free, completely automated service which mirrors most torrents posted on TokyoTosho's anime category, Nyaa.si's English translated anime category and AniDex's anime category (filtered). onto various file hosting (otherwise known as 'DDL') services, as well as Usenet.

!!!
Animetosho skips uploading most torrents over 16GB in size and some specific exceptions, such as duplicates/reposts and excessively large versions of a file (e.g. BD remuxes)
!!!

To use it, simply click the `NZB` button under the release title to download the `.nzb` file which you can then add to your SABnzbd and it'll start downloading.
![nzb](https://i.imgur.com/yLpavtq.png)

You can also directly copy the link and paste it in SABnzbd, similar to how magnet links work in torrent clients.
![nzblink](https://i.imgur.com/6XWXhBa.png)

## SeaDex
[SeaDex](https://releases.moe/) lists all the best releases for specific anime for you to download and enjoy. SeaDex releases which are under 16GB can be found on [Anime Tosho](https://animetosho.org/) while the rest ................

## PAR
A file that contains the computed parity bits from a source file. PAR files are generated from Usenet archives that have been broken into multiple files because of file size limitations on news servers. A PAR file would allow the complete archive to be reconstructed if one of the files became corrupt. PAR files can also be used to verify the integrity of the complete archive. Your download client, such as SABnzbd, will automatically repair or attempt to repair broken files using par files after download.
