---
label: Torrenting
order: -1
description: Get started with torrenting
image: https://user-images.githubusercontent.com/78981416/232287791-8ab2be5d-0944-4a2b-a1cf-1d25e50ca42e.png
---

# Torrenting

## Basic Terminology

- Peer - Any person who is uploading/downloading a torrent.
- Swarm - The entire set of peers.
- Seed - A peer who has finished downloading the torrent and is now uploading it to other peers.
- Leech - A peer who is still downloading.
- Racing - Being the first in swarm to maximise upload. Only relevant in [Private trackers](/sourcing/private-trackers/).
- Torrent file - A small file with a `.torrent` extension that you download to begin torrenting. It contains metadata that describes the to-be-downloaded content and tells your torrent client where to download the content from.
- Torrent Client - Your Client is the software that manages your BitTorrent connections. It is responsible for loading torrent links and connecting to peers in the swarm.
- Tracker - A tracker is a server that keeps track of which seeds and peers are in the swarm. Clients report information to the tracker periodically and in exchange, receive information about other clients to which they can connect. The tracker is not directly involved in the data transfer and does not have a copy of the file. It only receives information from the client.
- Freeleech - the download size of the torrent does not count towards your overall ratio, only the uploaded amount on the torrent counts toward your ratio.
- Neutralleech -  none of the data uploaded or downloaded is counted.
- Hit-and-Run (HnR or H&R) - a torrent is downloaded but not seeded for enough time as per the tracker's rules.
- Ratio - a comparison between your downloaded and uploaded data.

## Torrent Clients

All the torrent clients mentioned below are cross platform and open source.

- [qBittorrent](/tutorials/qbittorrent) - Feature-full client with several features like Categories, tags, RSS, Web-UI, and more. Highly recommended.
- [Transmission](/tutorials/transmission) - Stable and efficient client with minimal resource usage. Lacks alot of features that qBittorrent has but in turn can handle large quantities of torrents with ease.
- [Deluge](/tutorials/deluge) - Mostly used by seedboxes for racing torrents. Neither as efficient as Transmission nor as feature-full as qBittorent. Pretty much only use this if you intend to race on Private Trackers for buffer.

## VPN Binding

VPN killswitches aren't reliable, the best way to avoid exposing your IP address is by binding the VPN network interface to the torrent client. This means that you'll only be able to download/upload while the VPN tunnel is active, reducing the probability of having a leak to virtually zero.

!!!
qBittorrent supports network interface binding natively, while Deluge and Transmission do not.
!!!

To bind your client to your VPN follow these steps:

- Start the VPN and connect to a location.
- Open qBittorrent. Go to Preferences, and then Advanced tab.
- Change Network interface to the VPN (usually its name, like "Mullvad").
- Restart qBittorrent.

!!!
If the above didn't work for you, go [here](/tutorials/vpn-binding) for a more detailed guide.
!!!
