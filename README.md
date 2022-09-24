# Installing Linux on Windows 11

This content demonstrates how to setup Linux (Ubuntu) on Windows 11, to semi-replicate a Mac terminal setup for developing Ruby on Rails (or any other code you would want to use). I tried installing Debian, but it's thinner than Ubuntu (which comes with everything). You can literally following 90% of the install steps and use debian, but you will not be able to install linuxs tools using wajig, you'll have to use `apt` or `apt-get`.

### DISCLAMER

I use this for my own purposes, but you, the reader, can use this information for your own purposes as well. These instructions are given only for demonstration purposes, they are not a definitive guide. Follow the instructions at your own risk. You bear the full responsibility of the outcome for following these instructions. By using these instructions, you agree to own and accept the full liablity of the outcome or result. Additionally, you are welcome to fork this project/information and/or submit pull requests (PR), to add to the information. I retain the right to accept or reject any PR for any reason. You are welcome to write up your own doc and post it where you want if you don't agree with what's posted.

### INFO
Below is the step-by-step guide on how to install all the bits needed to get an Debian instance installed, set up, and customized to a state where you can use a console session to write and push code. The details of each of the steps (and there precise order of install) are a little hazy as I went through this process over the course of a few days. Additionally, I didn't anticipate creating a step-by-step guide, but I found myself creating notes as a safety measure to recover from destroying my Windows 11 installation (which never happened) and/or my Debian installation (which I did happen several times). That being said, this is the (fuzzy) list of things that need to be done in order to get things working:

1. [Install WSL2](https://github.com/scott-knight/linux-on-windows-11/blob/main/install-wsl2.md)
2. [Install VSCode (if you want)](https://github.com/scott-knight/linux-on-windows-11/blob/main/install-vscode.md)
3. [Install Windows Terminal](https://github.com/scott-knight/linux-on-windows-11/blob/main/install-windows-terminal.md)
4. [Install oh-my-posh and required libraries](https://github.com/scott-knight/linux-on-windows-11/blob/main/Install%20oh-my-posh-and-required-libraries.md)
5. [Install Linux](https://github.com/scott-knight/linux-on-windows-11/blob/main/install-linux.md)
6. [Configure Windows Terminal](https://github.com/scott-knight/linux-on-windows-11/blob/main/configure-windows-terminal.md)
7. [Customize Debian](https://github.com/scott-knight/linux-on-windows-11/blob/main/customize-linux.md)

### IMPORTANT NOTES

Steps 1-4 are Windows setup instructions. Once you have completed steps 1-4, you shouldn't have to do them again. You will be able to create and destroy Linux  installs without having to repeat steps 1-4. 

Once Windows Terminal is setup, you should be good for the lifetime of your Windows install (pending needed updates if there are any).

You will need to repeat steps 5-7 if you decide to delete your current install of Linux and reinstall it.
