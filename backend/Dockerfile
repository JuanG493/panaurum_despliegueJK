FROM php:8.2-apache

# Instalar extensiones necesarias
RUN apt-get update && apt-get install -y \
    unzip \
    git \
    curl \
    libpq-dev \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    && docker-php-ext-configure gd \
    && docker-php-ext-install gd pdo pdo_mysql

# Instalar Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Clone repository using HTTPS with token
ARG GITHUB_TOKEN
ARG BACKEND_REPO_URL
RUN git clone --depth=1 https://${GITHUB_TOKEN}@github.com/${BACKEND_REPO_URL}.git /var/www/html || \
    (cd /var/www/html && git fetch origin && git reset --hard origin/main)

# Configurar directorio de trabajo
WORKDIR /var/www/html

# Instalar dependencias de Laravel
RUN composer install --no-dev --optimize-autoloader

# Establecer permisos adecuados
RUN chown -R www-data:www-data /var/www/html/storage /var/www/html/bootstrap/cache

# Habilitar mod_rewrite para Apache
RUN a2enmod rewrite

CMD ["apache2-foreground"]
