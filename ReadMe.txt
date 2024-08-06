Installation
Releases can be found here.

Note: AMWin-RP will run on Windows 10 21H1 or later, but the Apple Music app requires at least Windows 10 22H2.

Which release do I use?
There are two release files: the standard one and one marked as NoRuntime.

If in doubt, use the unlabelled release (i.e. the one without NoRuntime).
This version works universally, but is larger in size because it bundles the components of .NET that are needed for the app to run.

The NoRuntime release is much smaller, but requires you to have the .NET 8.0 runtime installed.
If you don't have this runtime installed, the program will prompt you to do it when it opens.

Usage
You need the Microsoft store version of Apple Music to use AMWin-RP.
There is no support for iTunes, Apple Music via WSA or any other third-party players.

AMWin-RP runs in the background, minimised to the system tray.
Double clicking on the tray icon brings up the settings window.
From here you can adjust individual settings such as run on startup, scrobbling and song detection.
The app can be closed by right-clicking on the tray icon and selecting "Exit".
By default, the Apple Music app must be open and currently playing music (i.e. not paused) in order for the rich presence to show.
Reporting Bugs
Before creating a new issue, please make sure your problem does not fall under an existing issue.
If you are reporting a problem, please attach any relevant .log files (found in C:\Users\your_username_here\AppData\Local\AMWin-RichPresence).

Before posting, please double-check the following:

The problem isn't already covered by an existing open or closed issue.
You have RP display enabled in Discord (Settings > Activity Settings > Activity Privacy > Actvity Status).
Scrobbling
Last.FM
You will need your own API Key and API Secret from Last.FM.
To generate one, go to https://www.last.fm/api and select "Get an API Account."
Enter these in the settings menu with your Last.FM username and password.

The Last.FM password is stored in Windows Credentials Manager under your local Windows account.

This Scrobbler implementation does not support offline Scrobbles, which means any songs you listen to while not connected to the Internet will be lost.

ListenBrainz
You can scrobble to ListenBrainz by adding your user token in the settings.

⚠️ Technical details ahead ⚠️

How does it work?
The biggest challenge here is being able to extract song information out of the Apple Music app.

This is achieved using Microsoft's UI Automation API via the FlaUI library, which lets us access UI elements of any window on the user's desktop.

The general process is this:

We look for the AppleMusic.exe process.
We get the window belonging to that process.
We then navigate to known UI controls that hold the info we're after (e.g. song name).
We extract this information and send it to the part of the program that handles the Discord RPC.
The other problem is getting the song's cover art.

(It's not well documented, but Discord RPC now lets you specify arbitrary images by sending the image URL in place of the assets key.)

We can't use UI Automation to get the image being displayed in the window (as far as I know). Instead we send an HTTP request to the Last.FM or Apple Music website, where we try to search for the song and grab the cover image URL from there.
It's not ideal but gives us what we're looking for most of the time.
