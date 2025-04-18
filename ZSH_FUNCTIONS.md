[Home](README.md) | [Custmize the Ubuntu Environment](https://github.com/scott-knight/linux-on-windows-11/blob/main/customize-the-ubuntu-environment.md)

<br/>

# .zsh_functions

```zsh
#! /bin/zsh

function reload_zsh_tools () {
  echo 'Deleteing and reinstalling ZSH tools. Please wait...'
  echo ''
  cd $HOME &&
  rm -rf $HOME/.zsh &&
  install_zsh_tools &&
  echo 'Done!'
  echo ''
}

function install_zsh_tools () {
  cd $HOME && mkdir $HOME/.zsh &&
  git clone https://github.com/zsh-users/zsh-autosuggestions $HOME/.zsh/zsh-autosuggestions &&
  echo "" &&
  git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $HOME/.zsh/zsh-syntax-highlighting &&
  cd $HOME
}

function update_zsh_autosuggestions () {
  CURR_DIR="$PWD" &&
  echo ""
  echo 'Updating zsh-autosuggestions, please wait...'
  cd $HOME/.zsh/zsh-autosuggestions && git pull &&
  cd "$CURR_DIR"
  echo "Done updating zsh-autosuggestions!"
}

function update_zsh_syntax_highlighting () {
  CURR_DIR="$PWD" &&
  echo ""
  echo 'Updating zsh-syntax-highlighting, please wait...'
  cd $HOME/.zsh/zsh-syntax-highlighting && git pull &&
  cd "$CURR_DIR"
  echo "Done updating zsh-syntax-highlighting!"
}

function update_zsh_tools () {
  CURR_DIR="$PWD" &&
  update_zsh_autosuggestions &&
  update_zsh_syntax_highlighting
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

function upgrade_brew() {
  echo ''
  echo "Updating BREW"
  brew update &&
  echo ''
  echo "Upgrading BREW installs"
  brew upgrade &&
  echo ''
  echo "Cleaning up BREW"
  brew cleanup &&
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

function upgrade_yarn() {
  yarn set version stable
}
alias yarn_upgrade="upgrade_yarn"
alias yarn_upg="upgrade_yarn"

function yarnup() {
  yarn upgrade-interactive
}
alias yarnui="yarnup"

function update() {
  echo "Upgrading ALL the things..."
  update_rbenv &&
  upgrade_brew &&
  updateNVM &&
  upgrade_node_lts &&
  # upgrade_node &&
  echo ''
  echo "All updates complete!!"
}

function fixchrome () {
  echo 'Running command sudo xattr -r -d com.apple.quarantine $(which chromedriver)'
  echo 'You might be assed to provide your system password'
  sudo xattr -r -d com.apple.quarantine $(which chromedriver)
  echo 'Done!'
}
alias fixcd="fixchrome"
alias chromefix="fixchrome"

function install_nvm () {
  echo '' &&
  echo "Installing NVM (Node Version Manager)" &&  (
    git clone https://github.com/nvm-sh/nvm.git "$NVM_DIR"
    cd "$NVM_DIR"
    git checkout `git describe --abbrev=0 --tags --match "v[0-9]*" $(git rev-list --tags --max-count=1)`
  ) && \. "$NVM_DIR/nvm.sh"
}

function updateNVM() {
  echo '' &&
  echo "Updating NVM (Node Version Manager)" && (
    cd "$NVM_DIR" &&
    git fetch --tags origin &&
    git checkout `git describe --abbrev=0 --tags --match "v[0-9]*" $(git rev-list --tags --max-count=1)`
  ) && \. "$NVM_DIR/nvm.sh" && echo "NVM update complete."
}
alias unvm="updateNVM"
alias nvmu="updateNVM"
alias nvmupdate="updateNVM"
alias nvm_update="updateNVM"
alias nvmlist="nvm ls-remote"

function reload_nvm () {
  cd $HOME && echo '' && echo 'Deleting and reinstalling NVM. Please wait...'
  rm -rf $HOME/.nvm &&
  install_nvm &&
  echo 'Done!'
}

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
```

<br/>

[Home](README.md) | [Custmize the Ubuntu Environment](https://github.com/scott-knight/linux-on-windows-11/blob/main/customize-the-ubuntu-environment.md)
