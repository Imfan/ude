FROM php:7.2-fpm

COPY sources.list /etc/apt/sources.list

RUN apt-get update && apt-get install -yqq vim git && pecl channel-update pecl.php.net \
&& cp -f /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

###########################################################################
# PHP REDIS EXTENSION
###########################################################################

ARG INSTALL_PHPREDIS=false

RUN if [ ${INSTALL_PHPREDIS} = true ]; then \
    # Install Php Redis Extension
    printf "\n" | pecl install -o -f redis \
    &&  rm -rf /tmp/pear \
    &&  docker-php-ext-enable redis \
;fi

RUN docker-php-ext-install pdo_mysql

###########################################################################
# Zip EXTENSION
###########################################################################

RUN apt-get update && \
    apt-get install -y --no-install-recommends libzip-dev && \
    rm -r /var/lib/apt/lists/* && \
    docker-php-ext-install -j$(nproc) zip


###########################################################################
# Mysqli:
###########################################################################

ARG INSTALL_MYSQLI=false

RUN if [ ${INSTALL_MYSQLI} = true ]; then \
    docker-php-ext-install mysqli \
;fi


###########################################################################
# Yaf:
###########################################################################

ARG INSTALL_YAF=false

RUN if [ ${INSTALL_YAF} = true ]; then \
    docker-php-ext-install yaf \
;fi

###########################################################################
# Opcache:
###########################################################################

#ARG INSTALL_OPCACHE=false
#
#RUN if [ ${INSTALL_OPCACHE} = true ]; then \
#    docker-php-ext-install opcache \
RUN    docker-php-ext-install opcache
#;fi

# Copy opcache configration
COPY ./opcache.ini /usr/local/etc/php/conf.d/opcache.ini





###########################################################################
# Swoole EXTENSION
###########################################################################

ARG INSTALL_SWOOLE=false

RUN if [ ${INSTALL_SWOOLE} = true ]; then \
    # Install Php Swoole Extension
    if [ $(php -r "echo PHP_MAJOR_VERSION;") = "5" ]; then \
      pecl install swoole-2.0.10; \
    else \
      if [ $(php -r "echo PHP_MINOR_VERSION;") = "0" ]; then \
        pecl install swoole-2.2.0; \
      else \
        pecl install swoole; \
      fi \
    fi && \
    docker-php-ext-enable swoole \
    && php -m | grep -q 'swoole' \
;fi




###########################################################################
# MongoDB:
###########################################################################

ARG INSTALL_MONGO=false

RUN if [ ${INSTALL_MONGO} = true ]; then \
    # Install the mongodb extension
    if [ $(php -r "echo PHP_MAJOR_VERSION;") = "5" ]; then \
      pecl install mongo && \
      docker-php-ext-enable mongo \
    ;fi && \
    pecl install mongodb && \
    docker-php-ext-enable mongodb \
;fi



###########################################################################
# GD库:
###########################################################################

#echo "deb http://mirrors.163.com/debian/ stretch main contrib non-free\ndeb http://mirrors.163.com/debian/ stretch-updates main contrib non-free\ndeb http://mirrors.163.com/debian/ stretch-backports main contrib non-free" > /etc/apt/sources.list  #软件源修改为网易镜像站源
RUN apt update  && apt install -y libwebp-dev libjpeg-dev libpng-dev libfreetype6-dev && docker-php-source extract && cd /usr/src/php/ext/gd   && docker-php-ext-configure gd --with-webp-dir=/usr/include/webp --with-jpeg-dir=/usr/include --with-png-dir=/usr/include --with-freetype-dir=/usr/include/freetype2 &&   docker-php-ext-install gd      && php -m | grep gd




###########################################################################
# ping:
###########################################################################

USER root

ARG INSTALL_PING=false

RUN if [ ${INSTALL_PING} = true ]; then \
    apt-get update -yqq && \
    apt-get -y install inetutils-ping \
;fi


###########################################################################
# xDebug:
###########################################################################

ARG INSTALL_XDEBUG=false

RUN if [ ${INSTALL_XDEBUG} = true ]; then \
  # Install the xdebug extension
  if [ $(php -r "echo PHP_MAJOR_VERSION;") = "5" ]; then \
    pecl install xdebug-2.5.5; \
  else \
    pecl install xdebug; \
  fi && \
  docker-php-ext-enable xdebug \
;fi

# Copy xdebug configuration for remote debugging
COPY ./xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

RUN sed -i "s/xdebug.remote_autostart=0/xdebug.remote_autostart=1/" /usr/local/etc/php/conf.d/xdebug.ini && \
    sed -i "s/xdebug.remote_enable=0/xdebug.remote_enable=1/" /usr/local/etc/php/conf.d/xdebug.ini && \
    sed -i "s/xdebug.cli_color=0/xdebug.cli_color=1/" /usr/local/etc/php/conf.d/xdebug.ini




###########################################################################
# composer:
###########################################################################

COPY composer72 /usr/local/bin/composer
RUN set -xe && chmod -R 777 /usr/local/bin/composer \
    && composer config -g repo.packagist composer https://packagist.phpcomposer.com \
    && composer -v


###########################################################################
# Check PHP version:
###########################################################################
ARG PHP_VERSION=${PHP_VERSION}
RUN set -xe; php -v | head -n 1 | grep -q "PHP ${PHP_VERSION}"


COPY ./xlaravel.pool.conf /usr/local/etc/php-fpm.d/

USER root

# Clean up
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog

RUN usermod -u 1000 www-data


WORKDIR /var/www

CMD ["php-fpm"]

EXPOSE 9000