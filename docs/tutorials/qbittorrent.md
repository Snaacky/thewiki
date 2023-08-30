---
label: qBittorrent
description: Learn how to install qBittorrrent
image: https://user-images.githubusercontent.com/78981416/232305852-17383fd0-c7d7-4d1b-b14d-b227f0bd1ac4.png
---

# qBittorrent

[qBittorrent](https://www.qbittorrent.org) is a free, open-source, and cross-platform BitTorrent client. It is often recommened by most users due to its extensive features and ease of use.

## Installation

+++ Windows

1. Download the latest version of [qBittorrent](https://www.qbittorrent.org) from [FOSSHUB](https://www.fosshub.com/qBittorrent.html). For Windows, you'll want to download *qBitTorrent Windows x64*
2. Run the installer and follow the on-screen instructions to complete installation

==- Video tutorial

[!embed text="Installing qBittorrent on Windows"](https://files.catbox.moe/gbszk1.mp4)

===

+++ macOS

1. Download the latest version of [qBittorrent](https://www.qbittorrent.org) from [FOSSHUB](https://www.fosshub.com/qBittorrent.html). For macOS, you'll want to download *qBitTorrent Mac OS X*
2. Run the installer and follow the on-screen instructions to complete installation

+++ Linux

1. Download the latest version of [qBittorrent](https://www.qbittorrent.org) from [FOSSHUB](https://www.fosshub.com/qBittorrent.html). For Linux, you'll want to download *qBitTorrent AppImage*
2. Run the installer and follow the on-screen instructions to complete installation

+++

## Configuration

### VPN Binding

If you use a [VPN](/getting-started/torrenting/#vpn), you may want to consider binding it to your qBittorrent client. This prevents you from accidentally torrenting when your VPN is inactive or from exposing your IP.

1. Launch qBittorrent. In the top bar, go to **Tools** > **Options**
2. Head to **Advanced** and locate *Network interface*. In the dropdown menu, select your VPN's network adapter
3. Click **Apply** in the bottom-right and close the **Options** window

==- Video tutorial

[!embed text="Using VPN binding"](https://files.catbox.moe/3c9ebv.mp4)

==-

### RSS

RSS is an easy way to automatically fetch and download shows to your machine. This may be useful if you plan on downloading airing shows without having to manually add the torrent to your client.

+++ Enabling RSS

1. Launch qBittorrent. In the top bar, go to **Tools** > **Options**
2. Head to **RSS**. Under **RSS Reader**, check *Enable fetching RSS feeds*. You can also change how often your RSS feed is updated under *Feeds refresh interval*. *We recommend setting it to 15 minutes*
3. Under **RSS Torrent Auto Downloader**, check *Enable auto downloading of RSS torrents*

==- Video tutorial

[!embed text="Enabling RSS"](https://youtu.be/FEzaTjXQE4U)

==-

+++ Adding feeds

1. Choose an RSS feed to use. Popular RSS feeds for airing anime:
[!button size="s" variant="secondary" icon="rss" text="Erai-raws" margin="0 8 0 0"](https://www.erai-raws.info/rss-page/)
[!button size="s" variant="secondary" icon="rss" text="SubsPlease"](https://subsplease.org/rss-feeds/)
2. Launch qBittorrent. In the top bar, go to **View** and check *RSS Reader*
3. Select the **RSS** tab near the top bar. Click on **New subscription**
4. Paste the RSS URL and click **OK**.

==- Video tutorial

[!embed text="Adding feeds"](https://hitori.is-cute.moe/wf95PG.mp4)

==-

+++ Download rules

1. Launch qBittorrent. In the top bar, go to **View** and check *RSS Reader*
2. Select the **RSS** tab near the top bar. Click on **RSS Downloader...**
3. Click on the **+** to create a new download rule. Below is a list of important parameters:

Rule                 | Meaning                                                                       | Example Usage
---------------------|-------------------------------------------------------------------------------|-----------------------------------------
**Must Contain**     | The title of the show to follow the rule                                      | `Shiro Seijo to Kuro Bokushi`
**Must Not Contain** | Additional text to avoid downloading certain files, such as batch releases    | `batch`
**Save to**          | Optional directory to save to when *Save to a different directory* is checked | `D:\Anime\Shiro Seijo to Kuro Bokushi`

!!!
If your filter works properly, you should see the desired files under **Matching RSS Articles**.
!!!
4. Under **Apply Rule to Feeds**, check off the desired RSS feed for the download rule

==- Video tutorial

[!embed text="Download rules"](https://files.catbox.moe/soc7hp.mp4)

==-

+++

### Sequential Downloading

Sequential downloading is a feature that allows you to prioritize the order in which the pieces of a torrent file are downloaded.

Typically, qBittorrent fetches the pieces in a random order from different parts of the file. However, this option forces the client to go in sequential order, starting from the beginning to the end of the file. *This feature may be useful if you plan on watching shows immediately while the download is still in progress.*

1. Launch qBittorrent. Send your desired torrent to the client
2. Under **Torrent settings**, check *Download in sequential order* and *Download first and last pieces first*
3. Click **OK** to continue downloading the torrent

!!!
This can also be enabled in the right-click menu for the torrent.
!!!

==- Video tutorial

[!embed text="Using sequential downloading"](https://files.catbox.moe/zbb42w.mp4)

==-
