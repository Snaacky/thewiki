---
label: Shana
---

**Note**

Hi everyone, I'd like to share a little project I wrote over the past year + a workflow I've personally been using to reduce the manual work involved every season in downloading anime. This helps a lot especially if you're an avid anime watcher and follow 10+ anime per season. Everything from auto-downloading new releases to selecting the "correct" resolution is automated.

Requirements:

- An account on [ShanaProject](https://www.shanaproject.com/)
- [taiga2shana](https://github.com/zehric/taiga2shana/releases/)

Recommended:

- [Taiga](https://taiga.moe/) (note: please install to the default directory if you can!)

First, if you haven't already, follow the guide to [set up your follows](https://www.shanaproject.com/help/follows/) on ShanaProject. By the end of this step you should be able to add follows manually on the website and have them automatically downloaded on your torrent client.

**If you have Taiga:**

The next step is to add the anime that you want to automatically download to your "Currently Watching" list in Taiga. taiga2shana will look at only the anime in that section to figure out which anime to add to your ShanaProject follows.

Once that is done, you can just run `taiga2shana.exe`. It will prompt you for your ShanaProject username and password. For those who don't have specific requirements, you're done!

**If you don't have Taiga:**

Create a file of newline-separated anime titles in the same directory as where you put taiga2shana and name it `anime.txt`. For example, the contents of my `anime.txt` might be the following:

    boku no hero academia 4
    dr. stone
    psycho-pass 3
    shinchou yuusha
    vinland saga

Now you can just run `taiga2shana.exe`. The application will still work with partial names, so you don't need to be super precise with the titles. However, keep in mind that the closer it is, the easier the later steps will be. The application will prompt you when an anime title is ambiguous.

You can also run it from the command line with --help to see all the different command line options.

Final note, if you want to contact me about this workflow you won't find me on Reddit. You can find me on Discord at fool#6085 though.

**FAQ:**

How does the application decide which resolution to download?

I was recently informed that most anime is not actually produced in 1080p (I know, shocker!) but after looking into it I actually found a website that actually catalogs the native resolution of anime every season. That website is [anibin](http://anibin.blogspot.com/). taiga2shana pulls information from that blog to inform its decision on the resolution of the follow. Essentially, it will default to 1080p unless anibin says it is 720p. Currently this behavior only works if you have Taiga, if you use the custom list method it just defaults to 1080p.

Can I get this program on OSX or Linux?

Yes, you can. But you should message me first. I'll need to compile new binaries for those systems and I'll need your help making sure it runs.

Why don't I just use sonarr or an RSS feed from an existing torrent tracker?

There are two main benefits to using this app over things. Firstly, this app gets the correct resolution of the anime based on the resolution of the actual source material (some anime is produced in >720p, others are not). Secondly, if you add your anime to a site such as MyAnimeList or AniList, this saves the work of having to add to both that list as well as another program (Taiga manages everything).
