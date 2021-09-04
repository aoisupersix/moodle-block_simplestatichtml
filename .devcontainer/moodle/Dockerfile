FROM moodlehq/moodle-php-apache:7.4

ARG MOODLE_VERSION="v3.10.0"

ENV MOODLE_DBTYPE mariadb
ENV MOODLE_DBHOST localhost
ENV MOODLE_DBNAME moodle
ENV MOODLE_DBUSER moodle
ENV MOODLE_DBPASSWORD password
ENV MOODLE_PREFIX mdl_
ENV MOODLE_DBPORT 3306
ENV MOODLE_WWWROOT_URL http://localhost

# Set timezone
ENV TZ Asia/Tokyo
RUN echo "${TZ}" > /etc/timezone \
    && dpkg-reconfigure -f noninteractive tzdata

# Set locale
RUN echo 'ja_JP.UTF-8 UTF-8' >> /etc/locale.gen \
    && locale-gen \
    && update-locale LANG=ja_JP.UTF-8

# Install composer
COPY --from=composer /usr/bin/composer /usr/bin/composer

# Install xdebug
RUN pecl install xdebug-3.0.4 \
    && docker-php-ext-enable xdebug

USER www-data

# Clone moodle
RUN git clone https://github.com/moodle/moodle -b $MOODLE_VERSION --single-branch --depth 1 .

USER root
