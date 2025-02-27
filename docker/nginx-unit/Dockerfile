FROM unit:php8.2

# Обновим репозитории
RUN apt-get clean

RUN apt update && apt install -y curl apt-transport-https gnupg2 lsb-release  \
    &&  curl -o /usr/share/keyrings/nginx-keyring.gpg                         \
           https://unit.nginx.org/keys/nginx-keyring.gpg                      \
    && echo "deb [signed-by=/usr/share/keyrings/nginx-keyring.gpg]            \
            https://packages.nginx.org/unit/debian/ bullseye unit"    \
           > /etc/apt/sources.list.d/unit.list \
    && echo "deb-src [signed-by=/usr/share/keyrings/nginx-keyring.gpg]  \
            https://packages.nginx.org/unit/debian/ bullseye unit" \
           > /etc/apt/sources.list.d/unit.list

RUN apt-get update && apt-get install -y \
    libicu-dev \
    libjpeg-dev \
    libpng-dev \
    libfreetype6-dev \
    sudo \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install and enable the intl and gd extensions
RUN docker-php-ext-install pdo_mysql intl gd

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer --version

COPY ./*.pem /docker-entrypoint.d/
COPY ./*.json /docker-entrypoint.d/
COPY ./*.sh /docker-entrypoint.d/

ARG SITE_PATH=/var/www/app

USER root

RUN mkdir -p ${SITE_PATH} && \
chown -R unit:unit ${SITE_PATH} && \
chmod -R 775 ${SITE_PATH}

WORKDIR ${SITE_PATH}
