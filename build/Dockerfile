FROM nginx:1.21.1

RUN apt-get update && \
    apt-get install --no-install-recommends -y \
    composer \
    git \
    nano \ 
    php7.3 \
    php7.3-apcu \
    php7.3-cli \
    php7.3-common \
    php7.3-curl \
    php7.3-fpm \
    php7.3-gd \
    php7.3-intl \
    php7.3-mbstring \
    php7.3-mysql \
    php7.3-soap \
    php7.3-sqlite3 \
    php7.3-xml \
    php7.3-xmlrpc \
    php7.3-zip \
    python3 \
    supervisor \
    tar \
    wget \
    && rm -rf /var/lib/apt/lists/*  # remove the cache

# 1. Remove nginx default.conf
# 2. Create dir /run/php and make www-data the owner
# 3. Set upload_max_filesize to 25M
# 4. Set post_max_size to 25M
# 5. Create directory /var/www/html/mediawiki
RUN rm /etc/nginx/conf.d/default.conf && \
    mkdir /run/php/ && chown -R www-data:www-data /run/php && \
    sed -i 's/upload_max_filesize = 2M/upload_max_filesize = 25M/g' /etc/php/7.3/fpm/php.ini && \
    sed -i 's/post_max_size = 8M/post_max_size = 25M/g' /etc/php/7.3/fpm/php.ini && \
    mkdir -p /var/www/html/mediawiki

# Download mediawiki and extract it to /var/www/html/mediawiki
RUN wget -q https://releases.wikimedia.org/mediawiki/1.36/mediawiki-1.36.1.tar.gz -O mediawiki.tar.gz && \
    tar -zxf mediawiki.tar.gz --strip 1 -C /var/www/html/mediawiki && \
    chown -R www-data:www-data /var/www

# Download Extensions TemplateStyles and Math
RUN git clone --single-branch --branch REL1_36 https://github.com/wikimedia/mediawiki-extensions-TemplateStyles.git /var/www/html/mediawiki/extensions/TemplateStyles && \
    git clone --single-branch --branch REL1_36 https://github.com/wikimedia/mediawiki-extensions-Math /var/www/html/mediawiki/extensions/Math && \
    cd /var/www/html/mediawiki/extensions/TemplateStyles && composer install --no-dev && \
    cd /var/www/html/mediawiki/extensions/Math && composer install --no-dev

# Copy nginx config files
COPY ["nginx/general.conf", "nginx/nginx.conf", "nginx/php_fastcgi.conf", "nginx/security.conf", "/etc/nginx/"]
COPY nginx/wiki.conf /etc/nginx/conf.d/
# Copy supervisord config file
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
# Copy wiki logo
COPY --chown=www-data wiki.png /var/www/html/mediawiki/resources/assets/

WORKDIR /var/www/html/mediawiki

CMD ["/usr/bin/supervisord"]
