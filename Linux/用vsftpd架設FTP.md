# 在ubuntu16.04上用vsftpd架設ftp

## 安裝vsftpd
```
sudo apt-get install vsftpd libpam-pwdfile 
```

## 修改vsftpd設定檔
```
vi /etc/vsftpd.conf
```
特別需要改設定檔有關用戶的部份
```
# 設定 userlist
#userlist_enable=YES (NO) 
#是否藉助 vsftpd 的抵擋機制來處理某些不受歡迎的帳號，與底下的設定有關；
#userlist_deny=YES (NO) 
#userlist_enable=YES 時才會生效的設定，若此設定值為 YES 時，userlist_file為黑名單，若此設定值為 NO 時，userlist_file為白名單
#userlist_file=/etc/vsftpd.user_list userlist_file的位置

userlist_enable=YES  
userlist_deny=YES
userlist_file=/etc/vsftpd.user_list

```

## 利用sh產生ftp的用戶&並softlink到apahce下面
```
sh create_ftp_user.sh user1
```
```
#!/bin/bash

sudo mkdir /home/$1;
sudo useradd -d /home/$1 -s /bin/bash $1;
sudo echo "$1:$1" | sudo -S chpasswd;
sudo chown $1:$1 /home/$1;
echo $1 >> /etc/vsftpd.user_list;
sudo ln -s /home/$1/ /var/www/html/
```

## vsftpd設定檔範例
```

listen=NO
listen_ipv6=YES
local_enable=YES

userlist_deny=NO
userlist_enable=YES
userlist_file=/etc/vsftpd.user_list
#
# Uncomment this to enable any form of FTP write command.
write_enable=YES
#
# Default umask for local users is 077. You may wish to change this to 022,
# if your users expect that (022 is used by most other ftpd's)
local_umask=022
#
# Uncomment this to allow the anonymous FTP user to upload files. This only
# has an effect if the above global write enable is activated. Also, you will
# obviously need to create a directory writable by the FTP user.
#anon_upload_enable=YES
#
# Uncomment this if you want the anonymous FTP user to be able to create
# new directories.
#anon_mkdir_write_enable=YES
#
# Activate directory messages - messages given to remote users when they
# go into a certain directory.
dirmessage_enable=YES
#
# If enabled, vsftpd will display directory listings with the time
# in  your  local  time  zone.  The default is to display GMT. The
# times returned by the MDTM FTP command are also affected by this
# option.
use_localtime=YES
#
# Activate logging of uploads/downloads.
xferlog_enable=YES
#
# Make sure PORT transfer connections originate from port 20 (ftp-data).
connect_from_port_20=YES
#
# If you want, you can arrange for uploaded anonymous files to be owned by
# a different user. Note! Using "root" for uploaded files is not
# recommended!
#chown_uploads=YES
#chown_username=whoever
#
# You may override where the log file goes if you like. The default is shown
# below.
#xferlog_file=/var/log/vsftpd.log
#
# If you want, you can have your log file in standard ftpd xferlog format.
# Note that the default log file location is /var/log/xferlog in this case.
#xferlog_std_format=YES
#
# You may change the default value for timing out an idle session.
#idle_session_timeout=600
#
# You may change the default value for timing out a data connection.
#data_connection_timeout=120
#
# It is recommended that you define on your system a unique user which the
# ftp server can use as a totally isolated and unprivileged user.
#nopriv_user=ftpsecure
#
# Enable this and the server will recognise asynchronous ABOR requests. Not
# recommended for security (the code is non-trivial). Not enabling it,
# however, may confuse older FTP clients.
#async_abor_enable=YES
#
# By default the server will pretend to allow ASCII mode but in fact ignore
# the request. Turn on the below options to have the server actually do ASCII
# mangling on files when in ASCII mode.
# Beware that on some FTP servers, ASCII support allows a denial of service
# attack (DoS) via the command "SIZE /big/file" in ASCII mode. vsftpd
# predicted this attack and has always been safe, reporting the size of the
# raw file.
# ASCII mangling is a horrible feature of the protocol.
#ascii_upload_enable=YES
#ascii_download_enable=YES
#
# You may fully customise the login banner string:
#ftpd_banner=Welcome to blah FTP service.
#
# You may specify a file of disallowed anonymous e-mail addresses. Apparently
# useful for combatting certain DoS attacks.
#deny_email_enable=YES
# (default follows)
#banned_email_file=/etc/vsftpd.banned_emails
#
# You may restrict local users to their home directories.  See the FAQ for
# the possible risks in this before using chroot_local_user or
# chroot_list_enable below.
#chroot_local_user=YES
#
# You may specify an explicit list of local users to chroot() to their home
# directory. If chroot_local_user is YES, then this list becomes a list of
# users to NOT chroot().
# (Warning! chroot'ing can be very dangerous. If using chroot, make sure that
# the user does not have write access to the top level directory within the
# chroot)
chroot_local_user=YES
chroot_list_enable=YES
# (default follows)
chroot_list_file=/etc/vsftpd.chroot_list

#
# You may activate the "-R" option to the builtin ls. This is disabled by
# default to avoid remote users being able to cause excessive I/O on large
# sites. However, some broken FTP clients such as "ncftp" and "mirror" assume
# the presence of the "-R" option, so there is a strong case for enabling it.
#ls_recurse_enable=YES
#
# Customization
#
# Some of vsftpd's settings don't fit the filesystem layout by
# default.
#
# This option should be the name of a directory which is empty.  Also, the
# directory should not be writable by the ftp user. This directory is used
# as a secure chroot() jail at times vsftpd does not require filesystem
# access.
secure_chroot_dir=/var/run/vsftpd/empty
#
# This string is the name of the PAM service vsftpd will use.
pam_service_name=vsftpd
#
# This option specifies the location of the RSA certificate to use for SSL
# encrypted connections.
rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key
ssl_enable=NO

#
# Uncomment this to indicate that vsftpd use a utf8 filesystem.
#utf8_filesystem=YES

```

## 參考文件
```
https://www.linuxidc.com/Linux/2017-06/144807.htm
https://oranwind.org/-ftp-ubuntu-an-zhuang-ftp-yu-she-ding-apache-du-qu-quan-xian/
https://blog.xuite.net/rockmansyz/twblog/115534782-%28%E8%BD%89%E8%BC%89%29+vsftp%E5%8A%9F%E8%83%BD%E8%A9%B3%E7%B4%B0%E9%85%8D%E7%BD%AE
http://idobest.pixnet.net/blog/post/22040188-%5B%E8%BD%89%E8%B2%BC%5D-vsftpd-%E8%A8%AD%E5%AE%9A%E5%8F%83%E6%95%B8
```