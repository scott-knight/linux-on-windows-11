PREV ... NEXT

# Debian
Debiain is a thinner install of Linux than Ubuntu. As of 11/11/2022, `Systemd` is required to run services in Debian, but systemd isn't installed with the WSL version of Debian. Oddly, this wasn't the case during the summer of 2022, but something changed. This installation document addresses the current state of installing everything needed to get Debian running to run a Rails/Postgresql instance. 

*I find that Windows and WSL installs change often. If things don't work as described in this doc, they changed something. By the time you read this doc, it's possible that some of these steps may no longer be required. It's also possible that new steps will be required to get everything working. Either way, I do what I can to keep this document updated, but be prepared to troubleshoot issues as they arise.*

<br/>

## Removing Debian

*Important:* Everything in the debian instance will be lost when you uninstall it. If you have content (pictures, files, ssh keys, etc) that you want to save, you will need to save that content before removing the debian instance. If you need to remove an installation of debian, it's simple. In the `Windows PowerShell` (running as administrator), run the following

```bash
wsl.exe --shutdown
wsl.exe --unregister Debian
```

<br/>

## Installing Debian

1 . To install Debian, in the `Windows PowerShell` (running as administrator), run the following:

```sh
wsl.exe --install -d Debian
```

2. Once downloaded and installed, it will open a new terminal instance. You will be promted to create a username and password. Once entered, the setup will complete.

![debian-setup](https://user-images.githubusercontent.com/516548/192112953-e95b93a0-5c68-407a-8ae3-ea0b15ad8fe4.png)

<br/>

3. When finished, click the down-arrow in the console's tab bar, then click `Settings`.

![new-settings](https://user-images.githubusercontent.com/516548/192082679-8cc094a2-e920-4b00-943e-91a3e75ccb4b.png)

<br/>

4. On the left-nav, scroll down and click `Debian`. On the right, select `Apperance`. Change the color scheme to `Campbell` and the font face to `CaskaydiaCove Nerd Font Mono`. Then click `Save` and close the settings tab.

![debian-font](https://user-images.githubusercontent.com/516548/193384119-47cb6b12-ffdb-468d-b65b-76b004b6b062.png)

<br/>

## Update Debian and Install System Utilities

1. To update Debian, run the following:

```sh
sudo bash -c 'for i in update {,dist-}upgrade auto{remove,clean}; do apt-get $i -y; done'
```

2. Install system utilities by running the following:

```sh
sudo apt-get install -y aptitude autoconf bison build-essential checkinstall clang curl daemonize dbus gawk git gcc libc6 libssl-dev libpq-dev libyaml-dev libreadline-dev libncurses-dev libffi-dev libgdbm6 libgdbm-dev libdb-dev make pkg-config policykit-1 python3 python3-pip python3-psutil systemd systemd-container wget vim zlib1g-dev zsh
```

3. Install VIPS libraries (used by Rails to manage images):

```sh
sudo apt-get install -y libglib2.0-0 libglib2.0-dev libpoppler-glib8 libheif-dev libvips-dev libvips
```

4. Once everything is installed, run the following:

```sh
sudo aptitude full-upgrade
```
   
<br>

## Setup Systemd

This step is very important! We will install `Brew` which will run both `Postgres` and `Redis` instances (and any other service you choose to install via Brew). We will need to setup Debian to use [Genie for Debian](https://github.com/arkane-systems/genie#debian).

1. Follow the setup steps [found here](https://arkane-systems.github.io/wsl-transdebian/)

```bash
function setupTransdeb() {  
  DEB_URL="https://arkane-systems.github.io/wsl-transdebian/apt/ $(lsb_release -cs) main"

  sudo apt install lsb-release
  sudo wget -O /etc/apt/trusted.gpg.d/wsl-transdebian.gpg https://arkane-systems.github.io/wsl-transdebian/apt/wsl-transdebian.gpg
  sudo chmod a+r /etc/apt/trusted.gpg.d/wsl-transdebian.gpg
  sudo touch /etc/apt/sources.list.d/wsl-transdebian.list
  echo "deb ${DEB_URL}" | sudo tee -a /etc/apt/sources.list.d/wsl-transdebian.list
  echo "deb-src ${DEB_URL}" | sudo tee -a /etc/apt/sources.list.d/wsl-transdebian.list
  sudo apt update -y && sudo apt upgrade -y
  sudo apt install -y bottle-imp
}

setupTransdeb
```

Once completed, you are ready to do the [NEXT SETUP STEP]()

[PREVIOUS STEP]()
