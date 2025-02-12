# 使用 PHP 8.2 + Apache 官方映像
FROM php:8.2-apache

# 更新並安裝必要的 PHP 擴展和工具
RUN apt-get update && apt-get install -y \   更新 apt 套件管理器 && 安裝 PHP 需要的函式庫與工具
  以下是 Laravel 與 PHP 相關的函式庫：
  libpng-dev \   支援圖片處理（GD 擴展）。
  libxml2-dev \  與 XML 處理相關。
  zip \          允許 PHP 壓縮和解壓縮 zip 文件（Composer 需要）。
  unzip \        
  curl \         用來下載遠端檔案（像是 Composer 或 Laravel）。
  git \          Composer 可能需要 Git 來拉取 Laravel 的依賴。
  && docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd  
  
  以上最後一個是安裝 PHP 擴展: 
  pdo_mysql：MySQL 資料庫的 PDO 擴展。
  mbstring：處理 UTF-8、多字節字符（Laravel 必須）。
  exif：允許讀取圖像的 EXIF 資訊（可選）。
  pcntl：用於 CLI 多進程處理（可選）。
  bcmath：高精度數學運算（可選，Laravel 可能用到）。
  gd：圖像處理（用於上傳圖片的處理）。 

# 啟用 Apache mod_rewrite（Laravel 需要） 
# Laravel 需要 mod_rewrite 來處理 .htaccess 規則。a2enmod 是 Ubuntu 的 Apache 指令，用來開啟 Apache 模組。
RUN a2enmod rewrite

# 安裝 Composer : 這段的意思是 從 composer 官方映像直接複製 composer 命令到這個容器。
# 這樣我們就不需要手動安裝 Composer，節省步驟並確保使用最新版本。
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# 設定工作目錄
# 這告訴 Docker，從現在開始 所有的指令都會在 /var/www/html 這個目錄內執行。這是 Laravel 預設的應用程式目錄，Apache 也會預設讀取這裡的 index.php。
WORKDIR /var/www/html

# 設定 Laravel 權限（當 Laravel 專案已存在時）
RUN chown -R www-data:www-data /var/www/html \
  && chmod -R 775 /var/www/html/storage /var/www/html/bootstrap/cache

# 預設執行 Laravel 相關指令
CMD ["apache2-foreground"]