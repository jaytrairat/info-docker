# info-docker

## PHP Dockerfile

```Dockerfile
# Base image
FROM php:8.2.4-apache

# Set working directory
WORKDIR /var/www/html

# Install dependencies
RUN apt-get update 
RUN apt-get install -y libzip-dev zip unzip 
RUN docker-php-ext-install pdo_mysql zip

# Enable Apache rewrite module
RUN a2enmod rewrite

# Copy project files to the container
COPY . /var/www/html

# Set file permissions
RUN chown -R www-data:www-data /var/www/html 
RUN chmod -R 755 /var/www/html/storage

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install project dependencies
RUN composer install --optimize-autoloader --no-dev

# Generate application key
RUN php artisan key:generate

# Expose port 80
EXPOSE 80

# Start Apache server
CMD ["apache2-foreground"]
```

## PHP Docker Run

```sh
docker run -d -p xxxx:80 --add-host=host.docker.internal:host-gateway xxxx
```
