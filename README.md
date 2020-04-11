# welcome

> A more beautiful and informative shell login message

## Introduction

If you use the shell on your computer or server a lot, it makes sense to customize everything to your needs. Still, many people I know get greeted by a dollar sign and a boxy cursor.

I wanted to see at the first glance on which host I'm working and what the current state of the system is. All of that should be beautiful and flexible to configure. So I created welcome, which looks like this on my Uberspace account:

![](https://cdn.codesignd.net/projects/welcome/screenshot.png)

The information below the fancy dynamic ASCII art comes from a modular information provider setup. Each line is its own shell script that can do whatever it wants and output one or more lines of (colored) text. The information then gets layouted automatically by indenting it properly and adding a heading in front of it.

*BTW, before you ask: That is [iTerm 2](http://iterm2.com) with the Solarized Dark color preset and [oh-my-zsh](http://ohmyz.sh) with the agnoster theme.*

## Setup

### Install welcome

The `welcome` script from the `bin` directory can be installed anywhere in your `$PATH`. `~/bin` makes sense, but it can be any directory.

### Setup a configuration directory

The configuration of `welcome` can either be stored in `~/.welcome` or in `$XDG_CONFIG_HOME/welcome` (which defaults to `~/.config/welcome`). Please create this directory and place all files from the following steps into it.

### Install figlet, a dependency of welcome

[figlet](http://www.figlet.org) is a command line tool to generate fancy ASCII art from text and is a required depencency of welcome.

1. Install figlet itself. You can build it from source, but much easier (for example on Uberspace) is the following command to setup everything automatically:
	```bash
	toast arm ftp://ftp.figlet.org/pub/figlet/program/unix/figlet-2.2.5.tar.gz
	```

2. Get colossal, a figlet ASCII font that looks great but is not included by default: Download the file [colossal.flf](http://www.figlet.org/fonts/colossal.flf) from the figlet website and place it in `.welcome/colossal.flf`.

### Create your own information providers

The reason welcome is so flexible is the modular information provider system. There are two different kinds of information providers:

#### Summarizers

A summarizer is a function that returns one or more lines of text. Summarizers are expected to always output information and are therefore useful for information like the current date, the system uptime etc.

You can place as many summarizers as you need in `.welcome/summarizers`. Each summarizer is a shell script with a specific structure:

```bash
function helloWorldSummarizer() {
	echo "World"
}

registerSummarizer "Hello" helloWorldSummarizer
```

Each summarizer must contain at least one function that outputs arbitrary text. You then need to use `registerSummarizer <label> <function>` to register your function.
This summarizer will output a simple `Hello: World`.

The order of the summarizers gets defined by the order in the file system. So to force a specific order, simply prepend numbers, like so: `01-helloworld.sh`.

**There is one thing you need to do before this can work: Your summarizers must actually be executable files to make sure only files you wanted to run are run, so please set `chmod +x .welcome/summarizers/*`**.

#### Warners

Warners are the second kind of information provider. The difference to summarizers is that a warner does not have to output anything. They are useful for information that is only relevant in specific situations (for example when a package update is available like in the screenshot above).

You can place as many warners as you need in `.welcome/warners`. Warners are arbitrary binaries that simply output text.

The order of the warners also gets defined by the order in the file system. So to force a specific order, simply prepend numbers, like so: `01-helloworld.sh`.

**Same here: Please set `chmod +x .welcome/warners/*` to make sure only files you wanted to run are run**.

#### Examples

You can find the code for the example from the screenshot in the [my-welcome](https://git.lukasbestle.com/welcome/my-welcome) repository.

### Test and add to your shell startup file

You can now type `welcome` and the output should look quite similar to mine above. The only thing left to do is to add `welcome` to your `.bash_profile` or `.zshrc` file to run `welcome` when logging in.

## Author

- Lukas Bestle <mail@lukasbestle.com>

## License

This project was published under the terms of the MIT license. You can find a copy [over at the repository](https://git.lukasbestle.com/welcome/welcome/blob/master/LICENSE.md).
