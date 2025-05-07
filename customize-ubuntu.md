[Home](README.md) | [Previous Step](install-ubuntu.md) | [Next Step](https://github.com/scott-knight/linux-on-windows-11/blob/main/setup-ssh.md)
This document describes how to customize your installation of Ubuntu.

<br>

## Set Ubuntu as the Default

1. Open Windows Terminal. Click the down-arrow and select `Settings`

![terminal-select-settings](https://github.com/user-attachments/assets/dfd2dce4-527d-42b2-82c0-0dab4105718d)

<br>

2. In the default terminal setting, select the Ubuntu instance and click `Save`

![select-setting](https://github.com/user-attachments/assets/be9b70bf-681b-4f67-8c64-fc248d069fda)

<br>

3. Close and re-open Windows Terminal

<br>

## Update Ubuntu

1. To update Ubuntu, run the following:

```sh
sudo bash -c 'for i in update {,dist-}upgrade auto{remove,clean}; do apt-get $i -y; done'
```

<br><br>

## Install Wajig

[`Wajig`](https://wiki.debian.org/Wajig) is a simplifed and more unified command line interface for package management. It adds a more intuitive quality to the user interface.

To install wajig, run the following:

```sh
sudo apt install wajig aptitude -y && wajig update && wajig upgrade -y && wajig distupgrade -y && wajig autoremove && wajig autoclean
```

You should use the `wajig` command to update Ubuntu from now on.

<br><br>

## Install System Utilities

1. Install system utilities by running the following:

```sh
wajig install -y apt-transport-https  autoconf  bison  build-essential  checkinstall  clang  curl  gcc giflib-tools  git  gpg  gnupg2 libexpat1-dev libffi-dev libheif-dev libgdbm-dev libgdbm6 libglib2.0-0 libglib2.0-dev libgsf-1-dev libheif-dev liblzma-dev libjpeg-dev liblcms2-dev libpoppler-glib8 libpoppler-glib-dev libpng-dev libpq-dev libreadline-dev librsvg2-dev libtiff5-dev libssl-dev libwebp-dev libxml2-dev libxslt-dev libyaml-dev lsb-release make  patch  pkg-config  wget  vim  zlib1g zlib1g-dev  zsh
```

OR shorter version

```
wajig install -y apt-transport-https autoconf bison build-essential checkinstall curl git gpg gnupg2 libexpat1-dev libreadline-dev libffi-dev libssl-dev libxml2-dev libyaml-dev lsb-release make patch pkg-config wget vim zlib1g zlib1g-dev zsh
```

<br/>

2. Once everything is installed, run the following:

```sh
wajig update && wajig upgrade -y && wajig autoremove && wajig autoclean
```

<br>

## Connecting VSCode

1. If you've installed VSCode, in the Ubuntu console, type the following:

```sh
code .
```

2. This will load VSCode and connect to your Ubuntu instance, and you will see the files of your home directory. Click the checkbox `Trust the authors...` and the button `YES, I trust the authors` 

![code-trust](https://github.com/user-attachments/assets/1be841c2-9ec9-4df9-80b5-5b9b77c38b65)

<br>

2. Close VSCode and the restart the terminal

<br>

## Environment Setup

Next, we need system files for the environment. Run the following:

```sh
touch .zsh_functions .zsh_alias_list .zsh_projects .zsh_history .gemrc .gitconfig && mkdir dev.projects
```

<br>

## Setup Zsh

Make zsh your default shell, run the following:

```sh
chsh -s $(which zsh)
```

Once changed, restart the terminal instance.

You will be prompted to make a selection to create a `.zshrc` file. Select the option (0) that simply adds the comment code and creates the file.

Close all instances and start a new Linux instance.

<br/>

## Install BREW for Linux

Reference found [here](https://docs.brew.sh/Homebrew-on-Linux)

1. To install BREW for Linux, run the following:

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

2. You should be prompted about default settings and installation. Simply accept the defaults.
3. Once finished, run the following in the console:
   
```sh
echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> $HOME/.zshrc && eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
```

4. Reload the shell.
5. Add BREW taps

```sh
brew tap homebrew/linux-fonts
```

6. Install stuff

```sh
ulimit -n 8192 && brew install git font-caskaydia-cove-nerd-font font-caskaydia-mono-nerd-font
```

Once it completes installing, close and reopen the Windows Terminal

<br>

## Setting the Look and Feel of the Console

You don't have to do this step, however, if you prefer a dark console vs the default purple background of Ubuntu, you can customize it by doing the following steps.

1. Open Windows Terminal. Click the down-arrow and click `Settings`

![terminal-select-settings](https://github.com/user-attachments/assets/318c96d7-9520-4a68-9fd2-8a7decadab8d)

<br>

2. In the lower left corner of the settings, scroll until you see the instance of Ubuntu and click it

![select-ubuntu](https://github.com/user-attachments/assets/8c9a5771-9cc9-42b7-890f-4a03eefbd16b)

<br>

3. Once selected, on the right side of settings windows, scroll to the bottom and select `Appearance`

![select-apperance](https://github.com/user-attachments/assets/79cc661f-d9c0-4beb-9c0c-9b2961e12c17)

<br>

4. In the `Text` section, select `Campbell` in `Color scheme`. and select `CaskaydiaCove Nerd Font` in `Font Face`. Then click the `Save` button at the bottom

![select-console-options](https://github.com/user-attachments/assets/ba46ec45-0396-4549-bf7f-559b485aa1f9)

<br>

5. Close and re-open the Windows Terminal

<br>

[Home](README.md) | [Previous Step](install-ubuntu.md) | [Next Step](https://github.com/scott-knight/linux-on-windows-11/blob/main/setup-ssh.md)
