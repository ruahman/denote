---
title:      "undo"
date:       2023-07-31T11:55:44-04:00
tags:       ["git", "lit", "tech"]
identifier: "20230731T115544"
---

git revert
- undoes a commit and commits that change on top, but it dooes not reset your project upto that commit
- just pull out the commit from an individual commit
- reverts a single commit
- in revert commit do not go away, with reset commits go away
- undo a single commit but keeps commit in history
- if you want to undo a single change in your history
- undo the commit and make a new commit

``` shell
git revert <commit-hash>
```
- this does not remove the commit it just reverts the state of the repo to the point before the commit
- it shows in the log history that this commit was reverted
- undo a single commit

git reset
- resets your repo to the point of the hash commit
- if you want to bring you repo back to a point
- resets the entire repo to a point
- resets the head to a specified state
- move our chain of commit to that commit

``` shell
git reset <comitment-hash>
```
- the work directory stays the same

git reset --hard <commit-hash>
- also resets your working directroy
- this is a dangerous command because you won't be able to recommit your files
- undo commit and remove changes in working directory

git reset --soft <commit-hash>
- undo all the commits but will save the changes on stage

git reset --mixed <commit-hash>
- undo all the commits but will save the changes on working directory
- this is the default

git reset <filename>
- unstage a file that was commited

git reset
- unstage all the files

git reset --hard
- unstage and remove changes from working directory

you can't undo a reset command once you done it

git stash
- remove current changes from current directory but save the for future use

git stash save <name>
- stash it with a name

git stash pop
- put changes back to working directory from stash

git stash list
- list your stashes

`git checkout <file>`
- discard local changes

`git checkout .`
- discard all changes in working directory



git log --graph --oneline -decorate


git diff --staged
- diff what is in stage
