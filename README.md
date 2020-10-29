# MediaWiki-Docker
mediawiki-docker creates a container with mediawiki and a database container with mariadb.
The complete setup takes no longer than 30 minutes.

## Usage
Ideally the container should be executed in combination with [nginx-proxy](https://github.com/nginx-proxy/nginx-proxy) and 
[letsencrypt-nginx-proxy-companion](https://github.com/nginx-proxy/docker-letsencrypt-nginx-proxy-companion).

- If you want several containers on your host to be accessible via port 80/443, then 
[nginx-proxy](https://github.com/nginx-proxy/nginx-proxy)  will handle the routing for you automatically.
- [letsencrypt-nginx-proxy-companion](https://github.com/nginx-proxy/docker-letsencrypt-nginx-proxy-companion) handles the 
automated creation, renewal and use of Let's Encrypt certificates for proxied Docker containers


1. Clone this repository
2. Replace VIRTUAL_HOST and LETSENCRYPT_HOST in docker-compose.yml with your sub/domain
3. Run it with docker-compose
```
docker-compose up -d
```
4. Point your web browser to your new hosted wiki and follow the instruction on the setup screen.
5. Upload LocalSettings.php to your web server e.g. with scp
6. Copy LocalSettings.php into your mediawiki container
```
docker cp LocalSettings.php mediawiki:/var/www/html/LocalSettings.php
```

And you're done. Have fun with your new Wiki!
