FROM php:7.1-apache

RUN apt-get update && apt-get install -y

# MySQL
RUN apt-get install -y mariadb-client libmariadb-dev-compat libmariadb-dev && docker-php-ext-install mysqli

# PDO
RUN docker-php-ext-install pdo pdo_mysql

# GD for image processing
RUN apt-get update && apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev \
    libpng-dev \
    && docker-php-ext-install -j$(nproc) iconv mcrypt \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd

# Mail
RUN apt-get update && apt-get install -y sendmail

# Enable rewrite rules (ie. to route all requests to index.php)
RUN a2enmod rewrite
WORKDIR /var/www/html
RUN apt -y update
RUN apt install -y wget mariadb-client unzip
RUN mkdir /var/www/tmp
RUN mkdir /var/www/logs
RUN rm -f /var/www/html/index.html
RUN wget https://github.com/joomla/joomla-cms/releases/download/3.6.4/Joomla_3.6.4-Stable-Full_Package.zip -P /var/tmp/
ADD wait-for-it.sh /usr/local/bin/wait-for-it.sh
RUN chmod +x /usr/local/bin/wait-for-it.sh
ADD firstrun.sh /usr/local/bin/firstrun.sh
RUN chmod +x /usr/local/bin/firstrun.sh
RUN sed -i '3i/usr/local/bin/firstrun.sh' /usr/local/bin/docker-php-entrypoint
