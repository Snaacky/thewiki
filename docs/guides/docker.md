---
label: Docker automation
order: -2
description: Learn how to deploy a Docker stack for common media automation software
image: /static/tohsaka.gif
author:
  - name: "guyman"
    avatar: "https://github.com/Snaacky/thewiki/assets/78981416/2045b3c7-90a5-40ce-9d81-a81baa421227"
    link: "https://github.com/guyman624"
  - name: "bankai_phonk"
  - name: "perry"
---

# Setting up a Docker automation stack

Docker is a piece of software that allows containerize applications, avoiding dependency managment and installation time, and can carry around a solution that's easily deployable anywhere you go.

In this tutorial we will deploy a whole automation stack from a single configuration file.

The basic idea is that we will create a single "compose" file that tells the Docker engine what containers we want to download, and what configuration options we want to set for each one. A sample compose file is presented later in this guide, but we recommend reading through it, as you will need to change parts of it to match your setup. Take special note of `environment` variables, as those are essentially your settings options.

This guide will cover how to setup a Docker stack for common media automation programs, namely: a torrent client, a usenet client, a VPN with a killswitch, Sonarr, Radarr, Autobrr, and Prowlarr. These applications work together to automatically download files from chosen indexers, based on user-defined filters.

This guide is primarily focused on basic setup and deployment of these applications. For information and help with configuring and using them, please check the relevant documentation, or ask in #questions on Discord. We will cover some basic usecases but your needs will vary depending on what indexers you have access to. Our goal here is to get you acquainted with Docker Compose, and give you a good jumping off point.

## Installing Docker

