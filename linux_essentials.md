# linux and bash essentials

A set of usefull commands and technics. Mostly used in a wsl or vm env.

## get linux version
`cat /etc/issue` or  
alternative : `hostnamectl`

## create user with home (bonus add it to sudo)

connected with a sudoer : 
create user k1001: 
```
 sudo useradd -k k1001
 sudo passwd k1001
```
note :
- I prefer std useradd vs perl adduser 
- `-k` allow creation of home directory with predefined skeleton  if exists (else equivalent to -m)
 
add user to sudo/admin:
```
  adduser username admin
  adduser username root
 ```
disconnect/reconnect as k1001

run a command as another user (two syntax):
```
sudo runuser -l  userNameHere -c 'command arg1 arg2'
sudo runuser -u user -- command1 arg1 arg2
``` 

## switch to azerty keyboard
`loadkeys fr` (run as root)  

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
```
mycounter=0
mycounter=$((mycounter+1))
```

note : here ((...)) is used for artihmetic expansion (see alos let command)

## grep some char before/after
(todo)

## split file in column
cat or grep your file and pipe to awk  
awk default separator is 'space', 'tab' or 'newline'
exemple : `grep $pattern mylogs.log | awk '{printf "%s %s\n", $1,$NF}'`
this will print first and last column of the result of the pattern grep on mylogs

## WSL clock drift

Often, after sleep mode you wsl clock is out of sync, so any command that use a certificate will fail.  
run : `sudo hwclock -s`  to reset it.
