# 在 Ubuntu16.04 上安裝 Mysql Master Master Replication

先準備2台VM來裝Mysql

Name      |  Private IP 
----------|---------------
DB1       |  10.140.0.9 
DB2       |  10.140.0.10

## 2台DB都裝 MySQL Client
```
sudo apt-get update
sudo apt-get install mysql-server
```

## DB1設定

修改 /etc/my.cnf
```
[mysqld]

server_id = 1
log-bin = mysql-bin
replicate-wild-ignore-table=mysql.%
replicate-wild-ignore-table=information_schema.%
```

參數                                   |  描述
--------------------------------------|---------------------------------------------------------------
server_id = 1                         |  服務節點的id，需為唯一值，每一台db需不同
log-bin = mysql-bin                   |  開啟2進制log功能，mysql-bin是命名格式，會生成名为mysql-bin.000001
replicate-wild-ignore-table = mysql.% |  不同步這個db或table(可寫多行)，這邊不同步mysql下的table，因為不想同步user
slave-skip-errors = all               |  跳過同步時會遇到的錯誤，避免同步中斷
auto-increment-increment = 2          |  跳過同步時會遇到的錯誤，避免同步中斷
auto-increment-offset = 1             |  有用auto_increment時，從1開始增加(偏移量)
auto-increment-offset = 2             |  有用auto_increment時，每次加2(增長值)
expire_logs_days = 10                 |  log保留10天

若有2台db想要auto_increment不衝突時，可以這樣設
DB1                             |  DB2
--------------------------------|--------------------------------------
auto-increment-offset = 1       |   auto-increment-offset = 2
auto-increment-offset = 2       |   auto-increment-offset = 2
1,3,5,7,9這樣產生id              |   2,4,6,8,10這樣產生id

重啟mysql
```
sudo service mysql restart
```

建立同步用的user
```
mysql> CREATE USER 'rep'@'%' IDENTIFIED BY 'asdf1234';
mysql> GRANT REPLICATION SLAVE ON *.* TO 'rep'@'%';
mysql> flush privileges;
```
查看目前Binlog的Position，等下會用到
```
mysql> show master status;
+------------------+----------+--------------+------------------+-------------------+
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
+------------------+----------+--------------+------------------+-------------------+
| mysql-bin.000002 |      154 |              |                  |                   |
+------------------+----------+--------------+------------------+-------------------+
1 row in set (0.00 sec)
```

## DB2設定

修改 /etc/my.cnf
```
[mysqld]

server_id = 2
log-bin = mysql-bin
replicate-wild-ignore-table=mysql.%
replicate-wild-ignore-table=information_schema.%
```

重啟mysql
```
sudo service mysql restart
```

建立同步用的user
```
mysql> CREATE USER 'rep'@'%' IDENTIFIED BY 'asdf1234';
mysql> GRANT REPLICATION SLAVE ON *.* TO 'rep'@'%';
mysql> flush privileges;
```

查看目前Binlog的Position，等下會用到
```
mysql> show master status;
+------------------+----------+--------------+------------------+-------------------+
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
+------------------+----------+--------------+------------------+-------------------+
| mysql-bin.000002 |      154 |              |                  |                   |
+------------------+----------+--------------+------------------+-------------------+
1 row in set (0.00 sec)
```

## DB1啟動同步

DB1啟動同步
```
mysql> stop slave;
mysql> change master to master_host = '10.140.0.10' , master_user = 'rep' , master_password = 'asdf1234', master_log_file = 'mysql-bin.000002' ,  master_log_pos = 154, master_port = 3306;
mysql> start slave;
```

