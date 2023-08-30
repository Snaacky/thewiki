---
label: Torrenting
order: -1
description: Learn how to get started with torrenting
image: https://user-images.githubusercontent.com/78981416/247237870-7dd7da8f-cdbc-48e4-b096-1d790817aa18.png
---

# Torrenting

Torrenting is a peer-to-peer file sharing method that facilitates the distribution and downloading of files over the internet.

Unlike traditional file downloads, where files are obtained from a single source, torrenting leverages the collective resources of numerous participants, known as peers. This collaborative process enhances download speeds and reduces strain on individual servers, providing an efficient and resilient method for file transferring. A [torrent client](#torrent-client) allows you to connect with these peers and download the file in small fragments, which are put together to create the final result.

==- Understanding the Terminology

Term               | Definition
-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
**Leecher**        | A user who is currently downloading the file but has not yet completed the download or is not uploading (sharing) the downloaded pieces with others
**Magnet link**    | A hyperlink used by BitTorrent that allows users to directly connect to the tracker and peers without needing to download a `.torrent` file
**Peer**           | An individual computer or device connected to the P2P network
**Piece**          | A part of the file broken down during the BitTorrent process, allowing simultaneous downloading and uploading of different pieces across peers
**Ratio**          | The ratio between the amount of data uploaded and downloaded by the user
**Seeder**         | A user who has completed downloading the file and is sharing it with other peers by uploading the file pieces
**Swarm**          | The collective group of peers (seeders and leechers) participating in the distribution of the file through BitTorrent
**Torrent client** | The software or application used to download and upload files using the BitTorrent protocol
**Torrent file**   | A file (`.torrent`) that contains metadata about the files to be shared and the tracker information required to initiate and coordinate the downloading process
**Tracker**        | A server or web service that assists in coordinating the communication between peers in a BitTorrent network by keeping track of which peers possess which pieces of a file

===

For most people, [streaming services](/sourcing/streaming) are more convenient to use. However, torrenting is fairly simple and allows for better flexibility. Some reasons to use torrents are:

- Broad range of qualities and sizes
- [Greater quality](/guides/quality/#quality-comparisons) compared to most streaming sites
- Better availabilty
- Better subtitles/[fansubs](/guides/quality/#fansubs) and styling options
- Access to a larger variety of titles and [BD releases](/guides/quality/#blu-ray-vs-web)
- Downloaded files for rewatching with no additional data usage

!!!warning
Downloading anime torrents may be illegal depending on where you live. *To avoid receiving a copyright infringement notice from your ISP, you may want to consider using a [VPN](#vpn) or [seedbox](#seedbox).*
!!!

## Torrent Client

To start torrenting, you'll need a torrent client. This is the software that you will use to download torrents. Below is a list of some popular clients:

- [qBittorrent](https://www.qbittorrent.org) [!badge variant="info" text="Recommended"]
- [Deluge](https://www.deluge-torrent.org)
- [LibreTorrent](https://play.google.com/store/apps/details?id=org.proninyaroslav.libretorrent) [!badge variant="ghost" text="Android"]
- [rTorrent](https://github.com/rakshasa/rtorrent) [!badge variant="ghost" text="Linux"]
- [Transmission](https://github.com/transmission/transmission)

For most users, we recommend using [qBittorrent](https://www.qbittorrent.org). It's a free, open-source, and feature-full client that is easy to use and understand.

!!!warning
You should not use newer versions of uTorrent or BitTorrent, as they are known to be bundled with malware and adware.
!!!

See the [qBittorrent guide](/tutorials/qbittorrent) on how to install and configure the client for your device.

## Downloading

Torrents are shared using `.torrent` files or magnet links, which contain the necessary metadata of the file to be downloaded. These can be obtained through various trackers.

See the list of [public trackers](/sourcing/public-trackers) or [private trackers](/sourcing/private-trackers) for places to find anime torrents.

## Additional Tools

### VPN

A VPN is an encrypted network tunnel between your device and the internet, allowing you to mask your activity from your ISP. In the case of torrenting, this allows you to hide your torrent traffic and prevent someone from trying to spy on you.

Depending on which country you live in, your ISP may send you a copyright infringement notice, threaten to shut off your internet, or do absolutely nothing. On a good VPN service, this will never reach your ISP, as it is blocked by the VPN protecting your activity. *If your country is strict about it, you should highly consider buying a VPN, such as in North America or Europe.*

!!!warning
Do not use free VPNs. These services often have limitations, weak security, and questionable privacy concerns. *These may put you at greater risk compared to a premium service.*
!!!

!!!
If you use qBittorrent, you may want to consider [binding it to your client](/tutorials/qbittorrent/#vpn-binding).
!!!

### Seedbox

A seedbox is a dedicated server optimized for high-speed downloading and seeding of torrents. These are often remote machines hosted in non-DMCA-compliant regions, where infringement notices are ignored or never sent to you. Seedboxes also allow for better uptime for your torrents, especially if you plan to seed without having to leave your PC running or build ratio on various trackers.
