# nextcloud.docker
Install Nextcloud in docker with mariadb, redis, cron and caddy.

## Create a self signed certificate

1. Enter the folder `caddy/certs`

2. Create a self signed certificate with this command

   `openssl req -newkey rsa:4096 -x509 -sha512 -days 365 -nodes -out certificate.pem -keyout privatekey.pem`

## Edit docker-compose.yml and Caddyfile

1. Set the port you want to use to access Nextcloud (in this example the port is 1201)

   In the `docker-compose.yml` file

   ```yaml
   ports:
     - 1201:1201
   ```

   and int the `caddy/Caddyfile`

   ```
   :1201
   tls /etc/caddy/certificate.pem /etc/caddy/privatekey.pem
   reverse_proxy nextcloud:80
   ```

2. Set the database and redis passwords

3. Change the memory limits to something that suits your needs

   `mem_limit: "250M"`

4. Set the local folder you want to use for the Nextcloud data (both in nextcloud and in cron)

   `- /mnt/Volume/nextcloud/:/var/www/html/data`

5. Set the domain name from which you'll access nextcloud (if you are using a service like dynds add `dyndns.org` or the domain you are using)

   `- NEXTCLOUD_TRUSTED_DOMAINS=$YOUR_DOMAIN$`

   If you want to access Nextcloud only from localhost you can remove the line.

## Create and start the containers

Use this command to create and start all the containers

`docker compose up -d`



