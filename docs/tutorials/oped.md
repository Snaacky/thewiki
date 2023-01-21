---
label: OPED
---

# OP/ED

## Prerequisites and Setup

[Themes.moe](https://themes.moe/) is a super useful site that lets you search for your profile and shows each OP and ED associated with all the anime in your list.

[DownThemAll](https://www.downthemall.net/) - a browser extension that grabs all links on a page

[ffmpeg](https://ffmpeg.org/download.html) - to extract the audio from webm

Unzip the downloaded archive and copy over ffmpeg.exe from the folder called bin to where you'll be downloading music. Alternatively, [add the bin folder to path.](https://www.architectryan.com/2018/03/17/add-to-the-path-on-windows-10/)

## Downloading

1. Go to [themes.moe](https://themes.moe/), select Anilist or MyAnimeList, enter your username and click the search button.

2. When it shows all the OP/EDs for your profile, you will want to click on the "Filters" button and disable "Include Duplicates" so that you don't have the same audio twice.

3. You can also get rid of other things like stuff on your Plan To Watch list, or to only show OPs. Just customize it so that it only shows what you want to download.

4. Use DownloadThemAll!! to bring up all the links on that page, ensure that only the "Videos" filter is selected so that it only selects the webm files and click "Download".

5. When they are downloaded, move them to the same folder where ffmpeg is.

```batch
for %%f in (*.webm) do ffmpeg -n -i "%%f" -c:a copy "%%~nf.ogg"
```

6. Copy this into notepad and save it with the `.bat` extension and in the same folder as ffmpeg and all your webm files.

7. Run the .bat file. It will now start to extract the audio from each webm file in the folder and save them as .ogg.

!!!
The songs downloaded are TV size, i.e, typically 1 minute and 30 seconds long.
!!!
