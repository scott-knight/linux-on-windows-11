[Home](README.md) | [Custmize the Ubuntu Environment](https://github.com/scott-knight/linux-on-windows-11/blob/main/customize-the-ubuntu-environment.md)

<br/>

# .zsh_projects
```zsh
#! /bin/zsh

PROJDIR=$HOME/dev.projects
alias devprj="cd $PROJDIR"
alias dev="devprj && ll"

function projinfo () {
  echo ''
  echo '******  PROJECT Aliases **************************************'
  echo "PROJDIR      = $PROJDIR"
  echo "alias devprj = cd $PROJDIR"
  echo "alias dev    = devprj && ll"
}
alias proji=projinfo

####### LLM Engineering #######
LLME=$PROJDIR/llm_engineering
alias llme="cd $LLME"
alias llms="llme && conda activate llms"
alias llmj="llms && jupyter lab"

function llminfo () {
  echo ''
  echo '******  LLM Engineering Aliases **************************************'
  echo "LLME       = $PROJDIR/llm_engineering"
  echo "alias llme = cd $LLME"
  echo "alias llms = llme && conda activate llms"
  echo "alias llmj = llms && jupyter lab"
}
alias llmi=llminfo
```

<br>

[Home](README.md) | [Custmize the Ubuntu Environment](https://github.com/scott-knight/linux-on-windows-11/blob/main/customize-the-ubuntu-environment.md)
