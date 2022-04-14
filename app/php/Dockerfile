FROM php:8.1.4-fpm-alpine

RUN curl --silent --show-error https://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer

# 官方源替换为北外源解决国内鸡鸡访问过慢问题
# RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.bfsu.edu.cn/g' /etc/apk/repositories

RUN apk --upgrade add --no-cache freetype-dev \
    zlib-dev libzip-dev \
    libpng-dev libwebp-dev \
    freetype-dev libxpm-dev \
    libpng-dev sqlite-dev \
    libjpeg-turbo-dev \
    tzdata

RUN docker-php-ext-configure gd \
    --with-jpeg \
    --with-freetype

RUN pecl install redis \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install pdo_sqlite \
    && docker-php-ext-install sockets \
    && docker-php-ext-install gd \
    && docker-php-ext-enable redis

RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

RUN echo -e '[PHP]\ndate.timezone = "Asia/Shanghai"\n' > /usr/local/etc/php/conf.d/tzone.ini

COPY ./run.sh ./

ENV LANG C.UTF-8

ENTRYPOINT ["sh", "./run.sh"]
