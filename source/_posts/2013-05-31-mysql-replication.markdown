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
1.設定 Replication 的操作 (Master)
`$ sudo vim /etc/mysql/my.cnf`
<p># 下面是 Debian Linux 的設定, 找到下面的設定, 新增/修改 成下面這樣子.</p>

`bind-address           = 127.0.0.1`
`server-id               = 1`
`log_bin                 = /var/log/mysql/mysql-bin.log`

