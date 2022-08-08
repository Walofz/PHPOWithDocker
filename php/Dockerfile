FROM php:8.1.9-fpm-bullseye  

LABEL name="Walofz"

# Set Timezone
ENV TZ=Asia/Bangkok
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Install Dependencies
RUN apt-get update && apt-get install -y locales unixodbc libgss3 odbcinst iputils-ping \
    devscripts debhelper dh-exec dh-autoreconf libreadline-dev libltdl-dev libfreetype6 libfreetype6-dev \
    unixodbc-dev wget unzip libzip-dev zip libjpeg62-turbo-dev libpng-dev libjpeg62-turbo-dev \
    && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-install pdo mysqli pdo_mysql \
    && echo "en_US.UTF-8 UTF-8" > /etc/locale.gen

# Add Microsoft repo for Microsoft ODBC Driver 17 for Linux
RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - \
    && curl https://packages.microsoft.com/config/debian/11/prod.list > /etc/apt/sources.list.d/mssql-release.list
RUN apt-get update && ACCEPT_EULA=Y apt-get install -y apt-transport-https msodbcsql17 \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd zip


# Enable the php extensions.
RUN pecl install pdo_sqlsrv-5.10.0 sqlsrv-5.10.0 && docker-php-ext-enable pdo_sqlsrv sqlsrv 

# Install needed php extensions: ldap opcache postgreSQL
RUN apt-get update && apt-get install libldap2-dev libpq-dev -y && \
    rm -rf /var/lib/apt/lists/* && \
    docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ && \
    docker-php-ext-install ldap opcache pdo_pgsql

# Enable SSL
RUN sed -i -E 's/(CipherString\s*=\s*DEFAULT@SECLEVEL=)2/\11/' /etc/ssl/openssl.cnf

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

CMD ["php-fpm", "-F"]

EXPOSE 9000   
