[PREV](https://github.com/scott-knight/linux-on-windows-11/blob/main/install-debian.md)

# Customize Debian 

This doc outlines the specifics for setting up the Debian environment for Ruby, Rails, Node, etc.

<br/>

## Connecting VSCode

1. In the Linux console, type the following:

```sh
code .
```

This will load VSCode and connect to your Debian instance. I'm a little hazy on what happens after it connects, but you should be able to see the files of your home directory.

<br/>
e
2. Close VSCode and the terminal 

<br/>

## Environment Setup

Next, we need system files for the environment. Run the following:

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
brew tap Homebrew/homebrew-cask && brew tap Homebrew/homebrew-services && brew tap homebrew/cask-versions
```

6. Install stuff

```sh
ulimit -n 8192 && brew install gcc git python ruby vips
```

Then

```zsh
brew postinstall docbook docbook-xsl
```
<br/>

7. Install Elixir (if you need/want it)

```sh
brew install erlang elixir
```

<br/>

## Install Postgres

Install postgres by doing the following 

```sh
brew install postgresql@15 && echo 'export PATH="/home/linuxbrew/.linuxbrew/opt/postgresql@15/bin:$PATH"' >> $HOME/.zshrc && source $HOME/.zshrc
```

Then copy this in the console:

```zsh
echo '' >> $HOME/.zshrc
echo "# create the postgresql temp file if it doesn't exit" >> $HOME/.zshrc
echo 'function set_pg_dir() {' >> $HOME/.zshrc
echo '  POSTGRESTEMP=/var/run/postgresql' >> $HOME/.zshrc
echo '  if [ ! -d "$POSTGRESTEMP" ]; then' >> $HOME/.zshrc
echo '    echo Creating $POSTGRESTEMP for PostgreSQL' >> $HOME/.zshrc
echo "    echo '1' | sudo -S mkdir $POSTGRESTEMP && echo '1' | sudo -S chmod 0777 /var/run/postgresql" >> $HOME/.zshrc
echo '  else' >> $HOME/.zshrc
echo '    echo $POSTGRESTEMP exists!' >> $HOME/.zshrc
echo '  fi' >> $HOME/.zshrc
echo '}' >> $HOME/.zshrc
echo 'alias setpgdir="set_pg_dir"' >> $HOME/.zshrc
echo 'setpgdir' >> $HOME/.zshrc
echo '' >> $HOME/.zshrc
echo '' >> $HOME/.zshrc
echo 'function pgfix () {' >> $HOME/.zshrc
echo '  PGFILE=/tmp/.s.PGSQL.5432' >> $HOME/.zshrc
echo '  if [ -f "$PGFILE"]; then' >> $HOME/.zshrc
echo "    echo '1' | sudo ln -s $PGFILE /var/run/postgresql/.s.PGSQL.5432" >> $HOME/.zshrc
echo '    echo $PGFILE was linked in /var/run/postgresql/' >> $HOME/.zshrc
echo '  else' >> $HOME/.zshrc
echo '    echo $PGFILE is not available for the fix!' >> $HOME/.zshrc
echo '  fi' >> $HOME/.zshrc
echo '}' >> $HOME/.zshrc
echo '' >> $HOME/.zshrc
source .zshrc
```
<br/>

### Edit postgresql.conf

To find the config file run the following:

```zsh
psql -U [linux username] -d postgres -c 'SHOW config_file'
```
<br/>

EX:

```zsh
psql -U sknight -d postgres -c 'SHOW config_file'
```
<br/>

It will return something like this:

> /home/linuxbrew/.linuxbrew/var/postgresql@15/postgresql.conf

<br/>

Edit the file.

```zsh
nano /home/linuxbrew/.linuxbrew/var/postgresql@15/postgresql.conf
```

<br/> 

In the file, find `unix_socket_directories`. Uncomment it and change it to:

```txt
unix_socket_directories = '/var/run/postgresql,/tmp'
```

Then save the file.

<br/>

To create `/var/run/postgresql` (if it doesn't already exist) run the following:

```zsh
setpgdir
```
<br/>

### Start the Postgres server

If you have enabled `systemd`, you can run the following:

```zsh
brew services start postgresql@15
```

OR to simply start it using the postgres command:

```zsh
pg_ctl -D /home/linuxbrew/.linuxbrew/var/postgresql@15 start
```

<br/> 

Once it's running it will return something like this:

> waiting for server to start....2022-11-13 20:54:42.075 UTC [26837] LOG:  starting PostgreSQL 15.1 (Homebrew) on x86_64-pc-linux-gnu, compiled by gcc-11 (Ubuntu 11.3.0-1ubuntu1~22.04) 11.3.0, 64-bit    
2022-11-13 20:54:42.075 UTC [26837] LOG:  listening on IPv4 address "127.0.0.1", port 5432  
2022-11-13 20:54:42.079 UTC [26837] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"  
2022-11-13 20:54:42.085 UTC [26837] LOG:  listening on Unix socket "/tmp/.s.PGSQL.5432"  
2022-11-13 20:54:42.090 UTC [26840] LOG:  database system was shut down at 2022-11-13 20:23:23 UTC  
2022-11-13 20:54:42.095 UTC [26837] LOG:  database system is ready to accept connections  
  done  
server started  

<br/>

## Setup SSH Key

To connect via ssh to external services you will need to generate a new private and public key. Follow the instructions [found here](https://docs.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) to create a new set.

Once your keys are created, run the following:

(Run this if `.ssh` doesn't exist:
```zsh
mkdir -m a=rwx $HOME/.ssh
```

```sh
touch $HOME/.ssh/config && nano $HOME/.ssh/config
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

```sh
sudo apt-get install keychain
```

<br/>

Once installed, start `keychain` to create the `$HOME\.keychain` directory and start the `ssh-agent`:

```sh
keychain
```

<br/>

Using VIM or Nano, copy this to the bottom of your .zshrc file:

```zsh
nano .zshrc
```

<br/>

```
# BIGBRAIN is the hostname of my computer. If you run `ls -FlaG $HOME/.keychain` in the console,
# you will see the hostname of your computer. Replace BIGBRAIN with that name  
export HOSTNAME=BIGBRAIN 

/usr/bin/keychain --clear $HOME/.ssh/id_ed25519
source $HOME/.keychain/$HOSTNAME-sh
```

<br/>

## Copy gitconfig

Using VIM or Nano, replace the contents of `.gitconfig` with the following:

```zsh
nano .gitconfig
```

<br/>

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

## INSTALL RBENV

To install RBENV, run the following:

```sh
git clone https://github.com/rbenv/rbenv.git ${HOME}/.rbenv
```

Add the following to `.zshrc`:

```zsh
nano .zshrc
```

<br/>

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
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.2/install.sh | bash
```

Once installed, reload the shell; then run the following:

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
git clone https://github.com/spaceship-prompt/spaceship-prompt.git "$ZSH_CUSTOM/themes/spaceship-prompt" --depth=1 && ln -s "$ZSH_CUSTOM/themes/spaceship-prompt/spaceship.zsh-theme" "$HOME/.oh-my-zsh/themes/spaceship.zsh-theme"
```

<br/>

## Copy the .zshrc Content

Run the following to clear out .zshrc:

```sh
truncate -s 0 .zshrc
```

<br/>

Using VIM or NANO, replace the entire `.zshrc` file with this code:

```zsh
nano .zshrc
```

<br/>

Copy the [contents of .zshrc](ZSHRC.md) and save.

Reload the Debian instance

<br/>

## Copy .zsh_alias_list Content

Using VIM or NANO, replace the entire `.zsh_alias_list` file with this code:

```zsh
nano .zsh_alias_list
```

<br/>

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

# nvm
alias nvms="nvm list"
alias nvmlist="nvm ls-remote"

# Yarn
alias yup="yarn upgrade --latest"
alias yupdate="yup"
alias ylist="yarn list"
alias yl="ylist"
alias ylg="ylist | grep $1"
alias ydev="yarn dev"

alias pstart="pg_ctl -D /home/linuxbrew/.linuxbrew/var/postgresql@15 start"
alias pstop="pg_ctl -D /home/linuxbrew/.linuxbrew/var/postgresql@15 stop"
#alias pgstart="brew services start postgresql@15"
#alias pgstop="brew services stop postgresql@15"
alias pgupdate="brew postgresql-upgrade-database"


alias python="$(brew --prefix python)"
```

From now on, when you want to reload the shell, simply type `reload`

<br/>

## Copy .zsh_functions

Using VIM or NANO, replace the contents of `.zsh_functions` with the following:

```zsh
nano .zsh_functions
```

<br/>

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

function update_debian () {
  echo ''
  echo 'Updating Debian files, please wait...'
  sudo bash -c 'for i in update {,dist-}upgrade auto{remove,clean}; do apt-get $i -y; done' &&
  echo 'Update of Debian complete!'
}

function upgrade_ohmyzsh(){
  echo ''
  echo 'Updating oh-my-zsh, please wait...'
  omz update &&
  echo "Completed upgrading oh-my-zsh!"
}

function findpg () {
  echo "looking for system 'pg_config'"
  sudo find / -name pg_config
}

function pg_install () {
  echo 'Updating the PG gem'
  gem install pg -- --with-pg-config=/usr/bin/pg_config &&
  echo 'done!'
}
alias pginstall="pg_install"
alias pgi="pg_install"
alias updatepg="pg_install"

function ffi_install () {
  gem install ffi -- --disable-system-libffi
}
alias ffii="ffi_install"

function bundle_install () {
  echo 'Installing gems, please wait...'
  pg_install && ffi_install && bundle
}
alias bi="bundle_install"

function bundle_update () {
  echo 'Updating gems, please wait...'
  pg_install && ffi_install && bundle update
}
alias bu="bundle_update"

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
  ulimit -n 8192 && brew update &&
  echo ''
  echo "Upgrading BREW installs" &&
  ulimit -n 8192 && brew upgrade &&
  echo ''
  echo "Cleaning up BREW" &&
  ulimit -n 8192 && brew cleanup &&
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
  update_debian &&
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

function bundlei () {
  echo ''
  echo '******  bundle Commands  **************************************'
  echo ''
  echo "pg_install     = gem install pg -- --with-pg-config=/usr/bin/pg_config"
  echo "pgs            = shows pg commands"
  echo ''
  echo "ffi_install    = gem install ffi -- --disable-system-libffi"
  echo "ffii           = ffi_install"
  echo ''
  echo "bundle_install = pg_install && ffi_install && bundle"
  echo "bi             = bundle_install"
  echo ''
  echo "bundle_update  = pg_install && ffi_install && bundle update"
  echo "bu             = bundle_update"
  echo ''
}
alias bundles="bundlei"

function pgs () {
  echo ''
  echo '******  pg Commands  **************************************'
  echo ''
  echo "findpg    = sudo find / -name pg_config   # finds the system pg_config"
  echo "updatepg  = gem install pg -- --with-pg-config=/usr/bin/pg_config"
  echo "upg       = gem install pg -- --with-pg-config=/usr/bin/pg_config"
  echo "pginstall = gem install pg -- --with-pg-config=/usr/bin/pg_config"
  echo ''
  echo "pgstart   = pg_ctl -D /home/linuxbrew/.linuxbrew/var/postgresql@15 start"
  echo "pgstop    = pg_ctl -D /home/linuxbrew/.linuxbrew/var/postgresql@15 stop"
  echo "pgupdate  = brew postgresql-upgrade-database"  
  echo ''
}

function updatei () {
  echo ''
  echo '******  Update Commands  **************************************'
  echo ''
  echo "update = update_debian && update_omz && update_rbenv && update_brew && upgrade_npm"
  echo ''
  pgs
  bundles
}
alias updates="updatei"

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

## Copy .zsh_jobsauce

This is an example of a personal file you (might) have with specific content needed in console.

Using VIM or NANO, replace the contents of `.zsh_jobsauce` with the following:

```zsh
#! /bin/zsh

# These aliases and functions assume you have loaded docker things in .zsh_functions

# JOBSAUCE THING   ===============================================================================
JOBSAUCE=$HOME/dev.projects/jobsauce-app
alias js="cd $JOBSAUCE"

# RUN IT LOCALLY - NO DOCKER   ===================================================================
alias jsserv="js && rails s -b 0.0.0.0 -p 3000"
alias jscon="js && rails c"


# JOBSAUCE   ==============================================================================
alias jsf="js && foreman start -f Procfile.dev"
alias jsui="js && foreman start -f Procfile.ui"
alias jss="jsserv"
alias jsc="jscon"
alias jsw="js && yarn build --watch"
alias jscss="js && yarn build:css --watch"
alias jstc="js & open coverage/index.html"

# RUN DB THINGS
alias jsdbc="js && rails db:create db:schema:load db:migrate"
alias jsdbr="js && rails db:drop db:create db:schema:load db:migrate"


# DOCKER =======================================================================================
# alias djsup="js && docker-compose up"
# alias djsz="js && docker exec -it jobsauce-app /bin/zsh"

# alias djsc="js && docker exec -it jobsauce-app /bin/bundle exec rails c"
# alias djss="js && docker-compose up web"
# alias djsw="js && docker-compose up webpacker"

# # RUN DB THINGS
# alias djsdb="js && docker-compose up postgres"
# alias djsdbc="js && docker-compose run --rm web /bin/rails db:create db:schema:load db:migrate"
# alias djsdbr="js && docker-compose run --rm web /bin/rails db:drop db:create db:schema:load db:migrate"

# # RAILS THINGS
# alias djss="js && docker-compose up rails"

function jsinfo () {
  echo ''
  echo '******  JobSauce Settings **************************************'
  echo "JOBSAUCE = $HOME/scott.projects/jobsauce-app"
  echo ''
  echo '******  JobSauce Aliases **************************************'
  echo "js       = cd $JOBSAUCE"
  echo "jsf      = js && foreman start -f Procfile.dev"
  echo "jss      = js && rails s -b 0.0.0.0 -p 3000"
  echo "jsui     = js && foreman start -f Procfile.ui"
  echo "jsw      = js && yarn build --watch"
  echo "jscss    = js && yarn build:css --watch"
  echo "jsc      = js && rails c"
  echo "jstc     = js & open coverage/index.html"
  echo "jsdbc    = js && rails db:create db:schema:load db:migrate"
  echo "jsdbr    = js && rails db:drop db:create db:schema:load db:migrater"
  echo ''
}
alias jsi="jsinfo"
```

<br/>

## Copy .gemrc

Using VIM, replace the contents of `.gemrc` with the following:

```zsh
nano .gemrc
```

<br/>

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

## Reload

After you have completed the setup, close and reload the Linux instance.

<br/>

## Install Ruby via RBENV

Install the Ruby you want:

```sh
cd $HOME && rbenv install 3.1.2 --verbose
```
<br/>

### Issues When Installing Ruby on Ubuntu

If you run into issues installing Ruby because of SSL incompatibility, you will need to use Brew OpenSSL. Run the following:

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

NVM no longer honors global installs of yarn. You will need to install Yarn in the node version you are running. I also ran into an issue when using earlier versions of node while trying to run yarn. I ended up having use the current node to get yarn to work as expected.

To install yarn, run the following:

```sh
npm install --global yarn
```

<br/>

### Clone Your Repo - Download Yarn Libraries

Once you have your project downloaded, and yarn libraries are installed, you can see which libraries are outdated by running the following:

```zsh
yarn outdated
```

To upgrade outdated libraries, run the following:

```zsh
yarn upgrade --latest
```

<br/>

## Notes for Rails Setups

This section is here, mainly for documentation purposes. I ran into a different issues while trying to run Rails in Debain vs Ubuntu. These are the notes I compiled while trying to get things to run. Make sure you have Postgres running before you attempt to install/bundle gems for your project.

<br/>

### Installing PG 

Brew will install Postgres, but it doesn't supply the system compiler that gem pg needs to install. To find the sytem `pg_config` run the following:

```zsh
sudo find / -name pg_config
```

<br/>

You will see something like this 

> /home/linuxbrew/.linuxbrew/Cellar/postgresql@15/15.1/bin/pg_config

> /usr/bin/pg_config

<br/>

You want to use the system's `pg_config` and not the brew install

```zsh
gem install pg -- --with-pg-config=/usr/bin/pg_config
```

<br/>

### Installing libffi for Bootsnap gem (Debian)

Rails uses bootsnap which uses libffi. We installed `libffi-dev` in a previous step. However, Bootsnap may have an issue running when we spin up Rails. If that happens, you may see this error that looks something like this:

> LoadError: libffi.so.8: cannot open shared object file: No such file or 
> directory - /home/sknight/dev.projects/jobsauce-app/.gems/gems/ffi-1.15.5/lib/ffi_c.so

This means that the system library is unable to use the system ffi installed by default, and we need to install a specific version for our Rails app (in our example, we need version 1.15.1). 

To install the specific version needed (as outlined in the error), and to get the app to ignore the system version, you do the following:

```sh
gem install ffi --version '1.15.5' -- --disable-system-libffi
```

OR you can simply install `ffi`, disabling the system version

```
gem install ffi -- --disable-system-libffi
```

<br/>

### Rails Issues

When you pull your repo code, you may run into an issue where rails is not executable:

> Permission denied - bin/rails (Errno::EACCES)

<br/>

To fix this, run the following:

```zsh
chmod u+x bin/rails
```

<br/><br/>

## Setup Systemd (Only if Needed)

This step is onlu need if you plan to run services via Brew and any other service you choose to install and run with Debian. We will need to setup Debian to use `systemd` by following a few simple steps. Info taken from [here](https://devblogs.microsoft.com/commandline/systemd-support-is-now-available-in-wsl/)

*Initially I found that even with the native `systemd` support now available within WSL, that `systemd` and `systemctl` still didn't work as anticipated when enabled. I spent nearly 4 full days looking for the cause and happened upon the answer in an [obscure gihub repo](https://github.com/arkane-systems/bottle-imp#debian) for a utility named `bottle-imp`. The bottle-imp tool was used to provide systemd support for WSL prior to the summer-2022 WSL update, and the bottle-imp code required a list of utilities to be installed for systemd and systemctl to work properly.* 

Note: If you use this, systemd removes all content from the `/var/run/` directory when Bebian boots up. This means that the `/var/run/postgres` directory gets removed. However, we have a function in `.zshrc` that checks for this dir and creates it. Unfortunately, if the postgres server is running, you will need to restart it. Postges adds the temp file needed by the `PG` gem to connect the rails app to postgres. We also have a couple of functions for that. To stop the postgres server, in the console, call `pgstop`. To start the postgres server, call `pgstart`. This will load the temp file in the in dir. If you would rather just copy the file needed into the dir you can call the following: `pgfix` - this will create link to the tmp file.

<br/>

1. Install the tools that will make systemd and systemctl work as expected:

```bash
sudo apt-get install -y daemonize dbus gawk libc6 policykit-1 python3 python3-pip python3-psutil systemd systemd-container 
```
<br/>

2. In the Debian console, run the following:

```bash
sudo nano /etc/wsl.conf
```

<br/>

3. In the nano editor (or use vim, either way), copy the following:

```txt
[boot]
systemd=true
```

<br/>

4. To save the changes, use `ctrl+o` close the file; then hit `y` to save the file; then hit `enter` to close the file.  

<br/>

5. Open Windows `Powershell` as an administrator. Type the following:

```powershell
wsl.exe --shutdown
```

<br/>

6. After the Debian instance closes, reopen it and run the following:

```sh
systemctl list-unit-files --type=service
```

You should see a table of info:

![systemctl](https://user-images.githubusercontent.com/516548/201456791-a90c2475-c0e5-4dc8-a113-fc3637cb059e.png)

<br/>

If that doesn't work, I'm not sure what to tell you. WSL changes often and it requires vigilence to keep it working as expected.
<br/>

<br/>[Previous Step](https://github.com/scott-knight/linux-on-windows-11/blob/main/install-debian.md)
