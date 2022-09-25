# Installing Linux (Ubuntu)

This document demonstrates how to install the current supported release of Linux (Ubuntu) on Windows 11. This document assumes WSL2 has been installed and is ready to use. A complete list of [WSL commands can be found here](https://learn.microsoft.com/en-us/windows/wsl/basic-commands)

<br/>

## Update WSL

To ensure you have the latest version of WSL, open the Windows PowerShell as an Administrator

![run-as-administrator](https://user-images.githubusercontent.com/516548/192077877-6748108f-fdd2-4c83-b0ba-3ac31224c9bf.png)

<br/>

Once open, run the following:

```sh
wsl --update
```

<br/>

## Install Ubuntu

1. In the Windows Store, and search for Ubuntu:

![ubuntu22](https://user-images.githubusercontent.com/516548/192082167-6b8ee768-0684-4851-af15-2d3f13c99c1b.png)

2. We want to install the latest supproted version (in this example we see that 22.04 is the lastest. Click that tile and click `Install`.
3. Once downloaded, click `Open`

<br/>

NOTE: If you have trouble in the console when it opens (likely from reinstalling it several times like I did), run the following:

```sh
wsl --unregister Ubuntu-22.04
```

Then click `Open` again. It should open the dialog as expected.

<br/>

4. You will be prompted to select a default language, select `English`, then click continue.

![language](https://user-images.githubusercontent.com/516548/192082523-3744a840-f70d-411a-aae7-f550754cce02.png)

5. You will be prompted to create a UNIX user name. Type what you want
6. You will be prompted to create a new password. Type something that you will remember. This will be your password needed to do Admin things in the console.

![user-pass](https://user-images.githubusercontent.com/516548/192082563-30b260b3-4850-4a59-9f82-9274a4f88d1b.png)

7. In the advanced setup, simply accept the defaults and click `Setup`

![setup](https://user-images.githubusercontent.com/516548/192082591-11ddf920-81a6-4126-8735-75eaf9190088.png)

8. Once it completes, click `Finish`
9. Once it finishes, it will open a new terminal session. Click the down-arrow in the tab bar and click `Settings`.

![new-settings](https://user-images.githubusercontent.com/516548/192082679-8cc094a2-e920-4b00-943e-91a3e75ccb4b.png)

10. In the `Startup` section, in the `Default profile`, select `Ubutntu`, then click `Save`.

![select-ubuntu](https://user-images.githubusercontent.com/516548/192082727-17a7da64-b6f4-43a6-8385-2a0d3c9358a1.png)

11. On the left-nav, scroll down and click `Ubuntu`. On the right, select `Apperance`. Change the color scheme to `Campbell` and the font face to `Cascadia Code PL`. Then click `Save` and close the settings tab.

![ubuntu-apperance](https://user-images.githubusercontent.com/516548/192082890-1540c8e6-0ac9-4d07-a328-40ba8b2baa2b.png)


After you save the settings, when you click on the `Windows Terminal` icon in the `Start Menu`, it will automatically open the `Ubuntu` shell.

<br/>

## Installing Debian

If you would like to use Debain instead of Ubuntu, the setup is easy. However, the majority of the remaining documentation assumes you will install Ubuntu (not Debian). If you choose to use Debian, it's noted where there are installation differences. Where it mentions setup steps for Ubuntu specifically, you can assume you would replace Ubuntu with Debian. However, with Debian, when setting up Rails, using Brew to install Postresql, I was unable to resolve an issue with the pg gem install (it says there are missing libraries which I was unable to resolve -- this doesn't happen with Ubuntu).

To install Debian, in the `Windows PowerShell` (running as administrator), run the following:

```sh
wsl --install -d Debian
```

Once downloaded and installed, it will open a new terminal instance. You will be promted to create a username and password. Once entered, the setup will complete.

![debian-setup](https://user-images.githubusercontent.com/516548/192112953-e95b93a0-5c68-407a-8ae3-ea0b15ad8fe4.png)

<br/>

## Connecting VSCode to your Linux instance

1. In the Linux console, type the following:

```sh
code .
```

This will attempt to open VSCode and connect VSCode to your Linux instance. I'm a little hazy on what happens after it connects, but you should be able to see the files of your home directory.

<br/>

2. Close VSCode and the terminal 

Now you are ready to [Customize Linux](https://github.com/scott-knight/linux-on-windows-11/blob/main/customize-linux.md)
