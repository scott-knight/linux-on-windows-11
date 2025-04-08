[Home](README.md) | [Previous Step](https://github.com/scott-knight/linux-on-windows-11/blob/main/setup-ssh.md) | [Next Step](https://github.com/scott-knight/linux-on-windows-11/blob/main/customize-the-ubuntu-environment.md)

This document describes how to setup various tools use for app development.

<br>

## INSTALL RBENV

To install RBENV, run the following:

```sh
cd $HOME && git clone https://github.com/rbenv/rbenv.git $HOME/.rbenv && cd $HOME && mkdir $HOME/.rbenv/plugins && git clone https://github.com/rbenv/ruby-build.git $HOME/.rbenv/plugins/ruby-build && git clone https://github.com/jf/rbenv-gemset.git $HOME/.rbenv/plugins/rbenv-gemset && cd $HOME/.rbenv && cd $HOME
```

<br>

Add the following to bottom `.zshrc`:

```sh
# RBENV
export RBENV_ROOT="${HOME}/.rbenv"
export PATH="${RBENV_ROOT}/bin:${RBENV_ROOT}/shims:$PATH"
if which rbenv > /dev/null; then eval "$(${RBENV_ROOT}/bin/rbenv init - zsh)"; fi
```

<br>

Close and reload the terminal instance

<br>

## Install NVM

NVM documenation is [found here](https://github.com/nvm-sh/nvm#installing-and-updating). To install NVM, run the following:

```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
```

<br>

Once installed, reload the shell; then run the following:

```sh
nvm install --lts && npm install npm@latest -g
```

<br>

[Home](README.md) | [Previous Step](https://github.com/scott-knight/linux-on-windows-11/blob/main/setup-ssh.md) | [Next Step](https://github.com/scott-knight/linux-on-windows-11/blob/main/customize-the-ubuntu-environment.md)
