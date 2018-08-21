FROM php:7.2-apache
MAINTAINER Andre Bubner<info@andre-bubner.de>

WORKDIR /var/www

COPY ./config/php72/php.ini /usr/local/etc/php/

# Get repository and install wget and vim
RUN apt-get update && apt-get install --no-install-recommends -y \
        wget \
        vim \
        git \
        unzip \
        nano

# Install PHP extensions deps
RUN apt-get update && apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    zlib1g-dev \
    libicu-dev \
    g++ \
    apt-utils \
    libbz2-dev \
    libmcrypt-dev \
    libmhash2 \
    libxml2-dev \
    libssl-dev \
    freetds-dev \
    software-properties-common \
    openssl


# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- \
        --install-dir=/usr/local/bin \
        --filename=composer


# Install PHP extensions
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && pecl install redis \
    && docker-php-ext-install \
            iconv \
            mbstring \
            intl \
            gd \
            mysqli \
            pdo_mysql \
            soap \
            sockets \
            zip \
            pcntl \
            ftp \
            json \
            zip \
    && docker-php-ext-enable \
            redis \
            opcache


RUN a2enmod rewrite
RUN mv /var/www/html /var/www/public
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf && \
    sed -i "s/variables_order.*/variables_order = \"EGPCS\"/g" /usr/local/etc/php/php.ini

ENV ALLOW_OVERRIDE **False**

# Install PHPUnit
RUN wget https://phar.phpunit.de/phpunit.phar -O /usr/local/bin/phpunit \
    && chmod +x /usr/local/bin/phpunit

# Add image configuration and scripts
ADD ./config/run.sh /run.sh
RUN chmod 755 /*.sh

EXPOSE 80
EXPOSE 443

# Clean repository
RUN apt-get clean \
    && rm -rf /var/lib/apt/lists/*

CMD ["/run.sh"]