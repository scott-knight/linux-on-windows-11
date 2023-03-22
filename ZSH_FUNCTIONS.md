[Home](README.md) | [Customize Ubuntu](customize-ubuntu.md#copy-zsh_functions)

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
  echo "Updating NVM" &&
  (
    cd "$NVM_DIR"
    git fetch --tags origin
    git checkout `git describe --abbrev=0 --tags --match "v[0-9]*" $(git rev-list --tags --max-count=1)`
  ) && \. "$NVM_DIR/nvm.sh" &&
  echo "Completed updating NVM!"
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
  update_omz &&
  update_rbenv &&
  update_brew &&
  update_nvm &&
  upgrade_npm &&
  echo ''
  echo "All updates complete!!"
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
```

<br/>

[Home](README.md) | [Customize Ubuntu](customize-ubuntu.md#copy-zsh_functions)
