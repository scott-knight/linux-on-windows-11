[Home](https://github.com/scott-knight/linux-on-windows-11) | [Previous Step](install-ubuntu.md) | [Uninstall](unregister-and-uninstall.md)

<br/>

# Customize Ubuntu

This doc outlines the specifics for setting up the Ubunu environment for Ruby, Rails, Node, etc.

<br/>

## Connecting VSCode

1. In the Linux console, type the following:

```sh
code .
```

This will load VSCode and connect to your Debian instance, and you will see the files of your home directory.

<br/>

2. Close VSCode and the terminal

<br/>

## Environment Setup

Next, we need system files for the environment. Run the following:

```sh
touch .zsh_functions .zsh_alias_list .gemrc .gitconfig && mkdir dev.projects dev.projects/sandbox-js dev.projects/sandbox-api
```
<br/>

## Setup Zsh

Make zsh your default shell, run the following:

```sh
chsh -s $(which zsh)
```

Once changed, restart the Linux instance.

You will be prompted to make a selection to create a `.zshrc` file. Select the option that simply adds the comment code and creates the file.

Close all instances and start a new Linux instance.

<br/>

## Install BREW for Linux

Reference found [here](https://docs.brew.sh/Homebrew-on-Linux)

1. To install BREW for Linux, run the following:

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

2. You should be prompted about default settings and installation. Simply accept the defaults.
3. Once finished, add the following to your `.zshrc` file

```sh
echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> $HOME/.zshrc && eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
```

4. Reload the shell.
5. Add BREW taps

```sh
brew tap Homebrew/homebrew-cask && brew tap Homebrew/homebrew-services && brew tap homebrew/cask-versions && brew tap homebrew/cask-fonts
```

6. Install stuff

```sh
ulimit -n 8192 && brew install git vips
```

Then

```zsh
brew postinstall docbook docbook-xsl
```
<br/>

## Install Postgres

Install postgres by doing the following:

```sh
brew install postgresql@15 && echo 'export PATH="/home/linuxbrew/.linuxbrew/opt/postgresql@15/bin:$PATH"' >> ~/.zshrc && source $HOME/.zshrc
```




[Home](https://github.com/scott-knight/linux-on-windows-11) | [Previous Step](install-ubuntu.md) | [Uninstall](unregister-and-uninstall.md)
