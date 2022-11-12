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
  DEB_URL=deb https://arkane-systems.github.io/wsl-transdebian/apt/ $(lsb_release -cs) main

  sudo apt install lsb-release
  sudo wget -O /etc/apt/trusted.gpg.d/wsl-transdebian.gpg https://arkane-systems.github.io/wsl-transdebian/apt/wsl-transdebian.gpg
  sudo chmod a+r /etc/apt/trusted.gpg.d/wsl-transdebian.gpg
  sudo touch /etc/apt/sources.list.d/wsl-transdebian.list
  echo "deb ${DEB_URL}" | sudo tee -a /etc/apt/sources.list.d/wsl-transdebian.list
  echo "deb-src ${DEB_URL}" | sudo tee -a /etc/apt/sources.list.d/wsl-transdebian.list
  sudo apt update && sudo apt upgrade
}

setupTransdeb
```




================================================================================================================================================================

There are a couple of things to know before you proceed. 
  * The installed Debian version (if this is sub-versioned, simply use the main version number, ie `11.5` will be `11`).
  * The current WSL Debian [Genie version](https://github.com/arkane-systems/genie/tree/master/debian), 
  * The supported [.Net runtime](https://learn.microsoft.com/en-us/dotnet/core/install/linux-debian#supported-distributions) for the version of Debian that you're running. This will need to be the version and sub-version, ie `6.0`

<br/>

1. To get your installed Debian version, in the Debian console, run the following and note the version: 

```sh
cat /etc/debian_version
```

<br/>

2. To get the latest Genie version, [open this url](https://github.com/arkane-systems/genie/tree/master/debian). The version should be noted somewhere on the page. Note the version.

<br/>

3. To get the current supported .Net runtime for your version of Debian, [open this url](https://learn.microsoft.com/en-us/dotnet/core/install/linux-debian#supported-distributions). The version should be noted somewhere on the page. Note the version. In the example below, we can see that Debian 11 uses .Net runtime v6.

![net-runtimes](https://user-images.githubusercontent.com/516548/201444555-03b2f77a-ff2d-450c-9f65-9350913dfb6e.png)

<br/>

4. In the Debian console, navigate to the `tmp` directory

```sh
cd /tmp
```

<br/>

5. Create a file named `setup-systemd.sh` by running the following:

```sh
nano setup-systemd.sh
```

<br/> 

6. Here is where you will need to have the noted versions for `DEBIAN_VERSION`, `GENIE_VERSION`, and `NET_VERSION`. Copy the contents of code block below into your nano editor instance and update the version numbers:

```sh
#! /usr/bin/env bash
set -e

# Change these for your version of Debian
DEBIAN_VERSION="11"
GENIE_VERSION="2.5"
NET_VERSION="6.0"

GENIE_FILE="systemd-genie_${GENIE_VERSION}_amd64"
GENIE_FILE_PATH="/tmp/${GENIE_FILE}.deb"
GENIE_DIR_PATH="/tmp/${GENIE_FILE}"

function installDependencies() {
  sudo apt-get update && sudo apt-get install apt-transport-https

  wget --content-disposition \
    "https://packages.microsoft.com/config/debian/${DEBIAN_VERSION}/packages-microsoft-prod.deb"

  sudo dpkg -i packages-microsoft-prod.deb
  rm packages-microsoft-prod.deb

  sudo apt-get update
  sudo apt-get install -y \
    daemonize \
    dotnet-runtime-${NET_VERSION} \
    systemd-container

  sudo rm -f /usr/sbin/daemonize
  sudo ln -s /usr/bin/daemonize /usr/sbin/daemonize
}

function downloadDebPackage() {
  rm -f "${GENIE_FILE_PATH}"

  pushd /tmp

  wget --content-disposition \
    "https://github.com/arkane-systems/genie/releases/download/v${GENIE_VERSION}/systemd-genie_${GENIE_VERSION}_amd64.deb"

  popd
}

function installDebPackage() {
  # install repackaged systemd-genie
  sudo dpkg -i "${GENIE_FILE_PATH}"

  rm -rf "${GENIE_FILE_PATH}"
}

function main() {
  installDependencies
  downloadDebPackage
  installDebPackage
}

main
```

Use `ctrl+x` to close the file. Hit `y` to write the file. Hit `enter` to save the file.

<br/>

7. Make the file executable:

```sh
sudo chmod +x setup-systemd.sh
```

<br/>

8. Run the script:

```sh
./setup-systemd.sh
```

<br/>

9. Close the Debian teminal. 

<br/>

10. Open a `Windows PowerShell` terminal (running as administrator), and run the following:

```sh
wsl.exe --shutdown
```

<br/>

11. Re-ppen the Debian console and run the following:

```sh
sudo systemctl status time-sync.target
```

It should return something like this:

```sh
$ sudo systemctl status time-sync.target
● time-sync.target - System Time Synchronized
     Loaded: loaded (/lib/systemd/system/time-sync.target; static)
     Active: inactive (dead)
       Docs: man:systemd.special(7)
```

<br/>

Once completed, you are ready to do the [NEXT SETUP STEP]()

[PREVIOUS STEP]()
