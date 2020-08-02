ARG PHP_VERSION
FROM php:${PHP_VERSION}-fpm

ARG PHP_XDEBUG
ARG PHP_SWOOLE
ARG PHP_REDIS
#ARG PHP_YAF
ARG REPLACE_SOURCE_LIST

COPY ./sources.list /etc/apt/sources.list.tmp
RUN if [ "${REPLACE_SOURCE_LIST}" = "true" ]; then \
    mv /etc/apt/sources.list.tmp /etc/apt/sources.list; else \
    rm -rf /etc/apt/sources.list.tmp; fi
RUN apt update

#  设置时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Install extensions from source
COPY ./extensions /tmp/extensions
RUN chmod +x /tmp/extensions/install.sh \
    && /tmp/extensions/install.sh \
    && rm -rf /tmp/extensions

# More extensions
# 1. soap requires libxml2-dev.
# 2. xml, xmlrpc, wddx require libxml2-dev and libxslt-dev.
# 3. Line `&& :\` do nothing just for better reading.
RUN apt install -y libfreetype6-dev libjpeg62-turbo-dev libpng-dev \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install $mc gd \
    && :\
    && apt install -y libicu-dev \
    && docker-php-ext-install $mc intl \
    && :\
    && apt install -y libbz2-dev \
    && docker-php-ext-install $mc bz2 \
    && :\
    && docker-php-ext-install $mc zip \
    && docker-php-ext-install $mc pcntl \
    && docker-php-ext-install $mc pdo_mysql \
    && docker-php-ext-install $mc mysqli \
    && docker-php-ext-install $mc mbstring \
    && docker-php-ext-install $mc exif \
    && docker-php-ext-install $mc sockets \
    && docker-php-ext-install bcmath
#    && :\
#    && apt install -y libmagickwand-dev \
#    && pecl install imagick-3.4.3 \
#    && docker-php-ext-enable imagick \
#    && :\
#    && apt install -y libmemcached-dev zlib1g-dev \
#    && pecl install memcached-2.2.0 \
#    && docker-php-ext-enable memcached

# redis扩展
ENV PHPREDIS_VERSION 3.1.3
RUN curl -L -o /tmp/redis.tar.gz https://github.com/phpredis/phpredis/archive/$PHPREDIS_VERSION.tar.gz \
    && tar xfz /tmp/redis.tar.gz \
    && rm -r /tmp/redis.tar.gz \
    && mkdir -p /usr/src/php/ext \
    && mv phpredis-$PHPREDIS_VERSION /usr/src/php/ext/redis \
    && docker-php-ext-install redis \
    && rm -rf /usr/src/php #如果这段不加构建的镜像将大100M

# amqp
RUN  apt install -y librabbitmq-dev  \
     && curl -L -o /tmp/amqp.tgz  'http://pecl.php.net/get/amqp-1.9.3.tgz' \
     && pecl install /tmp/amqp.tgz \
     && rm -r /tmp/amqp.tgz \
     && docker-php-ext-enable amqp

# composer
RUN curl -sS https://getcomposer.org/installer \
    | php -- --install-dir=/usr/local/bin --filename=composer

#RUN pecl install yaf-2.2.9 && docker-php-ext-enable yaf

