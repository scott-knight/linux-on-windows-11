# Installing Linux on Windows 11

This content demonstrates how to setup Linux on Windows 11, to semi-replicate a Mac terminal setup for developing Ruby on Rails (or any other code you would want to use).

### DISCLAMER

I use this documentation for my own purposes, but you, the reader, are allowed to use this information for your own purposes as well. These instructions are given only for demonstration purposes, they are not a definitive guide. Follow (use) these instructions at your own risk. You bear the full responsibility of the outcome for following (using) them. By using these instructions, you agree to own and accept the full liablity of the outcome or result. Additionally, you are welcome to fork this project/information and/or submit pull requests (PRs), to add to the information. I retain the right to accept or reject any PR for any reason. You are welcome to write up your own doc and post it where you want if you don't agree with what's posted.

### Setup WSL, TERMNIAL, oh-my-posh, VSCode
Steps 1-3 are Windows setup instructions. Once you've completed these steps, you shouldn't have to do them again. Once Windows Terminal is setup, you should be good for the lifetime of your Windows install (pending needed updates if there are any).

1. [Install WSL2](install-wsl2.md)
2. [Install VSCode (if you want)](install-vscode.md)
3. [Install Windows Terminal](install-windows-terminal.md)
4. [Install oh-my-posh and required libraries](install-oh-my-posh-and-required-libraries.md)
5. [Configure Windows Terminal](configure-windows-terminal.md)


   
7. [Install Ubuntu](install-ubuntu.md)
8. [Customize Ubuntu](customize-ubuntu.md)
9. [Unregister (from WSL) and Uninstall](unregister-and-uninstall.md)

### IMPORTANT NOTES

Steps 1-5 are Windows setup instructions. Once you have completed steps 1-5, you shouldn't have to do them again. You will be able to create and destroy Linux  installs without having to repeat steps 1-5.

Once Windows Terminal is setup, you should be good for the lifetime of your Windows install (pending needed updates if there are any).

You will need to repeat steps 6-7 if you decide to delete your current install of Linux and reinstall it.

<br/>

## Ubuntu vs Debian

* The current Ubuntu version (23.x) seems to have a lot less bloat (than previous versions) and runs really well. It's my prefered Linux distro within WSL.
* Debian doesn't have any bloat. For a Debian install, this means you'd have install, configure, and maintain everything. If something goes wrong, it takes a while to figure out what failed and how to fix it. Not the best way to go if you don't know what you're doing.

 <br/>

 ### Debian Notes

I have created a set of Debian installation notes. I'm not sure how long I will maintain them. It takes more effort to maintain a Debian instance (which I'm not a fan of). If you want to use Debian, [here you go](https://github.com/scott-knight/debian-on-windows-11):

STEPS:

6. [Install Debian](https://github.com/scott-knight/debian-on-windows-11/blob/main/install-debian.md)
7. [Customize Debian](https://github.com/scott-knight/debian-on-windows-11/blob/main/customize-debian.md)

<br/>

## Available Distros

To see the available list of downloadable Linux distros via `Windows PowerShell`, run the following:

```sh
wsl.exe --list --online
```

<br/> 

There may be more options available within `Microsoft Store`.
