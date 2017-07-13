# Ubuntu Apache Web Server

### Include
* Ubuntu
* Apache2
  - Cache
  - Cache Disk
  - Expires
  - Headers
  - Rewrite
  - SSL
* PHP 7

### Docker Build
```sh
$ docker build -t webserver .
```

### Docker Run
```sh
$ docker run -it -p 80:80 -p 443:443 -v /etc/apache2 -v /var/www/html --name webserver forexliga/webserver
```
