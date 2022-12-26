[Home](https://github.com/scott-knight/linux-on-windows-11) | [Previous Step](https://github.com/scott-knight/linux-on-windows-11/blob/main/install-ubuntu.md) | [Uninstall](https://github.com/scott-knight/linux-on-windows-11/blob/main/unregister-and-uninstall.md)

<br/>

# Customize Ubuntu

This is the step-by-step guide for customizing Ubuntu.

<br/>

## Connecting VSCode

1. In the Linux console, type the following:

```sh
code .
```

This will load VSCode and connect to your Debian instance, and you will see the files of your home directory.

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

[Home](https://github.com/scott-knight/linux-on-windows-11) | [Previous Step](https://github.com/scott-knight/linux-on-windows-11/blob/main/install-ubuntu.md) | [Uninstall](https://github.com/scott-knight/linux-on-windows-11/blob/main/unregister-and-uninstall.md)
