FROM php:7.4-apache

RUN apt-get update && apt-get install -y \
    nano \
    gnupg2 \
    curl \
    zip \
    unzip \
    git \
    libzip-dev \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libxml2-dev \
    libonig-dev \
    libpq-dev \
    apt-transport-https software-properties-common \
    libldap2-dev \
    build-essential \
    pkg-config \
    libssl-dev \
    libcurl4-openssl-dev \
    libgssapi-krb5-2 \
    libltdl-dev \
    unixodbc \
    unixodbc-dev \
    wget \
    && docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu && \
    docker-php-ext-configure gd --with-freetype --with-jpeg && \
    docker-php-ext-install ldap pdo pdo_mysql pdo_pgsql mysqli mbstring exif pcntl gd zip

# Instala o driver ODBC da Microsoft (necessário para conectar ao SQL Server)
RUN curl -sSL https://packages.microsoft.com/keys/microsoft.asc | apt-key add - \
    && curl -sSL https://packages.microsoft.com/config/debian/10/prod.list -o /etc/apt/sources.list.d/mssql-release.list \
    && apt-get update \
    && ACCEPT_EULA=Y apt-get install -y msodbcsql17

# Instala extensões SQL Server (versão correta para o PHP 7.4)
RUN pecl install pdo_sqlsrv-5.9.0 sqlsrv-5.9.0 \
    && docker-php-ext-enable pdo_sqlsrv sqlsrv

# Compila e instala o OpenSSL 1.1.1p
RUN wget https://www.openssl.org/source/openssl-1.1.1p.tar.gz -O openssl-1.1.1p.tar.gz && \
    tar -zxvf openssl-1.1.1p.tar.gz && \
    cd openssl-1.1.1p && \
    ./config && \
    make -j$(nproc) && \
    make install && \
    ldconfig && \
    openssl version

RUN a2enmod rewrite

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer