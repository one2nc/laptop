# Laptop

Laptop is a script to set up a macOS or Linux laptop for backend and site reliability engineering work at One2N. We expect you to run the Linux or Mac scripts in your first week of joining One2N. Various useful tools (we are opinionated) are installed as part of these scripts.

It can be run multiple times on the same machine safely.
It installs, upgrades, or skips packages
based on what is already installed on the machine.

## Requirements

We support:

- macOS Monterey (12.3) on Apple Silicon and Intel

Older versions may work but aren't regularly tested.
Bug reports for older versions are welcome.

## Install

Download the script:

```sh
curl --remote-name https://raw.githubusercontent.com/one2nc/laptop/main/mac
```

Review the script (avoid running scripts you haven't read!):

```sh
less mac
```

Execute the downloaded script:

```sh
sh mac 2>&1 | tee ~/laptop.log
```

Optionally, review the log:

```sh
less ~/laptop.log
```

Optionally, [install thoughtbot/dotfiles][dotfiles].

[dotfiles]: https://github.com/thoughtbot/dotfiles#install

## Debugging

Your last Laptop run will be saved to `~/laptop.log`.
Read through it to see if you can debug the issue yourself.
If not, copy the lines where the script failed into a
[new GitHub Issue](https://github.com/thoughtbot/laptop/issues/new) for us.
Or, attach the whole log file as an attachment.

In case some package already exists (e.g. google-chrome), and the
script fails due to some error like

```
Error: It seems there is already an App at '/Applications/Google Chrome.app'.
```

use the `-f` (or `--force`) option and run that command again. In this case, run
`brew install --cask google-chrome --force`. This should reinstall Chrome properly.
Once done, you can run the main script again.

## What it sets up

macOS tools:

- [Homebrew] for managing operating system libraries.

[Homebrew]: http://brew.sh/

Unix tools:

- [Git] for version control
- [OpenSSL] for Transport Layer Security (TLS)
- [Zsh] as your shell

[Git]: https://git-scm.com/
[OpenSSL]: https://www.openssl.org/
[Zsh]: http://www.zsh.org/

GitHub tools:

- [GitHub CLI] for interacting with the GitHub API

[GitHub CLI]: https://cli.github.com/

IDEs:

- [Intellij community edition] for JVM languages designed to maximize developer productivity.
- [Visual studio code] a free-editor that helps the programmer write code, helps in debugging and corrects the code using the intelli-sense method.
- [Sublime text] is Sublime Text is a shareware cross-platform source code editor.
- [Vim] Vim is a highly configurable text editor built to make creating and changing any kind of text very efficient.

[intellij community edition]: https://www.jetbrains.com/idea/download/#section=mac
[Visual studio code]: https://code.visualstudio.com/
[Sublime text]: https://www.sublimetext.com/
[Vim]: https://www.vim.org/

Browsers:

- [Google Chrome] is an cross-platform web browser developed by Google.
- [Firefox] is an extensible free and open-source web browser developed by Mozilla corp.

[Google Chrome]: https://www.google.com/chrome/
[Firefox]: https://www.mozilla.org/

Markdown:

- [Obsidian] is a powerful knowledge base on top of a local folder of plain text Markdown files.
- [Macdown] is an open source Markdown editor for MacOS.

[Obsidian]: https://obsidian.md/
[Macdown]: https://macdown.uranusjr.com/

Slack:

- [Slack] is a messaging program designed specifically for the office.

[Slack]: https://slack.com

Notion:

- [Notion] is a project management and note-taking software.

[Notion]: https://notion.so

Maccy:

- [Maccy] for Clipboard management.

[Maccy]: https://maccy.app/

Libreoffice:

- [Libreoffice] is a free and open-source office productivity software suite.

[Libreoffice]: https://www.libreoffice.org/

Postman:

- [Postman] for testing API.

[Postman]: https://www.postman.com/

Scroll Reverser:

- [Scroll Reverser] is a free Mac app that reverses the direction of scrolling, with independent settings for trackpads and mice.

[Scroll Reverser]: https://pilotmoon.com/scrollreverser/

Rectangle:

- [Rectangle] Move and resize windows in macOS using keyboard shortcuts or snap areas.

[Rectangle]: https://rectangleapp.com/

Table Plus:

- [Table Plus] for database management.

[Table Plus]: https://tableplus.com/

WhatsApp:

- [WhatsApp] for messaging.

[WhatsApp]: https://www.whatsapp.com/

Alfred:

- [Alfred] is an alternative to Spotlight.

[Alfred]: https://www.alfredapp.com/

Zoom:

- [Zoom] is a video conferencing platform.

[Zoom]: htttps://zoom.us

Docker Desktop:

- [Docker Desktop] for containerization.

[Docker Desktop]: https://www.docker.com/products/docker-desktop

Google Drive:

- [Google Drive] backup to Google Photos, and access all of your content directly from your Mac.

[Google Drive]: https://drive.google.com

Lastpass:

- [lastpass] for password management.

[lastpass]: https://lastpass.com

It should take less than 15 minutes to install (depends on your machine).

## Customize in `~/.laptop.local`

Your `~/.laptop.local` is run at the end of the Laptop script.
Put your customizations there.
For example:

```sh
#!/bin/sh

brew bundle --file=- <<EOF
brew "Caskroom/cask/dockertoolbox"
brew "go"
brew "ngrok"
brew "watch"
EOF

default_docker_machine() {
  docker-machine ls | grep -Fq "default"
}

if ! default_docker_machine; then
  docker-machine create --driver virtualbox default
fi

default_docker_machine_running() {
  default_docker_machine | grep -Fq "Running"
}

if ! default_docker_machine_running; then
  docker-machine start default
fi

fancy_echo "Cleaning up old Homebrew formulae ..."
brew cleanup
brew cask cleanup

if [ -r "$HOME/.rcrc" ]; then
  fancy_echo "Updating dotfiles ..."
  rcup
fi
```

Write your customizations such that they can be run safely more than once.
See the `mac` script for examples.

Laptop functions such as `fancy_echo` and
`gem_install_or_update`
can be used in your `~/.laptop.local`.

See the [wiki](https://github.com/thoughtbot/laptop/wiki)
for more customization examples.

## Contributing

Edit the `mac` file.
Document in the `README.md` file.
Follow shell style guidelines by using [ShellCheck] and [Syntastic].

```sh
brew install shellcheck
```

[ShellCheck]: http://www.shellcheck.net/about.html
[Syntastic]: https://github.com/scrooloose/syntastic

### Testing your changes

Test your changes by running the script on a fresh install of macOS.
You can use the free and open source emulator [UTM].

Tip: Make a fresh virtual machine with the installation of macOS completed and
your user created and first launch complete. Then duplicate that machine to test
the script each time on a fresh install thats ready to go.

[UTM]: https://mac.getutm.app

Thank you, [contributors]!

[contributors]: https://github.com/thoughtbot/laptop/graphs/contributors

By participating in this project,
you agree to abide by the thoughtbot [code of conduct].

[code of conduct]: https://thoughtbot.com/open-source-code-of-conduct

## License

Laptop is © 2011-2022 thoughtbot, inc.
It is free software,
and may be redistributed under the terms specified in the [LICENSE] file.

[LICENSE]: LICENSE

## About thoughtbot

![thoughtbot](https://thoughtbot.com/brand_assets/93:44.svg)

Laptop is maintained and funded by thoughtbot, inc.
The names and logos for thoughtbot are trademarks of thoughtbot, inc.

We are passionate about open source software.
See [our other projects][community].
We are [available for hire][hire].

[community]: https://thoughtbot.com/community?utm_source=github
[hire]: https://thoughtbot.com?utm_source=github
