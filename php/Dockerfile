FROM php:7.3-fpm

ENV INSTALL_DIR /var/www/html
ENV COMPOSER_HOME /var/www/.composer/

# Install composer
RUN curl -sS https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer
COPY ./auth.json $COMPOSER_HOME

# Install PHP libs and modules
RUN requirements="libpng++-dev libzip-dev libmcrypt-dev libmcrypt4 libcurl3-dev libfreetype6 libjpeg62-turbo-dev libfreetype6-dev libicu-dev libxslt1-dev unzip cron" \
    && apt-get update \
    && apt-get install -y $requirements \
    && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd \
    && docker-php-ext-install mbstring \
    && docker-php-ext-install zip \
    && docker-php-ext-install intl \
    && docker-php-ext-install xsl \
    && docker-php-ext-install soap \
    && docker-php-ext-install bcmath \
    && docker-php-ext-install sockets

RUN yes '' | pecl install mcrypt-1.0.3 && echo 'extension=mcrypt.so' > /usr/local/etc/php/conf.d/mcrypt.ini

# Install Magento
RUN cd $INSTALL_DIR && composer create-project --repository=https://repo.magento.com/ magento/project-community-edition . --ignore-platform-reqs

# Setup permissions
RUN cd $INSTALL_DIR \
    && find var generated vendor pub/static pub/media app/etc -type f -exec chmod g+w {} + \
    && find var generated vendor pub/static pub/media app/etc -type d -exec chmod g+ws {} + \
    && chown -R :www-data . \
    && chmod u+x bin/magento

# Prepare post setup scripts
COPY ./install-magento /usr/local/bin/install-magento
RUN chmod +x /usr/local/bin/install-magento

COPY ./install-sampledata /usr/local/bin/install-sampledata
RUN chmod +x /usr/local/bin/install-sampledata

# Configure PHP
RUN echo 'memory_limit = 2G\n\
max_execution_time = 1800\n\
zlib.output_compression = On' >> /usr/local/etc/php/php.ini

# Cleanup
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

COPY startup /bin/startup

WORKDIR $INSTALL_DIR

ENTRYPOINT ["sh", "/bin/startup"]
