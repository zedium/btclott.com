FROM php:8.3-fpm

# نصب ابزارها
RUN apt-get update && apt-get install -y \
    git \
    unzip \
    libzip-dev \
    libicu-dev \
    libonig-dev \
    libpng-dev \
    libxml2-dev \
    mariadb-client \
    && docker-php-ext-install \
        pdo \
        pdo_mysql \
        intl \
        zip \
        opcache

# نصب Composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer
COPY php.ini /usr/local/etc/php/conf.d/custom.ini
# دریافت UID و GID از docker-compose و پیش‌فرض
ARG UID=1000
ARG GID=1000

# بررسی مقادیر و ایجاد گروه و کاربر
RUN if [ -z "${GID}" ] || [ -z "${UID}" ]; then \
        echo "UID or GID is empty, using 1000"; \
        UID=1000; GID=1000; \
    fi && \
    groupadd -g ${GID} appuser && \
    useradd -m -u ${UID} -g appuser appuser

# دایرکتوری کاری
WORKDIR /var/www/html
RUN git config --global --add safe.directory /var/www/html
# تغییر به کاربر appuser
USER appuser
