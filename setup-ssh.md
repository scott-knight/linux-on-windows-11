[Home](README.md) | [Previous Step](customize-ubuntu.md) | [Next Step](https://github.com/scott-knight/linux-on-windows-11/blob/main/setup-rbenv-nvm.md)

This document describes how to customize your installation of ssh keys and the ssh-agent.

<br>

## Info: WSL and Creating SSH Keys

**IMPORTANT:** When you create a new ssh key, you will be prompted to enter a passphrase. You can create the key with or without a passphrase. If you create the key with a passphrase: Adding a passphrase adds an additional layer of security to your SSH key. However, when you open a new WSL console session, you will be asked for your ssh key passphrase every time a new instance of the ssh-agent starts. This setup attempts minimize that annoyance. If you don't use a passphrase when you create the ssh key, you won't have the issue of entering it when you open an Ubuntu console and the ssh-agent starts. 

<br/>

## Create SSH Files

1. Open Windows Terminal, running Ubutnu, and run the following:

```sh
mkdir $HOME/.ssh && touch $HOME/.ssh/config
```

<br>

2. You can create new SSH keys by following the steps [outlined here](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#generating-a-new-ssh-key). Or, you can do the following:

    * In the terminal, paste the following (replace the email with your github account email):

    ```zsh
    ssh-keygen -t ed25519 -C "your_email@example.com"
    ```

    * When you see this prompt: `Enter file in which to save the key (/home/username/.ssh/id_ed25519):`, hit `enter` to accept the default.
 
    * When you see this prompt: `Enter passphrase (empty for no passphrase):` hit `enter` if you don't want use a passphrase, or type a passphrase and hit `enter`

    * When you see this prompt: `Enter same passphrase again:` hit `enter` if you didn't use a passphrase, or re-type the passphrase and hit `enter`.

<br>

### Update .ssh/config

In the console, run the following:

```zsh
nano $HOME/.ssh/config
```

Copy the following in the new `config` file, then save and close the file:

```sh
Host *
  AddKeysToAgent yes
  IdentityFile ~/.ssh/id_ed25519
```

<br>


### Update Github

You will need to [add the new public key to your github account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account#adding-a-new-ssh-key-to-your-account). You can copy the key by running the following:

```sh
clip.exe < ${HOME}/.ssh/id_ed25519.pub
```

<br/>

## Alternate SSH KEY Method

Rather than create a new set of keys, you can copy a previously created set of SSH keys from another Linux instance (or other location) into `$HOME/.ssh`. Once the files are there, update the permissions on the copy/pasted keys:

 ```sh
chmod 600 $HOME/.ssh/id_ed25519 && chmod 600 $HOME/.ssh/id_ed25519.pub
```

If you use this method, you won't need to add the key to github as it should already be there. 

<br/>

## INSTALL KEYCHAIN

Run the following:

```sh
wajig install -y keychain
```

<br/>

Once installed, start `keychain` to create the `$HOME\.keychain` directory and start the `ssh-agent`:

```sh
keychain
```

<br/>

Using VIM or Nano, open the .zshrc file:

```zsh
nano .zshrc
```

<br/>

Paste the following code snippet at the bottom of `.zshrc`.

```
# Keychain
eval $(keychain --eval $HOME/.ssh/id_ed25519)
```

Save and close the file.

<br/>

## Copy .gitconfig

Run the following:

```zsh
nano .gitconfig
```

<br>

Paste the following, replacing `[your email]` with your github email, and `[your-username]` with your github username:


```sh
[user]
  email = [your email]
  name = [your-username]

[pull]
  rebase = false

[alias]
  co = checkout
  ci = commit
  st = status
  br = branch

[pager]
  branch = false
```

Save and close the file. Close the terminal, then re-open the terminal instance.

<br>

[Home](README.md) | [Previous Step](customize-ubuntu.md) | [Next Step](https://github.com/scott-knight/linux-on-windows-11/blob/main/setup-rbenv-nvm.md)
