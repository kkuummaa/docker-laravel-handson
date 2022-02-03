# Dockerfileはテキストファイルであり、Dockerイメージを作り上げるために実行する命令をこのファイルに含めることができます。
# FROM命令はイメージビルドのためのベースイメージを設定します。FROM イメージ名:タグ名 で指定します。
FROM php:8.0-fpm-buster 
SHELL ["/bin/bash", "-oeux", "pipefail", "-c"]

# ENV命令はコンテナ内のサーバー環境変数を設定します。
ENV COMPOSER_ALLOW_SUPERUSER=1 \
  COMPOSER_HOME=/composer

COPY --from=composer:2.0 /usr/bin/composer /usr/bin/composer

# apt-get update インストール可能なパッケージの「一覧」を更新します。実際のパッケージのインストール、アップグレードなどは行いません。
RUN apt-get update && \
  # apt-get -y install xxx Laravelのインストールに必要なパッケージをインストールします。
  apt-get -y install git unzip libzip-dev libicu-dev libonig-dev && \
  # apt-get clean && rm -rf /var/lib/apt/lists/* ここはパッケージインストールで使用したキャッシュファイルを削除しています。
  apt-get clean && \
  rm -rf /var/lib/apt/lists/* && \
  # PHPの拡張ライブラリをインストールしています。
  docker-php-ext-install intl pdo_mysql zip bcmath

COPY ./php.ini /usr/local/etc/php/php.ini

WORKDIR /work