---
title:      "history"
date:       2023-07-31T11:28:56-04:00
tags:       ["git", "tech"]
identifier: "20230731T112856"
---

git amend

``` shell
git commit --amend --no-edit
```
- add to the previous commit

interactive rebase mode???

``` shell
git rebase -i HEAD~2
```
- reword, drop, fixup
  * you can squash the commits into one
