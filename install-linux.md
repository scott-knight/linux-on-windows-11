# Installing Linux (Ubuntu)

This document demonstrates how to install the current supported release of Linux (Ubuntu) on Windows 11. This document assumes WSL2 has been installed and is ready to bue used. A complete list of [WSL commands can be found here](https://learn.microsoft.com/en-us/windows/wsl/basic-commands)

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


2. You will be prompted to create a UNIX user name. Type what you want
4. You will be prompted to create a new password. Type something that you will remember. This will be your password needed to do Admin things in the console.

![new-debian](https://user-images.githubusercontent.com/516548/192078174-125877f1-ab0a-4a6f-8011-a9a3473c134b.png)

<br/>

## Connecting VSCode to your Debian instance

1. In the Debian console, type the following:

```sh
code .
```

This will attempt to open VSCode and connect VSCode to your Debian instance. I'm a little hazy on what happens after it connects, but you should be able to see the files of your home directory.

<br/>

2. Close VSCode and the terminal 

The next step, [update the settings of Windows Terminal](https://github.com/scott-knight/debian-on-windows-11/blob/main/configure-windows-terminal.md).
