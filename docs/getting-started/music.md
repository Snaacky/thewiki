---
label: Music
order: -4
description: Learn how and where to find your favorite music
image: https://user-images.githubusercontent.com/78981416/215862523-7c39ad32-2656-4e88-81a7-346428bc41f2.gif
author:  
  name: "zeraf1m"
  link: https://github.com/zeraf1m
  avatar: https://avatars.githubusercontent.com/u/124029849
---

# Music

## Content Resources

- [Soulseek](http://www.slsknet.org/news/) - P2P network where people share music files of their own. Great way to find lossless music and a lot of relevant music.

- [Redacted](https://redacted.ch/) - Commonly referred to as RED, it's the best source for music, and has everything in both FLAC and MP3. It's a private tracker, meaning you'll have to go through an [IRC Interview](https://interviewfor.red/en/index.html) to join. Thoroughly read the interview page.

- [Orpheus](https://orpheus.network/) - Another music private tracker like RED. Music selection is smaller than RED but it's easier to join with a more lenient economy. You'll have to go through an [IRC Interview](https://interview.orpheus.network/) to join this as well. Again, thoroughly read the interview page.

- [Jpopsuki](https://jpopsuki.eu/) - Asian music private tracker with an extremely easy economy but lacks quality control. The only way to join this is via invites on other private trackers.

!!!warning
Do not under any circumstances ask people to invite you. This is forbidden by private trackers and can lead to your accounts being permanently disabled. Trading/buying invites is also forbidden and will get you disabled/blacklisted across all private trackers.
!!!

## Spectral Analysis

- Since Soulseek doesn't have strict rules regarding upconversion (lossy->lossless), many files may be of dubious quality. This can be verified through spectral analysis. A full guide for doing so along with the required software can be found [here](https://interviewfor.red/en/spectrals.html) and [here](https://erikstechcorner.com/2020/09/how-to-check-if-your-flac-files-are-really-lossless/). In summary, you are looking for frequency cutoffs like the following:

  +++ Full Size

  ![](https://user-images.githubusercontent.com/78981416/246205066-d48507dd-a97b-493d-ac33-c04586298ac8.png)
  
  +++ Zoomed in
  
  ![](https://user-images.githubusercontent.com/78981416/246205087-e977fe1a-a4b0-41c5-a309-8e4568d34bf4.png)
  
  +++

- (Recommended) Spectrogram generation can be achieved using popular audio command-line utility [SoX](https://sox.sourceforge.net/). Some basic options to generate spectrograms like the ones shown above are:

  +++ Full Size
  
  ```batch
  sox track01.flac -n remix 1 spectrogram -x 3000 -y 513 -z 120 -w Kaiser -o "track01-full.png"
  ```
  
  +++ Zoomed in

  ```batch
  sox track01.flac -n remix 1 spectrogram -X 500 -y 1025 -z 120 -w Kaiser -S 1:00 -d 0:02 -o "track01-zoom.png"
  ```

  +++ Windows

  ```batch
  for %I in (*.flac) do sox "%I" -n remix 1 spectrogram -x 3000 -y 513 -z 120 -w Kaiser -o "%~nI-full.png"
  ```

  ```batch
  for %I in (*.flac) do sox "%I" -n remix 1 spectrogram -X 500 -y 1025 -z 120 -w Kaiser -S 1:00 -d 0:02 -o "%~nI-zoom.png"
  ```

  +++ Linux

  ```shell
  for file in *.flac; do sox "${file}" -n remix 1 spectrogram -x 3000 -y 513 -z 120 -w Kaiser -o "${file%.*}-full.png"; done
  ```

  ```shell
    for file in *.flac; do sox "${file}" -n remix 1 spectrogram -X 500 -y 1025 -z 120 -w Kaiser -S 1:00 -d 0:02 -o "${file%.*}-zoom.png"; done
  ```

  +++
  
- How this works, in a nutshell, is that the user's input audio file gets downmixed into a singular channel with `remix 1`, followed by the input track being replaced with a null file by flag `-n` (since spectrogram is a command that returns audio information and does not modify it, the audio itself isn't needed for much else). `-X,`, `-y`, and `-z` is used to specify the spectrogram's dimensions, where `X` is the length, `y` is the height (both in pixels), and `z` is the range in decibels. `-w Kaiser` produces the spectrogram calculated with the Kaiser window function. `-S` and `-d` are options that allow for the analysis of audio snippets, where S is the start and d is the end of the audio snippet. Finally, `-o "filename"` changes the name of the spectrogram.

- You can find detailed explanations on each option flag [here](https://sox.sourceforge.net/sox.html) as well a plethora of additional ones to fine-tune your spectrogram to your desire.

- If you're looking for something that can produce spectrograms of various files and directories quickly, [scourgeofgrozny/sox-spectrogram](https://github.com/scourgeofgrozny/sox-spectrogram) is a GitHub repo containing a useful script that takes away most of the heavy lifting. Feel free to fork or download the source code and customize it to fit your own uses and preferences :)

## Comprehensive Guides

- [The Mega Music Ripping Guide](https://ori5000.github.io/musicripping.html) guide containing instructions on how to rip music from a variety of different popular sources.

- [Sharky's Music Google Docs](https://docs.google.com/document/d/1Poj4p2W0C0Napmwd7bcustlgWrkgp71dy9HlZwUr46w/edit) is a massive document containing all sorts of tutorials and explanations for commonly-seen aspects of music ripping and uploading.
