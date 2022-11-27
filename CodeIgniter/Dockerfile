FROM php:7.3-apache
RUN apt -y update && apt -y install git zip unzip libmcrypt-dev libpng-dev libbz2-dev libcurl4-openssl-dev \
    net-tools nano less wget gettext libxml2-dev libxslt-dev libzip-dev libmemcached-dev zlib1g-dev mariadb-server
RUN pecl install igbinary mcrypt memcached \
    && docker-php-ext-install mysqli gd mbstring bz2 calendar pdo ctype curl exif fileinfo gd hash json \
    mbstring pcntl pdo_mysql posix session shmop sockets sysvmsg sysvsem sysvshm tokenizer zip gettext wddx xml xsl  \
    && docker-php-ext-enable memcached mysqli pdo mcrypt gd mbstring bz2 calendar ctype curl exif fileinfo gd hash json \
    mbstring pcntl pdo_mysql posix session shmop sockets sysvmsg sysvsem sysvshm tokenizer zip gettext igbinary wddx xml xsl 
RUN php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer
COPY apache2.conf /etc/apache2/apache2.conf
COPY 000-default.conf /etc/apache2/sites-available/000-default.conf
COPY .htaccess /var/www/html/.htaccess
RUN a2enmod rewrite && a2enmod headers && a2enmod expires && service apache2 restart
RUN mv wkhtmltox/bin/wkhtmltopdf /usr/local/bin/wkhtmltopdf
EXPOSE 80