FROM php:8.0-fpm

# set main params
ENV APP_HOME /var/www/html
ENV USERNAME=www
WORKDIR $APP_HOME

# install dependencies
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

# Add user for Symfony application
RUN groupadd -g 1000 $USERNAME
RUN useradd -u 1000 -ms /bin/bash -g $USERNAME $USERNAME

# Copy existing application directory permissions
COPY --chown=$USERNAME:$USERNAME . $APP_HOME

# Change current user to www
USER $USERNAME

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]