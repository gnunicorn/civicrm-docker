FROM michaelmcandrew/civicrm-base

ENV CIVICRM_UF=WordPress

ARG CIVICRM_VERSION

RUN curl https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar -o /usr/local/bin/wp \
  && chmod +x /usr/local/bin/wp

USER civicrm

RUN wp core download

RUN mkdir /var/www/html/wp-content/uploads

RUN cd /var/www/html/wp-content/plugins \
  && curl -L https://download.civicrm.org/civicrm-$CIVICRM_VERSION-wordpress.zip > civicrm-wordpress.zip \
  && unzip civicrm-wordpress.zip \
  && rm civicrm-wordpress.zip

RUN cd /var/www/html/wp-content/plugins \
  && curl -L https://download.civicrm.org/civicrm-$CIVICRM_VERSION-l10n.tar.gz > civicrm-l10n.tar.gz \
  && tar xzf civicrm-l10n.tar.gz \
  && rm civicrm-l10n.tar.gz

COPY --chown=civicrm:civicrm ./wp-config.php /var/www/html

COPY --chown=civicrm:civicrm ./civicrm.settings.php /var/www/html

COPY --chown=civicrm:civicrm ./.htaccess /var/www/html

# Help cv find the civicrm.settings.php file
ENV CIVICRM_SETTINGS=civicrm.settings.php

USER root

COPY ./init ./dump ./civicrm-docker-wp-plugin-install /usr/local/bin/
