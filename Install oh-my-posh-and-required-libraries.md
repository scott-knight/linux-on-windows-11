# Install oh-my-posh and Required Libraries

This document contains information on how to install oh-my-posh and various libraies used for powershell and zsh themes which use Powerline Icons in the console.

[Oh-my-posh](https://ohmyposh.dev/) is similar to [oh-my-zsh](https://ohmyz.sh/). It works in Windows PowerShell which is used in the Windows Terminal. There's minimal setup needed for running (my personal preference) zsh instances. YOU DO NOT HAVE TO DO THIS STEP if you don't plan on using a powershell or zsh theme which requires Powerline icons. Sources: [Powerline Setup](https://docs.microsoft.com/en-us/windows/terminal/tutorials/powerline-setup), [Cascadia Code](https://github.com/microsoft/cascadia-code)


## Install Git For Windows

I'm not entirely sure I understand why this step is needed. However, it says to do this in the [Powerline Setup doc](https://docs.microsoft.com/en-us/windows/terminal/tutorials/powerline-setup). Download and install [Git for Windows](https://git-scm.com/downloads)

<br/>

## Install Powerline

The next step is to install `Posh-Git` and `Oh-My-Posh`. [Posh-Git](https://github.com/dahlbyk/posh-git) adds Git status information to your prompt as well as tab-completion for Git commands, parameters, remotes, and branch names. [Oh-My-Posh](https://github.com/JanDeDobbeleer/oh-my-posh) provides theme capabilities for your PowerShell prompt.

1. Open PowerShell as Administrator: Click the `Windows Start` icon. Scroll to and click the `Windows PowerShell` folder. Right-click on the `Windows PowerShell` icon. Click on `Run as Administrator`. 

![run as administrator](https://user-images.githubusercontent.com/516548/112907889-2ee1e000-90b4-11eb-9763-953226696c63.png)

<br/>

2. Run the following at the command prompt, Type `y` if asked an install question.

```sh
Install-Module posh-git -Scope CurrentUser
```

3. Run the following at the command prompt, Type `y` if asked an install question.

```sh
Install-Module oh-my-posh -Scope CurrentUser
```

<br/>

> You may need to install NuGet if you don't already have it. Your PowerShell command line will ask if you want to install NuGet if this is the case. Select [Y] Yes. You may also need to approve that you are installing modules from PSGallery, an 'untrusted repository'. Select [Y] Yes.

<br/>

4. Run the following at the command prompt, Type `y` if asked an install question.

```sh
Install-Module -Name PSReadLine -Scope CurrentUser -Force -SkipPublisherCheck
```

<br/>

## Customize your PowerShell prompt

1. Open your PowerShell profile with:

```sh
notepad $PROFILE
```

2. Once notpad opens, copy the following, save and close the file:

```txt
Import-Module posh-git
Import-Module oh-my-posh
Set-PoshPrompt -Theme paradox
```

3. In the PowerShell (as Admin), run the following:

```sh
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope LocalMachine
```

4. Then this:

```sh
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser
```

<br/>

## Install Cascadia Fonts

1. Go [here](https://github.com/microsoft/cascadia-code/releases) and download the latest release `.zip` file. 
2. Unzip the file, open the `otf > static` folder.
3. Select all the fonts, righ-click and select `Install for all users`

![otf install](https://user-images.githubusercontent.com/516548/112909921-f512d880-90b7-11eb-84b2-b49a5747f7ef.png)

<br/>

4. Back up and open the `ttf` folder. Select the all the fonts, right-click and select `Install for all users`

![ttf](https://user-images.githubusercontent.com/516548/112910091-44f19f80-90b8-11eb-94f9-8ca660c2835c.png)

<br/>

5. Click the `static` folder. Select the all the fonts, right-click and select `Install for all users`

![ttf-static](https://user-images.githubusercontent.com/516548/112910146-66eb2200-90b8-11eb-9e0e-385c9d42bf8c.png)

<br/>

The fonts should now be available for the settings in terminal. How to set the correct font is demonstrated [here](https://github.com/scott-knight/ubuntu-on-windows-setup/blob/main/configure-windows-terminal.md)

## Notes for oh-my-posh

You can actually use `oh-my-posh` with Ubuntu. However, I prefer `oh-my-zsh`. You can read more about using `oh-my-posh` [here](https://docs.microsoft.com/en-us/windows/terminal/tutorials/powerline-setup#set-up-powerline-in-wsl-ubuntu).

Now you're ready for the next step, [install Ubuntu](https://github.com/scott-knight/ubuntu-on-windows-setup/blob/main/install-ubuntu.md).
