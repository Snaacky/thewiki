---
label: Sonarr
description: Beginner's Guide to Sonarr
image: https://user-images.githubusercontent.com/78981416/215346149-674de56b-b9d6-4a70-87c5-5799b66bd73c.png
---

# Sonarr

This guide was written with the purpose of having something to start with since I personally think other popular guides like [TRaSH Guides](https://trash-guides.info/) are still a bit too advanced for someone who has never touched the \*ARR family of apps.

!!!
This guide was written for Sonarr V3, although the initial setup covered here remains similar for Sonarr V4 with the biggest change being Custom Formats replacing Release Profiles.
!!!

## Prerequisite

- Install a torrent client like [qbittorrent](https://www.qbittorrent.org/download.php) and enable Web-UI.
- Install [Prowlarr](https://github.com/Prowlarr/Prowlarr/releases). Do not install it as a service.
- Install [Sonarr](https://sonarr.tv/). Do not install it as a service.

## Prowlarr

- Launch Prowlarr. It's accessible through http://localhost:9696/ by default.
- Click `Add Indexer`.
- Search `Nyaa` and add it along with any other tracker you want.
- Launch Sonarr. It's accessible through http://localhost:8989/ by default. Go to `Left Pane -> Settings -> General -> API Key -> Copy`
- Now go back to Prowlarr `Left Pane -> Settings -> Apps -> Add Sonarr`
  [![Add Sonarr](https://i.imgur.com/ski3AQt.gif "Add Sonarr")](https://i.imgur.com/ski3AQt.gif "Add Sonarr")

## Sonarr

- Launch Sonarr. It's accessible through http://localhost:8989/ by default.

### Indexers

- Click on `Show Advanced` at the top.
- Go to `Settings -> Indexers` and you'll see that the Indexers you added in Prowlarr have been added to Sonarr.
  - Tag it appropriately because that's how we'll be assigning these specific Indexers to a show. No Tag means it's globally applied.
  - If they don't appear, go back to Prowlarr `Left Pane -> Settings -> Apps -> Sync App Indexers`.
  - Set the RSS interval as per the rules of the Indexer.
    [![Indexer](https://i.imgur.com/xENPujg.png "Indexer")](https://i.imgur.com/xENPujg.png "Indexer")

### Download Client

- Go to `Settings -> Download Clients` and add your download client.
- The Category (or tag depending on your client of choice) here is important, as this is how Sonarr identifies a download. This is what Sonarr will assign to all the downloads to keep a track of them and will not see them unless they have the specified category/tag.
  [![Download Client](https://i.imgur.com/hfKQYcJ.png "Download Client")](https://i.imgur.com/hfKQYcJ.png "Download Client")

### Media Management

- Go to `Settings -> Media Management`
- Change the naming based on [TRaSH's Recommended Naming Scheme](https://trash-guides.info/Sonarr/Sonarr-recommended-naming-scheme/). You can also alter it based on your preferences.
- Add Root Folder. This is the folder where your files will be copied/moved/hardlinked to depending on your choice after organizing, e.g, `/HDD/plex/anime`
- **Note:** _Download Client's save location and Sonarr's Root Folder location must be **different folders** while also being on the **same disk**._

### Profiles

Currently, Sonarr's capability of auto-downloading non-seasonal anime is quite bad due to extremely bad naming used by most uploaders and most of the time will grab a sub-par release. This is why we won't be automating those.

- Go to `Left Pane -> Settings -> Profiles -> Release Profiles`

#### Seasonals

- We'll be making one release profile for each group or show.
- The first release profile will be for general seasonals and grabbing [`SubsPlease`](https://nyaa.si/user/subsplease). Tag it appropriately because that's how we'll be assigning these profiles to a show. No Tag means it's globally applied.
  [![Release Profile](https://i.imgur.com/QoLA0t8.png "SubsPlease Profile")](https://i.imgur.com/QoLA0t8.png "SubsPlease Profile")
- You can make as many profiles you want for fansub releases as you want.
- Once you are done adding all the groups you want to grab, it should look like this:
  [![Release Profiles](https://i.imgur.com/7m6Ybgp.png "Release Profiles")](https://i.imgur.com/7m6Ybgp.png "Release Profiles")
- **Note:**
  - If you want to grab a release from more than one group for a show (either based on scores, preferences or just whoever releases faster), you'll have to make a new `Release Profile` with multiple release groups in the `MUST CONTAIN` field to get the desired result ([example](https://i.imgur.com/SBpEoJQ.png)).
  - This is because for a release to be grabbed, all the `Release Profile` tags you assign to a show must be satisfied. Assigning two profiles tags like `ly` (LostYears) and `sp` (SubsPlease) to a single show will not work because that tells Sonarr to find a release with **BOTH** words present in the folder/filename ([example](https://i.imgur.com/mXQEJrn.png)).

### Adding a Seasonal Show

- Click on the Search bar on the top right and find the show you want to add.
- Select its location and assign the indexer tag and the release tag.
- Check the `Start search for missing episodes` option and click the `Add` button.
- From personal experience, it's better to use `Any` profile instead of `1080p` due to how multiple anime releasers don't bother putting `1080p` in the title, causing them to be rejected. You might also have to allow `Unknown` under `Any` profile because some releasers don't put any information in the title.
  [![Seasonal](https://i.imgur.com/0FQGR1E.gif "Seasonal")](https://i.imgur.com/0FQGR1E.gif "Seasonal")
- It'll now show up in your client along with the `tv-sonarr` tag and Sonarr will import a hardlinked copy into the Root Folder (`/HDD/plex/anime`).
  [![Qbit](https://i.imgur.com/271wNmN.png "Qbit")](https://i.imgur.com/271wNmN.png "Qbit")
- Your new folder structure should look like this:
  [![Folder](https://i.imgur.com/bmmGs6i.png "Folder")](https://i.imgur.com/bmmGs6i.png "Folder")

### Downloading Complete Seasons/Shows

- Click on the Search bar on the top right and find the show you want to add.
  - **Note:** _Do NOT check the `Start search for missing episodes` option._
- We will manually pick a release and then import it through Sonarr for organizing.
- (Optional) Head over to [SeaDex](https://releases.moe) to grab the best release.
- You can do this by simply navigating to your handpicked release's folder via `Left Pane -> Wanted -> Manual Import -> Navigate to the folder containing the episodes`
  [![Manual Import](https://i.imgur.com/UjvXrka.gif "Manual Import")](https://i.imgur.com/UjvXrka.gif "Manual Import")

### Hardlinks

- [What is a Hardlink?](https://trash-guides.info/Hardlinks/Hardlinks-and-Instant-Moves/#what-are-hardlinks)
- Hardlinks are enabled by default. So everything above should have been hardlinked instead of copied/moved.

#### Check Hardlinks

- For Windows, the best way to check Hardlinks is [FindLinks](https://docs.microsoft.com/en-us/sysinternals/downloads/findlinks)
  [![Hardlinks](https://i.imgur.com/pRZwkGG.png "Hardlinks")](https://i.imgur.com/pRZwkGG.png "Hardlinks")
- For Linux, [TRaSH](https://trash-guides.info/) already covers [three methods to check Hardlinks](https://trash-guides.info/Hardlinks/Check-if-hardlinks-are-working/) so I won't be covering those.

#### Troubleshooting

If your hardlinks aren't working, there could be a few oversights causing this:

- You cannot cross file systems between hardlinks so make sure you aren't doing that. TRaSH covers this well [here](https://trash-guides.info/Hardlinks/How-to-setup-for/).
- Permission issues with file systems, especially if you are using NTFS formatted drives on a Unix system. Keep in mind that you can't change the permissions on an NTFS file system the same way you would on an EXT4 file system
- Executing the commands listed down below will recursively change the permissions of the targeted folder/drive to `775/664` (`$USER`). Having the folder/drive permissions set to `775/664` will prevent Sonarr from running into permission issues when hardlinking/copying files.

```
sudo chown -R $USER:$USER /data
sudo chmod -R a=,a+rX,u+w,g+w /data

```

## Advanced Configuration Guide
[!ref TRaSH Guides](https://trash-guides.info/)
