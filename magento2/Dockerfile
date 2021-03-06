FROM alexcheng/apache2-php7:7.0.24

ENV MAGENTO_VERSION 2.1.15
ENV INSTALL_DIR /var/www/html
ENV COMPOSER_HOME /var/www/.composer/
RUN curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer
COPY ./auth.json $COMPOSER_HOME

RUN requirements="libpng12-dev libmcrypt-dev libmcrypt4 libcurl3-dev libfreetype6 libjpeg-turbo8 libjpeg-turbo8-dev libpng12-dev libfreetype6-dev libicu-dev libxslt1-dev zip unzip wget" \
    && apt-get update \
    && apt-get install -y $requirements \
    && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd \
    && docker-php-ext-install mcrypt \
    && docker-php-ext-install mbstring \
    && docker-php-ext-install zip \
    && docker-php-ext-install intl \
    && docker-php-ext-install xsl \
    && docker-php-ext-install soap \
    && requirementsToRemove="libpng12-dev libmcrypt-dev libcurl3-dev libpng12-dev libfreetype6-dev libjpeg-turbo8-dev" \
    && apt-get purge --auto-remove -y $requirementsToRemove

#ADD ./apache2/apache2.conf /etc/apache2/sites-enabled/000-default.conf
#ADD ./apache2/ports.conf /etc/apache2/ports.conf

RUN chsh -s /bin/bash www-data

RUN rm -rf /var/www/html/*
RUN chown -R www-data:www-data /var/www
RUN su www-data -c "composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition $INSTALL_DIR $MAGENTO_VERSION"

RUN cd $INSTALL_DIR \
    && find . -type d -exec chmod 770 {} \; \
    && find . -type f -exec chmod 660 {} \; \
    && chmod u+x bin/magento

COPY ./install-magento /usr/local/bin/install-magento
RUN chmod +x /usr/local/bin/install-magento

COPY ./install-sampledata /usr/local/bin/install-sampledata
RUN chmod +x /usr/local/bin/install-sampledata

RUN wget https://codeload.github.com/CoinPaymentsNet/coinpayments-magento2/zip/feature/direct-mode
RUN unzip direct-mode
RUN mkdir -p /var/www/html/app/code/Coinpayments/CoinPayments && \
    cp -r ./coinpayments-magento2-feature-direct-mode/* /var/www/html/app/code/Coinpayments/CoinPayments/ && \
    rm direct-mode && rm -rf coinpayments-magento2-feature-direct-mode
RUN a2enmod rewrite
RUN echo "memory_limit=2048M" > /usr/local/etc/php/conf.d/memory-limit.ini

RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

WORKDIR $INSTALL_DIR

# Add cron job
#ADD crontab /etc/cron.d/magento2-cron
#RUN chmod 0644 /etc/cron.d/magento2-cron \
#    && crontab -u www-data /etc/cron.d/magento2-cron