# What is Calibre?
From Wikipedia:
> [Calibre](https://calibre-ebook.com/) is a cross-platform open-source suite of e-book software. Calibre supports organizing existing e-books into virtual libraries, displaying, editing, creating and converting e-books, as well as syncing e-books with a variety of e-readers. Editing books is supported for EPUB and AZW3 formats.

## Download
### For Windows
Windows 64-bit build for Calibre can be downloaded from [here](https://calibre-ebook.com/download_windows64) whereas 32-bit can be downloaded from [here](https://calibre-ebook.com/download_windows).
Once downloaded, you can open the .exe file and run though the installation process.

### For Mac
MacOS build for Calibre can be downloaded from [here](https://calibre-ebook.com/download_osx).

### For Linux
Follow the installation instructions given on [this](https://calibre-ebook.com/download_linux) page.

Or you can install directly from your distributions package repository.

For Ubuntu:
`$sudo apt install calibre`

For Arch Linux:
`$sudo pacman -Syu calibre`

### NOTE:
Please read the NOTE section on the download pages for your respective Operating Systems as it contains important information.

# Adding Books to Calibre

## Welcome Wizard
If you are launching Calibre for the first time, it will ask you to: 
1. Set Directory for calibre library: Choose a location where calibre can store books. Create a new empty directory and set the location to that directory. Click on Next.
2. Select your e-book device: If you use an e-reader, you can select the brand and model, then click on Next. It may require some additional setup based on which e-device you select.
3. Click on Finish button to apply your settings.

## Adding books from device storage.
To add books which you downloaded from other sources:
1. Click on the Add books button on the top left corner.
2. Navigate to the directory where your books are stored.
3. Select books and then click on open.

Calibre will start importing books in it's library.

## Downloading LN/WN using FanFicFare.
Due to it's extensible nature, Calibre allows using external plugins. One such popular plugin for downloading Light Novels/Web Novels is [FanFicFare](https://github.com/JimmXinu/FanFicFare).

### To add FanFicFare to Calibre:
1. In Calibre, go to Preferences.
2. In Advanced section, go to plugins.
3. Click on Get new plugins.
4. In User Plugins, search for FanFicFare in Filter by name.
5. Select FanFicFare, and the click on install.
6. Click on Yes when the prompt for security risk appears (I have been using this plugin for many years and haven't faced any kind of security risk while using it).
7. When Success prompt appears, click on Restart calibre now.

### To download Novels using FanFicFare
1. Click on the arrow which is to the left of FanFicFare button in toolbar. If the icon is not there, click on three dots on the right-most corner of the toolbar to expand the toolbar.
2. Click on Download from URLs option from the extended menu. 
3. FanFicFare supports around 100+ sites from where you can download novels. List of supported sites can be found [here](https://github.com/JimmXinu/FanFicFare/wiki/SupportedSites).
4. Go to one of the supported sites and then click on the story you want to download.
5. Copy the URL of the story and then paste it in the text box shown when you click on Download from URLs in Calibre.
6. Select the format in which you want to download your novel and then click on OK.
7. You can see on the bottom right corner that a Job has started.
8. Since FanFicFare scrapes through thousands of pages for a novel, it may take *a lot* of time.
9. Once the Job finishes, the novel will appear in the form of e-book in your library.
10. In future, if new chapters are released for the novel, you can select the book and then click on FanFicFare icon to update your e-book to the latest chapter.

## Converting books in Calibre
To convert a book from one format to another:
1. Select the e-book which you want to convert.
2. Click on the Convert Books option in the toolbar.
3. Select the output format on the top right corner.
4. Click on OK and then wait for the job to finish.
5. In the information section on the right, you will be able to see the original format and converted format.

# Uploading books to Google Books/Kindle
Google Books and Kindle allows syncing reading position between devices, and is also preferred by many to upload and manage their e-books.

## Uploading books to Kindle.
1. Since kindle does not support all formats, to upload books to your kindle account, you need to convert your e-book to MOBI or AZW3.
2. Select the e-book that you want to send, and then click on Connect/share.
3. Select the kindle account to which you want to send.
4. If you have not added a kindle account, first add the recipient to which you want to send.
5. Make sure that the mail address you are using to send e-book to your kindle account has the permission to do so.
6. Open your kindle account or app and enjoy reading.

## Uploading books to Google Books.
1. Go to https://books.google.co.in/books from your web browser.
2. Click on My Books on Google Play.
3. Click on Upload books and upload the books which you want to upload.
4. Though it supports uploading epub as well as PDF, epub is preferred for comfortable reading on phone.
5. Now you can access your e-book from Google Play Books app as well.

### EPUB processing fails on Google Books
This may happen due to epub not following proper epub specification. To resolve this:
1. Right-click the epub in Calibre.
2. Click on Edit book option.
3. A new window with full fledged editor will open.
4. Click on Run Check button in the right section.
5. When it stops running checks, some errors will appear in left section of the editor.
6. Click on "Try to correct all fixable errors automatically" to fix some commonly found errors.
7. Double click on the remaining errors to go to the position of that error and try solving it by either commenting or removing the line on which the error is found.
8. When all the errors are gone, click on save button and the close the editor.
9. Try uploading the e-book again to Google Books.

# Calibre Web
To run a calibre content server over the net which you can use to access e-books on any device:
1. Go to Preferences > Sharing over the net.
2. Check "Run server automatically when calibre starts" if you want to start Calibre server everytime calibre is running.
3. Check "Require username and password to access the content server if you want to password protect your server, or to manage multiple accounts.
4. Go to User Accounts tab and new username and password in case you want to use calibre web with that account.
5. Go to main and click on Start server.
6. To open calibre web on same device, click on Test Server.
7. To open calibre web on any other device on the same local network, input ip and port given in Calibre in the browser of that device, in the format of:
`http://*ip*:*port*`
where ip is 192.168.x.x
and port is yyyy (Usually 8080).