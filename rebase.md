# rebase

下記のようなコミット状況だとする

```sh
git log --oneline
705d2fa (HEAD -> main) +3 before rebase
46a3e0a +2 before rebase
4a00e4c +1 before rebase
4ea52f3 :tada: init
```

下記のcommitを一つのコミットにしたい

- `705d2fa`
- `46a3e0a`
- `4a00e4c`

```sh
git rebase -i HEAD~3
```

下記のようなエディタ画面が出力される

```gitrebase
pick 4a00e4c +1 before rebase
pick 46a3e0a +2 before rebase
pick 705d2fa +3 before rebase

# Rebase 4ea52f3..705d2fa onto 4ea52f3 (3 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup <commit> = like "squash", but discard this commit's log message
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified). Use -c <commit> to reword the commit message.
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
```

最初のコミット(`4a00e4c`)に後のコミット2つをまとめる

`reword`(`r`)
`fixup`(`f`)だとコミットメッセージが最初のコミットメッセージになる(後のコミットメッセージは破棄される)

```diff
- pick 4a00e4c +1 before rebase
+ r 4a00e4c +1 before rebase
- pick 46a3e0a +2 before rebase
+ f 46a3e0a +2 before rebase
- pick 705d2fa +3 before rebase
+ f 705d2fa +3 before rebase

# Rebase 4ea52f3..705d2fa onto 4ea52f3 (3 commands)
```

下記のようなエディタが表示される

```gitcommit
+1 before rebase

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# Date:      Mon Sep 28 18:01:21 2020 +0900
#
# interactive rebase in progress; onto 4ea52f3
# Last command done (1 command done):
#    reword 4a00e4c +1 before rebase
# Next commands to do (2 remaining commands):
#    fixup 46a3e0a +2 before rebase
#    fixup 705d2fa +3 before rebase
# You are currently editing a commit while rebasing branch 'main' on '4ea52f3'.
```


```diff
- +1 before rebase
+ +1 after rebase
```

一つになった

コミットハッシュは変更される

```sh
git log --oneline
e7e2fdd (HEAD -> main) +1 after rebase
4ea52f3 :tada: init
```
