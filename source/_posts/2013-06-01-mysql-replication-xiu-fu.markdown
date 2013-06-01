---
layout: post
title: "Mysql Replication 修復"
date: 2013-06-01 16:45
comments: true
categories: 
---
之前有遇到Replication table error, 有給它緊張到~ XD 

後來找到了問題, 因為Master and slave兩邊的資料有不同步的問題, 導致無法複製！

以下是簡易的處理方法：

    mysql> REPAIR TABLE table_name
    mysql> SET GLOBAL SQL_SLAVE_SKIP_COUNTER=1; 暫時跳過一筆資料
    mysql> START SLAVE; 重新啟動SLAVE

但執行後, 還是要找時間同步Master & Slave兩邊的資料喔~~~
