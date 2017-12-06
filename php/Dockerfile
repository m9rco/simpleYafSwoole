FROM php:7.0-fpm
MAINTAINER pushaowei <pushaowei@360.cn>
EXPOSE 80 443
ENV INSTALL_LIB_DEP="wget zip unzip"

# ------------------------- 镜像源转到国内
RUN sed -i 's/archive.ubuntu.com/mirrors.aliyun.com/g' /etc/apt/sources.list \
      && apt-get update --fix-missing \
      && apt-get -y upgrade

# ------------------------- 安装工具包
RUN apt-get install -y $INSTALL_LIB_DEP zlib1g-dev

# ------------------------- 安装GD  pdo_mysql zip opcache sockets库
RUN set -ex \
      && apt-get install -y libpng12-dev libjpeg-dev git\
      && rm -rf /var/lib/apt/lists/* \
      && docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr \
      && docker-php-ext-install gd pdo_mysql zip opcache\
      && docker-php-ext-install sockets

# ------------------------- 安装Redis
RUN set -ex \
      && cd /opt \
      && wget https://github.com/phpredis/phpredis/archive/3.0.0.zip -O redis-3.0.0.zip \
      && unzip redis-3.0.0.zip \
      && cd phpredis-3.0.0 \
      && phpize \
      && ./configure \
      && make && make install \
      && docker-php-ext-enable redis

# ------------------------- 安装Yaf
RUN set -ex \
      && cd /opt \
      && wget https://github.com/laruence/yaf/archive/yaf-3.0.4.zip -O yaf-3.0.4.zip \
      && unzip yaf-3.0.4.zip \
      && cd yaf-yaf-3.0.4 \
      && phpize \
      && ./configure \
      && make && make install \
      && docker-php-ext-enable yaf

# ------------------------- 安装Swoole
RUN set -ex \
      && cd /opt \
      && wget https://github.com/swoole/swoole-src/archive/v2.0.10-rc3.zip -O swoole-2.0.zip \
      && unzip swoole-2.0.zip \
      && cd swoole-src-2.0.10-rc3\
      && phpize \
      && ./configure \
      && make && make install \
      && docker-php-ext-enable swoole

# ------------------------- 清除安装包
RUN set -ex \
  && apt-get remove -y $INSTALL_LIB_DEP \
  && apt-get clean && rm -r /var/lib/apt/lists/* \
  && rm -rf /opt/*
