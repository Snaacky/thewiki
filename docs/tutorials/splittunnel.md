---
label: Split Tunneling
description: Split tunneling on any Wireguard VPN
image: /static/tutorials/wireguard.png
author:
  name: "guyman"
  avatar: https://github.com/Snaacky/thewiki/assets/78981416/2045b3c7-90a5-40ce-9d81-a81baa421227
---

# Split Tunneling on Any VPN (Wireguard)

This guide can be used to achieve whitelist split tunneling on any Wireguard VPN. Most VPNs will provide you with a wireguard config or have some sort of method to extract one.

## Required

1. [Wiresock Secure Connect](https://www.wiresock.net/wiresock-secure-connect/download)
2. Torrent Client (qBittorrent preferred)
3. Your provider's wireguard .conf file

## Steps

1. Download and install Wiresock Secure Connect
2. Run the program
3. Click the settings icon around the bottom right corner.
4. In settings, enable `Virtual Adapter Mode` and click restart ![image](https://github.com/user-attachments/assets/8328d7de-fbf4-472c-b8e9-f425ac368636)
5. Now click Import Configuration.
6. Select your configuration and click `Edit` at the top right of the window
7. Scroll down until you see `Tunneled Applications` and type the exe name of the program you would like to be inside the VPN. Clicking the Windows icon will bring up a menu with all currently running programs. ![image](https://github.com/user-attachments/assets/8fa47af1-788f-4a78-912e-0b0e6c3abb0f)
8. Save, close the configuration window and enable the VPN.

At this point, you should be torrenting through the VPN as long as Wiresock Secure Connect is running and Activated. However, you may be concered about accidentally disabling the VPN while a torrent is running. To fix this, we can change a setting in qBittorrent.

1. Open qBittorrent settings
2. On the left hand bar, click Advanced
3. Look for `Network Interface`. The default setting is `Any Interface`
4. Expand the drop down. If you are lucky, the correct interface will be `WireSock Virtual Adapter` or the same as your .conf file. If you don't see this, continue on. Otherwise, skip to step 8.
5. Press Windows + R key together. Paste in `ncpa.cpl`
6. In the window that shows up, you should see all your network adapters, including virtual ones. Find the one that says `WireSock Virtual Adapter` on the third line. Write down it's name, or rename it to WireSock
  ![image](https://github.com/guyman624/thewiki/assets/82007920/f17e1c3c-1ca8-4d3d-b98b-edce36b5b6da)
7. Back in qBittorrent, look at `Network Interface`. You may have to close and open the settings window if you renamed the adapter to WireSock
8. Set `Network Interface` to `WireSock` or `Local Area Network x` or whatever other name you wrote down
  ![image](https://github.com/guyman624/thewiki/assets/82007920/5125b8de-309c-425a-b1a0-70ca1b775081)
9. Hit Apply then OK

You should be properly configured now. If you would like to test your configuration, continue on.

## Testing split tunneling

1. Go to [ipleak.net](https://ipleak.net)
2. At the top, you should see your home ip address. Remember this
3. Scroll down to `Torrent Address detection`. Click Activate
4. Click the `this Magnet Link` button and add the torrent to qbittorrent
5. Wait a few minutes. ipleak should show some ip addresses under the torrent section
6. Compare both the IPv4 and IPv6 that show up to the ones that show at the top for your home IP. If it is different, congratulations! You have successfully set up split tunneling!
![image](https://github.com/guyman624/thewiki/assets/82007920/c2ceef0d-5858-4dcd-b951-0c73bfc7e4e7)
