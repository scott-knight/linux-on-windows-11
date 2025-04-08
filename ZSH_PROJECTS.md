[Home](README.md) | [Custmize the Ubuntu Environment](https://github.com/scott-knight/linux-on-windows-11/blob/main/customize-the-ubuntu-environment.md)

<br/>

# .zsh_alias_list

```zsh
#! /bin/zsh

PROJDIR=$HOME/projects
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
```

<br>

[Home](README.md) | [Custmize the Ubuntu Environment](https://github.com/scott-knight/linux-on-windows-11/blob/main/customize-the-ubuntu-environment.md)