First we need to install Docker onto our system. Windows users can do so by downloading
[Docker Desktop](https://docs.docker.com/desktop/install/windows-install/), while Linux users can follow the instructions outlined
[here](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository), after which they can proceed onto the
[post-installation steps](https://docs.docker.com/engine/install/linux-postinstall).
Depending on how you've installed and configured Docker, you may need to append `sudo` before the commands listed in the rest of this guide.

<!--- 
i dont think this is necessary anymore...
## Setting up a Docker network

Now we'll be setting up a network so that our containers can communicate with each other. This step's quite simple, it's just `docker network create guide`. A list of all your current networks can be seen with `docker network ls`.
-->

## Preparing the VPN

For maximum privacy and security we'll be tunnelling our usenet and torrent clients through a VPN. Radarr, Sonarr and Prowlarr however won't go through it, as some trackers and indexers forbid this. This guide reccomends Gluetun, but the process is similar for other VPN container options.

What we need is a configuration file, usually `.conf`, obtained from your chosen VPN provider. Wireguard is preferable and recommended over OpenVPN. This configuration file contains the info needed to allow Gluetun, (or your VPN client of choice,) to connect to your VPN provider's servers. 

The process for getting a config file varies based on your provider, so we recommend searching for it online, or your provider's website. [This page](https://github.com/qdm12/gluetun-wiki/tree/main/setup/providers) also contains per-provider instructions for getting a config file, as well as some other configuration that we will cover shortly. If possible, make sure to enable forwarding of one or more ports for use with torrent and usenet clients.

## Deploying everything using `docker compose`

Now that we've gotten everything ready we can finally set up the stack. Create a new folder to house all everything compose related, for example `/home/user/projects/automation`. Inside this folder, make a new file named `compose.yaml`, and paste in the following:

==- compose.yaml
```yml
# Some notes about some important things so that they don't need to be repeated for every container.
#
#
# 1. Docker Compose's `volume parameter`
#
# The `volume` parameter lets you mount directories from your host system to the Docker container so that information can persist upon bringing a container
# down or up. Create these directories where you want and add them for each container following the template.
# See (https://docs.docker.com/engine/storage/) for more information on giving your containers access to storage locations.
# Generally, the structure follows a `/path/to/your/data:/path/your/container/sees` structure, where the path after the colon is how the folders you add will appear from the perspective of your container.
#
# Following the recommend directory structure from before, we've already filled out the volume mount settings, so that every service will have its
# own directory for its compose files, all in the form of `/home/user/projects/automation/<service name>`.
#
# This can of course be changed if you're experienced with Docker and would prefer to use custom mount locations.
#
# 2. Values for PUID and PGID
#
# These values refer to the host OS user's UID and GID, respectively. You can find this out by entering `id $user` in your terminal.
#
# 3. Timezones
#
# All instances of `TZ` in the `environment` section should be changed to `TZ=Your/Timezone`. To find your timezone and the correct way to format it
# for Docker, you can check this Wikipedia article (https://en.wikipedia.org/wiki/List_of_tz_database_time_zones#List) and get the information
# from the "TZ identifier" column
#
# 4. Gluetun and VPN providers
#
# environment variables and setup instructions for each provider can be found at (https://github.com/qdm12/gluetun-wiki/tree/main/setup/providers)
# To help with readability, settings have been filled out to roughly match a configuration for ProtonVPN.

---
version: "3.8"
services:
  gluetun:
    container_name: gluetun
    image: qmcgaw/gluetun:latest
    environment: # You will need to edit these settings based on the instructions listed for your provider. See above. Fields not needed for your provider can be removed.
      - VPN_SERVICE_PROVIDER= # Left blank intentionally. Put your provider here!
      - VPN_TYPE=wireguard # Do not change this unless you want to use OpenVPN
      - WIREGUARD_PRIVATE_KEY= # See 4. Gluetun for how to find this!
      - WIREGUARD_ADDRESS= # Same as above!
      - VPN_PORT_FORWARDING=on # This is mostly only useful for ProtonVPN
      - VPN_FORWARD_ONLY=on # Same as above!
      - VPN_PORT_FORWARDING_UP_COMMAND= # This can be useful to dynamically update your forwarded port in other programs if it isn't static.
    cap_add:
      - NET_ADMIN
      - NET_BIND_SERVICE
      - NET_RAW
    ports: # Anything using your VPN will need its ports added here to be reachable. 
      - 8123:8123 # qBittorrent WebUI
      - 8080:8080 # SABnzbd UI
      - 7474:7474 # Autobrr
    networks: 
      - guide
  sabnzbd:
    container_name: sabnzbd
    image: lscr.io/linuxserver/sabnzbd:latest
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London # Change this based on your timezone
    depends_on:
      - gluetun
    volumes:
      - './sabnzbd:/config'
      - '/home/user/data:/data'
    network_mode: container:gluetun
    restart: unless-stopped
  qbittorrent:
    container_name: qbittorrent
    image: linuxserver/qbittorrent:latest
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC # Change this based on your timezone
      - WEBUI_PORT=8123
      - TORRENTING_PORT= # Set this to your forwarded port, or delete it and use VPN_PORT_FORWARDING_UP_COMMAND
    ports:
      - 8123:8123
    network_mode: container:gluetun
    depdens_on:
      - gluetun
    restart: unless-stopped
    volumes:
      - ./qbittorrent:/config
      - /home/user/data:/data
    networks:
      - guide
  autobrr:
    container_name: autobrr
    image: ghcr.io/autobrr/autobrr:latest
    user: 1000:1000
    environment:
      - TZ=${TZ} # Change this based on your timezone
    depends_on:
      - gluetun
    volumes:
      - './autobrr:/config'
    network_mode: container:gluetun
    restart: unless-stopped
    networks:
      - guide
  sonarr:
    image: lscr.io/linuxserver/sonarr:latest
    container_name: sonarr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London # Change this based on your timezone
    ports:
      - 8989:8989
    volumes:
      - './sonarr:/config'
      - '/home/user/data:/data'
    networks:
      - guide
    restart: unless-stopped
  radarr:
    image: lscr.io/linuxserver/radarr:latest
    container_name: radarr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London # Change this based on your timezone
    ports:
      - 7878:7878
    volumes:
      - './radarr:/config'
      - '/home/user/data:/data'
    ports:
      - 7878:7878
    networks:
      - guide
    restart: unless-stopped
  prowlarr:
    image: lscr.io/linuxserver/prowlarr:latest
    container_name: prowlarr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London # Change this based on your timezone
    ports:
      - 9696:9696
    volumes:
      - './prowlarr:/config'
      - '/home/user/data:/data'
    networks:
      - guide
    restart: unless-stopped
networks:
  guide:
    external: true
```
==-

We would also recommend sorting your media-related folders in the following way for ease of use:
- `/home/user/data/torrents/` - where you torrent client will save files.
- `/home/user/data/usenet` - where your usenet client will save files.
- `/home/user/data/media` - where your hardlinked files will reside, so that they can be read by your media server (Plex/Jellyfin/etc.)
  - `/home/user/data/media/tv` - where Sonarr will hardlink files.
  - `/home/user/data/media/movies` - where Radarr will hardlink files.
  - You can take this concept further and make more subfolders in `media` if you'd like to have one more than one Sonarr or Radarr instance. People
  sometimes do this if they'd like both a 4K and an HD version of a piece of media, as both the *arrs can only hardlink one version at a time.

Paths for Docker are extremely important as configuring them incorrectly can break hardlinking and cause you to waste space!

After all of that you'll want to go into `/home/user/projects/automation` and run `docker compose up -d`.  This will deploy all of the containers
in a `-d`etached mode, but since it's your first time running them, Docker will pull the images for each of these containers. Any `docker compose` command must be run within the folder that contains `compose.yaml`.

Another useful command is `docker compose down`, which will shut down each of the containers.

By running in detached mode, we also block any terminal output from our containers. Some of these outputs contain vital information for setting up your container the first time, specifically the output from Qbittorrent. To view the console output, you can run `docker logs <name of container>`, for example `docker logs qbittorrent`.

We recommend looking at the `man` page or official documentation for Docker to familiarize yourself with available commands. `docker pull`, `docker ps`, and `docker system prune` can be useful, for example. See documentation for info on what these do!

## Setting up a killswitch

The information below is left only as a resource for users deviating from this guide and using a Wireguard container for their vpn. Gluetun contains killswitch functionality out of the box and should not require any manual intervention.

DO NOT FOLLOW THESE SETTINGS IF YOU ARE USING GLUETUN AS ADVISED!

==- If using a Wireguard container instead of Gluetun, a killswitch will not yet be implemented. That's where this bit comes in.

```bash
PostUp = DROUTE=$(ip route | grep default | awk '{print $3}'); HOMENET=192.168.1.0/24; HOMENET2=10.0.0.0/8; HOMENET3=172.16.0.0/12; ip route add $HOMENET3 via $DROUTE; ip route add $HOMENET2 via $DROUTE; ip route add $HOMENET via $DROUTE; iptables -I OUTPUT -d $HOMENET -j ACCEPT; iptables -A OUTPUT -d $HOMENET2 -j ACCEPT; iptables -A OUTPUT -d $HOMENET3 -j ACCEPT; iptables -A OUTPUT ! -o %i -m mark ! --mark $(wg show %i fwmark) -m addrtype ! --dst-type LOCAL -j REJECT
PreDown = HOMENET=192.168.1.0/24; HOMENET2=10.0.0.0/8; HOMENET3=172.16.0.0/12; ip route del $HOMENET3 via $DROUTE; ip route del $HOMENET2 via $DROUTE; ip route del $HOMENET via $DROUTE; iptables -D OUTPUT ! -o %i -m mark ! --mark $(wg show %i fwmark) -m addrtype ! --dst-type LOCAL -j REJECT; iptables -D OUTPUT -d $HOMENET -j ACCEPT; iptables -D OUTPUT -d $HOMENET2 -j ACCEPT; iptables -D OUTPUT -d $HOMENET3 -j ACCEPT
```

You'll want to copy it and paste it into `/home/user/projects/automation/wireguard/wg0.conf`, right between the `DNS` and `Peer` lines. You should
also keep in mind that you need to check what your home network subnet is for `HOMENET`, don't just blindly use the one in the code block as it can
be different for each person. Your file should look something like this by the end:

```bash
[Interface]
Address = xxx.xxx.xxx.xxx/xxx, abcd:abcd:abcd:abcd:abcd:abcd:abcd:abcd/128
PrivateKey = private-key
MTU = 1320
DNS = xxx.xxx.xxx.xxx, abcd:abcd:abcd:abcd::1
PostUp = DROUTE=$(ip route | grep default | awk '{print $3}'); HOMENET=192.168.0.0/16; HOMENET2=10.0.0.0/8; HOMENET3=172.16.0.0/12; ip route add $HOMENET3 via $DROUTE; ip route add $HOMENET2 via $DROUTE; ip route add $HOMENET via $DROUTE; iptables -I OUTPUT -d $HOMENET -j ACCEPT; iptables -A OUTPUT -d $HOMENET2 -j ACCEPT; iptables -A OUTPUT -d $HOMENET3 -j ACCEPT;  iptables -A OUTPUT ! -o %i -m mark ! --mark $(wg show %i fwmark) -m addrtype ! --dst-type LOCAL -j REJECT;

PreDown = DROUTE=$(ip route | grep default | awk '{print $3}'); HOMENET=192.168.0.0/16; HOMENET2=10.0.0.0/8; HOMENET3=172.16.0.0/12; ip route del $HOMENET3 via $DROUTE; ip route del $HOMENET2 via $DROUTE; ip route del $HOMENET via $DROUTE; iptables -D OUTPUT ! -o %i -m mark ! --mark $(wg show %i fwmark) -m addrtype ! --dst-type LOCAL -j REJECT; iptables -D OUTPUT -d $HOMENET -j ACCEPT; iptables -D OUTPUT -d $HOMENET2 -j ACCEPT; iptables -D OUTPUT -d $HOMENET3 -j ACCEPT

[Peer]
PublicKey = public-key
PresharedKey = public-key
Endpoint = xxx.xxx.xxx.xxx:xxxx
AllowedIPs = 0.0.0.0/0, ::/0
PersistentKeepalive = 15
```

Once you've done that run `docker compose up -d` once more and you're all ready to go.
==-
Note that if you use qBittorrent and its web UI you can optionally bind all connections to the VPN network inteface. This can be done by
going to settings and navigating to the advanced tab all the way on the right. You should immediately see the options to bind your connections to a
network interface, and to optionally bind your connections to the VPN IP. If using Gluetun, this should show up as `tun0`.

## Accessing your containers

Once everything is up and running, you should be able to access the Web UIs of your various Docker containers. Do so by opening a web browser and navigating to `http://ip:port`. For accessing a container on the same machine that's hosting it, `http://localhost:port` or `https://localhost:port` can be used. The `ip` field refers to the local machine ip (not your network's public ip) which can be found by running `ipconfig` on Windows and `ifconfig` or `ip address` on Linux. The `port` field is the port your container is configured to broadcast on, which is defined in `compose.yaml`. For example, qBittorrent will be at `http://localhost:8123` based on the file provided in this guide.

Most setup from this point on will be done through the relevant Web UIs. 

## Adding your clients to Sonarr and Radarr

Begin by pulling up Sonarr and Radarr in your browser as discussed in the previous step. We are going to give them the information needed to start automation!

Go to settings, navigate to download clients, and pick whatever clients you use (SABnzbd, Transmission, qBitorrent, Deluge, etc.) Follow on-screen instructions as necessary. If unclear, the clients recommended and configured in this guide are qBittorrent and SABnzbd. You will also want to configure your indexers here if adding them directly. If using Prowlarr you will need to configure those first, and then come back to add them.

## Prowlarr and Autobrr

These programs sit in-between Sonarr/Radarr and your indexers, and take the burden of API engagement away from them. They take requests for content from Sonarr/Radarr and translate them into API calls for specific files. Autobrr can be used to automatically download files announced over irc, and Prowlarr should be used for more specific searches. 

For example, let's say you wanted to download `Minions`. You'd add the title to Sonarr, Sonarr asks Prowlarr for the file, Prowlarr looks through your indexers and returns download options, Sonarr picks one, and sends it to your usenet or torrent client for download. 

As with before, configuration is done through the Web UI. Add your indexers as instructed on-screen, and add Prowlarr or Autobrr to your download clients or Sonarr/Radarr. 

## Finishing Touches

As stated previously in this guide, we highly recommend familiarizing yourself further with Docker and its commands. The purpose of this guide is to get you up and running as fast as possible, while minimizing areas of confusion. Docker, and Docker Compose, are very powerful, and can be used to run an entire home media setup. There are several sections in this guide that require user input, so make sure to check those first if you are having trouble!

The `compose.yaml` file provided will create seven total containers, and all of these have their own documentation pages available online; it is worth keeping them bookmarked in case of future issues. Searching for the `image` name listed is usually the most direct way to locate them if needed. Questions or support requests for this guide should be directed to the #questions channel in Snackbox.
