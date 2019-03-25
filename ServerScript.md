## Server
```sh
apt update -y
apt install docker-compose python3-pip php7.0-cli hhvm curl php7.0-mbstring git unzip
curl -sS https://getcomposer.org/installer -o composer-setup.php
php composer-setup.php --install-dir=/usr/local/bin --filename=composer
docker run -d \
    -p 80:80 -p 443:443 \
    -e ENABLE_IPV6=true \
    --restart always \
    -v /etc/nginx/conf.d:/etc/nginx/conf.d \
    -v /etc/nginx/certs:/etc/nginx/certs \
    -v /etc/nginx/vhost.d:/etc/nginx/vhost.d:ro \
    -v /var/run/docker.sock:/tmp/docker.sock:ro \
    --name proxy jwilder/nginx-proxy:alpine
```
## Docker
```sh
cd /root
# Docker
ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime
apt install python3 python3-pip curl git unzip systemd
apt install -y gcc make telnet whois vim git gettext cron mysql-client iputils-ping net-tools wget
apt install -y apache2 apache2-utils
apt install -y php libapache2-mod-php php-mysql php-pear php-mbstring php-curl php-gd php-imagick php-memcache php-xmlrpc php-geoip
pear install MIME_Type
curl -sS https://getcomposer.org/installer -o composer-setup.php
php composer-setup.php --install-dir=/usr/local/bin --filename=composer
rm composer-setup.php

# SYSCONF 설치
wget http://archive.ubuntu.com/ubuntu/pool/universe/s/sysv-rc-conf/sysv-rc-conf_0.99.orig.tar.gz
tar zxvf sysv-rc-conf_0.99.orig.tar.gz
cd sysv-rc-conf-0.99
wget http://archive.ubuntu.com/ubuntu/pool/universe/s/sysv-rc-conf/sysv-rc-conf_0.99.orig.tar.gz
tar zxvf sysv-rc-conf_0.99.orig.tar.gz
cd sysv-rc-conf-0.99
apt install make
sudo make
sudo make install
apt install libcurses-ui-perl libterm-readkey-perl libcurses-perl

# SSL
sysv-rc-conf apache2 on
sysv-rc-conf cron on
a2enmod ssl
mkdir /etc/apache2/ssl
openssl genrsa -out /etc/apache2/ssl/server.key 2048
openssl req -new -days 365 -key /etc/apache2/ssl/server.key -out /etc/apache2/ssl/server.csr -subj "/C=KR/ST=Daejeon/L=Daejeon/O=Docker/OU=IT Department/CN=localhost"
openssl x509 -req -days 365 -in /etc/apache2/ssl/server.csr -signkey /etc/apache2/ssl/server.key -out /etc/apache2/ssl/server.crt
#RUN cp /etc/apache2/sites-available/default-ssl.conf /etc/apache2/sites-enabled/000-default-ssl.conf
sed 's/\/etc\/ssl\/certs\/ssl-cert-snakeoil.pem/\/etc\/apache2\/ssl\/server.crt/g' /etc/apache2/sites-available/default-ssl.conf > /etc/apache2/sites-available/default-ssl.conf.tmp
sed 's/\/etc\/ssl\/private\/ssl-cert-snakeoil.key/\/etc\/apache2\/ssl\/server.key/g' /etc/apache2/sites-available/default-ssl.conf.tmp > /etc/apache2/sites-enabled/000-default-ssl.conf
rm /etc/apache2/sites-available/default-ssl.conf.tmp -f

# Apache Cache 설정
#RUN a2enmod cache
#RUN a2enmod cache_disk
#RUN a2enmod expires
#RUN a2enmod headers
#RUN a2enmod rewrite

# Service 등록
systemctl enable apache2
update-rc.d apache2 enable
update-rc.d cron enable

# Default Setting File Add
wget https://raw.githubusercontent.com/bluei98/Docker-ForexLiga/master/webserver/ubuntu/default-ssl.conf /etc/apache2/sites-enabled/000-default-ssl.conf

# Service 시작
RUN service apache2 start
RUN service cron start

VOLUME ["/var/www/html"]

EXPOSE 80
EXPOSE 443
```
