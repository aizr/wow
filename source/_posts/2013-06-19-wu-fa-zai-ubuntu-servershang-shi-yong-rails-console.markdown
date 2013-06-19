---
layout: post
title: "無法在ubuntu server上使用 Rails console"
date: 2013-06-19 10:54
comments: true
categories:
---

又遇到一個小小的問題, 但會整死你的問題！ 讓我想到牙痛不是病, 痛起來要人命.......

在自己的Production Server上 , 無意中要開啟 Rails console的時候卻出現無法開啟的error log!

問了Google大神找到解決方法, 可以先使用看看production ENV,    RAILS_ENV=production rails c

看看在production的環靜變數下是否能正常運作！ 可以的話就把這個變數值放進.bashrc裡！

    $ echo 'export RAILS_ENV=production' >> .bashrc

就是這樣囉...