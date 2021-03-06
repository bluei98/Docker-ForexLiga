FROM ubuntu
MAINTAINER SungYong EOM <mike@forexliga.com>

# 한국 전용
RUN cp /etc/apt/sources.list /etc/apt/sources.list.bak
RUN sed 's/archive.ubuntu.com/ftp.daumkakao.com/g' /etc/apt/sources.list.bak > /etc/apt/sources.list

# 레포지트 업데이트
RUN apt-get update -y
RUN apt-get upgrade -y

# 타임존 셋팅
RUN apt-get install -y tzdata
RUN ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime

# 기본 패키지 설치
RUN apt-get install -y gcc make telnet jwhois vim git gettext cron sysv-rc-conf mysql-client iputils-ping net-tools wget

# Apache PHP 설치
RUN apt-get install -y apache2 apache2-utils
RUN apt-get install -y php libapache2-mod-php php-mysql php-pear php-mbstring php-mcrypt php-curl php-gd php-imagick php-memcache php-xmlrpc php-geoip

# 라이브러리 설치
RUN pear install MIME_Type

# SSL 서비스 설정
RUN sysv-rc-conf apache2 on
RUN sysv-rc-conf cron on
RUN a2enmod ssl
RUN mkdir /etc/apache2/ssl
RUN openssl genrsa -out /etc/apache2/ssl/server.key 2048
RUN openssl req -new -days 365 -key /etc/apache2/ssl/server.key -out /etc/apache2/ssl/server.csr -subj "/C=KR/ST=Daejeon/L=Daejeon/O=Docker/OU=IT Department/CN=localhost"
RUN openssl x509 -req -days 365 -in /etc/apache2/ssl/server.csr -signkey /etc/apache2/ssl/server.key -out /etc/apache2/ssl/server.crt
#RUN cp /etc/apache2/sites-available/default-ssl.conf /etc/apache2/sites-enabled/000-default-ssl.conf
RUN sed 's/\/etc\/ssl\/certs\/ssl-cert-snakeoil.pem/\/etc\/apache2\/ssl\/server.crt/g' /etc/apache2/sites-available/default-ssl.conf > /etc/apache2/sites-available/default-ssl.conf.tmp
RUN sed 's/\/etc\/ssl\/private\/ssl-cert-snakeoil.key/\/etc\/apache2\/ssl\/server.key/g' /etc/apache2/sites-available/default-ssl.conf.tmp > /etc/apache2/sites-enabled/000-default-ssl.conf
RUN rm /etc/apache2/sites-available/default-ssl.conf.tmp -f

# Apache Cache 설정
RUN a2enmod cache
RUN a2enmod cache_disk
RUN a2enmod expires
RUN a2enmod headers
RUN a2enmod rewrite

# Service 등록
RUN update-rc.d apache2 enable
RUN update-rc.d cron enable

# Default Setting File Add
RUN wget https://raw.githubusercontent.com/bluei98/Docker-ForexLiga/master/webserver/ubuntu/default-ssl.conf /etc/apache2/sites-enabled/000-default-ssl.conf

# Service 시작
RUN service apache2 start
RUN service cron start

VOLUME ["/var/www/html"]

EXPOSE 80
EXPOSE 443
