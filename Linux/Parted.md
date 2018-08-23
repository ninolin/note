# Parted

Parted 是 linux 上的磁碟分割管理指令，可以新增、改大小、刪除磁碟分割  
一般最常用的指令是 fdisk，但是 fdisk 有 2TB 的限制

## 安裝 Parted

```
sudo apt-get install parted
```

## 進入&離開 Parted

```
sudo parted     //進入Parted
(parted) help   
(parted) quit   //離開Parted
```

## 選擇&查看 Parted

```
(parted) select /dev/sda //選擇查看/dev/sda
Using /dev/sda
(parted) print  //查看磁碟分割區
Model: ATA HGST HUH721008AL (scsi)
Disk /dev/sda: 8002GB
Sector size (logical/physical): 512B/4096B
Partition Table: gpt
Disk Flags:

Number  Start   End     Size    File system   Name  Flags
 1      1049kB  8000GB  8000GB  ext4
```

## 建立磁碟分割區

### 建立磁碟分割表
```
mklabel msdos //小於2TB
mklabel gpt //大於2TB
```

### 建立磁碟分割區
```
(parted) mkpart

//輸入參數
Partition name?  []? //分割區名稱
File system type?  [ext2]? //檔案系統格式，之後格式化後可以再改
Start? //輸入起始位置，可以輸入1
End? //輸入結束位置，輸入8000000是8000G，單位是mb

//查看分割區
(parted) print
Model: ATA HGST HUH721008AL (scsi)
Disk /dev/sdb: 8002GB
Sector size (logical/physical): 512B/4096B
Partition Table: gpt
Disk Flags:

Number  Start   End     Size    File system   Name  Flags
 1      1049kB  8000GB  8000GB  ext2

//離開parted
(parted) quit

//格式化分割區
mkfs.ext4 /dev/sdb
```

### 掛載使用
```
sudo mkdir /mnt/sdb
sudo mount /dev/sdb /mnt/sdb/
```

### 刪除磁碟分割區
```
(parted) print  //查看磁碟分割區
Model: ATA HGST HUH721008AL (scsi)
Disk /dev/sda: 8002GB
Sector size (logical/physical): 512B/4096B
Partition Table: gpt
Disk Flags:

Number  Start   End     Size    File system   Name  Flags
 1      1049kB  8000GB  8000GB  ext4

(parted) rm 1 //刪除 Number:1 的分割區
```
