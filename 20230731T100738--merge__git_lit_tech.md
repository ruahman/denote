---
title:      "merge"
date:       2023-07-31T10:07:38-04:00
tags:       ["git", "lit", "tech"]
identifier: "20230731T100738"
---

merge a branch to your current branch
``` shell
git merge <branch>
```

to abort the merge

``` shell
git merge --abort
```

resolve conflicts

``` shell
git add <file>
```

`git config mergetool nvim`
- setup nvim as merge tool

`git config mergetool.nvim.cmd 'nvim -d -c "wincmd l" -c "norm ]c"' "$LOCAL" "$MERGED" "$REMOTE"`
-c `nvim -d` opens in diffmode
-c "windcmd l" :: <c-w> l
-c "norm ]c" :: move to first change
- "$LOCAL" "$MERGED" "$REMOTE" :: defines arragemetn of displayed diffs 

:diffget local  :: merge local changes
:diffget remote :: merge remote change

]c :: go to next change

:diff RE :: get from remotge
:diff BA :: get from base
:diff LO :: fet from loacal

]c  :: next change
[c  :: prev change
