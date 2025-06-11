FROM php:7.4-fpm
RUN sed -i 's/deb.debian.org/mirrors.ustc.edu.cn/g' /etc/apt/sources.list \
    && sed -i 's|security.debian.org/debian-security|mirrors.ustc.edu.cn/debian-security|g' /etc/apt/sources.list \
    && apt-get update \
    # 相关依赖必须手动安装
    && apt-get install -y --no-install-recommends \
        --fix-missing \
        -o Acquire::Retries=5 \
        -o Acquire::http::Timeout=60 \
        -o APT::Acquire::Retries=5 \
        -o DPkg::options::="--force-confdef" \
        -o DPkg::options::="--force-confold" \
        libavcodec-dev \
        libavutil-dev \
        libavformat-dev \
        libswscale-dev \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        libmemcached-dev \ 
        ffmpeg \
        zlib1g-dev \
        libzip-dev \
        wget \
        unzip \
        libgmp-dev \
        procps && \
    apt-get -f install -y
# 安装扩展
RUN docker-php-source extract \
&& docker-php-ext-install pdo_mysql iconv zip mysqli bcmath gmp\
    # 如果安装的扩展需要自定义配置时
    && docker-php-ext-configure gd --with-freetype=/usr/include/ \
    --with-jpeg=/usr/include/   \
    && docker-php-ext-install gd zip \
&& pecl install redis memcached\
&& docker-php-ext-enable redis memcached\ 
&& docker-php-source delete

# 安装 php screw plus  https://codeload.github.com/del-xiong/screw-plus/zip/master
RUN cd / && wget https://codeload.github.com/del-xiong/screw-plus/zip/master && \
unzip master && \
cd /screw-plus-master && phpize && ./configure && \
sed -i 's/FwWpZKxH7twCAG4JQMO/uywdwdwdvcedfee/g' ./php_screw_plus.h && make && \
cp /screw-plus-master/modules/php_screw_plus.so /usr/local/lib/php/extensions/no-debug-non-zts-20190902/ && \
echo "extension=php_screw_plus.so" >> /usr/local/etc/php/php.ini && \
cd /screw-plus-master/tools && make && \
rm -f /master && rm -rf /screw-plus-master

#安装composer
RUN php -r "readfile('https://getcomposer.org/installer');" > composer-setup.php && php composer-setup.php && php -r "unlink('composer-setup.php');" && cp composer.phar /usr/local/bin/composer && composer config -g repo.packagist composer https://packagist.phpcomposer.com

# 安装git
RUN apt-get update && apt-get install -y git