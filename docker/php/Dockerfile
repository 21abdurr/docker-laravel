# default environment variable = development
ARG ENVIRONMENT=development

# **********************************************************************************
# 1. Base Image
FROM php:8.4-fpm AS base

# Install system dependencies + PHP extensions
RUN apt-get update && apt-get install -y \
    git unzip libzip-dev \
    && docker-php-ext-install pdo_mysql zip

RUN curl -fsSL https://deb.nodesource.com/setup_22.x | bash - \
    && apt-get update \
    && apt-get install -y nodejs

# Install Composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html

# **********************************************************************************
# 2. Development Stage
FROM base AS development

# Install additional PHP extensions for development
RUN pecl install xdebug && docker-php-ext-enable xdebug

CMD ["php-fpm"]

# **********************************************************************************
# 3. Build Stage
FROM base AS build

COPY ./laravel /var/www/html/

RUN composer install --no-interaction --optimize-autoloader

RUN chown -R www-data:www-data /var/www/html \
    && chmod -R 775 storage bootstrap/cache
    
# **********************************************************************************
# 4. Production Stage
from base AS production

COPY --from=build /var/www/html /var/www/html

USER www-data

CMD ["php-fpm"]


# **********************************************************************************
# 5. Final Stage
from ${ENVIRONMENT} AS final