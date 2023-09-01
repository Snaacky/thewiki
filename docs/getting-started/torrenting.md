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

- [qBittorrent](https://www.qbittorrent.org) [!badge icon=":heart:" variant="primary" text="Recommended"]
- [Deluge](https://www.deluge-torrent.org)
- [LibreTorrent](https://play.google.com/store/apps/details?id=org.proninyaroslav.libretorrent) [!badge variant="secondary" text="Android"]
- [rTorrent](https://github.com/rakshasa/rtorrent) [!badge variant="secondary" text="Linux"]
- [Transmission](https://github.com/transmission/transmission)

For most users, we recommend using [qBittorrent](https://www.qbittorrent.org). It's a free, open-source, and feature-full client that is easy to use and understand.

!!!warning
You should not use newer versions of uTorrent or BitTorrent, as they are known to be bundled with malware and adware.
!!!

*See the [qBittorrent guide](/tutorials/qbittorrent) on how to install and configure the client for your device.*

## Getting Torrents

Torrents are shared using `.torrent` files or magnet links, which contain the necessary metadata of the file to be downloaded. These can be obtained through various trackers.

`.torrent` files can be added to your torrent client by opening it or browsing for the file manually. Magnet links can be opened in your browser, where it will prompt you to choose the torrent client to be opened with. Alternatively, you can paste this magnet link in your client.

[!embed text="Video example of launching a torrent file with qBittorrent"](/static/torrenting/getting-torrents-file.mp4)

*See the list of [public trackers](/sourcing/public-trackers) or [private trackers](/sourcing/private-trackers) for places to find anime torrents.*

## Additional Tools

### Port Forwarding

Port forwarding allows computers on other networks to be able to access services behind your network. If you plan to seed, opening ports make it better for peers to connect to you, allowing you to get higher upload speeds.

For leechers, this is generally unnecessary. A quick breakdown of expected performance is below:

Situation                                                                              | Peer Download Speed |
---------------------------------------------------------------------------------------|---------------------|
<span style="color:#E53E3E">Leecher</span> - <span style="color:#E53E3E">Seeder</span> | Bad                 |
<span style="color:#36AD99">Leecher</span> - <span style="color:#E53E3E">Seeder</span> | Okay                |
<span style="color:#E53E3E">Leecher</span> - <span style="color:#36AD99">Seeder</span> | Good                |
<span style="color:#36AD99">Leecher</span> - <span style="color:#36AD99">Seeder</span> | Best                |

!!!
<span style="color:#36AD99">Green peers</span> have open ports. <span style="color:#E53E3E">Red peers</span> have closed ports.
!!!

==- Enabling port forwarding

!!!warning
The port forwarding tutorial below is intended for those using home routers. This will not work with [VPNs](#vpn). *Check with your provider to see if port forwarding is available.*
!!!

!!!danger
Do not enable all ports, as they may interfere with other services or put you at risk. Avoid enabling any [well-known ports](https://wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers#Well-known_ports) and [registered ports](https://wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers#Registered_ports).
!!!

- Find the port used by your client. In [qBittorrent](/tutorials/qbittorrent/), this can be found under **Tools** > **Options** > **Connection** > **Listening Port**
- Access your router's default gateway in your browser. You can find this on Windows by running Command Prompt and typing in `ipconfig`
- Locate the port forwarding option for your router. *We recommend using ports from [49152 or higher](https://wikipedia.org/wiki/Ephemeral_port) to avoid interference with other services as well as being blocked by your ISP, which is common on lower ports*
- After saving and restarting your router, visit [CanYouSeeMe.org](https://canyouseeme.org) to check if the port is open

===

### VPN

A VPN is an encrypted network tunnel between your device and the internet, allowing you to mask your activity from your ISP. In the case of torrenting, this allows you to hide your torrent traffic and prevent someone from trying to spy on you.

Depending on which country you live in, your ISP may send you a copyright infringement notice, threaten to shut off your internet, or do absolutely nothing. On a good VPN service, this will never reach your ISP, as it is blocked by the VPN protecting your activity. *If your country is strict about it, you should highly consider buying a VPN, such as in North America or Europe.*

!!!warning
Do not use free VPNs. These services often have limitations, weak security, and questionable privacy concerns. *These may put you at greater risk compared to a premium service.*
!!!

!!!
If you use qBittorrent, you may want to consider [binding it to your client](/tutorials/qbittorrent/#vpn-binding).
!!!

#### Split Tunneling

Split tunneling allows you to selectively route your internet traffic through the VPN tunnel or your internet connection simultaneously. With split tunneling, you can choose specific applications, websites, or services to be routed through the VPN while allowing other traffic to bypass the VPN and use the regular internet connection.

*See the [split tunneling guide for Wireguard](/tutorials/splittunnel) on how to set it up.*

### Seedbox

A seedbox is a dedicated server optimized for high-speed downloading and seeding of torrents. Unlike [VPNs](#vpn), seedboxes are separate from your network, allowing for better peace of mind when torrenting.

Other advantages include:

- Better uptime as it doesn't require your PC to run. *This is especially useful if you plan to build ratio on various trackers*
- Higher prioritization when seeding due to their high speed
- Avoiding copyright notices, as they are directed to your box instead (i.e. ignored)
- Synergy and integration with third-party applications/plugins (e.g. [media servers](/guides/playback/#media-servers))

Seedboxes can be run on your own virtual private server (VPS) or personal computer. Alternatively, they can be hosted through various seedbox providers.
