[Home](README.md) | [Previous Step](https://github.com/scott-knight/linux-on-windows-11/blob/main/setup-rbenv-nvm.md)

<br>

This document describes how to customize the Ubuntu environment. It uses theming and customization that makes managing and using the terminal much simpler use and much cleaner to look at. 

<br>

## Install [Spaceship Prompt](https://github.com/spaceship-prompt/spaceship-prompt)

The [spaceship-prompt](https://github.com/spaceship-prompt/spaceship-prompt) is a clean terminal theme. 

Run the following:

```zsh
brew install spaceship
```

<br>

Then run

```zsh
echo "source $(brew --prefix)/opt/spaceship/spaceship.zsh" >>! ~/.zshrc
```

<br>

After you install [spaceship-prompt](https://github.com/spaceship-prompt/spaceship-prompt), there are 2 plugins used for [syntax highlighting](https://github.com/zsh-users/zsh-syntax-highlighting/blob/master/INSTALL.md#oh-my-zsh) and [autosuggestions](https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md#oh-my-zsh). To install them, run the following:

```zsh
  cd $HOME && mkdir $HOME/.zsh &&
  git clone https://github.com/zsh-users/zsh-autosuggestions $HOME/.zsh/zsh-autosuggestions &&
  echo "" &&
  git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $HOME/.zsh/zsh-syntax-highlighting &&
  cd $HOME
```

<br>

## Copy [.zshrc](https://github.com/RK-BCR/BCR-Web/wiki/.zshrc)

Run the following to clear out .zshrc:

```sh
truncate -s 0 .zshrc
```

<br/>

Open `.zshrc`:

```zsh
nano .zshrc
```

<br/>

Copy the [content of .zshrc](https://github.com/scott-knight/linux-on-windows-11/blob/main/ZSHRC.md) and save.

<br>

Close the terminal and re-open the terminal. You should see something similar to this:

![final terminal](https://github.com/user-attachments/assets/f9d4cde8-f021-4b94-badd-c83645bdcd82)

<br>

## Copy [.zsh_alias_list](https://github.com/scott-knight/linux-on-windows-11/blob/main/ZSH_ALIAS_LIST.md)

Open `.zsh_alias_list`

<br/>

```zsh
nano .zsh_alias_list
```

<br/>

Copy the [content of .zsh_alias_list](https://github.com/scott-knight/linux-on-windows-11/blob/main/ZSH_ALIAS_LIST.md) and save.

Reload the terminal instance.

From now on, when you want to reload the shell, simply type `reload`

<br/>

## Copy [.zsh_functions](https://github.com/scott-knight/linux-on-windows-11/blob/main/ZSH_FUNCTIONS.md)

Open `.zsh_functions`

<br/>

```zsh
nano .zsh_functions
```

<br/>

Copy the [contents of .zsh_functions](https://github.com/scott-knight/linux-on-windows-11/blob/main/ZSH_FUNCTIONS.md) and save.

Type `reload` to reload the shell.

<br/>

## Copy [.zsh_projects](https://github.com/scott-knight/linux-on-windows-11/blob/main/ZSH_PROJECTS.md)

Open `.zsh_riskonnect`

<br/>

```zsh
nano .zsh_riskonnect
```

<br/>

Copy the [contents of .zsh_projects](https://github.com/scott-knight/linux-on-windows-11/blob/main/ZSH_PROJECTS.md) and save.

Type `reload` to reload the shell.

<br/>

## Set .gemrc

Open `.gemrc`

```zsh
nano .gemrc
```

<br>

Copy the following, save and close the file

```ruby
---
:backtrace: false
:bulk_threshold: 1000
:sources:
- https://rubygems.org/
:update_sources: true
:verbose: true
install: --no-rdoc --no-ri --no-document
update: --no-rdoc --no-ri --no-document
```

<br>

close all terminal instances and re-open when needed.

<br>

## Notes

Now that you have everything loaded up and running, you can run a number of commands at the console to maintain your dev environment.

* `update` - runs the `update` command found in `.zsh_functions`. It updates `brew`, `rbenv`, `nvm`. This is a command you will want to run periodically.

It's recommended that you review all of the aliases and functions found in, `.zsh_alias_list`, `.zsh_functions`, `.zsh_riskonnect` and learn how to utilize them.

<br>

[Home](README.md) | [Previous Step](https://github.com/scott-knight/linux-on-windows-11/blob/main/setup-rbenv-nvm.md)
