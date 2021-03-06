Sub-Zero for Plex, 1.3.20.396
=================

![logo](https://raw.githubusercontent.com/pannal/Sub-Zero.bundle/master/Contents/Resources/subzero.gif)

##### Subtitles done right
Originally based on @bramwalet's awesome [Subliminal.bundle](https://github.com/bramwalet/Subliminal.bundle)

Plex forum thread: https://forums.plex.tv/discussion/186575

If you like this, buy me a beer: [![Donate](https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=G9VKR2B8PMNKG)

### Installation
* go to ```Library/Application Support/Plex Media Server/Plug-ins/```
* ```rm -r Sub-Zero.bundle```
* get the release you want from *https://github.com/pannal/Sub-Zero.bundle/releases/*
* unzip the release
* restart your plex media server!!!
* more indepth: see [article](https://support.plex.tv/hc/en-us/articles/201187656-How-do-I-manually-install-a-channel-) on Plex website. 

### Usage
Use the following agent order:

1. Sub-Zero TV/Movie Subtitles
2. Local Media Assets
3. anything else

### Attention on the initial refresh
When you first use this plugin, and do a refresh on all of your media, you are most likely
to be shut out by some or all of the subtitle providers depending on your libraries' size.
This will result in a bunch of errors in the log files as well as missing subtitles.

Just be patient, after a day most of those providers will allow you access again and you can
refresh the remaining items. If you use the default settings, this will also skip the items
it has already downloaded all the wanted languages for.

### Encountered a bug?
* be sure to post your logs: 
  * set your log_level to DEBUG in the settings
  * get ```Library/Application Support/Plex Media Server/Logs/PMS Plugin Logs/com.plexapp.agents.subzero.log```; there may be multiple logs (com.plexapp.agents.subzero.log.*) depending on the amount of Videos you're refreshing
* **Remember: before you open a bug-ticket please double-check, that you've deleted the Sub-Zero.bundle folder BEFORE every update** (to avoid .pyc leftovers)

## Changelog

1.3.20.396

- core: fix logging handlers (when saving log_level settings loggers got duplicated)
- core: better movie matching by only hinting the filename and the last subdirectory to guessit (instead of the full path)
- core: don't fail on wrong detection/scanning of media file
- lower minimum tv series score from 85 to 67 (removed title; composed of: series=44 + season=11 + episode=11 + hearing_impaired=1)

[older changes](CHANGELOG.md)


Description
------------

Plex Metadata agent plugin based on Subliminal. This agent will search on the following sites for the best matching subtitles:
- OpenSubtitles
- ~~TheSubDB~~
- Podnapisi.NET
- Addic7ed
- TVsubtitles.net

All providers can be disabled or enabled on a per provider setting. Certain preferences change the behaviour of subliminal, for instance the minimum score of subtitles to download, or whether to download hearing impaired subtitles or not. The agent stores the subtitles as metadata, but can be configured (See Configuration) to store it next to the media files. 


Configuration 
-------------
Several options are provided in the preferences of this agent. 

* Addic7ed username/password: Provide your addic7ed username here, otherwise the provider won't work. Please make sure your account is activated, before using the agent.
* Plex.tv username/password: Generally recommended to be provided; needed if you use Plex Home to make the API work (the whole channel menu depends on it)
* Subtitle language (1)/(2)/(3): Your preferred languages to download subtitles for. 
* Additional Subtitle Languages: Additional languages to download; comma-separated; use [ISO-639-1 codes](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes))
* Provider: Enable ...: Enable/disable this provider. Affects both movies and series. 
* Addic7ed: boost over hash score if requirements met: if an Addic7ed subtitle matches the video's series, season, episode, year, and format (e.g. WEB-DL), boost its score, possibly over OpenSubtitles/TheSubDB direct hash match
* Scan: Include embedded subtitles: When enabled, subliminal finds embedded subtitles that are already present within the media file. 
* Scan: Include external subtitles: When enabled, subliminal finds subtitles located near the media file on the filesystem.
* Minimum score for download: When configured, what is the minimum score for subtitles to download them? Lower scored subtitles are not downloaded.
* Download hearing impaired subtitles: 
  * "prefer": score subtitles for hearing impaired higher
  * "don't prefer": score subtitles for hearing impaired lower
  * "force HI": skip subtitles if the hearing impaired flag isn't set
  * "force non-HI": skip subtitles if the hearing impaired flag is set
* Store subtitles next to media files (instead of metadata): See Store as metadata or on filesystem
* Subtitle folder: (default: current media file's folder) See Store as metadata or on filesystem
* Custom Subtitle folder: See Store as metadata or on filesystem 
* Treat IETF language tags as ISO 639-1: Treats subtitle files with IETF language identifiers, such as pt-BR, as their ISO 639-1 counterpart. Thus "pt-BR" will be shown as "Portuguese" instead of "Unknown"
* Scheduler: 
  * Periodically search for recent items with missing subtitles: self-explanatory, executes the task "Search for missing subtitles" from the channel menu regularly. Configure how often it should do that. For the average library 6 hours minimum is recommended, to not hammer the providers too heavily
  * Item age to be considered recent: The "Search for missing subtitles"-task only considers those items in the recently-added list, that are at most this old
  * Recent items to consider per library: How many items to consider for every section/library you have - used in "Search for missing subtitles"-task and "Items with missing subtitles"-menu. Change at your own risk!
* How verbose should the logging be?: Controls how much info we write into the log files (default: only warnings)
* Log to console (for development/debugging): You know when you need it

Scheduler
---------------------------------------
The built-in scheduler is capable of running a number of tasks periodically in a separate Thread of the plugin.
This currently is used to automatically periodically search for new subtitles for your media items.
See configuration above.

##### Ignore list
There are numerous occasions where one wouldn't want a certain item or even a library be included in the periodic "Search for missing subtitles"-task or the "Items with missing subtitles" menu function.
Anime libraries are a good example of that, or home videos. Perhaps you've got your favourite series in your native language and don't want subtitles for it.

The ignore list can be managed by going through your library using the "Browse all items" menu and the "Display ignore list" menu. 


The channel
-----------
Since 1.3.0 Sub-Zero not only comes as an agent plugin, but also has channel properties.
By accessing the Sub-Zero channel you can get viable information about the scheduler state, search for missing subtitles,
trigger forced-searches for individual items, and many more features yet to come.

Remoting the channel
--------------------
The features available in the channel menu are in fact accessible and usable from the outside,
just as any other channel with routes.
This means, that if you're not happy with the scheduler's interval for example, you can take the following URL:
`http://plex_ip:32400/video/subzero/missing/refresh?X-Plex-Token=XXXXXXXXXXXXXXX` (the X-Plex-Token part may not be needed outside of
a Plex Home) and open the URL using your favourite command line tool or script (curl, wget, ...).
This will trigger the same background task which would be started by the scheduler or by clicking the item in the channel menu.

You can find all available routes by querying `http://plex_ip:32400/video/subzero` (look for the key="" entries). 


Store as metadata or on filesystem
----------------------------------
By default, Plex stores posters, fan art and subtitles as metadata in a separate folder which is not managed by the user. 
In Sub-Zero, though, 'Store subtitles next to media files' is enabled by default.
The agent will write the subtitle files in the media folder next to the media file itself. 
The setting 'Subtitle folder' configures in which folder (current folder or other subfolder) the subtitles are stored. The expert user can also supply 'Custom Subtitle folder' which can also be an absolute path.

**When a subfolder (either custom or predefined) is used, the automatic scheduled refresh of Plex won't pick up your subtitles, only a manual refresh will!**

License
-------
The Unlicense

Libraries
---------
Uses the following libraries and their LICENSE:
- [babelfish](https://pypi.python.org/pypi/babelfish/) (BSD-3-Clause)
- [beautifulsoup4](https://pypi.python.org/pypi/beautifulsoup4/) (MIT)
- [chardet](https://pypi.python.org/pypi/chardet/) (LGPL)
- [dogpile.core](https://pypi.python.org/pypi/dogpile.core/) (BSD)
- [dogpile.cache](https://pypi.python.org/pypi/dogpile.cache/) (BSD)
- [enzyme](https://pypi.python.org/pypi/enzyme/) (Apache 2.0)
- [guessit](https://pypi.python.org/pypi/guessit/) (LGPLv3)
- [html5lib](https://pypi.python.org/pypi/html5lib/) (MIT)
- [pysrt](https://pypi.python.org/pypi/pysrt/) (GPLv3)
- [requests](https://pypi.python.org/pypi/requests/) (Apache 2.0)
- [stevedore](https://pypi.python.org/pypi/stevedore/) (Apache)
- [subliminal](https://pypi.python.org/pypi/subliminal/) (MIT)
- [xdg](https://pypi.python.org/pypi/pyxdg/) (LGPLv2)
- [setuptools](https://pypi.python.org/pypi/setuptools/) (PSF ZPL)
- [plexinc-agents/LocalMedia.bundle](https://github.com/plexinc-agents/LocalMedia.bundle) (Plex)
- [fuzeman/plex.py](https://github.com/fuzeman/plex.py) (plex.py)
