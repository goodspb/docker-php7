FROM ubuntu:16.04

ENV DEBIAN_FRONTEND noninteractive

COPY ./sources.list /etc/apt/sources.list
RUN apt-get update

RUN apt-get install software-properties-common python-software-properties language-pack-en-base vim git wget curl -y
RUN locale-gen en_US.UTF-8

ENV LC_ALL=en_US.UTF-8
ENV LANG=en_US.UTF-8

RUN add-apt-repository ppa:ondrej/php && \
    apt-get update

RUN apt-get install php7.1-fpm php7.1-gd php7.1-cli php7.1-curl php7.1-dev php7.1-json php7.1-mbstring php7.1-mcrypt php7.1-mysql php7.1-xml php7.1-zip php-redis php7.1-mongodb -y --fix-missing

# install phalcon
RUN curl -s https://packagecloud.io/install/repositories/phalcon/stable/script.deb.sh | bash && \
    apt-get install php7.1-phalcon -y

RUN pecl install swoole && \
    echo "extension=swoole.so" > /etc/php/7.1/mods-available/swoole.ini && \
    ln -s /etc/php/7.1/mods-available/swoole.ini /etc/php/7.1/cli/conf.d/20-swoole.ini && \
    ln -s /etc/php/7.1/mods-available/swoole.ini /etc/php/7.1/fpm/conf.d/20-swoole.ini

RUN cd / && \
    php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');" && \
    php composer-setup.php && \
    php -r "unlink('composer-setup.php');" && \
    mv composer.phar /usr/local/bin/composer && \
    composer config -g repo.packagist composer https://packagist.phpcomposer.com

# install supervisor , cron
RUN apt-get install supervisor cron --allow-unauthenticated -y
RUN service supervisor start

COPY ./.bashrc /root/.bashrc

# generate ssh key
RUN ssh-keygen -f 'id_rsa' -t rsa -N ''

# clean up
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

WORKDIR /home/wwwroot

COPY ./fpm-pool-www.conf /etc/php/7.1/fpm/pool.d/www.conf
RUN mkdir /run/php/ -p

EXPOSE 9000
CMD ["/usr/sbin/php-fpm7.1", "-F"]
