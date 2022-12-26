# Customize Ubuntu

THIS DOC IS NOT UP TO DATE - IT'S STILL HERE, MAINLY AS A PLACEHOLDER, JUST IN CASE I DECIDED TO UPDATE IT LATER. [PREFER THE DEBIAN INSTALL](https://github.com/scott-knight/linux-on-windows-11/blob/main/install-debian.md) OVER UBUNTU

This is the step-by-step guide for customizing Ubuntu to use all the settings, aliases, functions, etc., that I use every day. You don't have to do this step either. Linux will run without this customization. Also, if you go through the step-by-step guide and don't like the setup, you can simply [uninstall the Linux instance](https://learn.microsoft.com/en-us/windows/wsl/basic-commands#unregister-or-uninstall-a-linux-distribution) and [reinstall it](https://github.com/scott-knight/linux-on-windows-11/blob/main/install-linux.md). It will be a completely clean install of Linux, no settings will carry over.

<br/>

## Update Linux (Ubuntu)

1. Close all open `Windows Terminal` instances. Open a new `Windows Termial` instance and select Ubuntu (if you didn't set it as the default).
2. You should update `Ubuntu` with the latest security patches and other default updates by running the following:

```sh
sudo bash -c 'for i in update {,dist-}upgrade auto{remove,clean}; do apt-get $i -y; done'
```

Or run

```sh
sudo apt-get update && sudo apt-get upgrade -y && sudo apt-get dist-upgrade -y && sudo apt-get autoremove && sudo apt-get autoclean
```

<br/>

## Install Wajig

[`Wajig`](https://wiki.debian.org/Wajig) is a simplifed and more unified command line interface for package management. It adds a more intuitive quality to the user interface.

To install wajig, run the following:

```sh
sudo apt install wajig aptitude -y
```

Once finished, run the following:

```sh
wajig update && wajig upgrade -y && wajig distupgrade -y && wajig autoremove && wajig autoclean
```

You should use this command to update Ubuntu from now on.

<br/>

To install Linux libraries with `wajig`, run the following:

```sh
wajig install -y autoconf bison build-essential checkinstall clang git curl gcc libssl-dev libpq-dev libyaml-dev libreadline-dev libncurses5-dev libffi-dev libgdbm6 libgdbm-dev libdb-dev make pkg-config vim wget zlib1g-dev zsh
```

<br/>

To install the libraries using `apt-get`, run the following:

```sh
sudo apt-get install -y autoconf bison build-essential checkinstall clang curl git gcc libssl-dev libpq-dev libyaml-dev libreadline-dev libncurses5-dev libffi-dev libgdbm6 libgdbm-dev libdb-dev make pkg-config vim zlib1g-dev zsh
```

<br/>

To install VIPS libraries, run the following:

```sh
wajig install -y libglib2.0-0 libglib2.0-dev libpoppler-glib8 libheif-dev libvips-dev libvips
```

Or

```sh
sudo apt-get install -y libglib2.0-0 libglib2.0-dev libpoppler-glib8 libheif-dev libvips-dev libvips
```
