FROM php:7.2-fpm

ENV PHALCON_VERSION 3.4.2
ENV PHALCON_URL https://codeload.github.com/phalcon/cphalcon/tar.gz/v3.4.2

RUN set -x && \
    apt-get update && \
    # <--- Install dependencies --->
    apt-get install --no-install-recommends --no-install-suggests -y git unzip libpng-dev && \
    # <--- Build and install PHP extensions --->
    docker-php-ext-install -j$(nproc) pdo && \
    docker-php-ext-install -j$(nproc) pdo_mysql && \
    docker-php-ext-install -j$(nproc) bcmath && \
    docker-php-ext-install -j$(nproc) gd && \
    pecl install xdebug && docker-php-ext-enable xdebug

COPY ./config-file/xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

RUN cd /tmp/ && \
    # <--- Composer --->
    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php composer-setup.php --install-dir=/usr/bin --filename=composer && \
    php -r "unlink('composer-setup.php');" && \
    # <--- PHPUnit --->
    curl -L 'https://phar.phpunit.de/phpunit-7.phar' > /usr/bin/phpunit && \
    chmod a+x /usr/bin/phpunit && \
    # <--- Phalcon --->
    curl -O "${PHALCON_URL}" && \
    tar xvzf "v${PHALCON_VERSION}" && rm "v${PHALCON_VERSION}" && \
    cd "cphalcon-${PHALCON_VERSION}/build" && \
    ./install && \
    docker-php-ext-enable phalcon && \
    # <--- Eliminar cosas que ya no necesitamos :D --->
    # apt-get remove --purge --auto-remove -y libpng-dev libcurl4-openssl-dev libssl-dev pkg-config && \
    apt-get autoremove -y && \
    apt-get autoclean -y && \
    apt-get clean -y && \
    rm -fr /var/lib/apt/lists/* && \
    rm -fr /tmp/* /var/tmp/*
