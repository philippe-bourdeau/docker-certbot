## Pre-requisites: 
* git -- to upload code to your server (or you could scp code to remote instead)
* docker
* docker-compose

## Project structure : 

Place 2 projects side by side in remote server $HOME like the following
/home/pbb
         /certbot
         /web-application


* Start docker-compose deployment on the remote server (docker-compose up --build -d)
* Go to http://cloudhelp.ca to check index.html showing up

## Generate Certificate
Test command with --staging argument, then remove to generate actual certificates 
```shell
sudo docker run -it --rm -v /docker-volumes/certbot/etc/letsencrypt:/etc/letsencrypt \
-v /docker-volumes/certbot/var/lib/letsencrypt:/var/lib/letsencrypt \
-v /docker-volumes/certbot/var/log/letsencrypt:/var/log/letsencrypt \
-v $HOME/certbot/public:/data/letsencrypt \
certbot/certbot certonly --webroot --register-unsafely-without-email --agree-tos --webroot-path=/data/letsencrypt \
--staging -v \
-d cloudhelp.ca -d www.cloudhelp.ca 
```



## Ajustments

### Generate dh file if needed 

```shell
sudo openssl dhparam -out /$HOME/web-application/dh-param/dhparam-2048.pem 2048
```

## TODO
This is where is gets tricky to deal with certificates outside of deployment scope
@see readme for valuation