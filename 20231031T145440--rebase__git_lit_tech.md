---
title:      "rebase"
date:       2023-10-31T14:54:40-04:00
tags:       ["git", "lit", "tech"]
identifier: "20231031T145440"
---

merge vs rebase
* change history
  - makes history more linear

squash commits
- squiz all feature branch commit to a single commit

git merge
- ties a merge commit
`git merge <branch-you-want-to-merge>`


git rebase

`git rebase my_feature_branch`


# ammend #
ammend files to a commit
`git commit --amend --no-edit`

Interactive Rebase
==================

# reword # 
reword commit
`git rebase -i HEAD~2`
- interactive rebase
* write rewrite

# delete #
`git rebase -i HEAD~2`
- write drop

# reorder #
`git rebase -i HEAD~2`
- you can reorder commit with interact rebase

# squash #
`git rebase -i HEAD~3`
- fixup :: discard log messages and log to prevoous commit
- push changes up
- squash :: mesh commits into one
- fixup :: is like squash but it does not merge the commit logs but merges with the commit up
