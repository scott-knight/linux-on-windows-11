[Home](README.md) | [Previous Step](install-ubuntu.md) | [Uninstall](unregister-and-uninstall.md)

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

## Setup `wslopen` for letter_opener

In a rails project, it's possible to use the [letter_opener gem](https://github.com/ryanb/letter_opener) for posting email in the dev environment. The letter_opener gem uses the launchy gem to launch/post generated email to a browser tab. On a Mac (or possibly a native Linux setup) the browser is set by the system and launchy just works. However, with WSL we have to make the browser connection for launchy. In this example, we will point to location on the Windows drive for [Brave Browser](https://brave.com)

Brave is installed on the `C` drive in the following location: `C:\Program Files\BraveSoftware\Brave-Browser\Application\brave.exe`. This needs to be translated to a WSL2 path: `/mnt/c/Program\ Files/BraveSoftware/Brave-Browser/Application/brave.exe`.

We need to create a couple of files to get everything to work:

```zsh
mkdir $HOME/.local/bin
```

<br/>

In the code snippet below, you can replace `/mnt/c/Program\ Files/BraveSoftware/Brave-Browser/Application/brave.exe` with the browser of your choice. Once you have the path to your chosen browser, run the following:

```zsh
echo '#!/usr/bin/zsh' >> $HOME/.local/bin/wslopen && echo '/mnt/c/Program\ Files/BraveSoftware/Brave-Browser/Application/brave.exe "file://$(wslpath -m ${1/"file://"/})"' >> $HOME/.local/bin/wslopen && chmod +x $HOME/.local/bin/wslopen
```

<br/>

Run the following in the console:

```zsh
echo '# .local/bin -- used with letter_opener' >> .zshrc && echo 'export PATH="${HOME}/.local/bin:$PATH"' >> .zshrc && echo 'export BROWSER="wslopen"' >> .zshrc
```

Then reload the shell.

<br/>

## Setup Zsh

Make zsh your default shell, run the following:

```sh
chsh -s $(which zsh)
```

Once changed, restart the terminal instance.

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

<br/>

## Setup SSH Key

