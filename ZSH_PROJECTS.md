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
