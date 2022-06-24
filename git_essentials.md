# Git Essentials

## start a project localy
`mkdir <<project name>>`  
`cd <<project name>>`  
`git init`  

## push new local project to remote
choose namespace (aka group) and project name
be sure to have added and commit your files first
`git push --set-upstream https://gitlab.example.com/namespace/myproject.git master`

## clone a repo
`git clone <<repourl>>`

## checkout master
`git checkout master`

## create a new branch

`git branch <<branch name>>`  
`git checkout <<branch name>>`  
or all in one
`git checkout -b <<branch name>>`

we can create a branch from a given commit : `git branch <<branche name>> <<commit id>>`  
or from a given tag : `git branch <<branche name>> <<tag>>`

## make a commit
add files to stage area first
`git add <<filename>>` or `git add --all` at the root of the project
then 
`git commit -m "<<commit message>>"`

## list all branches and current branch
`git branch`
list also the remote
`git branch -r`

## get status 
`git status`

## check upstream
`git remote show origin`

## push new local branch to remote
`git push -u origin <<branch name>>`

## make master uptodate
*TODO check if there is no easier method*
make master uptodate  
`git fetch --all`  
`git checkout master`  
`git pull --rebase origin master`

## merge master into a new local branch 
make master uptodate  (see above)  
switch to branch and rebase from master (branch commit will be appended)  
`git checkout <<branch name>>`  
`git rebase master`  

## merge master into an existing remote branch
make master uptodate  (see above)  
switch to branch and merge master into branch  
`git merge master`
if merge failed, and is to hard to debunk
`git merge --abort` to revert the merge

##  delete branch no longer on remote
prune branch no longer on remote  
`git fetch --prune` or `git remote prune`
list branch no longer tracked 
`git branch --v | grep "\[gone\]"`  
delete branch one by one  
`git branch -D <<branch name>>`

all in one`git branch --v | grep "\[gone\]" | awk '{print $1}' |  xargs git branch -D`

## resole conflicts
list files in conflict (shell only)  
`grep -lr '<<<<<<<' .`  
accepting theirs    
`git checkout --theirs <<filename>>`  
accepting mine  
`git checkout --ours <<filename>>`
then add the files updated  
`git add <<filename>>`
probably need to continue a rebase
`git rebase --continue`
or abort it
`git rebase --abort`

## reset your branch to remote master (or branch)
get all branch info from remote
`git fetch --all`
then reset to master  
`git reset --hard origin/master`
or if for another branch  
`git reset --hard origin/<branch_name>`

## create a tag
Annotated tag (an entry in history)
`git tag -a <<tag name>> -m "<<tag message>>"`  
Ligthweight tag (just a pointer to a commit)
`git tag <<tag name>>` 
To checkout a tag in a branch
`git checkout tags/<tag> -b <branch>`

## amend last local commit
`git commit --amend`  
an editor open, change main message (lines starting with # will be ignored) save & close.  
check everything is ok with `git log`

## git log since 6 days
for a given branch
```
git checkout <<branch_name>>
git pull <<branch_name>>
git log <<branch_name>> --no-merges  --since="6 day ago" 
```

## rename a branch
`git branch -m <oldname> <newname>`

## commit on wrong branch
on wrong branch_name
`git reset HEAD~1`
then go to new branch
then add and commit

## push a local folder as a new remote repo
for a local repo 
first create on Gitlab an empty project
cd in the directory
then 
`git init`
`git remote add origin {here you git url}`
`git add .`
`git commit -m "Initial commit"
`git push -u origin master`

for a remote repo see :
https://superuser.com/questions/1412078/bring-a-local-folder-to-remote-git-repo/1412081

## clone a whole group at once
create a personnal access token with api access (in gitlab : 'user settings > access tokens')
get groupId in web portal (in gitlab it is disaplyed below group name)
in bash (with jq installed) :
```
TOKEN="yourtoken"
for repo in $(curl -s --header "PRIVATE-TOKEN: $TOKEN" https://<your-host>/api/v4/groups/<group_id> | jq -r ".projects[].ssh_url_to_repo"); do git clone $repo; done;
```

To include subgroups add include_subgroups=true query param like
To request clone with  http url use : http_url_to_repo instead of ssh_url_to_repo



