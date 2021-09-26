1. Install Docker
2. Install Docker-compose

sudo docker run -it --rm \
-v /docker-volumes/certbot/etc/letsencrypt:/etc/letsencrypt \
-v /docker-volumes/certbot/var/lib/letsencrypt:/var/lib/letsencrypt \
-v /docker-volumes/certbot/var/log/letsencrypt:/var/log/letsencrypt \
-v /docker-volumes/certbot/data/letsencrypt:/data/letsencrypt \
certbot/certbot \
certonly --webroot \
--register-unsafely-without-email --agree-tos \
--webroot-path=/data/letsencrypt \
--staging \
-d cloudhelp.ca -d cloudhelp.ca