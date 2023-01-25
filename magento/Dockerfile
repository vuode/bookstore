FROM php:7.4-apache

# Install unzip, vim
RUN apt-get update && apt-get install -y unzip cron vim

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- \
    --install-dir=/usr/local/bin --filename=composer

# Install docker-php-extension-installer
ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

# Fix permissions and install php extensions
RUN chmod +x /usr/local/bin/install-php-extensions && \
    install-php-extensions gd soap bcmath intl pdo_mysql xsl zip sockets

# Configure apache
RUN a2enmod rewrite
COPY ./000-default.conf /etc/apache2/sites-available/000-default.conf

# Download magento2
RUN curl https://github.com/magento/magento2/archive/refs/tags/2.4.5.zip -OL && \
    unzip 2.4.5.zip && mv magento2-2.4.5 magento2

WORKDIR /var/www/html/magento2

RUN composer update && composer install

# Fix permissions
RUN chown -R www-data:www-data /var/www/html/magento2

RUN service cron start
