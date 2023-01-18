# What is propolis?
[`propolis`](https://gitlab.com/passelecasque/propolis) is a tool that checks the files of FLAC releases. The objective is to automatically check as many rules as possible, without knowing anything about the release itself.
For example, it will not check that the album title is correct, or correctly spelled, but it will check that it is both in the folder name and the Vorbis tags.

It can be used:
- Before uploading, to make sure the files seem to follow the uploading rules and a few good practices.
- After downloading, to check for trumpable releases.

Note:
- This tool is a guide, not an authority on what is and is not correct.
- Some rules have exceptions. The files you are checking may or may not fit those exceptions. propolis cannot know.
- Some rules are only partially checked, some are checked together.

Color Code:
- Blue: check OK or neutral remark
- Yellow: Warning, or good practice not found
- Red: Denotes a serious problem

## Disclaimer
- Since `propolis` has no colors on Windows, making it much harder to spot errors, it's highly recommended to set-up [`WSL2`](https://docs.microsoft.com/en-us/windows/wsl/install) and use that to run `propolis`.
- This guide will focus on installing `propolis` in Ubuntu in `WSL2` which hopefully means you can follow this guide to install it on bare-metal Linux as well.

## Dependencies
- Before installing `propolis`, we need to install `sox` and `flac` with the following commands in your terminal:
```
sudo apt-get update
```
```
sudo apt -y install sox flac
```

## Installation
1. In the list of [pipelines](https://gitlab.com/passelecasque/propolis/-/pipelines), click on the download icon to the right of the topmost pipeline.
2. Download the compiled release by clicking the button underneath **Download artifacts**.
3. Extract the contents of the archive into a new folder. For the sake of this guide, I'll be extracting it to `/mnt/d/propolis-v0.5.5`
4. Run `./propolis` in the directory you extracted propolis into and it should look like this:
[![./propolis](https://i.imgur.com/buzcicv.png "./propolis")](https://i.imgur.com/buzcicv.png "./propolis")
This means that you have successfully installed `propolis`.

## Adding propolis to PATH
It's inconvenient to navigate to `/mnt/d/propolis-v0.5.5` every time you want to use `propolis` so we will add it as a PATH environment variable with the following commands:
```bash
echo 'export PATH="/path/to/propolis-v0.5.5:$PATH"' >> ~/.bashrc
source ~/.bashrc
```
If you did everything correctly, `propolis` is now installed and added to path. You can test this by running `propolis` from anywhere else. For example:

[![propolis](https://i.imgur.com/6FqepHA.png "propolis")](https://i.imgur.com/6FqepHA.png "propolis")
