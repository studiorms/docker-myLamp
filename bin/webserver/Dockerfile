FROM php:7.4-apache

ARG DEBIAN_FRONTEND=noninteractive

# Install useful tools and install important libaries
RUN apt-get -y update && \
    apt-get -y --no-install-recommends install nano wget dialog && \
    apt-get -y --no-install-recommends install zlib1g-dev libzip-dev libicu-dev && \
#    apt-get -y --no-install-recommends install --fix-missing apt-utils build-essential git curl && \
#    apt-get -y --no-install-recommends install --fix-missing libcurl4-openssl-dev zip unzip openssl && \
    apt-get -y --no-install-recommends install --fix-missing \
        iputils-ping \
        apt-utils \
        build-essential \
        git \
        curl \
        libpng-dev \
        libonig-dev \
        libxml2-dev \
        libxml2 \
        zip \
        unzip \
        openssl

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install xdebug
RUN pecl install xdebug && \
    docker-php-ext-enable xdebug

# Install Redis
RUN pecl install -o -f redis \
    &&  rm -rf /tmp/pear \
    &&  docker-php-ext-enable redis

# Install Freetype
RUN apt-get -y update && \
    apt-get --no-install-recommends install -y libfontconfig1 libfontconfig1-dev libfreetype6 libfreetype6-dev libjpeg62-turbo-dev libpng-dev && \
    rm -rf /var/lib/apt/lists/* && \
    docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ && \
    docker-php-ext-install -j$(nproc) gd

# Other PHP7 Extensions
RUN docker-php-ext-install pdo_mysql bcmath xml && \
    docker-php-ext-install zip && \
    docker-php-ext-install -j$(nproc) intl && \
    docker-php-ext-install gettext

# Enable apache modules
RUN a2enmod rewrite headers

# Add local host as server name
RUN echo "ServerName localhost" | tee /etc/apache2/conf-available/fqdn.conf && a2enconf fqdn

# Install PhantomJS
# RUN cd ~ && \
#     export OPENSSL_CONF="/etc/pki/openssl10.cnf" && \
#     export PHANTOM_JS="phantomjs-2.1.1-linux-x86_64" && \
#     wget https://bitbucket.org/ariya/phantomjs/downloads/$PHANTOM_JS.tar.bz2 && \
#     tar xvjf $PHANTOM_JS.tar.bz2 && \
#     mv $PHANTOM_JS /usr/local/share && \
#     ln -sf /usr/local/share/$PHANTOM_JS/bin/phantomjs /usr/local/bin && \
#     phantomjs --version

# Install mode
RUN curl -sL https://deb.nodesource.com/setup_14.x  | bash -
RUN apt-get -y update && apt-get -y --no-install-recommends install --fix-missing nodejs && \
    node -v && \
    npm -v

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/* && rm -rf /usr/src/*

# Configure volume/workdir
WORKDIR /var/www/
