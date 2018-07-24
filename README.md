<img align="right" height="200px" src="https://raw.githubusercontent.com/matcornic/subify/master/images/logo.png"/>

Subify is a tool to download subtitles for your favorite TV shows and movies.
It is directly able to open the video with your default player, once the subtitle is downloaded.

Subify combines [SubDB Web API](http://thesubdb.com/), [OpenSubtitles API](http://trac.opensubtitles.org/projects/opensubtitles/wiki) and [Addic7ed](http://www.addic7ed.com/) to get the best subtitles for your video. It also considers that you use a default player interpreting srt subtitles when the video file name is the same than the srt file (ex: [VLC](http://www.videolan.org/vlc/)).

Subify gets the best match from several APIs in this order. This default behavior can easily be changed. See the documentation below

1. SubDB
2. OpenSubtitles
3. Addic7ed

## Installing
Download the [latest version of Subify](https://github.com/matcornic/subify/releases), and that's it. No need to install something else. Works on Linux, Mac OS (Darwin) and Windows

If you use Golang, you can get Subify and its binary directly with :
```shell
go get -u github.com/matcornic/subify
```

### Mac OS

In MacOS, you can use the power of **Service Automator** to add *Subify* options in the Finder contextual menu for your videos.

![Subify in one click](./images/subify_macos_service.png)
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fmatcornic%2Fsubify.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com%2Fmatcornic%2Fsubify?ref=badge_shield)

* Open Automator
* Create a Service
* On top :
  * Service receives select => *Movie files*
  * In => *Any application*
* Drag & drop a *Run shell script* action
  * Shell => `bin/bash`
  * Pass input => *as argument*
  * Copy the following code in the input :

```shell
# Change "/projects/go/bin/subify" by the path of your downloaded binary
# Use the commaned you want. Commands are explained below
/projects/go/bin/subify dl "$1" -o
```

* Save to service with the name that you want to be displayed in the Finder menu (e.g. *Subify - download subtitle and play video*)
* Enjoy your video with subtitles in one click
* Create as many as service you need (*don't open video*, *change API order*, *change language*, and so on)

![Subify in Automator](./images/subify_macos_automator.png)

## Get started
Note : the binary is usable as is. If you want to run the command from anywhere on your OS, make sure to add Subify home installation to your PATH environment variable

```shell
# Download subtitle with default language (English) from default APIs (SubDB, then OpenSubtitles, then Addic7ed)
subify dl <path_to_your_video>
# Download subtitle with default language (English), from default APIs (SubDB, then OpenSubtitles, then Addic7ed), then open video with your default player
subify dl <path_to_your_video> -o
# Download subtitle with french language, from default APIs (SubDB, then OpenSubtitles, then Addic7ed), and open with your default player
subify dl <path_to_your_video> -o -l fr
# Download subtitle with french language, if not found spanish, if not found english, from default APIs (SubDB, then OpenSubtitles, then Addic7ed)
subify dl <path_to_your_video> -l fr,es,en
# Download subtitle with default language, by searching first in OpenSubtitles, then in SubDB
subify dl <path_to_your_video> -a os,subdb
# Download subtitle with default language, by searching only in OpenSubtitles
subify dl <path_to_your_video> -a OpenSubtitles
```

## Documentation
### Global usage
```
Tool to handle subtitles for your best TV Shows and movies
http://github.com/matcornic/subify

Usage:
  subify [command]

Available Commands:
  dl          Download the subtitles for your video - 'subify dl --help'
  list        List information about something (ex: languages)

Flags:
      --config string   Config file (default is $HOME/.subify.|json|yaml|toml). Edit to change default behaviour
      --dev             Instanciate development sandbox instead of production variables
  -v, --verbose         Print more information while executing

Use "subify [command] --help" for more information about a command.
```

### Downloading command
```
Download the subtitles for your video (movie or TV Shows)
Give the path of your video as first parameter and let's go !

Usage:
  subify dl <video-path> [flags]

Aliases:
  dl, download


Flags:
  -a, --apis value        Overwrite default searching APIs behavior, hence the subtitles are downloaded. Available apis at 'subify list apis' (default [SubDB,OpenSubtitles,Addic7ed])
  -l, --languages value   Languages of the subtitle separate by a comma (First to match is downloaded). Available languages at 'subify list languages' (default [en])
  -o, --open              Once the subtitle is downloaded, open the video with your default video player (OSX: "open", Windows: "start", Linux/Other: "xdg-open")

Global Flags:
      --config string   Config file (default is $HOME/.subify.|json|yaml|toml). Edit to change default behavior
      --dev             Instanciate development sandbox instead of production variables
  -v, --verbose         Print more information while executing
```

### Listing command

```
List available languages

Usage:
  subify list languages [flags]

Aliases:
  languages, lang


Global Flags:
      --all             Shows all languages
      --config string   Config file (default is $HOME/.subify.|json|yaml|toml). Edit to change default behaviour
      --dev             Instanciate development sandbox instead of production variables
  -v, --verbose         Print more information while executing
```
```
List the available apis used by Subify

Usage:
  subify list apis [flags]

Global Flags:
      --config string   Config file (default is $HOME/.subify.|json|yaml|toml). Edit to change default behaviour
      --dev             Instanciate development sandbox instead of production variables
  -v, --verbose         Print more information while executing
```

## Overriding default configuration

Default configuration can be overridden. Instead of passing the same parameters again and again to the command, you can write a `JSON/YAML/TOML` in your home folder (`$HOME/.subify.|json|yaml|toml`). Here is an example with a `.subify.toml` file :

```toml

# Root is for all commands
[root]
verbose = false # Turn on to print more information by default
dev = false # Don't turn on, just for development purpose

# download for the download/dl command
[download]
languages = "en" # Searching for theses languages. Can be a list like : "fr,es,en"
apis = "SubDB,OpenSubtitles,Addic7ed" # Searching from these sites
```

## Release Notes
* **0.3.0** Feb 23, 2018
  * Add system notifications (for Linux, Windows and MacOS)
  * Add version command
* **0.2.0** Feb 19, 2018
  * Addic7ed implementation
  * Refactoring
  * Vendoring with dep
  * Dropping 32bit support
  * Add system notifications
  * Add version command
* **0.1.1** Jan 31, 2016
  * Language checking
  * OpenSubtitles API implementation
  * List of favorite languages (Downloads the first to match)
  * Vendoring (with glide)
  * List of available apis
  * Usage of APIs is customizable (can order Subdb search before OpenSubtitles for ex)
  * customizable default configuration with a conf file (for example to change the default language for all downloads)
* **0.1.0** Jan 15, 2016
  * Implement first init

## License

Subify is released under the Apache 2.0 license. See LICENSE.txt


[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fmatcornic%2Fsubify.svg?type=large)](https://app.fossa.io/projects/git%2Bgithub.com%2Fmatcornic%2Fsubify?ref=badge_large)