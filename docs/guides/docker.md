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

Docker, simply, is a platform for running applications. It allows you to run apps packaged into [containers](https://www.docker.com/resources/what-container/) and gives you the tools to quickly and easily deploy them, anywhere you want, efficiently and securely.

In this tutorial we will deploy a whole automation stack from a single configuration file!

The basic idea is that we will create a single "compose" file that tells the Docker engine what containers we want to download, and what configuration options we want to set for each one. A sample compose file is presented later in this guide, but we recommend reading through it, as you will need to change parts of it to match your setup. Take special note of `environment` variables, as those are essentially your settings options.

This guide will cover how to setup a Docker stack for common media automation programs, namely: a torrent client, a Usenet client, a VPN, Sonarr, Radarr, and Prowlarr. These applications work together to automatically download files from chosen indexers, based on user-defined filters.

!!!
This guide is primarily focused on basic setup and deployment of these applications. For information and help with configuring and using them, please check the relevant documentation, or ask in #questions on Discord. We will cover some basic use cases but your needs will vary depending on what indexers you have access to. Our goal here is to get you acquainted with Docker Compose, and give you a good jumping off point.
!!!
## Installing Docker

First we need to install Docker onto our system. Windows users can do so by downloading
[Docker Desktop](https://docs.docker.com/desktop/install/windows-install/), while Linux users can follow the instructions outlined
[here](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository), after which they can proceed onto the
[post-installation steps](https://docs.docker.com/engine/install/linux-postinstall).
Depending on how you've installed and configured Docker, you may need to append `sudo` before the commands listed in the rest of this guide.

## Preparing the VPN

For maximum privacy and security we'll be tunneling our Usenet and torrent clients through a VPN. Radarr, Sonarr and Prowlarr however won't go through it, as some trackers and indexers forbid this. This guide recommends Gluetun, but the process is similar for other VPN container options.

What we need is a configuration file, usually `.conf`, obtained from your chosen VPN provider. Wireguard is preferable and recommended over OpenVPN. This configuration file contains the info needed to allow Gluetun, (or your VPN client of choice,) to connect to your VPN provider's servers. 

The process for getting a config file varies based on your provider, so we recommend searching for it online, or your provider's website. [This page](https://github.com/qdm12/gluetun-wiki/tree/main/setup/providers) also contains per-provider instructions for getting a config file, as well as some other configuration that we will cover shortly. If possible, make sure to enable forwarding of one or more ports for use with torrent and usenet clients.

## Deploying everything using `docker compose`

Now that we've installed Docker and retrieved our VPN configuration, we can set up the stack. Create a new folder to house all everything compose related, for example `/home/user/projects/automation`. Inside this folder, make a new file named `compose.yaml`, and paste in the following:

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
#
# Generally, the volumes follow a `/host:/container` structure. The path before the colon is where the files are located on your system, and the path afterward is how the container will see them internally.
# For example, if we were to add '/user/home:/example' our container would see a directory called /example that would have all the files from /user/home.
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
# 3. Time zones
#
# All instances of `TZ` in the `environment` section should be changed to `TZ=Your/Timezone`. To find your timezone and the correct way to format it
# for Docker, you can check this Wikipedia article (https://en.wikipedia.org/wiki/List_of_tz_database_time_zones#List) and get the information
# from the "TZ identifier" column
#
# 4. Gluetun and VPN providers
#
# environment variables and setup instructions for each provider can be found at (https://github.com/qdm12/gluetun-wiki/tree/main/setup/providers)
# To help with readability, settings have been filled out to roughly match a configuration for ProtonVPN.
# 
# 5. Forwarded ports
# 
# Forwarding a port roughly means opening it to incoming traffic. This concept applies both to internal ports (ports between devices on your home network or between Docker containers,) and public ports (ports forwarded through your VPN or on your router.) 
# 
# We make references to both types in this guide. To clarify the difference: 
# Ports listed under `ports:` in the below file are internal (between Docker containers.)
# Forwarded ports anywhere in `environment:` refers to ports forwarded through your VPN

---
services:
  gluetun:
    container_name: gluetun
    image: qmcgaw/gluetun:latest 
    devices:
      - /dev/net/tun:/dev/net/tun
    environment: # You will need to edit these settings based on the instructions listed for your provider. See above. Fields not needed for your provider should be removed. 
      - VPN_SERVICE_PROVIDER= # See 4. Gluetun above
      - VPN_TYPE=wireguard # Do not change this unless you want to use OpenVPN
      - WIREGUARD_PRIVATE_KEY= # See 4. Gluetun above for how to find this!
      - WIREGUARD_ADDRESS= # Same as above!
      - VPN_PORT_FORWARDING=on # Turn this off if you are not using port forwarding
      - VPN_FORWARD_ONLY=on # Same as above!
      - VPN_PORT_FORWARDING_UP_COMMAND=/bin/sh -c 'wget -O- --retry-connrefused --post-data "json={\"listen_port\":{{PORTS}}}" http://127.0.0.1:8123/api/v2/app/setPreferences 2>&1' #This example up command will update qBittorrent with your forwarded port automatically. See 4. Gluetun for more information. Can also be removed if not using port forwarding.
    cap_add:
      - NET_ADMIN
      - NET_BIND_SERVICE
      - NET_RAW
    ports: # Anything using your VPN will need its ports added here to be reachable. 
      - 8123:8123 # qBittorrent WebUI
      - 8080:8080 # SABnzbd UI
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
      - TZ=Europe/London # Change this based on your timezone
      - WEBUI_PORT=8123
      - TORRENTING_PORT= # Set this to your forwarded port, or delete it and use VPN_PORT_FORWARDING_UP_COMMAND. Delete entirely if not using port forwarding.
    network_mode: container:gluetun
    depends_on:
      - gluetun
    volumes:
      - './qbittorrent:/config'
      - '/home/user/data:/data'
    restart: unless-stopped
  sonarr:
    image: lscr.io/linuxserver/sonarr:latest
    container_name: sonarr
    ports:
      - 8989:8989
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London # Change this based on your timezone
    volumes:
      - './sonarr:/config'
      - '/home/user/data:/data'
    restart: unless-stopped
  radarr:
    image: lscr.io/linuxserver/radarr:latest
    container_name: radarr
    ports:
      - 7878:7878
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London # Change this based on your timezone
    volumes:
      - './radarr:/config'
      - '/home/user/data:/data'
    restart: unless-stopped
  prowlarr:
    image: lscr.io/linuxserver/prowlarr:latest
    container_name: prowlarr
    ports:
      - 9696:9696
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London # Change this based on your timezone
    volumes:
      - './prowlarr:/config'
      - '/home/user/data:/data'
    restart: unless-stopped
```
==-

==- Some extra sample Gluetun configurations for different VPNs
```yml
# The following segments are intended to fully replace the Gluetun section in the above compose file.
#
# You are welcome to use these directly, but we still recommend reading the page for your provider at (https://github.com/qdm12/gluetun-wiki/tree/main/setup/providers)
#
# Most of these examples have further configuration options that you may wish to utilize, located at the link above.
# These are as basic as possible, while still detailing the important differences between providers.

---
# ProtonVPN complete example:

  gluetun:
    container_name: gluetun
    image: qmcgaw/gluetun:latest 
    devices:
      - /dev/net/tun:/dev/net/tun
    environment:
      - VPN_SERVICE_PROVIDER=protonvpn
      - VPN_TYPE=wireguard
      - WIREGUARD_PRIVATE_KEY= # This is the same for all Proton servers
      - VPN_PORT_FORWARDING=on # Left on in this case because paid Proton supports port forwarding
      - VPN_FORWARD_ONLY=on # Same as above
      - VPN_PORT_FORWARDING_UP_COMMAND=/bin/sh -c 'wget -O- --retry-connrefused --post-data "json={\"listen_port\":{{PORTS}}}" http://127.0.0.1:8123/api/v2/app/setPreferences 2>&1' 
    cap_add:
      - NET_ADMIN
    ports: # Anything using your VPN will need its ports added here to be reachable. 
      - 8123:8123 # qBittorrent WebUI
      - 8080:8080 # SABnzbd UI
#
# Mullvad complete example:
  
  gluetun:
    container_name: gluetun
    image: qmcgaw/gluetun:latest 
    devices:
      - /dev/net/tun:/dev/net/tun
    environment:
      - VPN_SERVICE_PROVIDER=mullvad
      - VPN_TYPE=wireguard
      - WIREGUARD_PRIVATE_KEY= # This is the same for all Proton servers
      - WIREGUARED_ADDRESSES= # `Address` value found in the VPN config file you downloaded earlier. See (https://github.com/qdm12/gluetun/discussions/805#discussioncomment-2026642) if you are having trouble. This page is also linked on the provider instructions for Mullvad.
      - OWNED_ONLY=yes # Only use VPN servers owned by Mullvad
    cap_add:
      - NET_ADMIN
    ports: # Anything using your VPN will need its ports added here to be reachable. 
      - 8123:8123 # qBittorrent WebUI
      - 8080:8080 # SABnzbd UI
#
# AirVPN complete example:
  gluetun:
    container_name: gluetun
    image: qmcgaw/gluetun:latest 
    devices:
      - /dev/net/tun:/dev/net/tun
    environment:
      - VPN_SERVICE_PROVIDER=airvpn
      - VPN_TYPE=wireguard
      - WIREGUARD_PRIVATE_KEY= # As with other providers, get this value from the config file you generated earlier.
      - WIREGUARD_PRESHARED_KEY= # Same as above
      - WIREGUARED_ADDRESSES= # Same as above      - OWNED_ONLY=yes
      - FRIEWALL_VPN_INPUT_PORTS= # Put any forwarded ports here
    cap_add:
      - NET_ADMIN
    ports: # Anything using your VPN will need its ports added here to be reachable. 
      - 8123:8123 # qBittorrent WebUI
      - 8080:8080 # SABnzbd UI
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

After all of that you'll want to go into `/home/user/projects/automation` and run `docker compose up -d`.  This will deploy all of the containers in a `-d`etached mode, but since it's your first time running them, Docker will pull the images for each of these containers. Any `docker compose` command must be run within the folder that contains `compose.yaml`.

Another useful command is `docker compose down`, which will shut down each of the containers.

By running in detached mode, we also block any terminal output from our containers. Some of these outputs contain vital information for setting up your container the first time, specifically the output from qBittorrent. To view the console output, you can run `docker logs <name of container>`, for example `docker logs qBittorrent`.

We recommend looking at the `man` page or official documentation for Docker to familiarize yourself with available commands. `docker pull`, `docker ps`, and `docker system prune` can be useful, for example. See documentation for info on what these do!

!!!warning
Older versions of Docker and Compose used `docker-compose` instead of `docker compose`. These commands are similar, but with notable differences. See [the github page for Compose](https://github.com/docker/compose) for instructions on installing the latest version manually if needed.
!!!

!!!warning
The default password for qBittorrent will be located in its console output/logs. See above.
!!!
## VPN Safety

Many VPN solutions require extra effort to make sure torrent and Usenet traffic doesn't accidentally leak outside. The compose file given already confines your torrent and Usenet client to your VPN's internal. Gluetun comes bundled with a killswitch, which will block traffic not routed through a VPN, so there should be no way for traffic to escape based on this setup. 

Note that if you use qBittorrent and its web UI, you can optionally bind all connections to the VPN network interface. This can be done by going to settings and navigating to the advanced tab all the way on the right. You should immediately see the options to bind your connections to a network interface, and to optionally bind your connections to the VPN IP. If using Gluetun, this should show up as `tun0`. This setting should not be necessary, but can provide peace of mind in the event user error causes multiple systems to fail. 

## Accessing your containers

Once everything is up and running, you should be able to access the Web UIs of your various Docker containers. Do so by opening a web browser and navigating to `http://ip:port`. For accessing a container on the same machine that's hosting it, `http://localhost:port` can be used. The `ip` field refers to the local machine ip (not your network's public ip). The `port` field is the port your container is configured to broadcast on, which is defined in `compose.yaml`. For example, qBittorrent will be at `http://localhost:8123` based on the file provided in this guide.

!!!
A machine's `ip` can be found by running `ip address`, `ifconfig` or `hostname -I | awk '{print $1}'` on Linux. You are looking for the result listed as `inet` corresponding with your network adapter (will be listed as `enp_something` in most cases.) The third command should narrow the results to the machine's ip in case the first results are confusing. 
!!!

Most setup from this point on will be done through the relevant Web UIs. 

## Adding your clients to Sonarr and Radarr

Begin by pulling up Sonarr and Radarr in your browser as discussed in the previous step. We are going to give them the information needed to start automation!

Go to settings, navigate to download clients, and pick whatever clients you use (SABnzbd and qBittorrent.) Follow on-screen instructions as necessary. You will also want to configure your indexers here if adding them directly. If using Prowlarr you will need to configure those first, and then come back to add them.

## Prowlarr

Prowlarr sits in-between Sonarr/Radarr and your indexers, and take the burden of API engagement away from them. It takes requests for content from Sonarr/Radarr and translates them into API calls for specific files. 

For example, let's say you wanted to download a movie. You'd add the title to Sonarr, Sonarr asks Prowlarr for the file, Prowlarr looks through your indexers and returns download options, Sonarr picks one, and sends it to your usenet or torrent client for download. 

As with before, configuration is done through the Web UI. Add your indexers as instructed on-screen, and add Prowlarr to your download clients or Sonarr/Radarr. 

## Finishing Touches

As stated previously in this guide, we highly recommend familiarizing yourself further with Docker and its commands. The purpose of this guide is to get you up and running as fast as possible, while minimizing areas of confusion. Docker, and Docker Compose, are very powerful, and can be used to run an entire home media setup. There are several sections in this guide that require user input, so make sure to check those first if you are having trouble!

The `compose.yaml` file provided will create seven total containers, and all of these have their own documentation pages available online; it is worth keeping them bookmarked in case of future issues. Searching for the `image` name listed is usually the most direct way to locate them if needed. 

!!!
Questions or support requests for this guide should be directed to the #questions channel in Snackbox.
!!!
