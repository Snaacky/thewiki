# Prerequisites

This guide will use [qBittorrent](https://www.qbittorrent.org/download.php) and the [SubsPlease RSS feed](https://subsplease.org/rss-feeds/) as an example. The process should be similar for other clients and feeds.

# Enabling RSS

1. In qBittorrent, enable `RSS Reader` from the `View` menu on the toolbar. A new `RSS` tab will appear next to `Transfers`. 
2. Navigate to `Tools` -> `Options` -> `RSS` and 
3. Check `Enable fetching RSS feeds` and `Enable auto downloading of RSS torrents.`
4. Set `Feeds refresh interval` to 15 minutes.

![vmajcpa.png](/vmajcpa.png )

# Adding the RSS feed

1. Choose the resolution that you want to download and copy the corresponding link from https://subsplease.org/rss-feeds/. For example - `https://subsplease.org/rss/?t&r=1080`
2. Go to the `RSS` tab in qBittorrent and click `New subscription`
3. Paste the link you copied and click OK.

!!!
> Do not paste this link in a browser because it will do nothing.
!!!


# Setting up download rules

**To set up the seasonal anime that you want to download, follow the steps below:**

1. At the top-right of the RSS tab, click on `RSS Downloader`.

2. In `Download Rules`, press the file icon (next to the trash can icon). Pick a name for your filter (anything works).

3. Change the textbox `Must Contain:` to the title of the anime you need to download. Use the Japanese title of the show, as SubsPlease only offers the anime in their original title. For example, `Kimetsu no Yaiba`.

4. In the textbox `Must Not Contain:`, write `batch` to avoid downloading the batch when the season ends. 
   
5. If your filter works, you'll see the entries under `Matching RSS Articles`.
   
6. **Optional:** Check `Save to a Different Directory` and choose the download directory for the episodes.

7. Check `SubsPlease RSS` under `Apply Rule to Feeds`.

![rssrules.png](/rssrules.png)

> The client can be left running in the background. The anime episode will be automatically downloaded whenever it is released. {.is-info}

> When you first add an anime to the list, the previous episodes may start downloading automatically. This only happens once, but you can manually stop the download process for any unwanted episodes.
{.is-info}


# Advanced

- Erai-raws is an alternative for SubsPlease. They are sometimes inconsistent and late but they fix certain issues with subtitles that are left unfixed in subsplease. Exclude "v0" releases with the "Must not contain:" filter to avoid duplicated downloads.

- If you want something smaller try searching nyaa for mini encoders like [Judas](https://nyaa.si/user/Judas) or [ASW](https://nyaa.si/user/AkihitoSubsWeeklies). Clicking the RSS button on these pages gives you the required link for a particular uploader, doing the same on the homepage gives you an RSS feed of everything on nyaa.

- Alternatively, check out [this guide](https://iamscum.wordpress.com/guides/taiga/) for a better way of automating downloads and tracking synced with your Anilist account.
