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
  echo '******  Riskonnect Aliases **************************************'
  echo "PROJDIR    = $PROJDIR"
  echo "alias devprj = cd $PROJDIR"
  echo "alias dev    = devprj && ll"
}
alias proji=projinfo

####### LLM Engineering #######
LLME=$PROJDIR/llm_engineering
alias llme="cd $LLME"
alias llmeact="llme && conda activate llms"
alias llmej="llmeact && jupyter lab"

function llmeinfo () {
  echo ''
  echo '******  Riskonnect Aliases **************************************'
  echo "LLME          = $PROJDIR/llm_engineering"
  echo "alias llme    = cd $LLME"
  echo "alias llmeact = llme && conda activate llms"
  echo "alias llmej   = llmeact && jupyter lab"
}
alias llmei=llmeinfo
```

<br>

[Home](README.md) | [Custmize the Ubuntu Environment](https://github.com/scott-knight/linux-on-windows-11/blob/main/customize-the-ubuntu-environment.md)
