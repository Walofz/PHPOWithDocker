FROM walofz/php7.4.x-fpm:lastest

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

CMD ["php-fpm", "-F"]

EXPOSE 9000