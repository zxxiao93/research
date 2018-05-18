FROM php:7.1-fpm
MAINTAINER zxxiao93@outlook.com
ENV DEBIAN_FRONTEND noninteractive
ENV NODE_PATH /usr/local/lib/node_modules

COPY docker-php-pecl-install /usr/local/bin/
RUN chmod -R 700 /usr/local/bin/

RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
        openssl \
        git \
    && docker-php-ext-install -j$(nproc) iconv mcrypt \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd

RUN docker-php-ext-install mbstring pdo pdo_mysql zip opcache mysqli

# ssh2
RUN apt-get install -y \
    libssh2-1-dev

# PECL
RUN docker-php-pecl-install \
    redis-3.0 \
    apcu-5.1.7

# SSH2
# TODO PECL is buggy, we must compile it.
RUN git clone https://github.com/php/pecl-networking-ssh2.git /usr/src/php/ext/ssh2 \
    && docker-php-ext-install ssh2

RUN apt-get install -y sshpass
RUN apt-get install -y nodejs npm && npm install -g argparse

RUN apt-get clean && rm -rf /var/lib/apt/lists/*

RUN echo "cgi.fix_pathinfo=0 " >> /usr/local/etc/php/php.ini \
    && echo "mysql.default_socket = /tmp/mysql.sock" >> /usr/local/etc/php/php.ini \
    && echo "pdo_mysql.default_socket= /tmp/mysql.sock" >> /usr/local/etc/php/php.ini \
    && echo "mysqli.default_socket =/tmp/mysql.sock" >> /usr/local/etc/php/php.ini 
RUN echo "output_buffering=4096 " >> /usr/local/etc/php/php.ini 
WORKDIR /var/www

