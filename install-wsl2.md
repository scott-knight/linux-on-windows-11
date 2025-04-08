[Home](README.md) | [Next Step](install-vscode.md)

## Install WSL

This process is the same for Windows versions 10 and 11. It's possible this has already be done for you. However, if it hasn't, do the following:

<br>

### STEP 1

Open PowerShell as Administrator (Start menu > PowerShell > right-click > Run as Administrator) and enter this command:

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

<br>

### STEP 2

Before installing WSL 2, you must enable the Virtual Machine Platform optional feature.

Open PowerShell as Administrator and run:

```powershell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

<br>

### STEP 3

You'll need to set WSL 2 as your default version. To do this, open PowerShell and run this command to set WSL 2 as the default version:

```powershell
wsl --set-default-version 2
```

<br>

## WSL Commands

For more information about how to use WSL, please visit the [WSL homepage](https://learn.microsoft.com/en-us/windows/wsl/basic-commands)

<br>

[Home](README.md) | [Next Step](install-vscode.md)
