---
label: Music
order: -4
description: Learn How and Where to Listen to Your Favorite Music
image: https://user-images.githubusercontent.com/78981416/215862523-7c39ad32-2656-4e88-81a7-346428bc41f2.gif
author:  
  name: "scourgeofgrozny"
  link: https://github.com/scourgeofgrozny
  avatar: https://avatars.githubusercontent.com/u/124029849
---

# Music
## Content Resources

[Soulseek](http://www.slsknet.org/news/)
- P2P network where people share music files of their own. Great way to find lossless music and a lot of relevant music. 

## Spectral Analysis 

- Since Soulseek doesn't have strict rules regarding upconversion (lossy->lossless), many files may be of dubious quality. This can be verified through spectral analysis. A full guide for doing so along with the required software can be found [here](https://interviewfor.red/en/spectrals.html) and [here}(https://erikstechcorner.com/2020/09/how-to-check-if-your-flac-files-are-really-lossless/). In summary, you are looking for frequency cutoffs like the following:


    ![](https://user-images.githubusercontent.com/124029849/243498651-0193fe75-9a98-462d-aa2e-a3079f533bc2.png)

- (Recommended) Spectrogram generation can be achieved using popular audio command-line utility [SoX](https://sox.sourceforge.net/). Some basic options to generate spectrograms are `sox track01.flac -n remix 1 spectrogram -x 3000 -y 513 -z 120 -w Kaiser -o "track01-full.png"` and 
  `sox track01.flac -n remix 1 spectrogram -X 500 -y 1025 -z 120 -w Kaiser -S 1:00 -d 0:02 -o "track01-zoom.png"`.
  
- How this works in a nutshell is that the user's input audio file gets downmixed into a singular channel with `remix 1`, followed by the input track being replaced with a null file by flag `-n` (since spectrogram is a command that returns audio information and does not modify it, the audio itself isn't needed for much else). `-X,`, `-y`, and `-z` are used to specify the spectrogram's dimensions, where X is the length, y is the height (both in pixels), and z is the range in decibels. `-w Kaiser` produces the spectrogram calculated with the Kaiser window function. `-S` and `-d` are options that allow for the analysis of audio snippets, where S is the start and d is the end of the audio snippet. Finally, `-o "filename"` changes the name of the spectrogram. 

-   You can find detailed explanations on each option flag [here](https://sox.sourceforge.net/sox.html) as well a plethora of additional ones to fine-tune your spectrogram to your desire.

- If you're looking for something that can produce spectograms of various file and directories quickly, (https://github.com/scourgeofgrozny/sox-spectrogram) is a GitHub repo containing a useful script that takes away most of the heavy lifting. Feel free to fork or download the source code and customize it to fit your own uses and preferences :)

## Comprehensive Guides

- [The Mega Music Ripping Guide](https://ori5000.github.io/musicripping.html) guide containing instructions on how to rip music from a variety of different popular sources. 

- [Sharky's Music Google Docs](https://docs.google.com/document/d/1Poj4p2W0C0Napmwd7bcustlgWrkgp71dy9HlZwUr46w/edit) is a massive document containing all sorts of tutorials and explanations for commonly-seen aspects of music ripping and uploading. 
