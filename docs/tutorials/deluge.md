---
label: Deluge
description: Learn how to install Deluge
image: https://user-images.githubusercontent.com/78981416/232306295-0e123ed8-c837-4d19-b8a8-cb50229709fe.png
---

# Deluge

[Deluge](https://deluge-torrent.org) is a free, open-source, and cross-platform BitTorrent client. It is often used by seedboxes for racing and works as both a standalone client and a thin client for a remote server.

## Installation

+++ 🖥️ Windows

1. Download the latest `.exe` version of [Deluge](https://ftp.osuosl.org/pub/deluge/windows/?C=M;O=D)
2. Run the installer and follow the on-screen instructions to complete installation

![Latest 64-bit `.exe` for Deluge](https://user-images.githubusercontent.com/78981416/232305635-111a14c6-7dba-4ff1-a4b4-90e6dc0e4808.png)

+++ 🍎 macOS

1. Download the latest `.dmg` version of [Deluge](https://ftp.osuosl.org/pub/deluge/mac_osx/?C=M;O=D)
2. Run the installer and follow the on-screen instructions to complete installation

![Latest x64 `.dmg` for Deluge](https://user-images.githubusercontent.com/78981416/232306677-efd31d7f-57c6-4a19-bf0f-62e354ea0c99.png)

+++ 🐧 Linux

1. Download the latest version of [Deluge](https://dev.deluge-torrent.org/wiki/Download#Linux) for your distro
2. Run the installer and follow the on-screen instructions to complete installation

+++

## Configuration

### ltconfig

[ltConfig](https://github.com/ratanakvlun/deluge-ltconfig) is a popular plugin used for racing torrents on seedboxes. It modifies the libtorrent settings directly and comes with presets.

1. Download the correct version of [ltConfig](https://github.com/ratanakvlun/deluge-ltconfig/releases) for your version of Deluge:
    - Deluge v2.x or later: [!button size="s" variant="secondary" icon="download" text="v2.0.0"](https://github.com/ratanakvlun/deluge-ltconfig/releases/download/v2.0.0/ltConfig-2.0.0.egg)
    - Deluge v1.x: [!button size="s" variant="secondary" icon="download" text="v0.3.1"](https://github.com/ratanakvlun/deluge-ltconfig/releases/download/v0.3.1/ltConfig-0.3.1-py2.7.egg)
2. Launch Deluge. In the top bar, go to **Edit** -> **Preferences**
3. Under **Plugins**, click on **Install** and select the ltConfig file
4. Enable ltConfig from your plugin list and hit **Apply**
5. Under **ltConfig**, check **Apply settings on startup**. For racing, select the *High Performance Seed* preset -> **Load preset** -> **Apply**

==- :icon-video: Video tutorial

[!embed text="Installing ltConfig on Windows"](/static/torrenting/deluge/installing_ltconfig.mp4)

==-
