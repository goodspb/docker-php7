FROM php:7.1.5-fpm

COPY ./sources.list /etc/apt/sources.list
RUN apt-get update

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ --filename=composer
RUN composer config -g repo.packagist composer https://packagist.phpcomposer.com

RUN apt-get install  -y  --allow-unauthenticated vim git wget

# add php sources
RUN apt-get install apt-transport-https lsb-release  --allow-unauthenticated -y && \
    wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg --no-check-certificate && \
    echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list

# add phalcon source
RUN curl -s https://packagecloud.io/install/repositories/phalcon/stable/script.deb.sh | bash

RUN apt-get install php7.1-phalcon php7.1-mcrypt php7.1-mysql php7.1-zip -y --allow-unauthenticated

RUN export PHP_EXT_DIR=$(php-config --extension-dir) && \
    export PHP_EXT_DATE=$(php-config --extension-dir | sed -e 's/\/usr\/local\/lib\/php\/extensions\/no-debug-non-zts-//g') && \
    cp -R /usr/lib/php/$PHP_EXT_DATE/* $PHP_EXT_DIR/

RUN echo "extension=phalcon.so" > /usr/local/etc/php/conf.d/phalcon.ini && \
    echo "extension=mcrypt.so" > /usr/local/etc/php/conf.d/mcrypt.ini && \
    echo "extension=pdo_mysql.so" > /usr/local/etc/php/conf.d/pdo_mysql.ini && \
    echo "extension=zip.so" > /usr/local/etc/php/conf.d/zip.ini

# install swoole
RUN cd / && \
    git clone https://git.oschina.net/swoole/swoole.git swoole-src && \
    cd swoole-src && \
    phpize && \
    ./configure && \
    make && make install && \
    echo "extension=swoole.so" > /usr/local/etc/php/conf.d/swoole.ini

# install redis 3.1.2
RUN cd / && \
    wget -O redis-3.1.2.tar.gz https://github.com/phpredis/phpredis/archive/3.1.2.tar.gz --no-check-certificate && \
    tar -zxvf redis-3.1.2.tar.gz && \
    cd phpredis-3.1.2 && \
    phpize && \
    ./configure && \
    make && make install && \
    echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini

# install mongodb
RUN cd / && \
    git clone https://github.com/mongodb/mongo-php-driver.git mongodb && \
    cd mongodb && \
    git submodule update --init && \
    phpize && \
    ./configure --with-openssl-dir=/usr/include/openssl && \
    make && make install && \
    echo "extension=mongodb.so" > /usr/local/etc/php/conf.d/mongodb.ini

# install supervisor , cron
RUN apt-get install supervisor cron --allow-unauthenticated -y
RUN service supervisor start

COPY ./.bashrc /root/.bashrc

# generate ssh key
RUN ssh-keygen -f 'id_rsa' -t rsa -N ''

# clean up
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

WORKDIR /home/wwwroot
