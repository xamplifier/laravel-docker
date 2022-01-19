FROM php:8.0-fpm
LABEL maintainer="George Dimosthenous"

# Set working directory
WORKDIR /var/www

ARG UID
ARG GID

# Install dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    apt-utils \
    procps \
    libpng-dev \
    libc-client-dev \
    libkrb5-dev \
    libzip-dev \
    libxml2-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    locales \
    zip unzip \
    jpegoptim optipng pngquant gifsicle \
    vim

# Clear cache
RUN apt-get clean \
    && apt-get -y autoremove \
    && rm -rf /var/lib/apt/lists/*

# Install extensions
RUN docker-php-ext-install mysqli \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
    && docker-php-ext-install imap \
    && docker-php-ext-install zip \
    && docker-php-ext-install bcmath \
    && docker-php-ext-install soap \
    && docker-php-ext-install exif \
    && docker-php-ext-install pcntl \
    && docker-php-ext-configure gd --with-jpeg=/usr/include/ --with-freetype=/usr/include/ \
    && docker-php-ext-install gd \
    && pecl install redis \
    && pecl install mongodb \
    && docker-php-ext-enable redis

# strings
RUN apt-get update \
    && apt-get install -y libonig-dev \
    && docker-php-ext-install -j$(nproc) \
	    gettext \
	    mbstring


# images
RUN apt-get update \
	&& apt-get install -y \
	libfreetype6-dev \
	libjpeg62-turbo-dev \
	libpng-dev \
	libgd-dev \
	&& docker-php-ext-configure gd --with-freetype --with-jpeg \
	&& docker-php-ext-install -j$(nproc) \
		gd \
		exif

# ssh2
RUN apt-get update \
	&& apt-get install -y \
	libssh2-1-dev

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN usermod -u $UID www-data
RUN groupmod -g $GID www-data

USER www-data
