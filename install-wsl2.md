# Install Windows 11 WSL 2

This document explains how to install Windows 11 WSL 2. Information for this guide was taken from [here](https://www.tecklyfe.com/how-to-enable-wsl2-on-windows-10/), and [here](https://www.windowscentral.com/how-install-wsl2-windows-10).

## Installation Steps

1. Enter your computers BIOS and make sure (Hyper-V) virtualization is enabled (if it isn't already). Save the settings and reboot.
2. Open the PowerShell as Administrator: Click the Windows Start icon. Scroll to and click the `Windows PowerShell` folder. Right-click on the `Windows PowerShell` icon. Click on `Run as Administrator`. 

![run as administrator](https://user-images.githubusercontent.com/516548/112900455-27690980-90a9-11eb-9d0f-0d9f898070a1.png)

3. Run, one of the two, following commands in the console (either command does the same thing):

```sh
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```
OR

```sh
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

4. Reboot
5. After the reboot, open PowerShell as Administrator (repeat step 2)
6. Run, one of the two, following commands in the console (either command does the same thing):

```sh
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
OR 

```sh
Enable-WindowsOptionalFeature -Online -FeatureName VirtualMachinePlatform
```

7. Reboot
8. After the reboot, open PowerShell as Administrator (repeat step 2) 
9. Run the following commands in the console::

```sh
wsl --set-default-version 2
```

WSL2 will be installed and ready to go. Now you can move on to the next step, [install VSCode](https://github.com/scott-knight/debian-on-windows-11/blob/main/install-vscode.md).

