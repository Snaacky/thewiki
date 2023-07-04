---
label: Glossary
description: A glossary of common video, audio, subtitles, and more related terms
image: https://user-images.githubusercontent.com/78981416/246030855-c784e898-74a6-4fae-bbfa-1bb4a6bdbac1.gif
order: -4
---

# Glossary

## General

| Term         | Meaning                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Codec        | A codec is a software or hardware algorithm that compresses and decompresses audio or video data. There are different types of codecs, each with its own compression algorithms and characteristics. Lossless codecs preserve the original quality of the data during compression and decompression, while lossy codecs achieve higher compression ratios by discarding certain information that is considered less important or perceptually less noticeable. Generally, a codec on its own does not determine the quality. |
| Bitrate      | Refers to the amount of data that is processed or transmitted per unit of time. Higher bitrates generally produce higher-quality audio or video, but they also require more storage space and bandwidth for transmission. However, it's worth noting that the relationship between bitrate and quality is not always linear. Factors like the codec, compression algorithm, resolution, and content complexity also play significant roles in determining the perceived quality.                                             |
| Transparency | Transparency is a term used to describe the perceived quality of a lossy file. A lossy file is considered transparent if a human cannot tell the difference between a lossy file and a lossless file.                                                                                                                                                                                                                                                                                                                        |

## Video

| Term  | Meaning                                                                       |
| ----- | ----------------------------------------------------------------------------- |
| AV1   | Video codec used to compress videos. Currently in early stages of development |
| H.264 | Video codec used to compress videos                                           |
| H.265 | Video codec used to compress videos. Successor to H.264                       |
| x264  | Video encoder used to encode videos into H.264                                |
| x265  | Video encoder used to encode videos into H.265                                |

## Audio

| Term               | Meaning                                                                     |
| ------------------ | --------------------------------------------------------------------------- |
| AAC                | Lossy audio codec used by several official streaming sites                  |
| Dolby Atmos        | An extension of Dolby TrueHD. Cannot be converted to any other format       |
| Dolby Digital      | Proprietary Lossy audio codec. Commonly used by official streaming sites    |
| Dolby Digital Plus | Proprietary Lossy audio codec. Commonly used by official streaming sites    |
| Dolby TrueHD       | Proprietary lossless audio codec found in BluRays                           |
| DTS-HD             | Proprietary Lossy audio codec                                               |
| DTS-HD MA          | Proprietary lossless audio codec found in BluRays                           |
| DTS:X              | An extension of DTS-HD MA. Cannot be converted to any other format          |
| FLAC               | Fastest and most widely supported lossless audio codec                      |
| MP3                | Lossy audio codec generally used for music, is beaten by newer lossy codecs |
| Opus               | Open lossy audio codec, currently the best one                              |
| PCM                | Uncompress lossless audio codec generally found in BluRays                  |
| Vorbis             | Outdated lossy audio codec                                                  |

## Torrent

| Term            | Meaning                                                                                                                                                                                            |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| BitTorrent      | A peer-to-peer (P2P) file-sharing protocol used for distributing large amounts of data over the internet, where files are broken into small pieces and shared among multiple users simultaneously. |
| Buffer          | The difference between the amount of data a user has uploaded and downloaded on a private tracker                                                                                                  |
| DHT             | A decentralized method used by BitTorrent to track peers without relying on a central server, enabling peer discovery and file sharing even if the tracker is unavailable                          |
| Freeleech       | A status assigned to certain torrents on a private tracker where the downloaded data does not count towards a user's download ratio, encouraging increased participation and sharing               |
| Neutral leech   | A status assigned to certain torrents on a private tracker where none of the data uploaded or downloaded is counted                                                                                |     
| Hit and run     | A term used to describe when a user downloads a file from a private tracker but fails to continue seeding it to the required ratio or for a specified period                                       |
| Leecher         | A user who is in the process of downloading a file but has not yet completed the download or is not actively uploading (sharing) the downloaded pieces with others                                 |
| Magnet link     | A type of hyperlink used in BitTorrent that allows users to directly connect to the tracker and peers without needing to download a separate torrent file                                          |
| Peer            | An individual computer or device connected to a P2P network that participates in sharing and downloading files                                                                                     |
| Piece           | A small part of a file that is broken down during the BitTorrent process, allowing simultaneous downloading and uploading of different pieces from different peers                                 |
| Private tracker | A BitTorrent tracker that requires an invitation to join. Advantages of private trackers include speed, retention, selection, and quality control                                                  |
| Public tracker  | A BitTorrent tracker that allows any user to join and participate in file sharing without requiring registration or authentication                                                                 |
| Ratio           | The ratio between the amount of data uploaded by a user and the amount downloaded. Private trackers often enforce minimum ratio requirements to ensure fair sharing among users.                   |
| Seedbox         | A dedicated remote server optimized for high-speed downloading and seeding of torrents                                                                                                             |
| Seeder          | A user who has completed downloading a file and continues to share it with other users by uploading the file pieces they possess                                                                   |
| Swarm           | The collective group of peers (both seeders and leechers) participating in the distribution of a particular file through BitTorrent                                                                |
| Torrent client  | Software or application used to download and upload files using the BitTorrent protocol                                                                                                            |
| Torrent file    | A small file that contains metadata about the files to be shared and the tracker information required to initiate and coordinate the downloading process                                           |
| Tracker         | A server or web service that assists in coordinating the communication between peers in a BitTorrent network by keeping track of which peers possess which pieces of a file                        |
| Racing          | Being the first in a swarm to maximize upload                                                                                                                                                      |
