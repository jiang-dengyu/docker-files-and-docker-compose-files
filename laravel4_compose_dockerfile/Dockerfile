# 使用 PHP 8.2 官方映像
FROM php:8.2-fpm

# 更新並安裝必要的 PHP 擴展
RUN apt-get update && apt-get install -y \
  libpng-dev \
  libonig-dev \
  libxml2-dev \
  zip \
  unzip \
  curl \
  git \
  && docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd

# 安裝 Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# 設定工作目錄
WORKDIR /var/www/html

# 設定 Laravel 權限
RUN chown -R www-data:www-data /var/www/html \
  && mkdir -p /var/www/html/storage /var/www/html/bootstrap/cache \
  && chmod -R 775 /var/www/html/storage /var/www/html/bootstrap/cache

# 確保 Apache 日誌目錄存在
RUN mkdir -p /usr/local/apache2/logs/ \
  && touch /usr/local/apache2/logs/error.log /usr/local/apache2/logs/access.log \
  && chmod -R 777 /usr/local/apache2/logs/

# 預設啟動 PHP-FPM
CMD ["php-fpm"]