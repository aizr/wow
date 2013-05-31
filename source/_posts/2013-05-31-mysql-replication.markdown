---
layout: post
title: "mysql replication"
date: 2013-05-31 15:00
comments: true
categories: 
---
<p>MySQL 設定寫入 Master 後, 自動 Replication 到 Slave 去, 運作基本原理是: <br />

INSERT/UPDATE/DELETE 語法, 自動寫入 Master 的 binlog file.<br />
由 GRANT REPLICATION 授權的帳號, 自動將 SQL 語法 repl 到 Slave 的 DB 執行.<br />
因而完成 Replication 的動作.<br />

下述詳細可見: MySQL 5.0 Reference Manual :: 15 Replication
</p>

1.設定 Replication 的操作 (Master)<br />
<li>打開my.cnf</li>
    $ sudo vim /etc/mysql/my.cnf

<li> 下面是 Debian Linux 的設定, 找到下面的設定, 新增/修改 成下面這樣子.</li>

    bind-address            = 127.0.0.1
    server-id               = 1
    log_bin                 = /var/log/mysql/mysql-bin.log

<li>若是 innodb, 且有用 transaction 的話, 需再加入下面兩行<li>
    
    innodb_flush_log_at_trx_commit=1
    sync_binlog=1

<li>重新啟動mysql和進入mysql設定repl帳號密碼</li>
    $ sudo /etc/init.d/mysql restart
    $ mysql -u root -p

    mysql> GRANT REPLICATION SLAVE ON *.* TO 'repl'@'%' IDENTIFIED BY 'repl_pass';` # 先假設 帳號 repl, 密碼 repl

<li>鎖定DB</li>
    mysql> FLUSH TABLES WITH READ LOCK; 先讓 DB 不要再寫資料進去.
    mysql> SHOW MASTER STATUS; 這邊要記好 File & Position, 等一下設定 Slave 要用.
    +----------------------+------------+------------------+----------------------+
    | File                 | Position   | Binlog_Do_DB     | Binlog_Ignore_DB     |
    +----------------------+------------+------------------+----------------------+
    | mysql-bin.000014     |      232   |                  |                      |
    +----------------------+------------+------------------+----------------------+
<li>離開</li>
    mysql> quit

<li>以下是幾種倒DB的方法：</li>
    $ mysqldump -u root -p DB > dbdump.sql
    $ mysqldump --all-databases --lock-all-tables >dbdump.sql
    $ mysqldump --all-databases --master-data >dbdump.sql
<li>解除唯讀</li>    
    myql> UNLOCK TABLES;  dump 完資料後, 進去 mysql 解除唯讀

<p>再來就是將 dbdump.sql scp 到 Slave 去即可.
Master 就到此為止</P>

2.設定 Replication 的操作 (Slave)

    $ sudo vim /etc/mysql/my.cnf
    server-id               = 2  # server-id 不能與其它機器相同
    log_bin                 = /var/log/mysql/mysql-bin.log

    $ mysql -u root -p # 進入 mysql
    mysql> create database DBNAME;
    mysql> use DBNAME; source dbdump.sql; 或 $ mysql -u root -p DBNAME < dbdump.sql
    mysql> CHANGE MASTER TO
    MASTER_HOST='MASTER_HOSTNAME',
    MASTER_USER='repl',
    MASTER_PASSWORD='repl_pass',
    MASTER_LOG_FILE='mysql-bin.000014',
    MASTER_LOG_POS=232; # 這邊就要用到之前 Master 抄下來的值.

<li>開始Reolication</li>
    mysql> START SLAVE;  這樣子就會開始 Replication !

    mysql> show master status; 檢查一下設定！

    mysql> show slave status;  檢查一下設定, 看是不是有異常狀況.


