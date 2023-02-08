---
label: Unblock Guide
description: Simple Guide to Unblocking
image: https://user-images.githubusercontent.com/78981416/217623478-91268767-ba25-4498-9755-bc0eeca97033.gif
---

# Unblock Guide

## DNS Block

1. Go to the Control Panel.
2. Click Network and Internet > Network and Sharing Center > Change adapter settings. 
3. Select the connection for which you want to configure DNS. For example:
	- To change the settings for an Ethernet connection, right-click the Ethernet interface and select Properties.
	- To change the settings for a wireless connection, right-click the Wi-Fi interface and select Properties.
4. Under This connection uses the following items, select Internet Protocol Version 4 (TCP/IPv4) or Internet Protocol Version 6 (TCP/IPv6) and then click Properties.
5. Select Use the following DNS server addresses. If there are any IP addresses listed in the Preferred DNS server or Alternate DNS server, write them down for future reference.
6. Replace those addresses with the IP addresses of the Google DNS servers:
	- IPv4: 1.1.1.1 and 1.0.0.1
	- IPv4 (google): 8.8.8.8 and 8.8.4.4
	- IPv6: 2606:4700:4700::1111 and 2606:4700:4700::1001
	- IPv6 (google) : 2001:4860:4860::8888 and 2001:4860:4860::8844.

!!!
[1.1.1.1](https://1.1.1.1/) also has cloudflare warp installers/apps available for Android, ios, macOS, Windows and Linux.
!!!


## DPI Block

- [GoodbyeDPI](https://github.com/ValdikSS/GoodbyeDPI) is designed to bypass Deep Packet Inspection systems found in many Internet Service Providers which block access to certain websites. Modify service_install_russia_blacklist.cmd according to your selected configuration and run it to install goodbyedpi as a service.
- [PowerTunnel for Android](https://github.com/krlvm/PowerTunnel-Android) is a similar tool.
- DPI Block on linux can be bypassed by [Zapret](https://github.com/bol-van/zapret). Make sure to change DNS to NextDNS or Cloudflare in your browser's DoH settings.
