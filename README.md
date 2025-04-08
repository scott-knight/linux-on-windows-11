# Installing Linux on Windows 11

This content demonstrates how to setup Linux on Windows 11, to semi-replicate a Mac terminal setup for developing Ruby on Rails (or any other code you would want to use).

### DISCLAMER

I use this documentation for my own purposes, but you, the reader, are allowed to use this information for your own purposes as well. These instructions are given only for demonstration purposes, they are not a definitive guide. Follow (use) these instructions at your own risk. You bear the full responsibility of the outcome for following (using) them. By using these instructions, you agree to own and accept the full liablity of the outcome or result. Additionally, you are welcome to fork this project/information and/or submit pull requests (PRs), to add to the information. I retain the right to accept or reject any PR for any reason. You are welcome to write up your own doc and post it where you want if you don't agree with what's posted.

## Setup WSL, TERMNIAL, oh-my-posh, VSCode
Steps 1-3 are Windows setup instructions. Once you've completed these steps, you shouldn't have to do them again. Once Windows Terminal is setup, you should be good for the lifetime of your Windows install (pending needed updates if there are any).

1. [Install WSL2](install-wsl2.md)
2. [Install VSCode (if you want)](install-vscode.md)
3. [Install Windows Terminal](install-windows-terminal.md)
4. [Install oh-my-posh and required libraries](install-oh-my-posh-and-required-libraries.md)
5. [Configure Windows Terminal](configure-windows-terminal.md)

<br>

## Setup Ubuntu
   
1. [Install Ubuntu](install-ubuntu.md)
2. [Customize Ubuntu](customize-ubuntu.md)
3. [Setup SSH](https://github.com/scott-knight/linux-on-windows-11/blob/main/setup-ssh.md)
4. [Setup RBENV / NVM]()
5. [Custmize the Ubuntu Environment]()
6. [Unregister (from WSL) and Uninstall](unregister-and-uninstall.md)

<br/>

## Setup Development

- [Setup wslopen for (ruby) letter_opener gem](https://github.com/scott-knight/linux-on-windows-11/blob/main/setup-wslopen-for-letter-opener.md)

<br>

## Environment Files

- [.zsh_functions](https://github.com/scott-knight/linux-on-windows-11/blob/main/ZSH_FUNCTIONS.md)
- [.zsh_alist_list](https://github.com/scott-knight/linux-on-windows-11/blob/main/ZSH_ALIAS_LIST.md)
- [.zsh_projects](https://github.com/scott-knight/linux-on-windows-11/blob/main/ZSH_PROJECTS.md)
- [.zshrc](https://github.com/scott-knight/linux-on-windows-11/blob/main/ZSHRC.md)

<br>

## Available Distros

To see the available list of downloadable Linux distros via `Windows PowerShell`, run the following:

```sh
wsl.exe --list --online
```

<br/> 

There may be more options available within `Microsoft Store`.
