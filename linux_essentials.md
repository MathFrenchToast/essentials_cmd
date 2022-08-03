# linux essentials

# get linux version
cat /etc/issue

# assign default value to a var if not defined
`FOO="${MYVAR:=default}"`  If MYVAR not set or null, set it to default.
note : 
`MYVAR="${MYVAR:=default}"` is valid, hence can be use with externaly exported vars

# grep some char before/after
