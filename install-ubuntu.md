[Home](README.md) | [Previous Step](configure-windows-terminal.md) | [Next Step](customize-ubuntu.md) |  [WSL Basic Commands](https://learn.microsoft.com/en-us/windows/wsl/basic-commands)

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

1. Open the Windows Store, and search for `Ubuntu`:

2. Select the current LTS version listed



![select-ubuntu](https://github.com/user-attachments/assets/ad860e13-522f-4d9a-ab2b-4a22663f4bf2)

<br>

3. Once selected, click `Get`

![get-ubuntu](https://github.com/user-attachments/assets/7e6fe860-77de-4ea0-b6d6-b829c263b99a)

<br>

4. Once downloaded, click `Open`

![open-ubuntu](https://github.com/user-attachments/assets/0c4d0224-2839-4cf2-aadf-c4b6e02822c5)

<br>

5. Once prompted, enter a username and password:

![enter-username](https://github.com/user-attachments/assets/672969f4-2f8c-484d-b605-d8dab02f6061)

<br>

6. After you enter your password a second time, your install will be complete:

![ubuntu-install-complete](https://github.com/user-attachments/assets/95769a12-ec11-4f3b-bdd9-4cad99e466c8)

<br>

7. Close and re-open the Windows Terminal. Next to the terminal tab, you'll notice a down-arrow caret `V`. Click the down-arrow to open the selection list:

![terminal-select](https://github.com/user-attachments/assets/7e4ceb50-4706-4b51-ba04-e3a78e38747f)

<br>

Going forward, you can select the desired server instance from this list. In the next step, it demonstrates how to set the new Ubuntu instance as the default terminal instance. 

<br>

Now you're ready to [Customize Ubuntu](customize-ubuntu.md)

<br/>

[Home](README.md) | [Previous Step](configure-windows-terminal.md)
