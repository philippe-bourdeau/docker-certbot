## Pre-requisites: 
* git -- to upload code to your server (or you could scp code to remote instead)
* docker
* docker-compose

## Project structure : 

Place 2 projects side by side in remote server $HOME like the following
/home/pbb
         /certbot
         /valuation-table



* Go to http://cloudhelp.ca to check index.html showing up

## Generate Certificates 
 * Start certbot deployment on the remote server (docker-compose up --build -d)
 * If you have issues with certbot server, try displaying the public/index.html file
 
 -- Note -- Test command with --staging argument, then remove to generate actual certificates 
```shell
export APPLICATION_PATH=$HOME/valuation-table

sudo docker run -it --rm -v /docker-volumes/certbot/etc/letsencrypt:/etc/letsencrypt \
-v /docker-volumes/certbot/var/lib/letsencrypt:/var/lib/letsencrypt \
-v /docker-volumes/certbot/var/log/letsencrypt:/var/log/letsencrypt \
-v $HOME/certbot/public:/data/letsencrypt \
certbot/certbot certonly --webroot --register-unsafely-without-email --agree-tos --webroot-path=/data/letsencrypt \
--staging -v \
-d cloudhelp.ca -d www.cloudhelp.ca 
```

## Generate dh file if needed

```shell
sudo openssl dhparam -out $APPLICATION_PATH/docker/dhparam-2048.pem 2048
```

## Move files and change permissions

```shell
sudo cp /docker-volumes/certbot/etc/letsencrypt/live/cloudhelp.ca/fullchain.pem $APPLICATION_PATH/docker/fullchain.pem
sudo cp /docker-volumes/certbot/etc/letsencrypt/live/cloudhelp.ca/privkey.pem $APPLICATION_PATH/docker/privkey.pem
sudo chmod 0644 \
$APPLICATION_PATH/docker/fullchain.pem \
$APPLICATION_PATH/docker/privkey.pem \
$APPLICATION_PATH/docker/dhparam-2048.pem
sudo chown pbb: \
$APPLICATION_PATH/docker/fullchain.pem \
$APPLICATION_PATH/docker/privkey.pem \
$APPLICATION_PATH/docker/dhparam-2048.pem
```

## TODOs && Ajustments
1. Find a way to improve deployment (not having to move files manually, change permissions and etc. )
2. Renew certificate