## make new remote link
git remote add origin <url>
git remote show 
git push -u origin master

## reset and delete cache if it failed to track changes:
git rm --cached path/to/file
git reset path/to/file

## go back/forward to a particular commit
git checkout <commit id>
## checkout to a new commit 
git checkout -b <new branch>
## go back to the latest commit 
git checkout <branch name>
## restore files to the last commit:
git restore <filename>

##git create new branch
git branch <new branch>

## make new tag:
git tag -a v1.4 -m "my version 1.4"
git push origin v1.5
## delete local tag:
git tag -d v1.4-lw
## delete tag on remote:
git push origin :refs/tags/v1.4-lw

## remove file/folder from git index but keep the files
git rm --cached mylogfile.log
## and for folder:
git rm --cached -r mydirectory

