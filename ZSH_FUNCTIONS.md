[Home](README.md) | [Custmize the Ubuntu Environment](https://github.com/scott-knight/linux-on-windows-11/blob/main/customize-the-ubuntu-environment.md)

<br/>

# .zsh_functions

```zsh
#! /bin/zsh

# function to delete source and local git branch
function gitdel(){
  git push origin --delete $@
  git branch -D $@
  git fetch --all --prune
}

function install_rbenv () {
  cd $HOME && git clone https://github.com/rbenv/rbenv.git $HOME/.rbenv
}

function install_rbenv_plugins () {
  cd $HOME && mkdir $HOME/.rbenv/plugins &&
  git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build &&
  git clone https://github.com/jf/rbenv-gemset.git "$(rbenv root)"/plugins/rbenv-gemset &&
  cd $HOME/.rbenv && cd $HOME
}

function reload_rbenv () {
  echo 'Deleteing and reinstalling RBENV. Please wait...'
  echo ''
  cd $HOME &&
  rm -rf $HOME/.rbenv &&
  install_rbenv &&
  install_rbenv_plugins &&
  echo 'Done!'
}

function update_rbenv () {
  CURR_DIR="$PWD" &&
  echo ""
  echo 'Updating RBENV, please wait...'
  cd "$(rbenv root)" && git pull &&
  cd "$CURR_DIR"
  echo "Done updating RBENV!" &&
  update_ruby_build &&
  update_rbenv_gemset
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

function update_ubuntu () {
  echo ''
  echo 'Updating Ubuntu files, please wait...'
  wajig update &&
  wajig upgrade -y &&
  wajig distupgrade -y &&
  wajig autoremove &&
  wajig autoclean &&
  echo 'Update of Ubuntu complete!'
}

function upgrade_ohmyzsh(){
  echo ''
  echo 'Updating oh-my-zsh, please wait...'
  omz update &&
  echo "Completed upgrading oh-my-zsh!"
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

function update_nvm() {
  echo '' &&
  echo "Updating NVM" &&
  (
    cd "$NVM_DIR"
    git fetch --tags origin
    git checkout `git describe --abbrev=0 --tags --match "v[0-9]*" $(git rev-list --tags --max-count=1)`
  ) && \. "$NVM_DIR/nvm.sh" &&
  echo "Completed updating NVM!"
}

function upgrade_node_lts() {
  echo '' &&
  echo 'Updating Node LTS' &&
  nvm install --lts &&
  # nvm use --lts &&
  echo 'Completed updating Node!' &&
  upgrade_npm
}

function upgrade_node() {
  echo '' &&
  echo 'Updating Node' &&
  nvm install node &&
  # nvm use node
  echo 'Completed updating Node!' &&
  upgrade_npm
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
  update_ubuntu &&
  update_omz ;
  update_rbenv &&
  update_brew &&
  update_nvm &&
  # upgrade_node_lts &&
  upgrade_node &&
  echo ''
  echo "All updates complete!!"
}

function upgrade_yarn() {
  yarn set version stable
}
alias yarn_upgrade="upgrade_yarn"
alias yarn_upg="upgrade_yarn"

function yarnup() {
  yarn upgrade-interactive
}
alias yarnui="yarnup"

function yarns () {
  echo ''
  echo "******  Yarn commands  **************************************"
  echo 'upgrade_yarn = yarn set version stable'
  echo 'yarn_upgrade = upgrade_yarn'
  echo 'yarnupg      = upgrade_yarn'
  echo 'yarnup       = yarn upgrade-interactive'
  echo 'yarnui       = yarn upgrade-interactive'
  echo ''
}

function cedit () {
  EDITOR="code --wait" bin/rails credentials:edit
}

function cpedit () {
  EDITOR="code --wait" bin/rails credentials:edit --environment production
}

function csedit () {
  EDITOR="code --wait" bin/rails credentials:edit --environment staging
}

function fixrails () {
  chmod u+x bin/rails
}
alias railsfix="fixrails"
alias fixr="fixrails"

# ##### REDIS SERVER #####
function rediscom() {
  echo ''
  echo "'redstat' - checks the status of Redis; to see if it is running."
  echo "'redstart' - starts Redis"
  echo "'redstop' - stops Redis"
  echo "'redrestart' - stops Redis and then starts it again"
  echo "'rediscom' - shows the available aliases created for manipulating the Redis-CLI"
  echo ''
}

alias bredstart="brew services start redis"
alias bredstop="brew services stop redis"

function start_redis() {
  # nohup redis-server /usr/local/etc/redis.conf > /dev/null 2>&1 &

  redis-cli time
  if [ $? = 0 ]; then
    echo "Redis already is running."
  else
    echo "Starting redis server..."
    # redis-server --port 6379
    bredstart
  fi
}

function stop_redis() {
  # redis-cli shutdown
  bredstop
}

function redis_status () {
  redis-cli time
  if [ $? = 0 ]; then
    echo "Redis is running."
  else
    echo "Redis is NOT running."
  fi
}

alias redstart='start_redis'
alias redstop='stop_redis'
alias redrestart='stop_redis ; start_redis'
alias redstat='redis_status'


# ##### memcache SERVER #####
function memcachedstart() {
  brew services start memcached
}
alias memstart="memcachedstart"
alias memcstart="memcachedstart"

function memcachedrestart() {
  brew services restart memcached
}
alias memrestart="memcachedrestart"
alias memcrestart="memcachedrestart"
alias memres="memcachedrestart"

function memcachedstop() {
  brew services stop memcached
}
alias memstop="memcachedstop"
alias memcstop="memcachedstop"

function pgs () {
  echo ''
  echo '******  pg Commands  **************************************'
  echo ''
  echo '# alias pgstart="pg_ctl -D /home/linuxbrew/.linuxbrew/var/postgresql@15 start"'
  echo '# alias pgstop="pg_ctl -D /home/linuxbrew/.linuxbrew/var/postgresql@15 stop"'
  echo ''
  echo "pgstart  = brew services start postgresql@15"
  echo "pgstop   = brew services stop postgresql@15"
  echo "pgupdate = brew postgresql-upgrade-database"
  echo ''
}

function updatei () {
  echo ''
  echo '******  Update Commands  **************************************'
  echo ''
  echo "update = update_ubuntu && update_omz && update_rbenv && update_brew && update_nvm && upgrade_npm"
  echo ''
  pgs
  bundles
}
alias updates="updatei"

function editi () {
  echo ''
  echo '******  Editing Rails Credentials in VSCODE **************************************'
  echo 'cedit  = EDITOR="code --wait" bin/rails credentials:edit'
  echo 'cpedit = EDITOR="code --wait" bin/rails credentials:edit --environment production'
  echo 'csedit = EDITOR="code --wait" bin/rails credentials:edit --environment staging'
  echo
  echo ''
  echo '******  Editing Rails Credentials in VIM **************************************'
  echo 'vedit  = EDITOR=vi bin/rails credentials:edit'
  echo 'vedit  = EDITOR=vi bin/rails credentials:edit --environment production'
  echo 'vedit  = EDITOR=vi bin/rails credentials:edit --environment production'
}
alias edits="editi"
alias ecreds="editi"

function gits () {
  echo ''
  echo '****** GIT commands  **************************************'
  echo 'gbr    = git branch'
  echo 'gco    = git checkout'
  echo 'gnew   = git checkout -b'
  echo 'gadd   = git add'
  echo 'gadd.  = git add .'
  echo 'gcom   = git commit -m'
  echo 'gstash = git stash'
  echo 'gpop   = git stash pop'
  echo 'gdrop  = git stash drop'
  echo 'gdel   = git branch -D'
  echo 'godel  = git push origin --delete $1'
  echo 'gfetch = git fetch'
  echo 'gmerge = git merge'
  echo 'gpull  = git pull'
  echo 'gpush  = git push'
  echo 'gstat  = git status'
  echo 'glog   = git log --pretty=full'
  echo 'glogm  = git log --pretty=medium'
  echo ''
}

function railsi () {
  echo ''
  echo '****** rails commands  **************************************'
  echo "rollback = bundle exec rake db:rollback"
  echo "vedit    = EDITOR=vi bin/rails credentials:edit"
  echo 'rslh     = rails s -b 0.0.0.0 -p $1'
  echo "rc       = rails c"
  echo "rg       = rails g"
  echo "rfix     = chmod u+x bin/rails"
  echo "rpid     = lsof -wni tcp:3000"
  echo "rstop    = kill -9 [need the pid form running rpid]"
}
alias railss="railsi"

function rbenvi () {
  echo ''
  echo '****** RBENV ALIASES **************************************'
  echo 'rbv     = rbenv version             # shows the current ruby version'
  echo 'rbvs    = rbenv versions            # shows installed ruby versions'
  echo 'rbvlist = rbenv install --list-all  # show all availabe ruby versions'
  echo 'rehash  = rbenv rehash              # refreshes rbenv'
  echo 'active  = rbenv gemset active       # shows the active gemset'
  echo ''
}
alias rbvi='rbenvi'
alias rbenvs='rbenvi'
```

<br/>

[Home](README.md) | [Custmize the Ubuntu Environment](https://github.com/scott-knight/linux-on-windows-11/blob/main/customize-the-ubuntu-environment.md)
