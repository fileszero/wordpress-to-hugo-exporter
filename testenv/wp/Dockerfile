FROM wordpress:latest

# internal FW settings
COPY mycerts.crt /usr/local/share/ca-certificates/
RUN update-ca-certificates

## PHP ERROR!! routines:tls_process_server_certificate:certificate verify failed
## php -r 'print_r(openssl_get_cert_locations());'
## php -a
## file_get_contents("https://downloads.wordpress.org/plugin/classic-editor.1.4.zip");
RUN curl https://curl.haxx.se/ca/cacert.pem > /usr/lib/ssl/cert.pem \
    && cat /usr/local/share/ca-certificates/mycerts.crt >> /usr/lib/ssl/cert.pem

# 必要なツール郡をインストール
RUN apt-get update \
    && apt-get -y install wget unzip

# SETUP XDebug
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug
COPY xdebug.ini /usr/local/etc/php/conf.d/


# WP プラグイン (zip) ダウンロード --no-check-certificate
WORKDIR /tmp/wp-plugins
RUN wget https://downloads.wordpress.org/plugin/classic-editor.1.4.zip
RUN wget https://downloads.wordpress.org/plugin/syntaxhighlighter.zip
RUN wget https://downloads.wordpress.org/plugin/wp-lightbox-2.zip
# Zip ファイルを解凍してインストール
RUN unzip './*.zip' -d /usr/src/wordpress/wp-content/plugins

# 不要になった一時ファイルを削除
RUN apt-get clean
RUN rm -rf /tmp/*

# サーバが読めるように wp-content 以下の所有者を変更
RUN chown -R www-data:www-data /usr/src/wordpress/wp-content

# wordpress 環境定義上書き
WORKDIR /var/www/html
COPY wp-config.php /var/www/html/
RUN chown www-data:www-data /var/www/html/wp-config.php

