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
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

# clone moodle
RUN git clone https://github.com/moodle/moodle -b $MOODLE_VERSION .

RUN { \
    echo "<?php"; \
    echo "unset(\$CFG);"; \
    echo "global \$CFG;"; \
    echo "\$CFG = new stdClass();"; \
    echo "\$CFG->dbtype = getEnv('MOODLE_DBTYPE');"; \
    echo "\$CFG->dbhost = getEnv('MOODLE_DBHOST');"; \
    echo "\$CFG->dbname = getEnv('MOODLE_DBNAME');"; \
    echo "\$CFG->dbuser = getEnv('MOODLE_DBUSER');"; \
    echo "\$CFG->dbpass = getEnv('MOODLE_DBPASSWORD');"; \
    echo "\$CFG->dboptions = array("; \
    echo "    'dbport' => getEnv('MOODLE_DBPORT'),"; \
    echo "    );"; \
    echo "\$CFG->wwwroot = getEnv('MOODLE_WWWROOT_URL');"; \
    echo "\$CFG->dirroot = '/var/www/html';"; \
    echo "\$CFG->dataroot = '/var/www/moodledata';"; \
    echo "\$CFG->admin = 'admin';"; \
    echo "\$CFG->unicodedb = true;"; \
    echo "require_once(\"\$CFG->dirroot/lib/setup.php\");"; \
} > /var/www/html/config.php
