# Customize Linux (Ubuntu)

This is the step-by-step guide for customizing Linux to use all the settings, aliases, functions, etc., that I use every day. You don't have to do this step either. Linux will run without this customization. Also, if you go through the step-by-step guide and don't like the setup, you can simply [uninstall the Linux instance](https://learn.microsoft.com/en-us/windows/wsl/basic-commands#unregister-or-uninstall-a-linux-distribution) and [reinstall it](https://github.com/scott-knight/linux-on-windows-11/blob/main/install-linux.md). It will be a completely clean install of Linux, no settings will carry over.

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


<br/>

## Install Wajig (not available for Debian)

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

### Updating Debian

To update Debian, run the following:

```sh
sudo bash -c 'for i in update {,dist-}upgrade auto{remove,clean}; do apt-get $i -y; done'
```

<br/>

### Force Install of Updated Libraries

After you run updates, [you may see a notice that a couple of libraries were not upgraded](https://askubuntu.com/questions/601/the-following-packages-have-been-kept-back-why-and-how-do-i-solve-it). If you want to force update, you can run the following:

```sh
sudo aptitude full-upgrade
```

<br/>

## Install Linux Libraries

To install Linux libraries with `wajig`, run the following:

```sh
wajig install -y autoconf bison build-essential checkinstall clang git curl gcc libssl-dev libpq-dev libyaml-dev libreadline6-dev libncurses5-dev libffi-dev libgdbm6 libgdbm-dev libdb-dev make pkg-config vim zlib1g-dev zsh
```

<br/>

To install the libraries using `apt-get`, run the following:

```sh
sudo apt-get install -y autoconf bison build-essential checkinstall clang curl git gcc libssl-dev libpq-dev libyaml-dev libreadline6-dev libncurses5-dev libffi-dev libgdbm6 libgdbm-dev libdb-dev make pkg-config vim zlib1g-dev zsh
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

<br/>

## Install libff for bootsnap gem (DONT DO)

This section is here, mainly for documentation purposes.

Rails uses bootsnap which uses libff. We installed `libffi-dev` in a previous step. However, Bootsnap may have an issue running when we spin up Rails. If that happens, to install it, run the following:

```sh
wget http://es.archive.ubuntu.com/ubuntu/pool/main/libf/libffi/libffi8_3.4.2-4_amd64.deb
sudo dpkg -i libffi8_3.4.2-4_amd64.deb
```

<br/>

## Create System Files

Run the following:

```sh
touch .zsh_functions .zsh_alias_list .gemrc .gitconfig && mkdir dev.projects dev.projects/sandbox-js dev.projects/sandbox-api
```
<br/>

## Setup Zsh

Make zsh your default shell, run the following:

```sh
chsh -s $(which zsh)
```

Once changed, restart the Linux instance.

You will be prompted to make a selection to create a `.zshrc` file. Select the option that simply adds the comment code and creates the file.

Close all instances and start a new Linux instance.

<br/>


## Install BREW for Linux

Reference found [here](https://docs.brew.sh/Homebrew-on-Linux)

1. To install BREW for Linux, run the following:

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

2. You should be prompted about default settings and installation. Simply accept the defaults.
3. Once finished, add the following to your `.zshrc` file

```sh
echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> $HOME/.zshrc && eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
```

4. Reload the Linux shell.
5. Add BREW taps

```sh
brew tap Homebrew/homebrew-cask && brew tap Homebrew/homebrew-services && brew tap homebrew/cask-versions && brew tap Homebrew/homebrew-core
```

6. Install a Node and Ruby

```sh
brew install gcc node python ruby
```

7. Install DBs

```sh
brew install sqlite postgresql@14
```

8. Install image compression libraries

```sh
brew install vips
```

9. Run post installs

```sh
brew postinstall docbook docbook-xsl
```

10. Install Elixir (if you need/want it)

```sh
brew install erlang elixir
```

<br/>

## Setup SSH Key

To connect via ssh to external services you will need to generate a new private and public key. Follow the instructions [found here](https://docs.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) to create a new set.

Once your keys are created, run the following:

```sh
touch ${HOME}/.ssh/config
```

Copy the following to the new `config` file:

```sh
Host *
  AddKeysToAgent yes
  IdentityFile ~/.ssh/id_ed25519
```

You will need to add the new public key to your github account. You can copy the key by running the following:

```sh
clip.exe < ${HOME}/.ssh/id_ed25519.pub
```

<br/>

If you copy contents from another `.ssh` to this setup, you will may need to change the file level permissions:

```sh
chmod 400 $HOME/.ssh/id_ed25519
```

<br/>

## INSTALL KEYCHAIN

Run the following:

```
wajig install keychain
```

OR 

```sh
sudo apt-get install keychain
```

<br/>

Once installed, start `keychain` to create the `$HOME\.keychain` directory and start the `ssh-agent`:

```sh
keychain
```

<br/>

Using VIM, copy this to the bottom of your .zshrc file:

```
# BIGBRAIN is the hostname of my computer. If you run `ls -FlaG $HOME/.keychain` in the console,
# you will see the hostname of your computer. Replace BIGBRAIN with that name  
export HOSTNAME=BIGBRAIN 

/usr/bin/keychain --clear $HOME/.ssh/id_ed25519
source $HOME/.keychain/$HOSTNAME-sh
```

<br/>

## INSTALL RBENV

To install RBENV, run the following:

```sh
git clone https://github.com/rbenv/rbenv.git ${HOME}/.rbenv
```

Add the following to `.zshrc`:

```sh
# RBENV
export RBENV_ROOT="${HOME}/.rbenv"
export PATH="${RBENV_ROOT}/bin:${RBENV_ROOT}/shims:$PATH"
if which rbenv > /dev/null; then eval "$(rbenv init - zsh)"; fi
```

Close and reload the Linux instance

<br/>

## Install ruby-build rbenv-gemset rbenv-whatis rbenv-use

To install RBENV plugins, run the following:

```
mkdir -p "$(rbenv root)"/plugins "$(rbenv root)"/versions &&
git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)/plugins/ruby-build" &&
git clone https://github.com/jf/rbenv-gemset.git "$(rbenv root)/plugins/rbenv-gemset" &&
git clone https://github.com/rkh/rbenv-whatis.git "$(rbenv root)/plugins/rbenv-whatis" &&
git clone https://github.com/rkh/rbenv-use.git "$(rbenv root)/plugins/rbenv-use" &&
git clone https://github.com/tpope/rbenv-aliases.git "$(rbenv root)/plugins/rbenv-aliases" &&
rbenv alias --auto
```

<br/>

## Install NVM

NVM documenation is [found here](https://github.com/nvm-sh/nvm#installing-and-updating). To install NVM, run the following:

```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
```

Once installed, run the following:

```sh
nvm install --lts && npm install npm@latest -g
```

<br/>

## Install oh-my-zsh

Source found [here](https://ohmyz.sh/)

To install oh-my-zsh, run the following:

```sh
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
<br/>

### Install Plugins

Run the following:

```sh
git clone https://github.com/zsh-users/zsh-autosuggestions $HOME/.oh-my-zsh/custom/plugins/zsh-autosuggestions &&
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $HOME/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting && 
git clone https://github.com/gusaiani/elixir-oh-my-zsh.git $HOME/.oh-my-zsh/custom/plugins/elixir
```

### Install Spaceship-prompt

The [spaceship-prompt](https://github.com/spaceship-prompt/spaceship-prompt) is a clean .oh-my-zsh theme. To install it, run the following:

```sh


Once finished, run the following:

```sh
git clone https://github.com/spaceship-prompt/spaceship-prompt.git "$ZSH_CUSTOM/themes/spaceship-prompt" --depth=1 && ln -s "$ZSH_CUSTOM/themes/spaceship-prompt/spaceship.zsh-theme" "$HOME/.oh-my-zsh/themes/spaceship.zsh-theme"
```



## Copy the .zshrc Content

Run the following to clear out .zshrc:

```sh
truncate -s 0 .zshrc
```

<br/>

Using VIM, replace the entire `.zshrc` file with this code:

```zsh
# If you come from bash you might have to change your $PATH.
#export PATH=$HOME/bin:/usr/local/bin:$PATH

# Path to your oh-my-zsh installation.
export ZSH="$HOME/.oh-my-zsh"

# Set name of the theme to load --- if set to "random", it will
# load a random theme each time oh-my-zsh is loaded, in which case,
# to know which specific one was loaded, run: echo $RANDOM_THEME
# See https://github.com/ohmyzsh/ohmyzsh/wiki/Themes
ZSH_THEME="spaceship"

# Set list of themes to pick from when loading at random
# Setting this variable when ZSH_THEME="spaceship"
# a theme from this variable instead of looking in $ZSH/themes/
# If set to an empty array, this variable will have no effect.
# ZSH_THEME_RANDOM_CANDIDATES=( "robbyrussell" "agnoster" )

# Uncomment the following line to use case-sensitive completion.
# CASE_SENSITIVE="true"

# Uncomment the following line to use hyphen-insensitive completion.
# Case-sensitive completion must be off. _ and - will be interchangeable.
HYPHEN_INSENSITIVE="true"

# Uncomment one of the following lines to change the auto-update behavior
# zstyle ':omz:update' mode disabled  # disable automatic updates
# zstyle ':omz:update' mode auto      # update automatically without asking
# zstyle ':omz:update' mode reminder  # just remind me to update when it's time

# Uncomment the following line to change how often to auto-update (in days).
# zstyle ':omz:update' frequency 13

# Uncomment the following line if pasting URLs and other text is messed up.
# DISABLE_MAGIC_FUNCTIONS="true"

# Uncomment the following line to disable colors in ls.
# DISABLE_LS_COLORS="true"

# Uncomment the following line to disable auto-setting terminal title.
# DISABLE_AUTO_TITLE="true"

# Uncomment the following line to enable command auto-correction.
# ENABLE_CORRECTION="true"

# Uncomment the following line to display red dots whilst waiting for completion.
# You can also set it to another string to have that shown instead of the default red dots.
# e.g. COMPLETION_WAITING_DOTS="%F{yellow}waiting...%f"
# Caution: this setting can cause issues with multiline prompts in zsh < 5.7.1 (see #5765)
COMPLETION_WAITING_DOTS="true"

# Uncomment the following line if you want to disable marking untracked files
# under VCS as dirty. This makes repository status check for large repositories
# much, much faster.
DISABLE_UNTRACKED_FILES_DIRTY="true"

# Uncomment the following line if you want to change the command execution time
# stamp shown in the history command output.
# You can set one of the optional three formats:
# "mm/dd/yyyy"|"dd.mm.yyyy"|"yyyy-mm-dd"
# or set a custom format using the strftime function format specifications,
# see 'man strftime' for details.
# HIST_STAMPS="mm/dd/yyyy"

# Would you like to use another custom folder than $ZSH/custom?
# ZSH_CUSTOM=/path/to/new-custom-folder

# Which plugins would you like to load?
# Standard plugins can be found in $ZSH/plugins/
# Custom plugins may be added to $ZSH_CUSTOM/plugins/
# Example format: plugins=(rails git textmate ruby lighthouse)
# Add wisely, as too many plugins slow down shell startup.
plugins=(
  colorize
  git
  rbenv
  ruby
  python
  elixir
  vscode
  node
  npm
  yarn
  aws
  zsh-syntax-highlighting
  zsh-autosuggestions
)

source $ZSH/oh-my-zsh.sh

# User configuration

# export MANPATH="/usr/local/man:$MANPATH"

# You may need to manually set your language environment
export LANG=en_US.UTF-8

# Preferred editor for local and remote sessions
# if [[ -n $SSH_CONNECTION ]]; then
#   export EDITOR='vim'
# else
#   export EDITOR='mvim'
# fi

# Compilation flags
# export ARCHFLAGS="-arch x86_64"

# Set personal aliases, overriding those provided by oh-my-zsh libs,
# plugins, and themes. Aliases can be placed here, though oh-my-zsh
# users are encouraged to define aliases within the ZSH_CUSTOM folder.
# For a full list of active aliases, run `alias`.
#
# Example aliases
alias zshconfig="code ${HOME}/.zshrc"
alias ohmyzsh="code ${HOME}/.oh-my-zsh"

source "$HOME/.oh-my-zsh/themes/spaceship.zsh-theme"
# SPACESHIP_USER_SHOW=always
# SPACESHIP_DIR_TRUNC_PREFIX=../
SPACESHIP_TIME_COLOR=blue
SPACESHIP_TIME_SHOW=true
SPACESHIP_DIR_TRUNC=0
SPACESHIP_PACKAGE_SHOW=false
SPACESHIP_DIR_TRUNC_REPO=true
SPACESHIP_RUBY_SHOW=true
SPACESHIP_NODE_SHOW=true
SPACESHIP_ELIXIR_SHOW=true
SPACESHIP_BATTERY_SHOW=false
SPACESHIP_GIT_BRANCH_SHOW=true
SPACESHIP_PROMPT_ASYNC=true

# BREW
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
export BREW_OPT_PATH="/home/linuxbrew/.linuxbrew/opt"
export PATH="${BREW_OPT_PATH}/node/bin:$PATH"

# RBENV
export RBENV_ROOT="${HOME}/.rbenv"
export PATH="${RBENV_ROOT}/bin:${RBENV_ROOT}/shims:$PATH"
if which rbenv > /dev/null; then eval "$(rbenv init - zsh)"; fi

# NVM
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"

autoload -U add-zsh-hook
load-nvmrc() {
  local node_version="$(nvm version)"
  local nvmrc_path="$(nvm_find_nvmrc)"

  if [ -n "$nvmrc_path" ]; then
    local nvmrc_node_version=$(nvm version "$(cat "${nvmrc_path}")")

    if [ "$nvmrc_node_version" = "N/A" ]; then
      nvm install
    elif [ "$nvmrc_node_version" != "$node_version" ]; then
      nvm use
    fi
  elif [ "$node_version" != "$(nvm version default)" ]; then
    echo "Reverting to nvm default version"
    nvm use default
  fi
}
add-zsh-hook chpwd load-nvmrc


ZASYS=$HOME/.zsh_alias_list
if [ -f "$ZASYS" ]
then
  source "$ZASYS"
  echo "using ${ZASYS}"
else
  echo "not sure what happened with ${ZASYS}"
fi

ZFUNCT=$HOME/.zsh_functions
if [ -f "$ZFUNCT" ]
then
  source "$ZFUNCT"
  echo "using ${ZFUNCT}"
else
  echo "not sure what happened with ${ZFUNCT}"
fi

# ZSAUCE=$HOME/.zsh_jobsauce
# if [ -f "$ZSAUCE" ]
# then
#   source "$ZSAUCE"
#   echo "using ${ZSAUCE}"
# else
#   echo "not sure what happened with ${ZSAUCE}"
# fi

# BIGBRAIN is the hostname of my computer. If you run `ls -FlaG $HOME/.keychain` in the console,
# you will see the hostname of your computer. Replace BIGBRAIN with that name  
export HOSTNAME=BIGBRAIN 

/usr/bin/keychain --clear $HOME/.ssh/id_ed25519
source $HOME/.keychain/$HOSTNAME-sh
```

Reload the Linux instance

<br/>

## Copy .zsh_alias_list Content

Using VIM, replace the entire `.zsh_alias_list` file with this code:

```zsh
#! /bin/zsh

# SYSTEM
alias ll='ls -FlaG'
alias reload='omz reload'
alias home='cd ~'
alias dls="cd ~/Downloads"
alias myip="curl https://ipecho.net/plain; echo"
alias ehosts="sudo vim /private/etc/hosts"
alias pbcopy="clip.exe"
alias pbpaste="powershell.exe -command 'Get-Clipboard' | head -n -1"


# Projects code
PROJCS=$HOME/dev.projects
alias dev="cd $PROJCS && ll"
alias sdev="dev"
alias sui="cd $PROJCS/sandbox-js"
alias sjs="sui"
alias sapi="cd $PROJCS/sandbox-api"


# ERLANG
alias erlang="echo Erlang version is: ; erl -eval 'erlang:display(erlang:system_info(otp_release)), halt().'  -noshell ; echo use the command erl to start the Erlang CLI"


# Ruby/Rails
alias echomigrate="echo Performing rails migration"
alias echotestmigration="echo Performing rails migration for Test"
alias migrate="echomigrate ; rake db:migrate"
alias migratetest="echotestmigration ; RAILS_ENV=test bundle exec rake db:migrate"
alias rollback="echo Performing rollback... ; bundle exec rake db:rollback"
alias rnew="echo rails new <app_name> --webpack --skip-turbolinks --skip-spring --skip-coffee -T -B -d postgresql"
alias mkenvs="touch .ruby-version ; touch .ruby-gemset"
alias vedit='EDITOR=vi bin/rails credentials:edit'
alias rslh="rails s -b 0.0.0.0 -p $1"
alias rc='rails c'
alias rg='rails g'


# GIT commands
alias gbr="git branch "
alias gco="git checkout "
alias gnew="git checkout -b "
alias gstash="git stash"
alias gpop="git stash pop"
alias gdrop="git stash drop"
alias gdel="git branch -D "
alias godel="git push origin --delete $1"
alias gfetch="git fetch"
alias gmerge="git merge"
alias gpull="git pull"
alias gpush="git push"
alias glog="git log"
alias gstat="git status"


# rbenv list
alias rbv="rbenv version"                        # shows the current ruby version
alias rbvs="rbenv versions"                      # shows installed ruby versions
alias rbvlist="rbenv install --list-all"         # show all availabe ruby versions
alias rehash="rbenv rehash"                      # refreshes rbenv
alias active="rbenv gemset active"               # shows the active gemset
alias gemsloc="gem env home"                     # shows the current rubygems environment
alias gemloc="gem env home"                      # shows the current rubygems environment
alias gemenv="gem env home"                      # shows the current rubygems environment
alias gsi="rbenv gemset init \$1"                # allows you to create and set a gemset, ex: gsi rails5x
alias gsc="rbenv gemset create \$(ruby_ver) \$1" # allows you to create a gemset, ex: gsc rails5x
alias gsd="rbenv gemset delete \$(ruby_ver) \$1" # allows you to delete a gemset, ex: gsd rails5x
alias gsadd="echo -e > .ruby-gemset"             # creates a generic gemset file
alias rubyadd="echo \$1 > .ruby-version"         # allows you to set the desired ruby version, ex: rubyadd 2.3.1
alias rmgems='echo Removing .rbenv-gemsets ; rm -rf .rbenv-gemsets'
alias rbvdoc='curl -fsSL https://github.com/rbenv/rbenv-installer/raw/master/bin/rbenv-doctor | bash'

# Yarn
alias yup="yarn upgrade --latest"
alias yupdate="yup"
alias ylist="yarn list"
alias yl="ylist"
alias ylg="ylist | grep $1"
alias ydev="yarn dev"

alias pstart="pg_ctl -D /home/linuxbrew/.linuxbrew/var/postgresql@14 start"
alias pstop="pg_ctl -D /home/linuxbrew/.linuxbrew/var/postgresql@14 stop"
alias pupdate="brew postgresql-upgrade-database"

alias python="$(brew --prefix python)"
```

From now on, when you want to reload the shell, simply type `reload`

<br/>

## Copy .zsh_functions

Using VIM, replace the contents of `.zsh_functions` with the following:

```zsh
#! /bin/zsh

# function to delete source and local git branch
function gitdel(){
  git push origin --delete $@
  git branch -D $@
  git fetch --all --prune
}

# function update_ubuntu () {
#   echo ''
#   echo 'Updating Ubuntu files, please wait...'
#   wajig update &&
#   wajig upgrade -y &&
#   wajig distupgrade -y &&
#   wajig autoremove &&
#   wajig autoclean &&
#   echo 'Update of Ubuntu complete!'
# }

# function update_debian () {
#   echo ''
#   echo 'Updating Debian files, please wait...'
#   sudo bash -c 'for i in update {,dist-}upgrade auto{remove,clean}; do apt-get $i -y; done'n &&
#   echo 'Update of Debian complete!'
# }

function upgrade_ohmyzsh(){
  echo ''
  echo 'Updating oh-my-zsh, please wait...'
  omz update &&
  echo "Completed upgrading oh-my-zsh!"
}

function update_rbenv () {
  CURR_DIR="$PWD" &&
  echo ""
  echo 'Updating RBENV, please wait...'
  cd "$(rbenv root)" && git pull &&
  cd "$CURR_DIR"
  echo "Done updating RBENV!" &&
  update_ruby_build &&
  update_rbenv_gemset &&
  update_rbenv_whatis &&
  update_rbenv_use &&
  update_rbenv_aliases
}

function update_ruby_build () {
  CURR_DIR="$PWD" &&
  echo ""
  echo 'Updating ruby-build, please wait...'
  cd "$(rbenv root)"/plugins/ruby-build && git pull &&
  cd "$CURR_DIR"
  echo "Done updating ruby-build!"
}

function update_rbenv_gemset () {
  CURR_DIR="$PWD" &&
  echo ""
  echo 'Updating rbenv-gemset, please wait...'
  cd "$(rbenv root)"/plugins/rbenv-gemset && git pull &&
  cd "$CURR_DIR"
  echo "Done updating rbenv-gemset!"
}

function update_rbenv_whatis () {
  CURR_DIR="$PWD" &&
  echo ""
  echo 'Updating rbenv-whatis, please wait...'
  cd "$(rbenv root)"/plugins/rbenv-whatis && git pull &&
  cd "$CURR_DIR"
  echo "Done updating rbenv-whatis!"
}

function update_rbenv_use () {
  CURR_DIR="$PWD" &&
  echo ""
  echo 'Updating rbenv-use, please wait...'
  cd "$(rbenv root)"/plugins/rbenv-use && git pull &&
  cd "$CURR_DIR"
  echo "Done updating rbenv-use!"
}

function update_rbenv_aliases () {
  CURR_DIR="$PWD" &&
  echo ""
  echo 'Updating rbenv-aliases, please wait...'
  cd "$(rbenv root)"/plugins/rbenv-aliases && git pull &&
  cd "$CURR_DIR"
  echo "Done updating rbenv-aliases!"
}

function update_brew() {
  echo ''
  echo "Updating BREW"
  brew update &&
  echo ''
  echo "Upgrading BREW installs" &&
  brew upgrade &&
  echo ''
  echo "Cleaning up BREW" &&
  brew cleanup &&
  echo "Completed BREW updates!"
}

function upgrade_npm() {
  echo '' &&
  echo "Updating NPM" &&
  npm install npm@latest -g &&
  echo "Completed updating NPM!"
}
alias unpm="upgrade_npm"
alias npmu="upgrade_npm"
alias npm_update="upgrade_npm"
alias npmupdate="upgrade_npm"

function update_omz() {
  echo ''
  # $ZSH/tools/upgrade.sh
  omz update
}

function update() {
  echo "Upgrading ALL the things..."
  # update_ubuntu &&
  # update_debian &&
  update_omz &&
  update_rbenv &&
  update_brew &&
  upgrade_npm &&
  echo ''
  echo "All updates complete!!"
}

function cedit () {
  EDITOR="code --wait" bin/rails credentials:edit
}

function ecreds () {
  echo ''
  echo '******  Editing Rails Credentials  **************************************'
  echo 'cedit = EDITOR="code --wait" bin/rails credentials:edit'
  echo 'vedit = EDITOR=vi bin/rails credentials:edit'
}

function gits () {
  echo ''
  echo '****** GIT commands  **************************************'
  echo 'gbr      = git branch'
  echo 'gco      = git checkout'
  echo 'gnew     = git checkout -b'
  echo 'gstash   = git stash'
  echo 'gpop     = git stash pop'
  echo 'gdrop    = git stash drop'
  echo 'gdel     = git branch -D'
  echo 'godel    = git push origin --delete $1'
  echo 'gfetch   = git fetch'
  echo 'gmerge   = git merge'
  echo 'gpull    = git pull'
  echo 'gpush    = git push'
  echo 'glog     = git log'
  echo 'gstat    = git status'
  echo ''
}
```

<br/>

## Copy .gemrc

Using VIM, replace the contents of `.gemrc` with the following:

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

## Copy gitconfig

Using VIM, replace the contents of `.gitconfig` with the following:

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
  hist = log --pretty=format:\"%h %ad | %s%d [%an]\" --graph --date=short
  type = cat-file -t
  dump = cat-file -p

[pager]
  branch = false
```

Replace the email and username with your values.

<br/>

## Reload

After you have completed the setup, close and reload the Linux instance.

<br/>

## Install Ruby via RBENV

Install the Ruby you want:

```sh
cd $HOME && rbenv install 3.1.2 --verbose
```
<br/>

### Issues When Installing Ruby

If you run into issues installing Ruby because of SSL incompatibility (in Ubuntu), you will need to use Brew OpenSSL. Run the following:

```sh
RUBY_CONFIGURE_OPTS="--with-openssl-dir=$(brew --prefix openssl@1.1)" rbenv install 3.1.2 --verbose
```

<br/>

### Update Rubygems

```sh
gem update --system
```
<br/>

## Install Yarn

NVM no longer honors global installs of yarn. You will need to install Yarn in node version you are running. To install yarn, run the following:

```sh
npm install yarn
```

