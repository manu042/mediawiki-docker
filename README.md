# MediaWiki-Docker
mediawiki-docker creates a container with mediawiki and a database container with mariadb.
The complete setup takes no longer than 30 minutes.


## Features 
- [Mediawiki 1.36.1](https://www.mediawiki.org/wiki/MediaWiki)
  - Configured with [Short URLs](https://www.mediawiki.org/wiki/Manual:Short_URL)
- [Nginx](https://nginx.org/)
- [MariaDB](https://mariadb.org/)
- [Php-FPM](https://www.php.net/manual/de/install.fpm.php)
- [Git](https://git-scm.com/)


## Extensions
- [Bundled Extensions](https://www.mediawiki.org/wiki/Bundled_extensions_and_skins)
- Additional extensions:
  - [Math](https://www.mediawiki.org/wiki/Extension:Math)
  - [TemplateStyles](https://www.mediawiki.org/wiki/Extension:TemplateStyles)


## Usage
Ideally the container should be executed in combination with [nginx-proxy](https://github.com/nginx-proxy/nginx-proxy) and 
[letsencrypt-nginx-proxy-companion](https://github.com/nginx-proxy/docker-letsencrypt-nginx-proxy-companion).

- If you want several containers on your host to be accessible via port 80/443, then 
[nginx-proxy](https://github.com/nginx-proxy/nginx-proxy)  will handle the routing for you automatically.
- [letsencrypt-nginx-proxy-companion](https://github.com/nginx-proxy/docker-letsencrypt-nginx-proxy-companion) handles the 
automated creation, renewal and use of Let's Encrypt certificates for proxied Docker containers


1. Clone this repository
2. Replace VIRTUAL_HOST and LETSENCRYPT_HOST in docker-compose.yml with your sub-/domain
3. Insert a proper password for MYSQL_PASSWORD in docker-compose.yml
5. Run it with docker-compose
```
docker-compose up -d
```
6. Point your web browser to your new hosted wiki and follow the instruction on the setup screen.
   - Note: You have to use "mariadb" for Database host.  
7. Upload LocalSettings.php to your web server e.g. with scp
   - To get short URL's add the following to lines to your LocalSettings.php
```
$wgArticlePath = "/wiki/$1";
$wgUsePathInfo = true;
```
8. Copy LocalSettings.php into your mediawiki container
```
docker cp LocalSettings.php mediawiki:/var/www/html/mediawiki/LocalSettings.php
```

And you're done. Have fun with your new Wiki!
