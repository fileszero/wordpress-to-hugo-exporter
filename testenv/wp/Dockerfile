FROM wordpress:latest

# internal FW settings
COPY mycerts.crt /usr/local/share/ca-certificates/
RUN update-ca-certificates

# 必要なツール郡をインストール
RUN apt-get update
RUN apt-get -y install wget unzip

# WP プラグイン (zip) ダウンロード
WORKDIR /tmp/wp-plugins
RUN wget --no-check-certificate https://downloads.wordpress.org/plugin/classic-editor.1.4.zip
RUN wget --no-check-certificate https://downloads.wordpress.org/plugin/syntaxhighlighter.zip
RUN wget --no-check-certificate https://downloads.wordpress.org/plugin/wp-lightbox-2.zip
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

