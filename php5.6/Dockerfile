FROM php:5.6-fpm
LABEL maintainer="hong_lt@hotmail.com"

ENV TZ "Asia/Shanghai"

RUN set -x \
	&& apt-get update && apt-get install -y \
	nginx \
	# vim openssh-server \
	zlib1g-dev \
	&& apt-get purge -y --auto-remove

# RUN pecl install memcache \
#     && docker-php-ext-enable memcache
RUN curl 'http://pecl.php.net/get/memcache-2.2.7.tgz' -o memcache-2.2.7.tgz
RUN pecl install memcache-2.2.7.tgz \
    && docker-php-ext-enable memcache

# RUN pecl install redis && docker-php-ext-enable redis
RUN curl 'http://pecl.php.net/get/redis-2.2.8.tgz' -o redis-2.2.8.tgz
RUN pecl install redis-2.2.8.tgz \
    && docker-php-ext-enable redis

RUN docker-php-ext-install pcntl pdo_mysql

# RUN sed -i 's/PermitRootLogin without-password/PermitRootLogin yes/' /etc/ssh/sshd_config
# RUN echo "root:server123&*(" | chpasswd
# RUN mkdir -p /var/run/sshd
# # RUN /etc/init.d/ssh start

# ADD nginx/nginx.conf /etc/nginx/
ADD nginx/default /etc/nginx/sites-available/
RUN mkdir /etc/nginx/ssl
ADD nginx/ssl /etc/nginx/ssl

RUN echo "#!/bin/bash" > /home/start.sh
# RUN echo "nohup php ./dsmp/schemas/process.php > /dev/null &" >> /home/start.sh
# RUN echo "nohup php ./api/spserver/gameaccount/deductexpqueue.php > /dev/null &" >> /home/start.sh
RUN chmod +x /home/start.sh

EXPOSE 80
WORKDIR /home

# CMD php-fpm -D && nginx && /usr/sbin/sshd -D
# CMD php-fpm -D && nginx -g "daemon off;"
CMD ./start.sh && php-fpm -D && nginx -g "daemon off;"
