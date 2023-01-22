---
label: Torrenting
order: -3
---

# Torrenting

## Disclaimer

The process of sharing files through torrents is completely legal. Transferring copyright infringing files may be illegal depending on your local laws. Copyright organizations may scrape lists of peers, and send takedown notices to the internet service provider of users participating in the swarms of files that are under copyright. In some jurisdictions, copyright holders may start lawsuits against uploaders or downloaders for infringement. However, they are less common for anime compared to Movies/TV shows.

!!!info
You may want to use a [VPN](/en/faq/vpn) or a [seedbox](/en/guides/torrenting#what-is-a-seedbox) for downloading torrents to avoid receiving a copyright infringement notice from your ISP or a copyright body, and having your internet limited or revoked.
!!!

## Basics

### Why should I use torrents?

For many people, streaming services are more convenient to use. However, torrenting is fairly simple and allows for more flexibility. Some reasons to use torrents are:

- A range of qualities and sizes.
- Better subtitles and style options.
- Easier access to a larger variety of titles.
- Downloaded files for rewatching with no additional data usage.

### What is torrenting?

The BitTorrent Protocol is an internet protocol for peer-to-peer (P2P) file sharing, where the large file will be broken down into many small pieces called "packets". Those packets will be exchanged between all the devices in a P2P network consist of uploaders (a.k.a. "seeders") and downloaders (a.k.a. "leechers"). The seeders will send the packets that the leechers are still missing until all the needed packets are received, and finally merged into the complete file that you originally wanted to download.

!!!info
**Key difference:** The file is not being hosted on any specific servers and ready to be downloaded like the commonly found direct download links on the internet. Instead, each computer in the P2P network acts as a small server that will share you small pieces of the file.
!!!

### Torrent clients

This is the program where you add torrents, connect to other peers, and download the files.

!!!warning Warning
Keep in mind that some clients (such as newer versions of uTorrent and BitTorrent) have been known to be bundled with crypto miners and adware.
!!!

**Windows, Linux, macOS:**

- [qBittorrent](https://www.qbittorrent.org/download.php)
- [Transmission](https://transmissionbt.com/about/)
- [Deluge](https://dev.deluge-torrent.org/wiki/Download)
- [rTorrent](https://github.com/rakshasa/rtorrent) (Linux, CLI only)

**Android:**

- [LibreTorrent](https://play.google.com/store/apps/details?id=org.proninyaroslav.libretorrent) - Open source, ad free
- [Flud](https://play.google.com/store/apps/details?id=com.delphicoder.flud) - Closed-source, contains ads

**Some popular client plugins and mods for automation:**

- [Jackett](https://github.com/Jackett/Jackett): Meta indexer for torrent sites. Can be used as an addition to qBittorrent search plugins.
- [Transdrone](https://f-droid.org/packages/org.transdroid.full/) - Remote torrent management app for your torrent client running on your main PC. Supports several torrent clients.
- [nzb360](https://play.google.com/store/apps/details?id=com.kevinforeman.nzb360) - Remote torrent management app with support for managing Sonarr, Radarr etc.
- [ruTorrent for rTorrent](https://github.com/Novik/ruTorrent) - Web UI for rtorrent.
- [Flood for rTorrent](https://github.com/Flood-UI/flood) - Sleek web UI for rtorrent.
- [Sonarr](https://github.com/Sonarr/Sonarr): Automation of downloads through RSS.
- [Taiga](https://taiga.moe/) - Automatically detects watched anime and synchronizes progress with online services. Includes download automation through RSS.

### How do I torrent?

- First, find a torrent file or magnet link for the content you want to download. Such files can be found on websites (called [trackers](/en/guides/torrenting#trackers)) like nyaa.si.

- A `.torrent` file can be added to the client by double clicking it or use the client and browse to the location of the file manually.

- A magnet link can be opened by clicking on it in your web browser, where it will prompt you to choose the torrent client to be opened with. You can also paste the magnet link into the torrent client manually.

- Most clients will begin leeching/downloading when you add the torrent, and will start seeding (uploading) immediately to other peers once the first few packets are downloaded, and will continue to seed even after the torrent finishes downloading.

- After the torrent finishes downloading, you should seed it at least for a while (ideally forever). Seeding is the way you give back to the swarm that you leeched from. Leeching without seeding back (hit-n-run) is extremely ill-advised due to the nature of how torrenting works.

- The "ratio" is your upload size divided by download size. A ratio of 1 implies that you seeded as much as you leeched.

- Often times, you will notice that there are no uploads from your actively seeding torrents. This happens when there are no peers downloading on the P2P network, or they are prioritizing other faster seeders over you (this is controlled by the BitTorrent protocol).

- If there are no seeders, or rather, connectable seeders, you will be unable to get the files, because there is no one left to upload the files to you. In such a situation, try waiting, or add more trackers and announce to those to try and get more seeds. Peers may also connect and upload to you, however, they do not hold the entirety of the torrent, and can only upload the parts they possess.

## Advanced

### qBittorrent features

!!!info
The features mentioned below are available in most torrent clients, but qBittorrent will be used as an example.
!!!

The functionality is broadly divided into 3 tabs visible in the top bar - Transfers, Search and RSS. If they are not visible, they can be enabled from the "View" menu.

- **Transfers**: This is where you can see all the torrents, which can be filtered by status, category and tags in the left pane. Categories and tags can be updated by selecting the torrents > right click.
- **Search**: This is one of the most powerful features of qBittorrent. The search plugin can be enabled by clicking "Search plugins..." at the bottom right, and choose the second option "Check for updates". This will populate the list with some of the common trackers. Unfortunately, while nyaa is not one of the default plugins, it is available to be installed [here](http://plugins.qbittorrent.org) with instructions.

A better method is to add multiple trackers through the default search plugin called Jackett. Jackett is a separate program which has to be installed and can be configured with most private and public trackers, this plugin allows qBittorrent to search through Jackett.

### RSS

- This is the simplest way of automating downloads. It is a link on a website that is autogenerated by that website whenever a new torrent is listed on it. It takes the metadata of that torrent and lists it on its site, but it also puts the information into an file called an RSS feed.

- Most torrent clients can automatically check this file every once in a while and automatically download the torrents within, and can be filtered based on various things like name, uploader, size, format, quality, or encoding, as long as it is listed in the information on the feed.


Check out the [RSS Guide](/tutorials/rss) for instructions on using it with qBittorrent. More sophesticated alternatives utilizing RSS are Taiga and Sonarr.


### Connectability

!!!info
Being connectable means that you accept "direct" connections from seeders or leechers. In a rough metaphor, your "address" (IP) is known, and your "door" (port) is open to meet new peers.
!!!

- In the BitTorrent protocol, a transaction between 2 peers can only happen if at least one of them is connectable (one "open door" is enough).

- When you attempt to download something from a peer, you must first establish the connection with them. If their port is open, then you can directly connect with them and start downloading. If their port is not open, you must wait for them to connect with you.

- While being connectible does not magically boost your upload speed, a properly configured setup increases the pool of potential peers to connect to. This will not make a difference with torrents that have hundreds of seeds, but can drastically increase upload speed on torrents with less seeds.

!!!info
Being non-connectable will still allow you to seed and leech, unless you are attempting to connect with another non-connectable peer. It also means you will get less upload next to a connectable seeder.
!!!

That said, you might be wondering on how to become connectable. This can be achieved relatively easily by port forwarding - which will be covered down below.

### What Is Port Forwarding?

- Your connection is assigned with a public "address" (IP address) by your ISP to your router. Your router will then assign a private (local) IP address to the devices that are connecting to it. On your devices, services which need to connect with another deivce over a network needs to have a "door" (port) opened (such as FTP on port 21, SSH & SFTP on port 22, HTTP on port 80, or HTTPS on port 443, etc.). The IP address and the port number combination (for example - http://192.168.0.1:8080/, where "192.168.0.1" is your private IP address, and "8080" is the port number) can be used to access the service in question.

- Similarly, your torrent client also needs to run on a port on your device's private IP address. However, for anyone on the internet to be able to "see" your torrent client through this port, it has to be forwarded/connected to the equivalent port on your public IP address.

- Metamorphically, think of your torrent client like a laser pointer. When it runs, it tries to shoot a beam of light (the connection) to another person, but is blocked by a "wall" - which is your router. If your router does not have an appropriate "hole" (port) prepared, nobody from the other side will be able to see the beam of light.

### How To Port Forward

!!!warning
This guide is not applicable to VPN users. If you are using one, please check with your VPN service provider and see if it allows port forwarding, as well as any instructions they might provide you with. If it does not, there is nothing you can do about it and it also has nothing to do with your router - either disable your VPN, or choose a different VPN service provider that allows port forwarding.
!!!

- The port used by your client can be found in Settings > Connection. Avoid using the [well-known ports](https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers#Well-known_ports) and the [registered ports](https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers#Registered_ports). To simplify - and preferably, use the ports from [49152 or higher](https://en.wikipedia.org/wiki/Ephemeral_port) to avoid interference with other services as well as being blocked by your ISP on some lower number ports.

- Use https://canyouseeme.org/ to check if this port is open. If it already is, you can skip this instruction. If you are already portforwarding via a VPN, often times, it will show your port as unopened. However, this is completely normal because those ports are ephemeral and only opened when you're actively uploading/downloading something.

!!!info
If you are already port forwarding via a VPN, often times, it will show your port as closed. However, this is completely normal since those ports are ephemeral and only opened up when you are actively uploading/downloading something.
!!!

The easy way to do this is with UPnP - though it is considered a security risk by some, this is something enabled by default on both your router and in qBittorrent. It will automatically open the port needed by your torrent client without needing to manually change router settings. You can find and enable this on your torrent client and your router's settings (by logging into it).

Alternatively, find your router manufacturer and model [in this list](https://portforward.com/router.htm) for a concise, step-by-step guide for your exact router model.

In case you have unknown issues preventing you from port forwarding despite correctly followed the instructions, try the following:

- Disable network rules such as flood protection, DDoS protection, traffic shaping, QOS etc. in your router settings. However, it is generally ill-advised due to security reasons.
- Sometimes port forwarding will fail if you're doing so through a proxy.
- Double check your torrent client configurations.
- Flush your DNS cache by typing "ipconfig /flushdns" in the Windows command prompt. For Linux distributions, it varies from each other slightly as well as having many ways to do so. A quick Google search should help.
- Also make sure to create inbound/outbound rules for your torrent client on your firewall settings.

!!!warning
Do not use port triggering - they are meant to be used for local network devices and those ports will be closed when they are not in use. Hence, they are not suitable for torrenting purpose.
!!!

### What Is a Seedbox?

A seedbox is a server - a computer in a data center with high bandwidth and upload/download speed, that can be used to store your torrents virtually and seed reliably around the clock. You can directly upload/download from the seedbox to your computer by using a file transfer protocol (FTP) client (such as WinSCP, FileZilla, Cyberduck, etc.). Since the seedbox is more likely to have a high upload speed compared to your internet connection as well as other peers in the swarm, it is more likely to be prioritized when seeding the bittorent packets. Typical advantages include:

- Utilize a faster download/upload speed than your internet connection.
- High uptime - a seedbox is on 24/7 seeding your torrents at high speed, that allows you to maintain a good ratio on private (and public) trackers.
- Avoiding copyright/DMCA problems, especially if you are not using a VPN.
- Many seedboxes also support third-party plugins such as sonarr, plex etc.

If you are somewhat tech-savvy, you can run your own virtual private server (VPS) or a dedicated server and use it as a virtual computer that can either be used as a seedbox, a VPN, or just about anything like your own personal computer. This is one of the more luxury options and tend to cost significantly more.

!!!info
Check out [/r/seedboxes](https://www.reddit.com/r/seedboxes/) for more information. They are geared towards private trackers so be sure to check for public tracker support, as well as unlimited bandwidth with any seedbox provider.
!!!

### Glossary

- Peer: Any person who is uploading/downloading a torrent.
- Swarm: The entire set of peers.
- Seed: A peer who has finished downloading the torrent and is now uploading it to other peers.
- Leech: A peer who is still downloading or someone who downloads more than they upload (slang).
- Seeding: Uploading to other peers in the swarm.
- Leeching: Downloading from other peers in the swarm.
- Ratio: The number of bytes of data you have seeded to others divided by the number of bytes of data you have downloaded from others. If you download a 2GB movie, and you have seeded 1.5GB, your ratio on that torrent is 0.75 (1.5/2.0).
- Torrent file: A small file with a .torrent extension that you download to begin torrenting. Contains metadata about the torrent (checksum, list of files and their sizes, etc).
- Checksum/hash: A fixed-length value that is calculated based on all the data in the complete torrent that is intended to be unique to the specific torrent. Think of it like the torrent's unique ID. Used to identify the torrent to trackers and DHT.
- Magnet link: a link containing at minimum the checksum of the torrent. May also contain the torrent name and/or one or more trackers. Since the magnet link does not contain torrent metadata, the .torrent file must be downloaded from the swarm first before the actual torrent can begin. Without any trackers, a magnet link requires DHT to find initial peers and begin downloading the .torrent file.
- Tracker: A server that clients talk to to get lists of peers from the swarm. Each torrent may have zero, one, or many trackers. Private trackers are websites that run their own tracker. All private torrents will use only that private tracker.
- Trackerless torrent: A torrent that does not use any trackers. Peers share lists of peers with each other instead.
- DHT: Distributed hash table, the basis of trackerless torrents. Basically it is a giant table that, as the name implies, is distributed among all torrent clients. This table basically functions like a giant distributed tracker of all torrents that use DHT.
- PEX: Peer exchange. This protocol allows peers on the same torrent to exchange lists of peers with each other. It is not as powerful as DHT because it cannot be used to join a new trackerless torrent; you have to know which peers to ask before you can ask.
- Announcing: The act of registering yourself as a peer for a torrent on a tracker. The tracker will then return a list of peers on the torrent back to your torrent client.
- Hash checking: Verifying the integrity of the downloaded files using the file's hash.

### Related Subreddits

- /r/trackers - For discussion of public and private torrent trackers. No requests of specific pirated titles.
- /r/Invites - Request and offer invites for private trackers that allow for invites to be distributed through such means.
- /r/OpenSignups - Announcements of private trackers that have opened up registration or applications. opentrackers.org is another great resource
- /r/VPN - Discussion of VPNs
- /r/VPNTorrents - Discussion of VPNs for the purpose of torrenting
- /r/torrents - For torrents dicussion and news
- /r/seedboxes - For discussion, sourcing, and recommendations about seedboxes.
