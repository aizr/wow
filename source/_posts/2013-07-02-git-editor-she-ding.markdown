---
layout: post
title: "Git editor 設定"
date: 2013-07-02 18:52
comments: true
categories:
---

使用 Git rebase 的時候, Git 會自動使用系統內的編輯器！ 例如： vi , vim ......

預設的情形下有可能開不了編輯器, 輸入下列command line即可使用！

    git config --global core.editor "/usr/bin/vim"

要注意的是, 必需先知道自己的編輯器路徑, 所以先找路徑的command line

    $ which <編輯器名稱>