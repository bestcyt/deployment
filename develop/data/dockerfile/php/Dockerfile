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
        procps
# 安装扩展
RUN docker-php-source extract \ 
&& docker-php-ext-install pdo_mysql iconv zip mysqli \
    # 如果安装的扩展需要自定义配置时
    && docker-php-ext-configure gd --with-freetype=/usr/include/ \
    --with-jpeg=/usr/include/   \
    && docker-php-ext-install gd \
&& pecl install redis memcached \
&& docker-php-ext-enable redis memcached \ 
&& docker-php-source delete