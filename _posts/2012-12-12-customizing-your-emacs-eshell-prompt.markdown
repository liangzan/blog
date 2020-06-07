---
layout: post
title: "Customizing your Emacs Eshell prompt"
date: 2012-12-12 05:38
comments: true
categories:
---

If you use the shell in Emacs and you are not already using [eshell](http://emacswiki.org/emacs/CategoryEshell), convert to eshell now! Eshell is the best terminal emulator available in Emacs. If you need convincing, please read [this article from Mastering Emacs](http://www.masteringemacs.org/articles/2010/12/13/complete-guide-mastering-eshell/). Using __grep__ in eshell is the killer feature for me. What the Master Emacs article didn't elaborate is how to customize your eshell prompt. I'm going to share how to add colors and show the Git branch on your eshell prompt. This is how my prompt look like currently.

![Eshell prompt](https://s3.amazonaws.com/static.liangzan.net/blog/eshell-prompt.png)

<!-- more -->

## Adding colors

To add colors we need to change the property of the text. We make use of the __propertize__ function. The snippet below changes the string to be green.

``` scheme
(propertize "some-string" 'face `(:foreground "green"))
```

You can take a look at the available styles you can apply to text by simply running __M-x list-faces-display__.

## Showing the Git branch name

To get the current branch name from your Git repository, we make use of the function below. I copied this from [Jim Mernard's](http://www.jimmenard.com/) emacs [configuration](https://github.com/jimm/elisp/blob/master/eshell-customize.el).

``` scheme
(defun curr-dir-git-branch-string (pwd)
  "Returns current git branch as a string, or the empty string if
PWD is not in a git repo (or the git command is not found)."
  (interactive)
  (when (and (eshell-search-path "git")
             (locate-dominating-file pwd ".git"))
    (let ((git-output (shell-command-to-string (concat "cd " pwd " && git branch | grep '\\*' | sed -e 's/^\\* //'"))))
      (concat "["
              (if (> (length git-output) 0)
                  (substring git-output 0 -1)
                "(no branch)")
              "]")
      )))
```

## Installing

I customized Jim's code to add colors. The end result look like this

``` scheme
(setq eshell-history-size 1024)
(setq eshell-prompt-regexp "^[^#$]*[#$] ")

(load "em-hist")			; So the history vars are defined
(if (boundp 'eshell-save-history-on-exit)
    (setq eshell-save-history-on-exit t)) ; Don't ask, just save
;(message "eshell-ask-to-save-history is %s" eshell-ask-to-save-history)
(if (boundp 'eshell-ask-to-save-history)
    (setq eshell-ask-to-save-history 'always)) ; For older(?) version
;(message "eshell-ask-to-save-history is %s" eshell-ask-to-save-history)

(defun eshell/ef (fname-regexp &rest dir) (ef fname-regexp default-directory))


;;; ---- path manipulation

(defun pwd-repl-home (pwd)
  (interactive)
  (let* ((home (expand-file-name (getenv "HOME")))
	 (home-len (length home)))
    (if (and
	 (>= (length pwd) home-len)
	 (equal home (substring pwd 0 home-len)))
	(concat "~" (substring pwd home-len))
      pwd)))

(defun curr-dir-git-branch-string (pwd)
  "Returns current git branch as a string, or the empty string if
PWD is not in a git repo (or the git command is not found)."
  (interactive)
  (when (and (eshell-search-path "git")
             (locate-dominating-file pwd ".git"))
    (let ((git-output (shell-command-to-string (concat "cd " pwd " && git branch | grep '\\*' | sed -e 's/^\\* //'"))))
      (propertize (concat "["
              (if (> (length git-output) 0)
                  (substring git-output 0 -1)
                "(no branch)")
              "]") 'face `(:foreground "green"))
      )))

(setq eshell-prompt-function
      (lambda ()
        (concat
         (propertize ((lambda (p-lst)
            (if (> (length p-lst) 3)
                (concat
                 (mapconcat (lambda (elm) (if (zerop (length elm)) ""
                                            (substring elm 0 1)))
                            (butlast p-lst 3)
                            "/")
                 "/"
                 (mapconcat (lambda (elm) elm)
                            (last p-lst 3)
                            "/"))
              (mapconcat (lambda (elm) elm)
                         p-lst
                         "/")))
          (split-string (pwd-repl-home (eshell/pwd)) "/")) 'face `(:foreground "yellow"))
         (or (curr-dir-git-branch-string (eshell/pwd)))
         (propertize "# " 'face 'default))))

(setq eshell-highlight-prompt nil)
```

To install, I put the code above in a new file called __eshell_customizations.el__. Then I loaded the script from my __init.el__.

``` scheme
(load "~/.emacs.d/scripts/eshell-customizations.el")
```

Restart your emacs and your eshell should look better now.
