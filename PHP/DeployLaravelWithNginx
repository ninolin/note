# 在 Ubuntu16.04 上部署 Laravel 和 Nginx

分別安裝 Laravel 和 Nginx 後，再把 Nginx 的 root 目錄設在 Laravel 的 public 資料夾

## Laravel
### 安裝 PHP7.2
```
sudo apt-add-repository ppa:ondrej/php
sudo apt-get update
sudo apt-get -y install php7.2 php7.2-mysql php7.2-fpm php7.2-mbstring php7.2-xml php7.2-curl zip unzip php7.2-zip php-memcached
echo ‘cgi.fix_pathinfo=0’ >> /etc/php/7.2/fpm/php.ini
```
### 安裝 composer
```
cd ~
curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer
```
### 下載 Laravel 專案
```
git clone https://github.com/laravel/quickstart-basic quickstart
cd quickstart
composer install
```
### 設定 Laravel 設定檔
```
sudo vi /var/www/html/quickstart/.env
```
## Nginx
### 安裝 Nginx
```
sudo apt-get update && apt-get -y install nginx
```

### 設定 Nginx 設定檔
設定檔在 /etc/nginx/sites-available/default
```
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /home/nino/quickstart/public;

    index index.php index.html index.htm index.nginx-debian.html;

    server_name _;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        #nginx無法執行php，所以遇到副檔名為php的檔案，要透過fpm執行
        #如果php是在container，然後該container expose 9000 port的話，下面這行改 fastcgi_pass   127.0.0.1:9000;
        fastcgi_pass unix:/run/php/php7.2-fpm.sock; 
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}
```
重新執行nginx後就可以在網址上看到了
```
service nginx restart
```