# 在 Ubuntu16.04 上安裝 ProxySQL

## 安裝 MySQL Client
```
sudo apt-get update
sudo apt-get install mysql-client
```

## 安裝 ProxySQL

先在tmp資料夾下載ProxySQL的deb檔再安裝，最新的ProxySQL在這https://github.com/sysown/proxysql/releases
```
cd /tmp
curl -OL https://github.com/sysown/proxysql/releases/download/v2.0.2/proxysql_2.0.2-ubuntu16_amd64.deb
sudo dpkg -i proxysql_*
rm proxysql_*
sudo systemctl start proxysql
systemctl status proxysql
```

## 設定 ProxySQL
用mysql客户端連接ProxySQL的管理接口，並把Percona的結點IP寫入
```
mysql -uadmin -padmin -h127.0.0.1 -P6032 --prompt 'admin> '
insert into mysql_servers(hostgroup_id,hostname,port) values (10,'10.40.1.199',3306),(10,'10.40.1.202',3306),(10,'10.40.1.203',3306);
load mysql servers to runtime;
save mysql servers to disk;
```
在Percona的任一結點建立monitor user
```
create user monitor@'10.40.1.%' identified by 'P@ssword1!';
grant replication client on *.* to monitor@'10.40.1.%';
```
在ProxySQL的設定monitor user
```
set mysql-monitor_username='monitor';
set mysql-monitor_password='P@ssword1!';
load mysql variables to runtime;
save mysql variables to disk;
```
在Percona的任一結點建立一個供ProxySQL連結的用戶
```
create user root@'10.40.1.%' identified by 'P@ssword1!';
grant all on *.* to root@'10.40.1.%';
```

在ProxySQL的設定供ProxySQL連結Percona的用戶
```
insert into mysql_users(username,password,default_hostgroup,transaction_persistent)values ('root', 'P@ssword1!', 10, 1);
load mysql users to runtime;
save mysql users to disk;
```
## 測試可否透過 ProxySQL 連 Percona
```
mysql -uroot -pP@ssword1! -h127.0.0.1 -P6033 -e "SELECT * FROM testDB.testTable;"
```
