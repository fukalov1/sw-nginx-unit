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

#RUN apt update && apt install -y --no-install-recommends \
#    php-mysql


#RUN apt install unit

#RUN apt install unit-dev unit-jsc17 unit-perl  \
#          unit-php unit-python3.11 unit-ruby unit-wasm

#RUN apt-get -qq install -y --no-install-recommends \
#  certbot \
#  python3-certbot-nginx \
#  php-common \
#  php-bcmath \
#  php-curl \
#  php-gd \
#  php-imagick \
#  php-mbstring \
#  php-mysql \
#  php-opcache \
#  php-xml \
#  php-zip

COPY ./*.pem /docker-entrypoint.d/
COPY ./*.json /docker-entrypoint.d/
COPY ./*.sh /docker-entrypoint.d/

ARG SITE_PATH=/var/www/app

USER root

RUN mkdir -p ${SITE_PATH} && \
chown -R unit:unit ${SITE_PATH} && \
chmod -R 775 ${SITE_PATH}

WORKDIR ${SITE_PATH}
