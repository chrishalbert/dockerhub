FROM php:5.6-cli

RUN mkdir /var/sandbox

# Update and install
RUN apt update && \
    apt install curl git unzip vim -y

RUN cd ~

# Install xdebug
RUN curl https://xdebug.org/files/xdebug-2.5.5.tgz > xdebug.tgz && \
    tar xvzf xdebug.tgz && \
    cd xdebug-2.5.5 && \
    phpize && \
    ./configure --enable-xdebug && \
    make && \
    cp /usr/local/etc/php/php.ini-development /usr/local/etc/php/php.ini && \
    cp modules/xdebug.so /usr/local/etc/php/conf.d/ && \
    echo 'zend_extension=/usr/local/etc/php/conf.d/xdebug.so' >> /usr/local/etc/php/php.ini

# Set timezone
RUN sed "s/\[Date\]/\[Date\]\ndate\.timezone=\'America\/New_York\'/" /usr/local/etc/php/php.ini > /usr/local/etc/php/php.ini

# Install xsl extension
RUN apt install libxslt-dev -y && \
    docker-php-ext-install xsl && \
    docker-php-ext-enable xsl


# Install Composer
RUN curl -sS https://getcomposer.org/installer -o composer-setup.php && \
    HASH=a5c698ffe4b8e849a443b120cd5ba38043260d5c4023dbf93e1558871f1f07f58274fc6f4c93bcfd858c6bd0775cd8d1 && \
    php -r "if (hash_file('SHA384', 'composer-setup.php') === '$HASH') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" && \
    php composer-setup.php --install-dir=/usr/local/bin --filename=composer


WORKDIR /var/sandbox

CMD ["php", "-a"]
