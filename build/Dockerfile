FROM nginx:1.19.3

RUN apt-get update && \
    apt-get install --no-install-recommends -y \
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
    supervisor \
    tar \
    wget \
    && rm -rf /var/lib/apt/lists/*  # remove the cache

# Remove nginx default.conf
RUN rm /etc/nginx/conf.d/default.conf

# Copy nginx config files
COPY nginx/general.conf /etc/nginx/
COPY nginx/nginx.conf /etc/nginx/
COPY nginx/php_fastcgi.conf /etc/nginx/
COPY nginx/security.conf /etc/nginx/
COPY nginx/wiki.conf /etc/nginx/conf.d/
# Copy supervisord config file
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Download mediawiki and extract it to it's destination 
RUN wget -q https://releases.wikimedia.org/mediawiki/1.35/mediawiki-1.35.0.tar.gz -O mediawiki.tar.gz && \
    mkdir -p /var/www/html/wiki && \
    tar -zxf mediawiki.tar.gz --strip 1 -C /var/www/html/wiki && \
    chown -R www-data:www-data /var/www

# Create dir /run/php and make www-data the owner 
RUN mkdir /run/php/ && chown -R www-data:www-data /run/php
# Copy wiki logo
COPY --chown=www-data wiki.png /var/www/html/wiki/resources/assets/

CMD ["/usr/bin/supervisord"]
