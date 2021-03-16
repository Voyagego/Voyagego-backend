FROM php:8.0-fpm

# Set workdir
WORKDIR /var/www/

# Args
ARG user
ARG uid

# Install dependencies
RUN apt-get update && apt-get upgrade -y && apt-get install -y --no-install-recommends \
        htop \
        vim \
        git \
        zip \
        unzip \
        cron \
        wget \
        jpegoptim \
        pngquant \
        gifsicle \
        libicu-dev \
        zlib1g-dev \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libwebp-dev \
        libpng-dev \
        libxml2-dev \
        libssl-dev \
        libonig-dev \
        libpq-dev \
        libwebp-dev \
        libzip-dev

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install and configure extensions
RUN docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp && \
docker-php-ext-install pdo_pgsql opcache soap zip exif intl gd && \
pecl install xdebug && docker-php-ext-enable xdebug opcache

# Install composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Install symfony
RUN curl -sS https://get.symfony.com/cli/installer | bash && \
mv /root/.symfony/bin/symfony /usr/local/bin/symfony

# Create user to run composer and symfony commands
RUN useradd -G www-data,root -u $uid -d /home/$user $user && \
mkdir -p /home/$user/.composer && \
mkdir -p /home/$user/.symfony && \
chown -R $user:$user /home/$user

# Change current user to www
USER $user