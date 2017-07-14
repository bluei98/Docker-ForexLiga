# Docker-ForexLiga


## Init Docker Network
```sh
$ docker network create service
```

## Set WebServer

### Pull Docker Image And Run Container
```sh
$ docker run -it -p 80:80 -p 443:443 -v /etc/apache2 -v /var/www/html --name webserver forexliga/webserver
```

### Join Docker Network
```sh
$ docker network connect service webserver
```


## Set DB Server(MariaDB)

### Pull Docker Image And Run Container
```sh
$ docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=dbserver --name dbserver mariadb
```

### Join Docker Network
```sh
$ docker network connect service dbserver
```
