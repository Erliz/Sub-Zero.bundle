1.3.19.379
- core: new recent items implementation (used in "Items with missing subtitles"), now really picking up everything instead of using Plex's recently_added API endpoint
- core: be more strict about title matching - a matched title doesn't automatically mean season and episode are correct, too
- core: rewrote the hash matching algorithm to not blindly trust hash matches anymore, but instead episodes have to match the series name, season number, episode number and format (BluRay, HDTV...); movie have to at least match the title, format and codec for the hash to be considered
- core: remove TheSubDB support for now, as it only supports hash-based matching
- scheduler: more robust item-fail-handling (fixes #81)
- config: "Scan: include embedded subtitles" now by default is off, as embedded subs have proven to be pretty unreliable
- config: add configuration option for how many items per library are to be considered recent (default: 200)
- config: make logging verbosity configurable, default: WARNING - log files should be considerably smaller now
- config: make console logging optional, default: off - good for development/debugging
- config: removed the ignore lists
- menu: added "Browse all items", where you can browse all your libraries and manage your ignore list (add/remove sections/series/items)
- menu: added "Display ignore list", where you can manage your ignored sections, series and items
- menu: the submenu titles are now dynamically composed of a breadcrumb-style tree so you see where you are
- menu: show the current and past state of the important menu actions such as (force)-refresh an item or refreshing the menu, on the Refresh-button's description
- plugin now isn't in the dev mode by default and has logging to the console off (in certain configurations this resulted in huge syslogs)


1.3.6.316
- scheduler: missing subtitles task now able to handle huge libraries (thanks @chopeta, @comrade)
- scheduler: detect item-stalling, add wait and retry logic to make missing subtitles task more robust
- scheduler: report failed items to logs after task run completion
- hint series name and episode title, or movie title to guessit to make detection way better (e.g. for Mr. Robot)

1.3.6.304
- scheduler: correct the recent-determination of the search for missing subtitles in recently_added task
- scheduler: rewrote search for missing subtitles task; it now requests refreshes one by one and not in bulk anymore (hopefully fixes stalling)
- handle rare cases of weird file system encodings (ANSI_X3.4-1968 for example)
- fix simplejson warning on startup

1.3.6.297
- rename Sub-Zero to Sub-Zero.bundle (requirement for adding Sub-Zero to the Plex channel directory)
- channel: add logging actions for the internal storage to the advanced menu
- channel: handle item titles with foreign characters in them correctly
- (hopefully) fix handling file names with foreign characters in them when scanning for local media
- reformat the whole project, mostly honoring pep8
- scheduler: fixed some serious bugs; broken tasks (stalled) and some errors many of you have seen should be gone now
- scheduler: partly rewritten to be more robust, again
- settings: move Plex.tv credentials to the top

1.3.5.281
- fix tasks broken for 1.2 -> 1.3.5 upgraders

1.3.5.273 (same build as Beta Release 1.3.0.273) - changes from previous stable 1.2.11.180
- add a channel menu, making this plugin a hybrid (Agent+Channel)
- add a generic background task scheduler
- add a task to search for subtitles for items with missing subtitles (manually triggered and automatic)
- add artwork
- add Plex.tv credentials/token-generation support (needed for Plex Home users for the API to work)
- addic7ed: improve show name matching again
- channel: able to browse current on-deck and recently-added items, and refresh or force-refresh (search for new subtitles) single items
- add library/series/video blacklist for items which should be skipped in "Search for missing subtitles"-task
- add donation links
- change the license to The Unlicense (while keeping the original MIT license from subliminal.bundle intact)
- store subtitle information in internal plugin storage (for later usage)
- many internal code improvements
- update documentation

1.3.0.273
- more robust update functionality
- menu: add refresh button to menu (to see the task state updating)
- scheduler: actually skip a task if it's already running
- scheduler: better behaviour when a task is running and a single item is refreshed at the same time
- menu: enforce ascii on item titles

1.3.0.261
- removed localization again

1.3.0.259
- forgot locale-data

1.3.0.256
- fix force-refresh single items to actually force-refresh
- re-add babel library

1.3.0.253
- rewrote background tasks subsystem
- keep track of the status of a task and its runtime
- add task state in channel menu to "Search for missing subtitles"
- add date/time localization to channel menu
- hide plex token from logs, when requesting
- fix addic7ed show id parsing for shows with year set
- test PMS API connectivity and fail miserably if needed (channel disabled, scheduler disabled)
- feature-freeze for 1.3.0 final

1.3.0.245
- add the option to buy me a beer
- clarify menu items
- more robust scheduler handling (should fix the issues of scheduler runs in the past)
- internal cleanups
- add date_added to stored subtitle info (all of the 1.3.0 testers: please delete your internal subtitle storage using the channel->advanced menu)

1.3.0.232
- integrate plex.tv authentication for plex home users (test phase)
- menu cleanup
- more info in the menu (scheduler last and next run for example)
- hopefully fixed intent handling (should throw less errors now)
- fix version display in agent names

1.3.0.222
- bugfix for search missing subtitles
- schedduler: honor "never"

1.3.0.216
- add channel menu
- add generic task scheduler
- add functionality to search for missing subtitles (via recently added items)
- add artwork
- change license to The Unlicense
- ...

1.2.11.180
- fix #49 (metadata storage didn't work)
- add better detection for existing subtitles stored in metadata

1.2.11.177
- updated naming scheme to reflect rewrite.major.minor.build (this release is the same as 1.1.0.5)

1.1.0.5
- addic7ed: fixed error in show id search
- addic7ed: even better show matching
- adjusted default scores: TV: 85, movies: 23
- add support for com.plexapp.agents.xbmcnfo/xbmcnfotv (proposed to the author [here](https://github.com/gboudreau/XBMCnfoMoviesImporter.bundle/pull/63) and [here](https://github.com/gboudreau/XBMCnfoTVImporter.bundle/pull/70))

1.1.0.3
- addic7ed/tvsubtitles: be way smarter about punctuation in series names (*A.G.E.N.T.S. ...*)
- ditch LocalMediaExtended and incorporate the functionality in Sub-Zero (**RC-users: delete LocalMediaExtended.bundle and re-enable LocalMedia!**)
- remove (unused) setting "Restrict to one language"
- add "Treat IETF language tags as ISO 639-1 (e.g. pt-BR = pt)" setting (default: true)
- change default external storage to "current folder" instead of "/subs"
- adjust default scores

RC-5.2
- revert back to /plexinc-agents/LocalMedia.bundle/tree/dist instead of /plexinc-agents/LocalMedia.bundle/tree/master, as the current public PMS version is too old for that

RC-5.1
- make hearing_impaired option more configurable and clear (see #configuration-)

RC-5
- fix wrong video type matching by hinting video type to guessit
- update to newest LocalMediaExtended.bundle (incorporated plex-inc's changes)
- show page links for subtitles in log file instead of subtitle ID
- add custom language setting in addition to the three hardcoded ones
- if a subtitle doesn't match our hearing_impaired setting, ignore it
- add an optional boost for addic7ed subtitles, if their series, season, episode, year, and format (e.g. WEB-DL) matches

RC-4
- rename project to Sub-Zero
- incorporate LocalMediaExtended.bundle
- making this a multi-bundle plugin
- update default scores
- add icon

RC-3
- addic7ed/tvsubtitles: punctuation fixes (correctly get show ids for series like "Mr. Poopster" now)
- podnapisi: fix logging
- opensubtitles: add login credentials (for VIPs)
- add retry functionality to retry failed subtitle downloads, including configurable amount of retries until discarding of provider
- move possibly not needed setting "Restrict to one language" to the bottom
- more detailed logging
- some cleanup

RC-2
- fix empty custom subtitle folder creation
- fix detection of existing embedded subtitles (switch to https://github.com/tonswieb/enzyme)
- better logging
- set default TV score to 15; movie score to 30

RC-1
- fix subliminal's logging error on min_score not met (fixes #15)
- separated tv and movies subtitle scores settings (fixes #16)
- add option to save only one subtitle per video (skipping the ".lang." naming scheme plex supports) (fixes #3)

beta5
- fix storing subtitles besides the actual video file, not subfolder (fixes #14)
- "custom folder" setting now always used if given (properly overrides "subtitle folder" setting)
- also scan (custom) given subtitle folders for existing subtitles instead of redownloading them on every refresh (fixes #9, #2)

beta4
- ~~increased score of addic7ed subtitles a bit~~ (not existing currently)
- **support for newest Subliminal ([1.0.1](27a6e51cd36ffb2910cd9a7add6d797a2c6469b7)) and guessit ([0.11.0](2814f57e8999dcc31575619f076c0c1a63ce78f2))**
- **plugin now also [works with com.plexapp.agents.thetvdbdvdorder](924470d2c0db3a71529278bce4b7247eaf2f85b8)**
- providers fixed for subliminal 1.0.1 ([at least addic7ed](131504e7eed8b3400c457fbe49beea3b115bc916))
- providers [don't simply fail and get excluded on non-detected language](1a779020792e0201ad689eefbf5a126155e89c97)
- support for addic7ed languages: [French (Canadian)](b11a051c233fd72033f0c3b5a8c1965260e7e19f)
- support for additional languages: [pt-br (Portuguese (Brasil)), fa (Persian (Farsi))](131504e7eed8b3400c457fbe49beea3b115bc916)
- support for [three (two optional) subtitle languages](e543c927cf49c264eaece36640c99d67a99c7da2)
- optionally use [random user agent for addic7ed provider](83ace14faf75fbd75313f0ceda9b78161895fbcf) (should not be needed)
