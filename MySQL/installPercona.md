# 在 Ubuntu16.04 上安裝 Percnoa XtraDB Cluster

先準備4台VM,預計裝3台Percnoa XtraDB Cluster和1台proxySQL

Name      | Public IP       | Private IP 
----------|:---------------:|---------------
DB1       | 61.66.218.172   |  172.16.0.180 
DB2       | 61.66.218.192   |  172.16.0.183
DB3       | 61.66.218.203   |  172.16.0.181
proxySQL  | 61.66.218.187   |  172.16.0.182

## 前置作業
每1台要安裝Percnoa XtraDB Cluster都要開3306,4444,4567,4568,22(22要開才能ssh)
```
ufw allow 3306/tcp
ufw allow 4444/tcp
ufw allow 4567/tcp
ufw allow 4568/tcp
ufw allow 22/tcp
ufw enable
ufw status
```

## 安裝 Percnoa XtraDB Cluster
```
wget https://repo.percona.com/apt/percona-release_0.1-6.$(lsb_release -sc)_all.deb
sudo dpkg -i percona-release_0.1-6.$(lsb_release -sc)_all.deb
sudo apt-get update
sudo apt-get install percona-xtradb-cluster-57
service mysql stop
service mysql status
```

## 設定config

修改DB1的/etc/mysql/my.cnf
```
[mysqld]

wsrep_provider=/usr/lib/libgalera_smm.so

wsrep_cluster_name=pxc-cluster
wsrep_cluster_address=gcomm://172.16.0.180,172.16.0.183,172.16.0.181

wsrep_node_name=pxc1
wsrep_node_address=172.16.0.180

wsrep_sst_method=xtrabackup-v2
wsrep_sst_auth=sstuser:passw0rd

pxc_strict_mode=ENFORCING

binlog_format=ROW
default_storage_engine=InnoDB
innodb_autoinc_lock_mode=2
```

修改DB2的/etc/mysql/my.cnf，設定跟DB1一樣，只需要改下面設定
```
wsrep_node_name=pxc2
wsrep_node_address=172.16.0.183
```

修改DB3的/etc/mysql/my.cnf，設定跟DB1一樣，只需要改下面設定
```
wsrep_node_name=pxc3
wsrep_node_address=172.16.0.181
```

## 節點1啟動Percnoa XtraDB Cluster
節點1以bootstrap形式啟動MySQL
```
[root@pxc1 ~]# /etc/init.d/mysql bootstrap-pxc
```
檢查目前節點數量
```
mysql> SHOW global STATUS LIKE 'wsrep_cluster_size';
```
新增跟設定檔中一樣的user
```
mysql> CREATE USER 'sstuser'@'localhost' IDENTIFIED BY 'passw0rd';
mysql> GRANT RELOAD, LOCK TABLES, PROCESS, REPLICATION CLIENT ON *.* TO
  'sstuser'@'localhost';
mysql> FLUSH PRIVILEGES;
```

## 節點2,3啟動mysql
啟動mysql
```
/etc/init.d/mysql start
```
檢查目前節點數量
```
mysql> SHOW global STATUS LIKE 'wsrep_cluster_size';
```

## 建一個table測試一下3個db是否同步
```
mysql> CREATE DATABASE testDB;
mysql> CREATE USER 'testUser'@'%' IDENTIFIED BY '12345';
mysql> GRANT ALL PRIVILEGES ON testDB.* TO 'testUser'@'%';
mysql> FLUSH PRIVILEGES;
mysql> USE testDB;
mysql> CREATE TABLE testTable (
    -> id INT(6) UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    -> firstname VARCHAR(30) NOT NULL,
    -> lastname VARCHAR(30) NOT NULL
    -> );
mysql> INSERT INTO `testTable` (`firstname`, `lastname`) VALUES ('nino', 'lin');
```
