# Ubuntu + Apache2 + PHP 7


### RUN CONTAINER
```bash
docker run -it -p 80:80 -p 443:443 -v /etc/apache2 -v /var/www/html --name webserver forexliga/webserver
```
