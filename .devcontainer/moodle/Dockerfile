FROM moodlehq/moodle-php-apache:7.4

ARG MOODLE_VERSION="v3.10.0"

ENV MOODLE_DBTYPE mariadb
ENV MOODLE_DBHOST localhost
ENV MOODLE_DBNAME moodle
ENV MOODLE_DBUSER moodle
ENV MOODLE_DBPASSWORD password
ENV MOODLE_DBPORT 3306
ENV MOODLE_WWWROOT_URL http://localhost

# Install composer
COPY --from=composer /usr/bin/composer /usr/bin/composer

# Install xdebug
RUN pecl install xdebug-2.9.8 \
    && docker-php-ext-enable xdebug

# clone moodle
RUN git clone https://github.com/moodle/moodle -b $MOODLE_VERSION .

RUN chown -R www-data:www-data /var/www/html
