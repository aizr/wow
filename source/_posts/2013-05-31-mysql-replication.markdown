---
layout: post
title: "mysql replication"
date: 2013-05-31 15:00
comments: true
categories: 
---
<p>MySQL 設定寫入 Master 後, 自動 Replication 到 Slave 去, 運作基本原理是:

INSERT/UPDATE/DELETE 語法, 自動寫入 Master 的 binlog file.
由 GRANT REPLICATION 授權的帳號, 自動將 SQL 語法 repl 到 Slave 的 DB 執行.
因而完成 Replication 的動作.

下述詳細可見: MySQL 5.0 Reference Manual :: 15 Replication
</p>
<li>1.設定 Replication 的操作 (Master)</li>
