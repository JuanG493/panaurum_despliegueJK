FROM php:8.2-fpm

# Install required dependencies
RUN apt-get update && apt-get install -y git unzip curl

# Clone repository using HTTPS with token
ARG GITHUB_TOKEN
ARG BACKEND_REPO_URL
RUN git clone --depth=1 https://${GITHUB_TOKEN}@github.com/${BACKEND_REPO_URL}.git /app

WORKDIR /app

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install dependencies
RUN composer install --no-interaction

# Copy .env.example to .env if .env doesn't exist
RUN if [ ! -f .env ]; then cp .env.example .env; fi

# Generate application key
RUN php artisan key:generate

# Set permissions
RUN chown -R www-data:www-data /app/storage /app/bootstrap/cache

# Expose port
EXPOSE 8000

# Start Laravel server
CMD ["php", "artisan", "serve", "--host=0.0.0.0", "--port=8000"]
