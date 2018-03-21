# Poe::Sniper

[![Build Status](https://travis-ci.org/thisismydesign/poe-sniper.svg?branch=master)](https://travis-ci.org/thisismydesign/poe-sniper)
[![Coverage Status](https://coveralls.io/repos/github/thisismydesign/poe-sniper/badge.svg?branch=master)](https://coveralls.io/github/thisismydesign/poe-sniper?branch=master)

### Make instant offers for items listed for trading in Path of Exile without any interruptions to the game

*You are viewing the README of the 0.4.0 version. You can find the README of the 0.3.0 release [here](https://github.com/thisismydesign/poe-sniper/tree/v0.3.0).*

## How it works

- Copy search links from poe.trade
- [Wait for notification to appear ingame](http://i.imgur.com/RkTK4DN.png)
- [Press button](http://i.imgur.com/QpZqHJD.png)
- Enjoy

## Features

- 100% ToS compliant
- Queries new listsings instantly via poe.trade live mode
- Handles any custom search created via poe.trade
- Uses Windows notifications which:
  - don't remove focus from the game
  - have a configurable timeout
  - are easily dismissable anytime
- Places whisper message on clipboard for faster interaction
- Consumes far less resources than running poe.trade in browser
- Queues listings if more than one arrives within the configured notification timeframe (alerts that there are further items)

## Side effects

- PoE needs to run in windowed or windowed fullscreen mode for the notifications to show ingame
- Your clipboard will be altered with every new notification regardless whether you have any other content on it

## Installation and usage

Download the latest release (poe-sniper.zip) from the [releases page](https://github.com/thisismydesign/poe-sniper/releases).
Extract it, inside the poe-sniper folder you will find 3 files:
- poe-sniper.exe
- config.ini
- input/example_input.json

Modify `config.ini` and `example_input.json` according to your needs.

[`example_input.json`](https://github.com/thisismydesign/poe-sniper/blob/master/input/example_input.json) contains links to the searches you want to be notified about and a custom name for each search in [JSON format](https://www.w3schools.com/js/js_json.asp). Assuming the linked example you will receive notifications like: `New Tabula on BSC listed. ~b/o 10 chaos`. Be sure to include links in the same format as the example.

[`config.ini`](https://github.com/thisismydesign/poe-sniper/blob/master/input/config.ini) contains Poe::Sniper's configuration such as how long the notifications should last, where the input file is located, etc.

Run poe-sniper.exe

You're all set. Enjoy your interrupt free PoE.

Additionally [here's a modified version of the Lutbot AutoHotkey Macro](https://github.com/thisismydesign/poe-lutbot-ahk) where the 'Paste' option is added allowing you to hotkey sending messages from the clipboard. That's right. One click to set up a trade. It's awesome, I know!

## Disclaimer

This tool was created to improve the trading experience. As we saw in the past these great things used by the wrong people can quickly turn into bad things. With that said we believe the solution is creating a fair competition by making the best technology available to everyone. This is why we're releasing this to the public.

## Technical details

Written in Ruby, packaged as standalone Windows executable with [OCRA](https://github.com/larsch/ocra/).

*Please be aware that we're sending anonymous data to gather app usage statistics.*

### Usage from source

Create `.env` file as described in the [analytics chapter](#analytics). See default tasks in [`Rakefile`](Rakefile) for running tests and building the executable.

#### Start in normal mode

```
bundle && bundle exec ruby -e 'require "#{File.dirname __FILE__}/lib/poe/sniper"; Poe::Sniper.run("#{File.dirname __FILE__}/input/config.ini")'
```

#### Start in offline mode with example socket data

```
bundle && bundle exec ruby -e 'require "#{File.dirname __FILE__}/lib/poe/sniper"; Poe::Sniper.offline_debug("#{File.dirname __FILE__}/input/config.ini", "#{File.dirname __FILE__}/spec/resources/example_socket_data.json")'
```

#### Debug

- Test socket connection retry by providing an incorrect `api_url` in `config.ini`
- Test HTTP connection retry by going offline

### Analytics

The app is using [Segment](https://segment.com/) (via the [analytics-ruby](https://segment.com/docs/sources/server/ruby/) gem) to gather anonymous usage statistics. Open source client side key handling sucks, the current approach to maximize obscurity is to read the Base64 encoded write key from the uncommitted `ANALYTICS_KEY` environment variable stored in a `.env` file which is packaged with the build.

Certificates are stored in [config/cacert.pem](config/cacert.pem) and are referenced in the `SSL_CERT_FILE` env var in order to fix the `certificate verify failed` issue on Windows ([see](https://gist.github.com/fnichol/867550)).

### Improvements / TODO

- Get rid of Nokogiri
- Use a non-EM solution for websokets
  - Run sockets on their separate threads
    - Increase retry timeout freely since other sockets are no longer blocked
- Make module namespacing consistent
- Make OCRA build work in CI ([AppVeyor issue](https://github.com/larsch/ocra/issues/134))

## Development

This gem is developed using Bundler conventions. A good overview can be found [here](http://bundler.io/v1.14/guides/creating_gem.html).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/thisismydesign/poe-sniper.
