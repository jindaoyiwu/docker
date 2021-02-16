FROM centos:7 

ENV PHP_INI_DIR /usr/local/php/etc

RUN yum -y install gcc gcc-c++ kernel-devel libxml2 libxml2-devel openssl openssl-devel curl-devel libjpeg-devel libpng-devel freetype-devel libmcrypt-devel libzip-devel pcre-devel sqlite-devel bzip2 bzip2-devel

RUN yum install -y https://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/o/oniguruma-6.8.2-1.el7.x86_64.rpm && yum install -y https://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/o/oniguruma-devel-6.8.2-1.el7.x86_64.rpm
RUN yum -y install wget
RUN cd /tmp && wget https://www.php.net/distributions/php-7.4.7.tar.xz && xz -d php-7.4.7.tar.xz && tar -xvf php-7.4.7.tar && pwd

RUN cd /tmp/php-7.4.7 \
	&& ./configure \
	--prefix=/usr/local/php74 \
    --with-config-file-path="$PHP_INI_DIR" \
    --with-pear --enable-bcmath --with-curl --enable-gd --with-libdir=lib64 --with-bz2 --with-zlib --enable-soap --enable-sockets --with-mysqli --with-pdo-mysql  --with-curl \
    --enable-fpm --enable-opcache \
    --enable-mbstring --enable-sockets --enable-gd --with-openssl \
    && make && make install

RUN cp /tmp/php-7.4.7/php.ini-production /usr/local/php74/etc/php.ini
# 如有需要可以打开
RUN cp /usr/local/php74/etc/php-fpm.conf.default /usr/local/php74/etc/php-fpm.conf \
	&& cp /usr/local/php74/etc/php-fpm.d/www.conf.default /usr/local/php74/etc/php-fpm.d/www.conf

RUN source /etc/profile
RUN ln -s /usr/local/php74/bin/php /usr/local/bin
RUN php -m
RUN php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');" && php composer-setup.php && php -r "unlink('composer-setup.php');" && mv composer.phar /usr/local/bin/composer && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
#RUN pecl channel-update pecl.php.net && \ pecl install -o -f redis && \ echo extension=redis.so >> /usr/local/php74/etc/php.ini 
RUN rm -rf /tmp/php-7.4.7.tar
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
CMD /usr/local/php74/sbin/php-fpm -F
ENTRYPOINT [] WORKDIR /workdir