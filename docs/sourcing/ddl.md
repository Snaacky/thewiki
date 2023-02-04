---
label: DDL/XDCC
order: -4
---

# DDL/XDCC

## IRC

IRC (Internet Relay Chat) is a protocol that allows communication in the form of text. XDCC is a computer file sharing method which relies on IRC as a hosting service. The main advantage to using this method is that IRC bots are usually the fastest at uploading new anime episodes, thus being able to download them much earlier. They usually upload even faster than anime websites.

## Connecting to the network

1. Download and install [HexChat](https://hexchat.github.io/).
2. The network list tab will open on starting the app, pick a username and nickname here, along with second and third choices.
3. Find and select the `Rizon network`.
4. Type `#nibl` in "Join this channel" and click OK.

Repeat steps 3 and 4 for other networks and channels where your target IRC bot is hosted. These can usually be found on the Nyaa releases of any group. For example -

```
#MK@irc.xertion.org
#XDCCLeech@irc.xertion.org
```

## Downloading Anime

1. Go to https://nibl.co.uk/bots (Rizon) or https://animk.info/xdcc (Xertion).
2. Click on Search and type the name and resolution of the anime you want to download. For example, `No Game No Life 1080`
3. Clicking the results will copy a command like `/msg Rory|XDCC xdcc send #22430` to your clipboard. You can make this command yourself by looking at the bot name and pack # columns.
4. Paste this command in hexchat and send it. The download should start immediately.

## Batch downloading

Note the pack numbers for all the episodes you need and modify the command as follows -

```
/msg Rory|XDCC xdcc batch 22430-22441
/msg Rory|XDCC xdcc batch 22430,22435,22440
/msg Rory|XDCC xdcc batch 22430,22432,22435-22441
```

XDCC commands reference - https://wiki.xertion.org/w/XDCC_Commands

## Advanced

Automation - https://github.com/Vodes/XDCC-AutoDL
