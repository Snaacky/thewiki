---
label: Visual Novels
description: A guide to extract Visual Novel assets
image: 
---

# Visual Novels

The following guide will explain how to extract and acquire the assets from Visual Novels. These assets include CGs (the backgrounds, character models etc.), sound effects, dialogue, background music, and script. The script may be particularly useful if you're planning to translate the Visual Novel.

## Tools

1. [Locale Emulator](https://github.com/xupefei/Locale-Emulator/releases) (optional) - It allows you to emulate the system locale and set it to Japanese locale, since most visual novels won't launch without the system locale set to Japanese.

2. [KrkrExtract](https://github.com/xmoezzz/KrkrExtract/releases/) - This is to extract data from Visual Novels running on the KiriKiri engine. It can also repack the folder into a game file, essentially allowing you to mod the game (make translations, changes to the assets, etc.)

3. [GARBro](https://github.com/morkt/GARbro) - Probably the swiss army knife of Visual Novel asset extractors. It contains extractors for many visual novels which are made with many different kinds of engines. List of supported formats and games [here](https://morkt.github.io/GARbro/supported.html).

!!!info Info
Not all Visual Novels and their archival schemes are supported. Some may even be encrypted to prevent tampering.
!!!

## Extracting a game's assets with GARBro

1. Download GARBro from the above given link and extract/install it. Run `GARbro.GUI.exe` to start the application. You should be presented with a screen that is similar to the one below. ![https://files.catbox.moe/lza6ql.jpg](https://files.catbox.moe/lza6ql.jpg)

The left panel is your file browser and will be your primary method of navigating through the files. Your right pane will display a preview (if possible) of any readable files.

2. Navigate to your game folder, and look for a folder or file that holds data. It may be called `data`, or may be called `image`, `CG` etc. depending on what game it is.

3. Now head over to one of the archives. They may be in any format, depending on the engine used, but `.xp3`, `.bin`, `.dat` are common formats. Try clicking one of them and GARBro will attempt to open it for you. If it is not encrypted, then it will simply open and you'll be able to browse the files inside. Here's a screenshot of a game's assets folder for a sample: ![https://files.catbox.moe/k5ip9a.jpg](https://files.catbox.moe/k5ip9a.jpg)

The below archive is probably an archive for event CGs. ![https://files.catbox.moe/8tle3x.jpg](https://files.catbox.moe/8tle3x.jpg)
4. You're done! You may need to explore and experiment a lot at times to find certain things, and many files may be named poorly, but they should all generally be visible in the preview. To save a file, right click it and hit Extract. You may also choose to save the entire extracted data folder for further use.

!!!info Info
You may encounter the following opening certain archives. This just means that the tool was unable to automatically determine what format the archive was in, or maybe the archive is encrypted. You can select one of the games (if it is your game) in the dropdown, or just select NoCrypt and attempt to open it. 
!!!

![https://files.catbox.moe/4pjqou.jpg](https://files.catbox.moe/4pjqou.jpg)

