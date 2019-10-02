FROM php:7-fpm

ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /composer
ENV PATH $PATH:/composer/vendor/bin
ENV LANG C.UTF-8
ENV LANGUAGE en_US:

RUN apt-get update \
  && apt-get install -y libzip-dev zlib1g-dev unzip vim zip libpq-dev \
  # Dockerコンテナ内でIlluminate\Http\UploadedFileクラスを利用するには PHP GD が必要
  libpng-dev libjpeg-dev \
  && pecl install xdebug \
  && docker-php-ext-enable xdebug \
  # Dockerコンテナ内でIlluminate\Http\UploadedFileクラスを利用するには PHP GD が必要
  && docker-php-ext-configure gd --with-png-dir=/usr/include --with-jpeg-dir=/usr/include \
  && docker-php-ext-install gd zip pdo_mysql \
  # composer
  && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
  && php -r "if (hash_file('sha384', 'composer-setup.php') === trim(file_get_contents('https://composer.github.io/installer.sig'))) { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
  && php composer-setup.php \
  && php -r "unlink('composer-setup.php');" \
  && mv composer.phar /usr/local/bin/composer \
  && composer global require hirak/prestissimo \
  && composer global require laravel/installer \
  # Install nvm
  && curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.0/install.sh | bash \
  && echo 'export NVM_DIR="$HOME/.nvm"' >> ~/.bashrc \
  && echo '[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm' >> ~/.bashrc \
  # Install node.js lts version
  && bash -c 'source $HOME/.nvm/nvm.sh && nvm install --lts && nvm alias default lts/* && nvm cache clear && node -v'

# port
EXPOSE 22 80 443
