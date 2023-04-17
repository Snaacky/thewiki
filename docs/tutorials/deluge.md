---
label: Deluge
description: Learn how to install Deluge
image: https://user-images.githubusercontent.com/78981416/232306295-0e123ed8-c837-4d19-b8a8-cb50229709fe.png
---

# Deluge

[Deluge](https://deluge-torrent.org/) is a free, open source, and cross platform bittorrent client, primarily used by seedboxes for racing. Works as both a stand alone client and a thin client for a remote server. Visit <https://dev.deluge-torrent.org/wiki/UserGuide> for docs. 

## Install

1. For Windows, go to <https://ftp.osuosl.org/pub/deluge/windows/?C=M;O=D>, then download the latest `.exe` file.

    ![](https://user-images.githubusercontent.com/78981416/232305635-111a14c6-7dba-4ff1-a4b4-90e6dc0e4808.png)

2. For macOS, go to <https://ftp.osuosl.org/pub/deluge/mac_osx/?C=M;O=D>, then download the latest `.dmg` file.

    ![](https://user-images.githubusercontent.com/78981416/232306677-efd31d7f-57c6-4a19-bf0f-62e354ea0c99.png)

3. For Linux, go to <https://dev.deluge-torrent.org/wiki/Download#Linux>

## ltconfig

[Deluge-ltconfig](https://github.com/ratanakvlun/deluge-ltconfig/releases) is a popular plugin used for racing torrents on seedboxes. It modifies the libtorrent settings directly and comes with presets.

### Install ltconfig

1. Most seedboxes offer Deluge `1.3.15`. So that's what we'll use.
2. Download [`ltConfig-0.3.1-py2.7.egg`](https://github.com/ratanakvlun/deluge-ltconfig/releases/tag/v0.3.1)
3. Click on `Preferences`.

    ![](https://user-images.githubusercontent.com/78981416/232335499-4be73157-341d-4bbc-b463-0c7380bbdaa1.png)

4. Click on `Plugins` -> `Install` and then upload the `ltConfig-0.3.1-py2.7.egg` you downloaded earlier.

    ![](https://user-images.githubusercontent.com/78981416/232335563-7450b9e2-1427-4daf-8f6f-17963f68d047.png)

5. `ltConfig` will appear in your plugin list, select it, apply, and then it'll show up in `Categories` pane.

    ![](https://user-images.githubusercontent.com/78981416/232335700-665a5dd9-fd7c-4509-b07a-d5abd682ec02.png)

6. Check `Apply settings on startup`, select the preset you want which in this case is `High Performace Seed`, click load preset, apply, and you're done.

    ![](https://user-images.githubusercontent.com/78981416/232335845-f8464500-c2c8-43dd-9149-2ee5f9579686.png)