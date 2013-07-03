---
layout: post
title: "Git 修正已commit的內容"
date: 2013-07-03 11:33
comments: true
categories:
---

很多時候可能手誤或其它原因把程式給commit上去了, 更慘的是還push到遠端的機器上！

Git 的指令很多, 但就是不知道如何拼湊才能正確的修改已經commit的錯誤......

今日找到以下的方法,可以試試！

首先在本地端可以先下

    ＄git log

找出下列這段

    commit 6b0832665f91cb102f9

這是git commit 後產生的識別代碼,請複製一下！

再將識別碼貼進下列command line

    ＄git rebase -i <commit 識別代碼>

如此就會進入編輯程序囉～ 大致上會長得像下面的那樣子.....

    1 pick 6b08326 new post
    2
    3 # Rebase d65e784..6b08326 onto d65e784
    4 #
    5 # Commands:
    6 #  p, pick = use commit
    7 #  r, reword = use commit, but edit the commit message
    8 #  e, edit = use commit, but stop for amending
    9 #  s, squash = use commit, but meld into previous commit
    10 #  f, fixup = like "squash", but discard this commit's log message
    11 #  x, exec = run command (the rest of the line) using shell
    12 #
    13 # These lines can be re-ordered; they are executed from top to bottom.
    14 #
    15 # If you remove a line here THAT COMMIT WILL BE LOST.
    16 # However, if you remove everything, the rebase will be aborted.
    17 #
    18 # Note that empty commits are commented out

有關操作指令的解釋如下：

* pick = 使用這個 commit,不做更動.
* reword = 使用這個 commit ，但要更改 commit message.
* edit = 使用這個 commit，但要更動 commit 的內容.
* squash = 使用這個 commit，並與前面那個commit合併，並保 commit messages.
* fixup = squash + 只使用前面那條 commit 的 message ，捨棄這條 message
* exec = 執行一條指令

然後將檔案儲存, 再來就可以將commit 的內容倒出來修正了！

    $ git reset --HEAD^ #將所有comit的內容倒出來.

修正完畢後將修正完的檔案加入git暫存區

    $ git add <file name>
    $ git commit -a -c -ORIG_HEAD #如果不想重新commit ,可以使用舊的commit訊息.

現在都做完修正也commit了,再來就是

    $ git rebase --continue

git 會透過 commit --amend 一起重新commit, 接下來就是push到遠端囉！

    $ git push -f origin master # -f 是強制push

這樣就大功告成了！