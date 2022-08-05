# linux and bash essentials

## get linux version
cat /etc/issue

## assign default value to a var if not defined
`FOO="${MYVAR:=default}"`  If MYVAR not set or null, set it to default.
note : 
`MYVAR="${MYVAR:=default}"` is valid, hence can be use with externaly exported vars

## result of a command in a var
myvar=$(command here)

## use a counter
mycounter=0
mycounter=$((mycounter+1))

note : here ((...)) is used for artihmetic expansion (see alos let command)

## grep some char before/after
