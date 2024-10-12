FROM php:8.3-fpm
RUN adduser www-data root
RUN sed -i 's|^www-data:.*:/usr/sbin/nologin$|www-data:x:33:33:www-data:/var/www:/bin/bash|' /etc/passwd
RUN usermod -u 1000 www-data && groupmod -g 1000 www-data
RUN apt-get update && apt-get install -y \
    cron \
    default-mysql-client \
    git \
    gnupg \
    gzip \
    wget \
    libbz2-dev \
    libfreetype6-dev \
    libicu-dev \
    libjpeg62-turbo-dev \
    libmagickwand-dev \
    libmcrypt-dev \
    libonig-dev \
    libpng-dev \
    libsodium-dev \
    libssh2-1-dev \
    libwebp-dev \
    libxslt1-dev \
    libzip-dev \
    lsof \
    mailutils \
    msmtp \
    procps \
    vim \
    zip \
    default-jdk \
    default-jre \
    net-tools \
    vim \
    && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-configure \
    gd --with-freetype --with-jpeg --with-webp \
  && docker-php-ext-install \
    bcmath \
    calendar \
    exif \
    gd \
    gettext \
    intl \
    mbstring \
    mysqli \
    opcache \
    pcntl \
    pdo_mysql \
    soap \
    sockets \
    sodium \
    sysvmsg \
    sysvsem \
    sysvshm \
    xsl \
    zip

RUN pecl channel-update pecl.php.net && pecl install \
    redis \
    ssh2-1.3.1 \
  && pecl clear-cache \
  && rm -rf /tmp/pear \
  && docker-php-ext-enable \
      redis \ 
      ssh2 


RUN curl -sS https://getcomposer.org/installer | \
  php -- --install-dir=/usr/local/bin --filename=composer


RUN wget https://github.com/mailhog/mhsendmail/releases/download/v0.2.0/mhsendmail_linux_amd64 \
    && chmod +x mhsendmail_linux_amd64 \
    &&  mv mhsendmail_linux_amd64 /usr/local/bin/mhsendmail

COPY conf/php.ini /usr/local/etc/php/

#RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
#RUN curl -fsSL https://packages.microsoft.com/keys/microsoft.asc |  gpg --dearmor -o /usr/share/keyrings/microsoft-prod.gpg
#RUN curl https://packages.microsoft.com/config/debian/12/prod.list |  tee /etc/apt/sources.list.d/mssql-release.list

RUN apt-get update

#RUN ACCEPT_EULA=Y  apt-get install -y msodbcsql18 mssql-tools18

#RUN  apt-get install -y unixodbc-dev


#RUN  pecl install sqlsrv \
#    && pecl install pdo_sqlsrv
#    && printf "; priority=20\nextension=sqlsrv.so\n" > /etc/php/8.3/mods-available/sqlsrv.ini \
#    && printf "; priority=30\nextension=pdo_sqlsrv.so\n" > /etc/php/8.3/mods-available/pdo_sqlsrv.ini \
#    && phpenmod -v 8.3 sqlsrv pdo_sqlsrv

#RUN docker-php-ext-enable sqlsrv pdo_sqlsrv


RUN mkdir /var/www/.cache
RUN chmod 777 /var/www/.cache
#RUN groupmod --gid 1000 www-data
#RUN usermod -u 1000 www-data


WORKDIR /var/www/html


# Init per il CRON
COPY init.sh /init.sh
RUN chmod +x /init.sh

CMD ["/init.sh"]