DB1查看同步狀態(Slave_IO_State: Waiting for master to send event就是有連上master等同步資料)
```
mysql> show slave status\G ;
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: 10.140.0.10
                  Master_User: rep
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.000002
          Read_Master_Log_Pos: 154
               Relay_Log_File: db-master1-relay-bin.000004
                Relay_Log_Pos: 367
        Relay_Master_Log_File: mysql-bin.000002
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB: 
          Replicate_Ignore_DB: 
           Replicate_Do_Table: 
       Replicate_Ignore_Table: 
      Replicate_Wild_Do_Table: 
  Replicate_Wild_Ignore_Table: 
                   Last_Errno: 0
                   Last_Error: 
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 154
              Relay_Log_Space: 745
              Until_Condition: None
               Until_Log_File: 
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File: 
           Master_SSL_CA_Path: 
              Master_SSL_Cert: 
            Master_SSL_Cipher: 
               Master_SSL_Key: 
        Seconds_Behind_Master: 0
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 0
                Last_IO_Error: 
               Last_SQL_Errno: 0
               Last_SQL_Error: 
  Replicate_Ignore_Server_Ids: 
             Master_Server_Id: 2
                  Master_UUID: 1893f19d-5b60-11e9-b225-42010a8c000a
             Master_Info_File: /var/lib/mysql/master.info
                    SQL_Delay: 0
          SQL_Remaining_Delay: NULL
      Slave_SQL_Running_State: Slave has read all relay log; waiting for more updates
           Master_Retry_Count: 86400
                  Master_Bind: 
      Last_IO_Error_Timestamp: 
     Last_SQL_Error_Timestamp: 
               Master_SSL_Crl: 
           Master_SSL_Crlpath: 
           Retrieved_Gtid_Set: 
            Executed_Gtid_Set: 
                Auto_Position: 0
         Replicate_Rewrite_DB: 
                 Channel_Name: 
           Master_TLS_Version: 
1 row in set (0.00 sec)
```

## DB2啟動同步

DB2啟動同步
```
mysql> stop slave;
mysql> change master to master_host = '10.140.0.9' , master_user = 'rep' , master_password = 'asdf1234', master_log_file = 'mysql-bin.000002' ,  master_log_pos = 154, master_port = 3306;
mysql> start slave;
```

DB2查看同步狀態(Slave_IO_State: Waiting for master to send event就是有連上master等同步資料)
```
mysql> show slave status\G ;
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: 10.140.0.9
                  Master_User: rep
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.000002
          Read_Master_Log_Pos: 154
               Relay_Log_File: db-master2-relay-bin.000005
                Relay_Log_Pos: 320
        Relay_Master_Log_File: mysql-bin.000002
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB: 
          Replicate_Ignore_DB: 
           Replicate_Do_Table: 
       Replicate_Ignore_Table: 
      Replicate_Wild_Do_Table: 
  Replicate_Wild_Ignore_Table: 
                   Last_Errno: 0
                   Last_Error: 
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 154
              Relay_Log_Space: 532
              Until_Condition: None
               Until_Log_File: 
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File: 
           Master_SSL_CA_Path: 
              Master_SSL_Cert: 
            Master_SSL_Cipher: 
               Master_SSL_Key: 
        Seconds_Behind_Master: 0
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 0
                Last_IO_Error: 
               Last_SQL_Errno: 0
               Last_SQL_Error: 
  Replicate_Ignore_Server_Ids: 
             Master_Server_Id: 1
                  Master_UUID: fc33d5f2-5b5f-11e9-b397-42010a8c0009
             Master_Info_File: /var/lib/mysql/master.info
                    SQL_Delay: 0
          SQL_Remaining_Delay: NULL
      Slave_SQL_Running_State: Slave has read all relay log; waiting for more updates
           Master_Retry_Count: 86400
                  Master_Bind: 
      Last_IO_Error_Timestamp: 
     Last_SQL_Error_Timestamp: 
               Master_SSL_Crl: 
           Master_SSL_Crlpath: 
           Retrieved_Gtid_Set: 
            Executed_Gtid_Set: 
                Auto_Position: 0
         Replicate_Rewrite_DB: 
                 Channel_Name: 
           Master_TLS_Version: 
1 row in set (0.00 sec)
```

#測試同步

DB1
```
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| dbname             |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.01 sec)

mysql> create database testdb;
Query OK, 1 row affected (0.01 sec)
```

DB2
```
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| dbname             |
| mysql              |
| performance_schema |
| sys                |
| testdb             |
+--------------------+
6 rows in set (0.00 sec)
```

#預到問題
```
error connecting to master，- retry-time: 60 retries: 86400
```
設定同步後若發現2個db在連接有問題，可能是mysql的bind-address預設只讓本地端連造成的，修改 /etc/mysql/mysql.conf.d，把bind-address註解掉


```
Slave I/O: Fatal error: The slave I/O thread stops because master and slave have equal MySQL server UUIDs; these UUIDs must be different for replication to work. Error_code: 1593
```
可能因為data(/var/lib/mysql)目錄下有一個auto.cnf裡面紀錄的id相同，把每台mysql的auto.cnf砍掉就好了