To connect via ssh to external services you will need to generate a new private and public key. Follow the instructions [found here](https://docs.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) to create a new set.

Once your keys are created, run the following:

(Run this if `.ssh` doesn't exist:
```zsh
mkdir -m a=rwx $HOME/.ssh
```

<br/>

### If You Have Existing SSH Files

If you already have ssh files created and stored somewhere, copy and paste them into you .ssh dir

![ssh-files](https://user-images.githubusercontent.com/516548/209577940-c9db2c03-8820-45d0-8c7d-9493adb55405.png)

<br/>

### If You Need to Create SSH Files

```sh
touch $HOME/.ssh/config && nano $HOME/.ssh/config
```

Copy the following to the new `config` file:

```sh
Host *
  AddKeysToAgent yes
  IdentityFile ~/.ssh/id_ed25519
```

You will need to add the new public key to your github account. You can copy the key by running the following:

```sh
clip.exe < ${HOME}/.ssh/id_ed25519.pub
```

<br/>

If you copy contents from another `.ssh` to this setup, you will may need to change the file level permissions:

```sh
chmod 400 $HOME/.ssh/id_ed25519
```

<br/>

## INSTALL KEYCHAIN

Run the following:

```sh
wajig install -y keychain
```

<br/>

Once installed, start `keychain` to create the `$HOME\.keychain` directory and start the `ssh-agent`:

```sh
keychain
```

<br/>

Using VIM or Nano, copy this to the bottom of your .zshrc file:

```zsh
nano .zshrc
```

<br/>

BIGBRAIN is the hostname of my computer. If you run `ls -FlaG $HOME/.keychain` in the console,
you will see the hostname of your computer. Replace BIGBRAIN with that name

```
# Keychain
export HOSTNAME=BIGBRAIN
/usr/bin/keychain --clear $HOME/.ssh/id_ed25519
source $HOME/.keychain/$HOSTNAME-sh
```

<br/>

## Copy .gitconfig

Using VIM or Nano, replace the contents of `.gitconfig` with the following:

```zsh
nano .gitconfig
```

<br/>

```sh
[user]
  email = [your email]
  name = [your-username]

[pull]
  rebase = false

[alias]
  co = checkout
  ci = commit
  st = status
  br = branch

[pager]
  branch = false
```

Replace the email and username with your values.

Close the terminal, then open a new terminal instance.

<br>

## INSTALL RBENV

To install RBENV, run the following:

```sh
git clone https://github.com/rbenv/rbenv.git ${HOME}/.rbenv
```

Add the following to `.zshrc`:

```zsh
nano .zshrc
```

<br/>

```sh
# RBENV
export RBENV_ROOT="${HOME}/.rbenv"
export PATH="${RBENV_ROOT}/bin:${RBENV_ROOT}/shims:$PATH"
if which rbenv > /dev/null; then eval "$(rbenv init - zsh)"; fi
```

Close and reload the terminal instance

<br/>

## Install NVM

NVM documenation is [found here](https://github.com/nvm-sh/nvm#installing-and-updating). To install NVM, run the following:

```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
```

Once installed, reload the shell; then run the following:

```sh
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
```

Then

```sh
nvm install --lts && npm install npm@latest -g
```

<br/>

## Install oh-my-zsh

Source found [here](https://ohmyz.sh/)

To install oh-my-zsh, run the following:

```sh
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

<br/>

### Install Oh-My-ZSH Plugins

Run the following:

```sh
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/plugins/zsh-autosuggestions &&
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting &&
echo "source ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc &&
git clone https://github.com/gusaiani/elixir-oh-my-zsh.git $HOME/.oh-my-zsh/custom/plugins/elixir
```

<br/>

### Install Spaceship-prompt

The [spaceship-prompt](https://github.com/spaceship-prompt/spaceship-prompt) is a clean .oh-my-zsh theme. To install it, run the following:

```sh
git clone https://github.com/spaceship-prompt/spaceship-prompt.git "$ZSH_CUSTOM/themes/spaceship-prompt" --depth=1 && ln -s "$ZSH_CUSTOM/themes/spaceship-prompt/spaceship.zsh-theme" "$HOME/.oh-my-zsh/themes/spaceship.zsh-theme"
```

<br/>

## Copy the .zshrc Content

Run the following to clear out .zshrc:

```sh
truncate -s 0 .zshrc
```

<br/>

Using VIM or NANO, replace the entire `.zshrc` file with this code:

```zsh
nano .zshrc
```

<br/>

Copy the [contents of .zshrc](ZSHRC.md) and save.

Reload the terminal instance.

<br/>

## Copy .zsh_alias_list Content

Using VIM or NANO, replace the entire `.zsh_alias_list` file with this code:

```zsh
nano .zsh_alias_list
```

<br/>

Copy the [contents of .zsh_alias_list](ZSH_ALIAS_LIST.md) and save.

Reload the terminal instance.

From now on, when you want to reload the shell, simply type `reload`

<br/>

## Copy .zsh_functions

Using VIM or NANO, replace the contents of `.zsh_functions` with the following:

```zsh
nano .zsh_functions
```

<br/>

Copy the [contents of .zsh_functions](ZSH_FUNCTIONS.md) and save.

Type `reload` to reload the shell.

<br/>

## Copy .zsh_jobsauce

This is an example of personal/project content you may needed within console. (You can see how this file is called in [.zshrc](ZSHRC.md))

Using VIM or NANO, replace the contents of `.zsh_jobsauce` with the following:

```zsh
nano .zsh_jobsauce
```

<br/>

Copy the [contents of .zsh_jobsauce](ZSH_JOBSAUCE.md) and save.

Type `reload` to reload the shell.

<br/>

## Copy .gemrc

Using VIM, replace the contents of `.gemrc` with the following:

```zsh
nano .gemrc
```

<br/>

```ruby
---
:backtrace: false
:bulk_threshold: 1000
:sources:
- https://rubygems.org/
:update_sources: true
:verbose: true
install: --no-rdoc --no-ri --no-document
update: --no-rdoc --no-ri --no-document
```

<br>

## Reload

After you have completed the setup, close and reload the terminal instance.

<br/>

## Setup for Ruby and Rails

Setup instructions for Ruby and Rails can be [found here](RUBY_RAILS_SETUP.md)

<br/>

## Setup Systemd

This is enabled by default with the current distros of Ubuntu. Settings can be view by running the following:

```zsh
sudo nano /etc/wsl.conf
```

<br/>

You can view a list of running services by calling the following:

```zsh
systemctl list-unit-files --type=service
```

You should see a table of info:

![systemctl](https://user-images.githubusercontent.com/516548/201456791-a90c2475-c0e5-4dc8-a113-fc3637cb059e.png)

<br/><br/>

[Home](README.md) | [Previous Step](install-ubuntu.md) | [Uninstall](unregister-and-uninstall.md)
