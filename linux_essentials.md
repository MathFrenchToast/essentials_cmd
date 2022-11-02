# linux and bash essentials

## get linux version
`cat /etc/issue` or  
alternative : `hostnamectl`

## check port status
logged on the machine : `netstat -tuplen | grep {port_nb}`
from another machine : `nmap -p{port_nb} {server_ip}` e.g. : nmap -p3306 192.168.100.10

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
