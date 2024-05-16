---
title:      "branches"
date:       2023-08-01T13:03:39-04:00
tags:       ["git", "lit", "tech"]
identifier: "20230801T130339"
---

to checkout a remote branch in git

first setup the remote repository

`git remote add <remote> <url>`

then fetch the remote

`git fetch <remote>`

the checkout remote branch

`git checkout -b <branch>  <remote>/<branch>`

ex: `git checkout -b feature-branch origin/feature-branch`

if you want to push a branch upstream

`git push -u <remote> <branch>`
ex: `git push -u origin my-feature-branch`
or
`git push --set-upstream <remote> <branch>`

to delete a branch
`git branch -d <branch-name>`

to delete branch remotly
`git push <remote> --delete <branch-name>`

rename branch
`git branch -m <old-branch> <new-branch>`
