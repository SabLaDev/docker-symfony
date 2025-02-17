FROM php:7.4.5-fpm

LABEL author="Khalid Sookia"
RUN apt-get update \
    && apt-get install -y --no-install-recommends vim curl debconf git apt-transport-https apt-utils \
    build-essential locales acl mailutils wget zip unzip supervisor libsodium-dev librabbitmq-dev \
    libpng-dev libicu-dev libzip-dev libxml2-dev libexif-dev wkhtmltopdf \
    gnupg gnupg1 gnupg2 \
    && printf "\n" | pecl install amqp \
    && docker-php-ext-install gd \
    && docker-php-ext-install intl \
    && docker-php-ext-install zip \
    && docker-php-ext-install pdo \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install xml \
    && docker-php-ext-install sockets \
    && docker-php-ext-install exif \
    && docker-php-ext-enable gd \
    && docker-php-ext-enable intl \
    && docker-php-ext-enable zip \
    && docker-php-ext-enable pdo \
    && docker-php-ext-enable pdo_mysql \
    && docker-php-ext-enable xml \
    && docker-php-ext-enable sockets \
    && docker-php-ext-enable exif \
    && docker-php-ext-enable amqp

COPY php-fpm-pool.conf /etc/php/7.4.5/pool.d/www.conf
COPY supervisor-messenger-invitation.conf /etc/supervisor/conf.d/supervisor-messenger-invitation.conf

RUN echo 'date.timezone="Europe/Madrid"' >> /usr/local/etc/php/conf.d/date.ini

RUN curl -sSk --silent --show-error https://getcomposer.org/installer | php -- --disable-tls && \
   mv composer.phar /usr/local/bin/composer

RUN groupadd dev -g 999
RUN useradd dev -g dev -d /home/dev -m

WORKDIR /var/www/bookie

RUN rm -rf /var/lib/apt/lists/*
RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && \
    echo "fr_FR.UTF-8 UTF-8" >> /etc/locale.gen && \
    locale-gen

EXPOSE 9000
CMD ["php-fpm"]
