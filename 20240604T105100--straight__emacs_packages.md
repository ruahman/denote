---
title:      "straight"
date:       2024-06-04T10:51:00-04:00
tags:       ["emacs", "packages"]
identifier: "20240604T105100"
---

straight is an alternative package manager that installs packages through Git.

make sure your using the development branch

`(setq straight-repository-branch "develop")`

this install straight in init.el

```elisp
;; Install straight.el
(defvar bootstrap-version)
(let ((bootstrap-file
       (expand-file-name "straight/repos/straight.el/bootstrap.el" user-emacs-directory))
      (bootstrap-version 6))
  (unless (file-exists-p bootstrap-file)
    (with-current-buffer
        (url-retrieve-synchronously
         "https://raw.githubusercontent.com/radian-software/straight.el/develop/install.el"
         'silent 'inhibit-cookies)
      (goto-char (point-max))
      (eval-print-last-sexp)))
  (load bootstrap-file nil 'nomessage))
```

Disable package.el in favor of straight.el
that way it will only load packages that were loaded through straight.el

`(setq package-enable-at-startup nil)`

use straight to install a package for just session

`M-x straight-use-package <RET> evil-commentary <RET>`

to use package during run time

`(straight-use-package 'evil-commentary)`

to update an installed package

` M-x straight-pull-package <RET> evil-commentary <RET> `

to create/update version lock file

` M-x straight-freeze-versions <RET> `

To install the versions of the packages specified in your version lockfiles, 

` M-x straight-thaw-versions ` 


To configure use-package to always use straight.el, 
use use-package to configure straight.el to turn on straight-use-package-by-default1:

```elisp
;; Configure use-package to use straight.el by default
(use-package straight
  :custom
  (straight-use-package-by-default t))
```
