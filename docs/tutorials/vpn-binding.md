---
label: VPN Binding
description: Bind VPN network interface to torrent client to avoid exposing your IP
---

# VPN Binding

!!!
This guide was originally written by [u/daiqo](https://www.reddit.com/user/daiqo/) in their reddit post [here](https://www.reddit.com/r/VPNTorrents/comments/ssy8vv/guide_bind_vpn_network_interface_to_torrent/)
!!!

This guide assumes you already have a VPN. VPN killswitches aren't reliable, the best way to avoid exposing your IP address is by binding the VPN network interface to the torrent client. This means that you'll only be able to download/upload while the VPN tunnel is active, reducing the probability of having a leak to virtually zero.

## Requirements

A torrent client that supports binding, eg. qBittorrent.

## Windows

- Start the VPN and connect to a location.
- Open qBittorrent. Go to Preferences, and then Advanced tab.
- Change Network interface to the VPN (usually its name, like "Mullvad").
- Restart qBittorrent.

## macOS

- Start the VPN and connect to a location.
- Open the Terminal app (it's in Applications/Utilities).
- Run the command `ifconfig | grep -A 2 utun`
- Take note of the utun interface with the internal IP `inet 10.x.x.x` (eg. `utun3`).
- Open qBittorrent. Go to Preferences, and then Advanced tab.
- Change Network interface to the utun interface you found above.
- Restart qBittorrent.

!!!
The utun interface may change if you reboot or reconnect.
!!!

## Linux

- Start the VPN and connect to a location.
- Open qBittorrent. Go to Preferences, and then Advanced tab.
- Change Network interface to one of the following depending on the app and protocol you are using (Mullvad VPN as example)
- Mullvad app using OpenVPN: `tun0`
- Mullvad app using WireGuard kernel: `wg-mullvad`
- Mullvad app using WireGuard userspace: `tun0`
- WireGuard standalone: `mlvd-xx`
- OpenVPN standalone: `tun0`
- Restart qBittorrent.

## How to test?

- You can download the [official Ubuntu 22.10 torrent](https://releases.ubuntu.com/22.10/ubuntu-22.10-desktop-amd64.iso.torrent) and open it on qBittorrent. If the binding is properly set, the download will only start if the VPN is connected. If you disconnect your VPN, the download will stop.

- Visit [TorGuard's Check My Torrent IP Address](https://torguard.net/checkmytorrentipaddress.php)
