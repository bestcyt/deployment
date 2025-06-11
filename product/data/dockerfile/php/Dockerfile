FROM php:7.4-fpm
RUN sed -i 's/deb.debian.org/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
RUN sed -i 's|security.debian.org/debian-security|mirrors.ustc.edu.cn/debian-security|g' /etc/apt/sources.list 
RUN apt-get update \
	# 相关依赖必须手动安装
	&& apt-get install -y \
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
        procps
# 安装扩展
RUN docker-php-source extract \
&& docker-php-ext-install pdo_mysql iconv zip mysqli \
    # 如果安装的扩展需要自定义配置时
    && docker-php-ext-configure gd --with-freetype=/usr/include/ \
    --with-jpeg=/usr/include/   \
    && docker-php-ext-install gd zip \
&& pecl install redis memcached \
&& docker-php-ext-enable redis memcached \ 
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