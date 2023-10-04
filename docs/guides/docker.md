---
label: Docker automation
order: -2
description: Learn how to deploy a Docker stack for common media automation software.
image: /static/tohsaka.gif
---

# Setting up a Docker automation stack

Docker is a piece of software that allows you to virtualize applications into containers, so that you don't have to deal with dependency
management and can carry around a solution that's easily deployable anywhere you go.

This guide will cover how to setup a Docker stack for common media automation programs, namely: a torrent client, a usenet client, a
VPN with a killswitch, Sonarr, Radarr, and Jackett.

## Installing Docker

First we need to install Docker onto our system. Windows users can do so by downloading
[Docker Desktop](https://docs.docker.com/desktop/install/windows-install/), while Linux users can follow the instructions outlined
[here](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository), after which they can proceed onto the
[post-installation steps](https://docs.docker.com/engine/install/linux-postinstall).

## Setting up a Docker network

Now we'll be setting up a network so that our containers can communicate with each other. This step's quite simple, it's just `docker network create guide`. A list of all your current networks can be seen with `docker network ls`.

## Generating a Wireguard configuration file

For maximum privacy and security we'll be tunnelling our usenet and torrent clients through a VPN. Radarr, Sonarr and Jackett however won't go
through that, as some trackers and indexers might not like it if you send requests from a random non-whitelisted IP.

The process for getting a configuration file is different from provider to provider, so we'd recommend looking around your provider's website,
forum and support articles if you can't find it immediately.

Once you've generated a configuration file we'd also recommend carrying out port forwarding so that you're connectable.

## Deploying everything using `docker compose`

Now that we've gotten everything ready we can finally set up the stack. Paste the following into a file called `docker-compose.yml`. We'd recommend
this file being a directory that is purely for this Docker stack, such as `/home/user/projects/automation/`.

We would also recommend sorting your media-related folders in the following way for ease of use:
- `/home/user/data/torrents/` - where you torrent client will save files.
- `/home/user/data/usenet` - where your usenet client will save files.
- `/home/user/data/media` - where Sonarr and Radarr will hardlink files so that they can be read by Plex/Jellyfin/etc. later on.

```yml
# Some notes about some important things so that they don't need to be repeated for every container.
#
# 1. The usage of a VPN
#
# The usenet and torrent clients sit behind a Wireguard VPN, if you don't want to mimic that setup, you can remove the Wireguard container.
#
# However if you do remove it you'll need to edit the file a bit for everything to work.
#
# Firstly, remove `depends_on:` and `network_mode:` from the Transmission and SABnzbd containers. After that you'll need to move the `ports:`
# section from the Wireguard container and add it to the Transmission and SABnzbd containers. The unused "default Wireguard listening port"
# line can also be safely removed.
#
# 2. Docker Compose's `volume parameter`
#
# The `volume` parameter lets you mount directories from your host system to the Docker container so that information can persist upon bringing a container
# down or up.
#
# Following the recommend directory structure from before, we've already filled out the volume mount settings, so that every service will have it's
# own directory for it's compose files, all in the form of `/home/user/projects/automation/<service name>`.
#
# This can of course be changed if you're experienced with Docker and would prefer to use custom mount locations.
#
# 3. Values for PUID and PGID
#
# These values refer to the host OS user's UID and GID, respectively. You can find this out by entering `id $user` in your terminal.
#
# 4. Timezones
#
# All instances of `TZ` in the `environment` section should be changed to `TZ=Your/Timezone`. To find your timezone and the correct way to format it
# for Docker, you can check this Wikipedia article (https://en.wikipedia.org/wiki/List_of_tz_database_time_zones#List) and get the information
# from the "TZ identifier" column

---
version: "3.8"
services:
  wireguard:
    container_name: wireguard
    image: linuxserver/wireguard
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1
      - net.ipv6.conf.all.disable_ipv6=0
    ports: # Port openings are handled through the VPN container when a VPN sits in front of the container.
      - '51820:51820/udp' # Default WireGuard listening port
      - '8080:8080' # SABnzbd UI
      - '9091:9091' # Transmission ui
	    - '7474:7474' # Autobrr
    dns:
      - 1.1.1.1
    volumes:
      - './wireguard:/config'
      - '/lib/modules:/lib/modules' # Don't touch this unless you know what you're doing!
    networks:
      - guide
    restart: unless-stopped
  sabnzbd:
    container_name: sabnzbd
    image: lscr.io/linuxserver/sabnzbd:latest
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London
    depends_on:
      - wireguard
    volumes:
      - './sabnzbd:/config'
      - '/home/user/data/usenet:/downloads'
    network_mode: "service:wireguard"
    restart: unless-stopped
  transmission:
    image: lscr.io/linuxserver/transmission:latest
    container_name: transmission
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London
      - PEERPORT=1000 # Replace this with the port that you forwarded during the `Generating a Wireguard configuration file` step.
      - TRANSMISSION_RPC_PORT=9091
    depends_on:
      - wireguard
    volumes:
      - './transmission:/config'
      - '/home/user/data/torrents:/downloads'
    network_mode: "service:wireguard"
    restart: unless-stopped
  autobrr:
    container_name: autobrr
    image: ghcr.io/autobrr/autobrr:latest
    user: 1000:1000
    environment:
      - TZ=${TZ}
    depends_on:
      - wireguard
    volumes:
      - './autobrr:/config'
    network_mode: "service:wireguard"
    restart: unless-stopped
  sonarr:
    image: lscr.io/linuxserver/sonarr:latest
    container_name: sonarr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London
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
      - TZ=Europe/London
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
  jackett:
    image: lscr.io/linuxserver/jackett:latest
    container_name: jackett
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London
      - AUTO_UPDATE=true # Optional
      - RUN_OPTS= # Optional
    ports:
      - 9117:9117
    volumes:
      - './jackett:/config'
      - '/home/user/data:/data'
    networks:
      - guide
    restart: unless-stopped
networks:
  guide:
    external: true
```

After all of that you'll want to go into `/home/user/projects/automation` and run `docker compose up -d`. This will deploy all of the containers
in a detached mode, but since it's your first time running them, Docker will pull the images for each of these containers.

Once all of that's done, you'll want to run `docker compose down`, which will shut down each of the containers.

Now you'll want to take that Wireguard configuration file you got from earlier, rename it to `wg0.conf` and place it in
`/home/user/projects/automation/wireguard` (newly created folder after we deployed the containers). Your VPN will now be functional the next time
you run your stack!

## Setting up a killswitch

While we've gotten our VPN working, we still don't have a killswitch yet. That's where this bit comes in.

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
PostUp = DROUTE=$(ip route | grep default | awk '{print $3}'); HOMENET=192.168.1.0/24; HOMENET2=10.0.0.0/8; HOMENET3=172.16.0.0/12; ip route add $HOMENET3 via $DROUTE; ip route add $HOMENET2 via $DROUTE; ip route add $HOMENET via $DROUTE; iptables -I OUTPUT -d $HOMENET -j ACCEPT; iptables -A OUTPUT -d $HOMENET2 -j ACCEPT; iptables -A OUTPUT -d $HOMENET3 -j ACCEPT;  iptables -A OUTPUT ! -o %i -m mark ! --mark $(wg show %i fwmark) -m addrtype ! --dst-type LOCAL -j REJECT
PreDown = HOMENET=192.168.1.0/24; HOMENET2=10.0.0.0/8; HOMENET3=172.16.0.0/12; ip route del $HOMENET3 via $DROUTE; ip route del $HOMENET2 via $DROUTE; ip route del $HOMENET via $DROUTE; iptables -D OUTPUT ! -o %i -m mark ! --mark $(wg show %i fwmark) -m addrtype ! --dst-type LOCAL -j REJECT; iptables -D OUTPUT -d $HOMENET -j ACCEPT; iptables -D OUTPUT -d $HOMENET2 -j ACCEPT; iptables -D OUTPUT -d $HOMENET3 -j ACCEPT

[Peer]
PublicKey = public-key
PresharedKey = public-key
Endpoint = xxx.xxx.xxx.xxx:xxxx
AllowedIPs = 0.0.0.0/0, ::/0
PersistentKeepalive = 15
```

Once you've done that run `docker compose up -d` once more and you're all ready to go.

Note that if you use qBittorrent and it's web UI you can optionally bind all connections to the Wireguard network inteface. This can be done by
going to settings and navigating to the advanced tab all the way on the right. You should immediately see the options to bind your connections to a
network interface, and to optionally bind your connections to the VPN IP.

## Adding your clients to Sonarr and Radarr

This can be done by going to settings, navigating to download clients and picking whatever clients you use (SABnzbd, Transmission, qBitorrent,
Deluge, etc.)

For the hostname it would be your local network address, which you can [find like so on Windows](https://www.whatismybrowser.com/detect/what-is-my-local-ip-address) and [like so on Linux](https://www.ionos.com/digitalguide/hosting/technical-matters/get-linux-ip-address/).

The port would just be whatever was on the left hand side of the `port` declarations in the Compose file.