[Home](https://github.com/scott-knight/linux-on-windows-11) | [Previous Step](configure-windows-terminal.md) | [Next Step](customize-ubuntu.md) |  [WSL Basic Commands](https://learn.microsoft.com/en-us/windows/wsl/basic-commands)

<br/>

# Install Ubuntu

This document describes how to install the Ubuntu on Windows 11. This document assumes WSL2 has been installed and is ready to use.

*I find that Windows and WSL installs change often. If things don't work as described in this doc, they changed something. By the time you read this doc, it's possible that some of these steps may no longer be required. It's also possible that new steps will be required to get everything working. Either way, I do what I can to keep this document updated, but be prepared to troubleshoot issues as they arise.*

<br/>

## Removing Ubuntu

*Important:* Everything in the Ubuntu instance will be lost when you uninstall it. If you have content (pictures, files, ssh keys, etc) that you want to save, you will need to save that content before removing the instance. If you need to remove an installation of ubuntu, it's simple. In the `Windows PowerShell` (running as administrator), run the following:

```bash
wsl.exe --shutdown
wsl.exe --unregister Ubuntu-Preview
```

[Complete removal instructions](https://github.com/scott-knight/linux-on-windows-11/blob/main/unregister-and-uninstall.md)

<br/>

## Update WSL

To ensure you have the latest version of WSL, open the Windows PowerShell as an Administrator.

<br/>

![run-as-administrator](https://user-images.githubusercontent.com/516548/192077877-6748108f-fdd2-4c83-b0ba-3ac31224c9bf.png)

<br/>

Once open, run the following:

```sh
wsl --update
```

A complete list of [WSL commands can be found here](https://learn.microsoft.com/en-us/windows/wsl/basic-commands)

<br/><br/>

## Installing Ubuntu

There are 2 ways to install Ubuntu. 1) Download it from the PowerShell command-line, 2) Download it from Microsoft Store.

<br/>

### Download with PowerShell (LTS version)

*Note:* This will download and install the latest LTS version of Ubuntu. If you want to install the `Preview` version, you have to use the `Microsoft Store` download.

To install Ubutnu, in the `Windows PowerShell` (running as administrator), run the following:

```sh
wsl.exe --install -d Ubuntu
```

<br/>

### Download with Windows Store

1. In the Windows Store, and search for Ubuntu:

<br/>

![Screenshot 2022-12-26 083036](https://user-images.githubusercontent.com/516548/209558996-6738c20f-d499-4721-a097-de07fa4d32e5.png)

<br/>

2. Pick a version to install and click `Install` (this doc will assume you installed version `preview`)
3. Once downloaded, click `Open`. It will start the setup dialog.
4. You will be prompted to select a default language, select `English`, then click continue.

<br/>

![language](https://user-images.githubusercontent.com/516548/209565740-a900ae4a-a9a8-410f-acf4-ca1bc2fa9671.png)

<br/>

5. You will be prompted to create a UNIX user account and password. The password will be needed to run `sudo` in the console.

<br/>

![user-pass](https://user-images.githubusercontent.com/516548/209565908-853e9a19-9bf3-4534-b27e-3d366565e0c6.png)

<br/>

6. In the advanced setup, (use the default settings) click `Setup`

<br/>

![setup](https://user-images.githubusercontent.com/516548/209565963-356d74e8-d865-4b6d-8bf7-99330428c91b.png)

<br/>

7. Once it finishes, it will open a new terminal session. Click the down-arrow in the tab bar and click `Settings`.

<br/>

![new-settings](https://user-images.githubusercontent.com/516548/192082679-8cc094a2-e920-4b00-943e-91a3e75ccb4b.png)

<br/>

8. In the `Profile` section, click `+ Add a new profile`.

<br/>

![add-new-profile](https://user-images.githubusercontent.com/516548/209566418-b7493c4f-efe2-4b06-8d33-80b7389e59e7.png)

<br/>

9. Select Ubuntu, then Dupilcate

<br/>

![preview-dup](https://user-images.githubusercontent.com/516548/209566532-dc959837-453d-4f54-845f-bc24c4057a5c.png)

<br/>

10. Select the profile you created an rename it to the version of Ubuntu that is being used, then click `save`

<br/>

![rename](https://user-images.githubusercontent.com/516548/209566740-3bc99dbe-8336-4476-909f-6fd6558f0102.png)

<br/>

11. Once saved, scroll down to the `Additional settings` section and click `Apperance`. Change the color scheme to `Campbell` and the font face to `CaskaydiaCove Nerd Font Mono` (or any other Nerd font or Powerline font). Then click `Save` and close the settings tab.

<br/><br/>

## Update Ubuntu

1. To update Ubuntu, run the following:

```sh
sudo bash -c 'for i in update {,dist-}upgrade auto{remove,clean}; do apt-get $i -y; done'
```

<br/><br/>

## Install Wajig

[`Wajig`](https://wiki.debian.org/Wajig) is a simplifed and more unified command line interface for package management. It adds a more intuitive quality to the user interface.

To install wajig, run the following:

```sh
sudo apt install wajig aptitude -y && wajig update && wajig upgrade -y && wajig distupgrade -y && wajig autoremove && wajig autoclean
```

You should use `wajig command to update Ubuntu from now on.

<br/><br/>

## Install System Utilities

1. Install system utilities by running the following:

```sh
wajig install -y apt-transport-https autoconf bison build-essential checkinstall clang curl gcc git gpg gnupg2 libssl-dev libpq-dev libyaml-dev libreadline-dev libncurses-dev libffi-dev libgdbm6 libgdbm-dev libdb-dev lsb-release libxml2-dev libxslt-dev make patch pkg-config wget vim zlib1g-dev liblzma-dev zsh
```

<br/>

2. Install VIPS libraries (used by Rails to manage images):

```sh
wajig install -y libglib2.0-0 libglib2.0-dev libpoppler-glib8 libheif-dev libvips-dev libvips
```

<br/>

3. Once everything is installed, run the following:

```sh
wajig update && wajig upgrade -y && wajig autoremove && wajig autoclean
```

<br/>

Now you're ready to [Customize Ubuntu](customize-ubuntu.md)

<br/>

[Home](https://github.com/scott-knight/linux-on-windows-11) | [Previous Step](configure-windows-terminal.md)
