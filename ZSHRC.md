[Home](README.md) | [Customize Ubuntu](customize-ubuntu.md#copy-the-zshrc-content)

<br/>

# .zshrc

```zsh
#! /bin/zsh

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
SPACESHIP_PROMPT_ASYNC=false

# BREW
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
export BREW_OPT_PATH="/home/linuxbrew/.linuxbrew/opt"

# .local/bin -- used with letter_opener
export PATH="${HOME}/.local/bin:$PATH"
export BROWSER="wslopen"

# RBENV
export RBENV_ROOT="${HOME}/.rbenv"
export PATH="${RBENV_ROOT}/bin:${RBENV_ROOT}/shims:$PATH"
if which rbenv > /dev/null; then eval "$(${RBENV_ROOT}/bin/rbenv init - zsh)"; fi

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

# NVM
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"

autoload -U add-zsh-hook
load-nvmrc() {
  local nvmrc_path
  nvmrc_path="$(nvm_find_nvmrc)"
  if [ -n "$nvmrc_path" ]; then
    local nvmrc_node_version
    nvmrc_node_version=$(nvm version "$(cat "${nvmrc_path}")")
    if [ "$nvmrc_node_version" = "N/A" ]; then
      nvm install
    elif [ "$nvmrc_node_version" != "$(nvm version)" ]; then
      nvm use
    fi
  elif [ -n "$(PWD=$OLDPWD nvm_find_nvmrc)" ] && [ "$(nvm version)" != "$(nvm version default)" ]; then
    echo "Reverting to nvm default version"
    nvm use default
  fi
}
add-zsh-hook chpwd load-nvmrc
load-nvmrc

# load files
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

ZPROJECTS=$HOME/.zsh_projects
if [ -f "$ZPROJECTS" ]
then
  source "$ZPROJECTS"
  echo "using ${ZPROJECTS}"
else
  echo "not sure what happened with ${ZPROJECTS}"
fi

# Keychain
eval $(keychain --eval $HOME/.ssh/id_ed25519)

source $(brew --prefix)/opt/spaceship/spaceship.zsh
source $HOME/.zsh/zsh-autosuggestions/zsh-autosuggestions.zsh
source $HOME/.zsh/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh

```

<br/>

[Home](README.md) | [Customize Ubuntu](customize-ubuntu.md#copy-the-zshrc-content)
