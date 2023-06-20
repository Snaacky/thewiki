---
label: Torrenting
order: -1
description: Get started with torrenting
image: https://user-images.githubusercontent.com/78981416/247237870-7dd7da8f-cdbc-48e4-b096-1d790817aa18.png
---

# Torrenting

Torrenting is a peer-to-peer file sharing method that facilitates the distribution and downloading of files over the internet. Unlike traditional file downloads, where files are obtained from a single source, torrenting leverages the collective resources of numerous participants, known as peers.
When engaging in torrenting, a torrent client allows you to connect with these peers and download files in small fragments. Each peer contributes a portion of the desired file, and the torrenting software intelligently combines these fragments to create the complete file on the user's device.
An inherent advantage of torrenting is its distributed nature, as users simultaneously download and upload fragments of files. This collaborative process enhances download speeds and reduces strain on individual servers, providing an efficient and resilient method for file transfer.

==- Basic Terminology

- **Leecher** - A user who is in the process of downloading a file but has not yet completed the download or is not actively uploading (sharing) the downloaded pieces with others
- **Magnet link** - A type of hyperlink used in BitTorrent that allows users to directly connect to the tracker and peers without needing to download a separate torrent file
- **Peer** - An individual computer or device connected to a P2P network that participates in sharing and downloading files
- **Piece** - A small part of a file that is broken down during the BitTorrent process, allowing simultaneous downloading and uploading of different pieces from different peers
- **Ratio** - The ratio between the amount of data uploaded by a user and the amount downloaded
- **Seedbox** - A dedicated remote server optimized for high-speed downloading and seeding of torrents
- **Seeder** - A user who has completed downloading a file and continues to share it with other users by uploading the file pieces they possess
- **Swarm** - The collective group of peers (both seeders and leechers) participating in the distribution of a particular file through BitTorrent
- **Torrent client** - Software or application used to download and upload files using the BitTorrent protocol
- **Torrent file** - A small file that contains metadata about the files to be shared and the tracker information required to initiate and coordinate the downloading process
- **Tracker** - A server or web service that assists in coordinating the communication between peers in a BitTorrent network by keeping track of which peers possess which pieces of a file

===

## Torrent Client

To get started with torrenting, you'll need a torrent client. This is the software that you will use to download torrents.
The recommended client is [qBittorrent](/tutorials/qbittorrent). It's a free and open source feature-full client with several features like Categories, tags, RSS, Web-UI, and more. Pretty much the best client to get started with.

==- How to download qBittorrent

1. Go to qBittorrent's [download page](https://www.fosshub.com/qBittorrent.html)

2. Download `qBittorrent Windows x64` for Windows, `qBittorrent AppImage` for Linux, and `qBittorrent Mac OS X` for macOS. You'll see a few confusing terms here, like `lt20`, `qt5`, and `qt6`. You don't really have to know anything about them besides the fact that they are builds with underlying breaking changes which are not yet widely adopted which is why you should avoid them.

3. Run the installer and you're pretty much done. You can look around the settings if you want but the default values are already good.

===

### Sequential Downloading

Sequential downloading in qBittorrent refers to a feature that allows you to prioritize the order in which pieces of a torrent file are downloaded. Normally, when downloading a torrent, the pieces are retrieved in a random order from different parts of the file. However, with sequential downloading enabled, qBittorrent fetches the pieces in a sequential manner, starting from the beginning of the file and progressing towards the end.
For example, if you are downloading a video file and want to start watching it before the entire download is complete, enabling sequential downloading ensures that the initial parts of the file, which often include the beginning of the video, are prioritized. This way, you can begin playback sooner, even if the download is still in progress.
To enable sequential downloading, check the box that says `Download in sequential order` and `Download first and last pieces first` in the pop-up window when you add a torrent or by right clicking a selected torrent.

==- Enabling sequential download in qBittorrent

[![](https://user-images.githubusercontent.com/78981416/246656860-016c8815-bea7-4037-ac86-c54c1845a40c.png)](https://user-images.githubusercontent.com/78981416/246656860-016c8815-bea7-4037-ac86-c54c1845a40c.png)

[![](https://user-images.githubusercontent.com/78981416/246629896-a00c9781-30a2-41b7-a17d-3a885cd01644.png)](https://user-images.githubusercontent.com/78981416/246629896-a00c9781-30a2-41b7-a17d-3a885cd01644.png)

===

### RSS

Say you want to download weekly uploads but doing that manually is tedious. To automatically download stuff via qBittorrent, you can set up an RSS feed. You can read the RSS guide [here](/docs/tutorials/rss)

## VPN

A VPN helps protect your online privacy and security. When you connect to a VPN, it creates a secure and encrypted tunnel between your device and the internet. It encrypts (locks) your internet traffic (the information you send and receive online) and sends it through a secure tunnel. This means that even if someone tries to intercept or spy on your internet activity, they won't be able to understand or access your data because it's encrypted. In the case of torrenting, it'll hide your torrent traffic from your ISP. Depending on which country you live in, your ISP may send you a DMCA letter or threaten to shut off your internet or do absolutely nothing. If your country is strict about it, you should highly consider buying a VPN. If you're in North America or Europe you probably need a VPN, otherwise check your local laws.

!!!warning
Do not use free VPNs. Free VPNs often have limitations, weak security, and privacy concerns. They may show ads, sell your data, or have slower speeds.
!!!

### VPN Binding

The best way to avoid exposing your IP address is by binding the VPN network interface to the torrent client. This means that you'll only be able to download/upload while the VPN tunnel is active, reducing the probability of having a leak to virtually zero.

To bind your client to your VPN follow these steps:

- Start the VPN and connect to a location.
- Open qBittorrent. Go to Preferences, and then Advanced tab.
- Change Network interface to the VPN (usually its name, like "Mullvad").
- Restart qBittorrent.

!!!
Visit the [VPN binding](/tutorials/vpn-binding) guide for more details
!!!

### Split Tunneling

Split tunneling allows you to selectively route your internet traffic through either the VPN tunnel or your regular internet connection. It provides the flexibility to divide network traffic between the VPN and the local network simultaneously.
With split tunneling, you can choose specific applications, websites, or services to be routed through the VPN while allowing other traffic to bypass the VPN and use the regular internet connection.

!!!
Visit the [Split tunneling on any wireguard VPN](/tutorials/splittunnel) guide for more details
!!!
