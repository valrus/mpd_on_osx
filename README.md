# MPD with Quicksilver

## What is this?
Tools for making the playing of albums, organized by iTunes, using [mpd](http://www.musicpd.org) on OS X downright pleasant.
Well. Pleasant-ish.
See [my blog post about it](http://ian.mccowan.space/2015/12/21/MPD_OSX/) for more background info.

This setup takes care of my main music playing needs. I can do the following in very few keystrokes:
* Search and play albums that iTunes knows about using Quicksilver's fuzzy searching
* Manage playback using media keys on a standard Apple keyboard
* Get the name of the song being played

## Installation
### Get stuff
* Install [mpd](http://www.musicpd.org). I recommend [Homebrew](http://brew.sh).
* Install [mpc](http://www.musicpd.org/clients/mpc/). Homebrew has it as well.
* Install [Quicksilver](http://qsapp.com) and the following plugins:
  * iTunes Plugin
  * Automator Plugin
* Clone this repo or download it or whatever.
* (Optional, but needed for media keys support) Install [osxmpdkeys](https://pypi.python.org/pypi/osxmpdkeys/).
* (Optional) Install [Growl](http://growl.info).
* (Optional) Install [LaunchRocket](https://github.com/jimbojsb/launchrocket).

### Put stuff where it goes
* NB: I assume all destination folders in these commands exist; you may have to create them yourself.
* From this repo's directory:

  ```
  cp -r "Play with mpd.workflow" ~/Library/Services
  cp -r "Show mpd status.workflow" "~/Library/Application Support/Quicksilver/Workflows"
  ```
  Note: The "(Growl)" versions use Growl rather than native notifications but are otherwise identical.
* If you installed LaunchRocket, add `mpd.plist` (and, optionally, `osxmpdkeys.plist`) using its "Add .plist" button. Otherwise, also from this repo's directory:

  ```
  cp mpd.plist ~/Library/LaunchAgents
  cp osxmpdkeys.plist ~/Library/LaunchAgents
  ```
* If you want to play individual tracks as well:

  ```
  cp "Play Track with mpd.scpt" "~/Library/Application Support/Quicksilver/Actions"
  ```
* Edit the `mpd` config file `mpd.conf` that I included in this repo so that the `music_directory` entry (the first line) has a path to your iTunes music library.
* Install `mpd.conf` and create its data files:

  ```
  mkdir ~/.mpd
  cp mpd.conf ~/.mpd
  cd ~/.mpd
  mkdir playlists
  touch database pid state sticker.sql
  ```


### Set up Quicksilver
In the "Actions" section of the Quicksilver preferences, there should be two new actions:
* Play with mpd
* Play Track with mpd
If not you might have to restart it or something. I don't know.

If you want, set up a new trigger: `Browse Albums > Search Contents` (note: Browse Albums is provided by the iTunes plugin)
Hitting this trigger will do a fuzzy search on just your iTunes albums.
But the default action might not be "Play with mpd" like you probably want it to.
If it isn't, you can tab to the action pane and hit right-arrow to see where it is in the list.
Then go back to the Preferences tab and then to Actions and change the rank of "Play with mpd"
to be a lower number than everything that was ahead of it in the list.
You probably don't want to set it too low, as it will come up even when you're not trying to play music
and you probably don't want it too prominent then.
(This is because it works on strings, not the "iTunes Genre/Artist/Album" type like you might think it should.
Unfortunately doing Browse Contents on your iTunes catalog doesn't seem to work that way.)

You'll likely want another trigger: `Show mpd status > Execute Workflow`.
Because `mpd` runs in the background,
this will be the most convenient way to see what it's doing at any time.
(Your other option is using the `mpc status` command in the terminal.)

You can probably do something similar for tracks if you want.
I generally only play albums, so I haven't bothered.
