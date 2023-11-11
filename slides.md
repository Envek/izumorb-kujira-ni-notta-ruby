---
theme: default
highlighter: shiki
lineNumbers: false
title: "クジラに乗ったRuby: Dockerを使って、便利な開発環境の構築"
author: Andrey Novikov
keywords:
  - Ruby on Rails
  - Docker
  - Docker Compose
  - local development environment
  - developer experience
  - ローカルの開発環境
drawings:
  persist: false
fonts:
  provider: none
  fallback: false
  local: Martian Grotesk, Martian Mono
  sans: Martian Grotesk
  serif: Martian Grotesk
  mono: Martian Mono
aspectRatio: '16/9'
mdc: true

transition: slide-left
---

# クジラに乗ったRuby

Dockerを使って、便利な開発環境の構築{class="text-3xl"}

<div class="absolute top-0 right-0 p-4 w-96">
  <img alt="鯨に乗ったルビー" src="/images/ruby-on-whales-logo.png"/>
</div>

<div class="absolute bottom-0 left-0 w-full px-14 py-8 flex justify-between items-end gap-4">
  <div class="text-left text-xl">
    Andrey Novikov, Evil Martians<br />
    <small><a href="https://izumonomadrubymeetup.peatix.com/">Izumo Ruby meet-up</a></small><br />
    <small><time datetime="2023-11-11">2023年11月11日</time></small>
  </div>

  <div class="flex gap-4">
  <div class="w-28 scaled-image justify-self-end">
    <a href="https://evilmartians.com/"><img alt="Evil Martians" src="/images/01_Evil-Martians_Logo_v2.1_RGB.svg" class="block dark:hidden" /><img alt="Evil Martians" src="/images/02_Evil-Martians_Logo_v2.1_RGB_for-Dark-BG.svg" class="hidden dark:block" /></a>
  </div>
  <div class="w-28 scaled-image justify-self-end self-center">
    <a href="https://www.sami-japan.com/"><img alt="SAMI Japan" src="/images/SAMI_logo.png" class="block" /></a>
  </div>
  </div>
</div>

<style>
  a {
    border-bottom: none !important;
  }
</style>

<!--
皆さん、こんにちは！

-->

---
layout: image-right
image: ./images/20230305_193526.jpg
class: annotated-list
---

## **アンドレイ**といいます

- バックエンドエンジニア
  
  <small>フロントエンドもできます</small>

- Ruby, Go, TypeScriptを使っています

  <small>SQL, Dockerfiles, TypeScript, bash…</small>

- オープンソースの大ファン

  <small>ルビージェムをいくつかを作成して、維持しています</small>

- １年前ロシアから日本に引っ越しました

  <small>大阪の近くに日本のくらしを楽しんでいます</small>

- 原付を乗っています。スーパーカブです。

  <small>ママチャリも乗って、子供を幼稚園に連れていきます。</small>

<div class="text-center absolute bottom-0">
<img src="/images/01_Evil-Martians_Logo_Lurkers_v2.0_on-Transparent.png" class="max-w-25% scaled-image mx-auto" />
</div>

<!--
自己紹介から始めます。アンドレイと申します。バックエンドエンジニアで、RubyやGoを使っています。オープンソースの大ファンで、いくつかのRubyのジェムを作りました。

もう一年以上大阪の近くに住んでいて、家族と一緒に日本のくらしを楽しんでいます。

よろしくお願いします！

I'm Andrey, back-end engineer at Evil Martians. I and my family are living in Japan for 1 year already, just a bit north of Osaka.
-->

---
layout: image
image: ./images/photo_2023-11-11_10-22-42.jpg
---


---

<a href="https://evilmartians.com/?utm_source=izumorb&utm_medium=slides&utm_campaign=kujira-ni-notta-ruby">
<img alt="Evil Martians" src="/images/01_Evil-Martians_Logo_v2.1_RGB.svg" class="block dark:hidden object-contain text-center m-auto max-h-100" />
<img alt="Evil Martians" src="/images/02_Evil-Martians_Logo_v2.1_RGB_for-Dark-BG.svg" class="hidden dark:block object-contain text-center m-auto max-h-100" />
<p class="text-2xl text-center">evilmartians.com</p>
</a>
<a href="https://evilmartians.jp/?utm_source=izumorb&utm_medium=slides&utm_campaign=kujira-ni-notta-ruby">
<p class="text-xl text-center">🇯🇵 evilmartians.jp 🇯🇵</p>
</a>
<div class="absolute bottom-32px left-32px rotate-10 text-2xl">邪悪な火星人？</div>
<div class="absolute bottom-32px right-32px rotate-350 text-2xl">イービルマーシャンズ！</div>

<!--
さらに自分は火星人です。我々は、平和目的で地球に来ました。

真面目に言うと、「イービル・マーシャンズ」という会社に勤めています。

我々は開発者ツールのスタートアップと協力して、それらのスタートアップをユニコーンに変え、素晴らしいオープンソースプロジェクトも開発します。いろいろなスタートアップや大企業にコンサルティングもしています。バックエンドをもちろん、フロントエンドやデザインも含めてプロダクトをターンキー開発しています。

イービル・マーシャンズは元々からRuby on Railsに集中する開発ショップとして知られてきましたが、今はそれをはるかに超えています。
-->

---
layout: image
image: ./images/osaka-izumo-distance-map.png
---

---

# 火星で作ったオープンソース

<div class="grid grid-cols-4 grid-rows-2 gap-4">
  <a href="https://github.com/yabeda-rb/yabeda">
    <figure>
      <img alt="Yabeda" src="/images/martian-oss/yabeda.svg" class="scaled-image h-36 mx-auto" />
      <figcaption>Yabeda: Ruby application instrumentation framework</figcaption>
    </figure>
  </a>
  <a href="https://github.com/evilmartians/lefthook">
    <figure>
      <img alt="LeftHook" src="/images/martian-oss/lefthook.svg" class="scaled-image h-36 mx-auto" />
      <figcaption>Lefthook: git hooks manager</figcaption>
    </figure>
  </a>
  <a href="https://anycable.io/">
    <figure>
      <img alt="AnyCable" src="/images/martian-oss/anycable.svg" class="scaled-image h-36 mx-auto" />
      <figcaption>AnyCable: a real-time server for Rails and beyound</figcaption>
    </figure>
  </a>
  <a href="https://postcss.org/">
    <figure>
      <img alt="PostCSS" src="/images/martian-oss/postcss.svg" class="scaled-image h-36 mx-auto" />
      <figcaption>PostCSS: A tool for transforming CSS with JavaScript</figcaption>
    </figure>
  </a>
  <a href="https://imgproxy.net/">
    <figure>
      <img alt="Imgproxy" src="/images/martian-oss/imgproxy-light.svg" class="scaled-image h-36 mx-auto block dark:hidden" />
      <img alt="Imgproxy" src="/images/martian-oss/imgproxy-dark.svg" class="scaled-image h-36 mx-auto hidden dark:block" />
      <figcaption>Imgproxy: Fast and secure standalone server for resizing and converting remote images</figcaption>
    </figure>
  </a>
  <a href="https://github.com/evilmartians/figma-polychrom">
    <figure>
      <img alt="Polychrom" src="/images/martian-oss/polychrom.svg" class="scaled-image h-36 mx-auto block dark:hidden" />
      <figcaption>A Figma plugin that ensures UI text is readable by leveraging the new APCA algorithm</figcaption>
    </figure>
  </a>
  <a href="https://github.com/DarthSim/overmind">
    <figure>
      <img alt="Overmind" src="/images/martian-oss/overmind.svg" class="scaled-image h-36 mx-auto" />
      <figcaption>Overmind: Process manager for Procfile-based applications and tmux </figcaption>
    </figure>
  </a>
  <a href="https://evilmartians.com/oss">
    <figure>
      <div class="h-36 text-2xl flex items-center justify-center">
        <qr-code-vue value="https://evilmartians.com/oss" class="scaled-image w-full h-full mx-auto p-4" render-as="svg" margin="1" />
      </div>
      <figcaption style="font-size: 1rem; margin-top: 0; line-height: 1.25rem;">詳しくはevilmartians.com/oss</figcaption>
    </figure>
  </a>
</div>

<style>
  a { border-bottom: none !important; }
  figcaption {
    margin-top: 0.5rem;
    font-size: 0.6rem;
    line-height: 1rem;
    text-align: center;
  }
</style>

<!--
それに、我々はオープンソースの大ファンなので、できる限りオープンソースソフトウェアを使ったり、貢献したり、そしてよく自分のライブラリやプロダクトを作って維持しています。このスライドでは一番有名なものですが、今は火星で作ったオープンソースプロジェクトが百以上のものが存在しています。多分、皆さんのアプリでは我々が作ったジェムはいくつかあると思います。

One of our pillars is open source. We use open source products, and we create our own. And most probably there is a gem or two in your application Gemfile as well! Here is just a small part of our open source projects, but you can find much more at our website.
-->

---

# 目次

 1. 開発環境って何？

 2. 新人エンジニアが来たら

 3. Dockerを使って開発環境を構築する

 4. Evil Martians流のアプローチ

---

# 開発環境って何？

 1. オペレーティングシステム
 2. 依存関係ライブラリー (OpenSSLなど)
 3. ルビー
 4. ルビージェム
 5. データベース (PostgreSQL/MySQL, Redis)
 6. ソースコードエディタ/IDE
 7. git
 8. …

---

# 開発環境って何？

 1. オペレーティングシステム
 2. <mark>依存関係ライブラリー (OpenSSLなど)</mark>
 3. <mark>ルビー</mark>
 4. <mark>ルビージェム</mark>
 5. <mark>データベース (PostgreSQL/MySQL, Redis)</mark>
 6. ソースコードエディタ/IDE
 7. git
 8. …


---

## 開発環境の種類

 1. ローカル

    便利ですが、設定は面倒になるかも、OSによっては問題になるかも

 2. 仮想マシン (Vagrantなど)

    重たいです。RAMをたくさん使います。

 3. コンテナ (Dockerなど)

    軽いですが、便利な設定は簡単とは言えないです。 **今日の話題です。**

 4. リモート

    高速なインターネットが必要です。遅いとDXが悪くなります。

 5. 未来のこと (WebAssembly?)

    [Stackblitz](https://stackblitz.com/)のようなプロジェクトはWebAssemblyでブラウザーの中で全てを実現していますが、Rubyはまだ使えないみたいんです。

---
layout: statement
---

# 初めにローカル

普段、みんなはローカルで環境を設定して開発しています。

---
class: text-xl
---

## ローカルの設定の問題

 1. Rubyのインストール

    古いバージョンは新しいOSではコンパイルされず、新しいバージョンは古い OSではコンパイルされません。(OpenSSL?)
    
    アプリが古いバージョンで残ってしまうことがあります。

    この問題はRbenvやasdfで解決されていますが、問題がまだありえます。

<!--«»
asdfはすごいですが
-->

---
class: text-xl
---

## ローカルの設定の問題

 2. ジェムのインストール

    悪名高い「Nokogiriのネイティブ拡張機能をコンパイルできない問題」 (現在はなくなっているはずですが)

    PostgreSQL/MySQLの正しいバージョンのクライアントライブラリをローカルにインストールする必要があります。

---
class: text-xl
---

## ローカルの設定の問題

 3. データベース

    適切なPostgreSQLのバージョンをインストールする必要がある（productionと同じ）

    古すぎたり、新しすぎたりしたらどうなるでしょうか？ それとも、標準以外の拡張機能をインストールする必要がある場合は?

---

## プロジェクトの環境設定時間は?

新しい開発者を雇用しましたが、新しいコンピューターを入手してコーディングを開始できるまでにどれくらいの時間がかかりますか?

<Transform scale="1.5">

 1. 数分

 2. 数時間

 3. 数日間

</Transform>

---

## ローカル開発環境設定のよくある仕方

 1. 手順が記載されたREADMEファイル

    長所:
    少なくとも文書があります

    短所:
    長くて面倒くさい、見逃しやすい、古いことが多い,　一つのOSだけに対応しています

 2. Bash scripts / Makefiles

    長所:
    一度に実行できます

    短所:
    普段一つのOSだけに対応しています、古いことが多い

 3. Docker

    長所:
    一度に実行できます、OSに依存しません、OSを汚染しません

    短所:
    遅くて、不便、毎日使わないと古くなります

---
class: annotated-list
---

## 火星のプロジェクトの特有点

<Transform scale="1.25" class="mt-20">

  - プロジェクトはよく短時間（1-2週間）

    できるだけ速く結果を出さないといけません

  - プロジェクトのデータベースやRubyのバージョンはいろいろ
 
    ローカルOSにはたくさんのライブラリをインストールしたくないです。

</Transform>

---
layout: statement
---

## じゃぁ、Dockerを使いましょう

---
class: annotated-list
---

## Dockerの開発環境のよくある問題

- イメージを再ビルドの必要がよくあります

  毎回は時間がかかります。

- 実行が遅いです

  得にMacOSで

- デバッグは難しいです。

  `binding.irb`をどうやって使いますか？

- 長いコマンドを入力する必要で、面倒くさい

  ```sh
  docker-compose run --rm app bundle exec rails c # 😫
  ```

---
layout: section
---

## 解決方法：火星流のDockerの設定

---

## 火星流のDockerの設定の一覧

 1. DockerのイメージはRubyとライブラリだけを含めています

 2. ジェムは別のボリュームにインストールされています

 3. データベースも別のボリュームです

 4. いろいろな性能の設定

 3. dip: 便利なコマンドラインツール
 
 6. dip: シェルとの統合

 7. Railsテンプレートでマイグレーションスクリプト

---

<a href="https://evilmartians.com/chronicles/ruby-on-whales-docker-for-ruby-rails-development"><img alt="Ruby on Whales blog post cover" src="/images/ruby-on-whales-docker-for-ruby-rails-development.jpg" class="text-center mx-auto w-75%"></a>

原本の記事: [Ruby on Whales: Dockerizing Ruby and Rails development](https://evilmartians.com/chronicles/ruby-on-whales-docker-for-ruby-rails-development)

日本語翻訳: [クジラに乗ったRuby: Evil Martians流Docker+Ruby/Rails開発環境構築](https://techracho.bpsinc.jp/hachi8833/2022_04_07/116843)

<div class="absolute bottom-4 right-4"> 
<qr-code url="https://evilmartians.com/chronicles/ruby-on-whales-docker-for-ruby-rails-development" caption="原本" class="w-32 mt-2" />

<qr-code url="https://techracho.bpsinc.jp/hachi8833/2022_04_07/116843" caption="日本語翻訳" class="w-32 mt-2" />
</div>

---

## 火星流のDockerの設定の歴史


- レシピ集付きのマニュアルとして始まりました。

- 2019年にブログ記事として公開

- 2022年にマイグレーションスクリプトも追加されました。

  ```sh
  bundle exec rails app:template \
  LOCATION='https://railsbytes.com/script/z5OsoB'
  ```

  このコマンドで、既にあるRailsプロジェクトにDocker開発環境を追加できます。


---
class: text-xs
---

## Dockerの環境の問題解決: `Dockerfile`

<Transform scale="0.3">

```dockerfile
# syntax=docker/dockerfile:1

ARG RUBY_VERSION
ARG DISTRO_NAME=bullseye

FROM ruby:$RUBY_VERSION-slim-$DISTRO_NAME

ARG DISTRO_NAME

# Common dependencies
RUN --mount=type=cache,target=/var/cache/apt,sharing=locked \
  --mount=type=cache,target=/var/lib/apt,sharing=locked \
  --mount=type=tmpfs,target=/var/log \
  rm -f /etc/apt/apt.conf.d/docker-clean; \
  echo 'Binary::apt::APT::Keep-Downloaded-Packages "true";' > /etc/apt/apt.conf.d/keep-cache; \
  apt-get update -qq \
  && DEBIAN_FRONTEND=noninteractive apt-get install -yq --no-install-recommends \
    build-essential \
    gnupg2 \
    curl \
    less \
    git

# Install PostgreSQL dependencies
ARG PG_MAJOR
RUN curl -sSL https://www.postgresql.org/media/keys/ACCC4CF8.asc | \
    gpg --dearmor -o /usr/share/keyrings/postgres-archive-keyring.gpg \
    && echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/postgres-archive-keyring.gpg] https://apt.postgresql.org/pub/repos/apt/" \
    $DISTRO_NAME-pgdg main $PG_MAJOR | tee /etc/apt/sources.list.d/postgres.list > /dev/null
RUN --mount=type=cache,target=/var/cache/apt,sharing=locked \
  --mount=type=cache,target=/var/lib/apt,sharing=locked \
  --mount=type=tmpfs,target=/var/log \
  apt-get update -qq && DEBIAN_FRONTEND=noninteractive apt-get -yq dist-upgrade && \
  DEBIAN_FRONTEND=noninteractive apt-get install -yq --no-install-recommends \
    libpq-dev \
    postgresql-client-$PG_MAJOR

# Install NodeJS and Yarn
ARG NODE_MAJOR
ARG YARN_VERSION=latest
RUN --mount=type=cache,target=/var/cache/apt,sharing=locked \
    --mount=type=cache,target=/var/lib/apt,sharing=locked \
    --mount=type=tmpfs,target=/var/log \
    apt-get update && \
    apt-get install -y curl software-properties-common && \
    curl -fsSL https://deb.nodesource.com/gpgkey/nodesource.gpg.key | apt-key add - && \
    echo "deb https://deb.nodesource.com/node_${NODE_MAJOR}.x $(lsb_release -cs) main" | tee /etc/apt/sources.list.d/nodesource.list && \
    apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -yq --no-install-recommends nodejs
RUN npm install -g yarn@$YARN_VERSION

# Application dependencies
# We use an external Aptfile for this, stay tuned
COPY Aptfile /tmp/Aptfile
RUN --mount=type=cache,target=/var/cache/apt,sharing=locked \
  --mount=type=cache,target=/var/lib/apt,sharing=locked \
  --mount=type=tmpfs,target=/var/log \
  apt-get update -qq && DEBIAN_FRONTEND=noninteractive apt-get -yq dist-upgrade && \
  DEBIAN_FRONTEND=noninteractive apt-get install -yq --no-install-recommends \
    $(grep -Ev '^\s*#' /tmp/Aptfile | xargs)

# Configure bundler
ENV LANG=C.UTF-8 \
  BUNDLE_JOBS=4 \
  BUNDLE_RETRY=3

# Store Bundler settings in the project's root
ENV BUNDLE_APP_CONFIG=.bundle

# Uncomment this line if you want to run binstubs without prefixing with `bin/` or `bundle exec`
# ENV PATH /app/bin:$PATH

# Upgrade RubyGems and install the latest Bundler version
RUN gem update --system && \
    gem install bundler

# Create a directory for the app code
RUN mkdir -p /app
WORKDIR /app

# Document that we're going to expose port 3000
EXPOSE 3000
# Use Bash as the default command
CMD ["/bin/bash"]
```

</Transform>

<qr-code url="https://github.com/evilmartians/ruby-on-whales/blob/b3edbc5663b460eadfd76d1b91a5e358b0f66884/example/.dockerdev/Dockerfile" caption="Ruby on Whales: Dockerfile" class="absolute bottom-4 right-4 w-48" />

<!--
Dockerfileの中身は長くて、恐ろしいですが、ビルドされたイメージは非常に薄くて、シンプルです。再ビルド必要はほとんどありません。
-->

---
class: annotated-list
---

## Dockerの環境の問題解決: `Dockerfile`

中身は長いですが、イメージは非常に薄くて、シンプルです。再ビルド必要はほとんどありません。

<div class="grid grid-cols-2 text-xl">
<div>

**含めている:**

 - Ruby
 - Node.js
 - ジェムのコンパイルの為のライブラリ
 - PostgreSQLのクライアント
 
   `rails dbconsole -p`を使うために

</div>
<div>

**せっかく含めていない:**

 - Rubyのジェム
 - NPMのパッケージ
 - データベースのファイル
 - アプリのコード

</div>
</div>

<!--
Dockerfileは、Rubyアプリケーションの環境を定義します。この環境でサーバーを実行したり、rails cでコンソールを実行したり、テストやrakeタスクを走らせたり、その他にも開発者としてあらゆる形でのコードとのやりとりを行います。
性能を高めるため、そしてビルドの時間を減らすために、Ruby自体以外はほとんど何も含めていません。コードを実行する為の環境だけを含めています。それに空いているcontextのディレクトリが使用されたり、RUNコマンドのキャッシュ機能も使われています。
-->

---

# Dockerのビルド高速化

Dockerのレイヤーキャッシュをよく効くために、`RUN --mount`を使っています。

```dockerfile
# Application dependencies
# We use an external Aptfile for this, stay tuned
COPY Aptfile /tmp/Aptfile
RUN --mount=type=cache,target=/var/cache/apt,sharing=locked \
  --mount=type=cache,target=/var/lib/apt,sharing=locked \
  --mount=type=tmpfs,target=/var/log \
  apt-get update -qq && DEBIAN_FRONTEND=noninteractive apt-get -yq dist-upgrade && \
  DEBIAN_FRONTEND=noninteractive apt-get install -yq --no-install-recommends \
    $(grep -Ev '^\s*#' /tmp/Aptfile | xargs)
```

TODO: Add link to docs

---
class: text-sm
---

## `docker-compose.yml`

<Transform scale="0.175">

```yaml
x-app: &app
  build:
    context: .
    args:
      RUBY_VERSION: '3.2.2'
      PG_MAJOR: '15'
      NODE_MAJOR: '18'
  image: example-dev:1.0.0
  environment: &env
    NODE_ENV: ${NODE_ENV:-development}
    RAILS_ENV: ${RAILS_ENV:-development}
  tmpfs:
    - /tmp
    - /app/tmp/pids

x-backend: &backend
  <<: *app
  stdin_open: true
  tty: true
  volumes:
    - ..:/app:cached
    - bundle:/usr/local/bundle
    - rails_cache:/app/tmp/cache
    - node_modules:/app/node_modules
    - packs:/app/public/packs
    - packs-test:/app/public/packs-test
    - history:/usr/local/hist
    - ./.psqlrc:/root/.psqlrc:ro
    - ./.bashrc:/root/.bashrc:ro
  environment: &backend_environment
    <<: *env
    REDIS_URL: redis://redis:6379/
    DATABASE_URL: postgres://postgres:postgres@postgres:5432
    WEBPACKER_DEV_SERVER_HOST: webpacker
    MALLOC_ARENA_MAX: 2
    WEB_CONCURRENCY: ${WEB_CONCURRENCY:-1}
    BOOTSNAP_CACHE_DIR: /usr/local/bundle/_bootsnap
    XDG_DATA_HOME: /app/tmp/cache
    YARN_CACHE_FOLDER: /app/node_modules/.yarn-cache
    HISTFILE: /usr/local/hist/.bash_history
    PSQL_HISTFILE: /usr/local/hist/.psql_history
    IRB_HISTFILE: /usr/local/hist/.irb_history
    EDITOR: vi
  depends_on: &backend_depends_on
    postgres:
      condition: service_healthy
    redis:
      condition: service_healthy

services:
  rails:
    <<: *backend
    command: bundle exec rails

  web:
    <<: *backend
    command: bundle exec rails server -b 0.0.0.0
    ports:
      - '3000:3000'
    depends_on:
      webpacker:
        condition: service_started
      sidekiq:
        condition: service_started

  sidekiq:
    <<: *backend
    command: bundle exec sidekiq

  postgres:
    image: postgres:14
    volumes:
      - .psqlrc:/root/.psqlrc:ro
      - postgres:/var/lib/postgresql/data
      - history:/usr/local/hist
    environment:
      PSQL_HISTFILE: /usr/local/hist/.psql_history
      POSTGRES_PASSWORD: postgres
    ports:
      - 5432
    healthcheck:
      test: pg_isready -U postgres -h 127.0.0.1
      interval: 5s

  redis:
    image: redis:6.2-alpine
    volumes:
      - redis:/data
    ports:
      - 6379
    healthcheck:
      test: redis-cli ping
      interval: 1s
      timeout: 3s
      retries: 30

  webpacker:
    <<: *app
    command: bundle exec ./bin/webpack-dev-server
    ports:
      - '3035:3035'
    volumes:
      - ..:/app:cached
      - bundle:/usr/local/bundle
      - node_modules:/app/node_modules
      - packs:/app/public/packs
      - packs-test:/app/public/packs-test
    environment:
      <<: *env
      WEBPACKER_DEV_SERVER_HOST: 0.0.0.0
      YARN_CACHE_FOLDER: /app/node_modules/.yarn-cache

volumes:
  bundle:
  node_modules:
  history:
  rails_cache:
  postgres:
  redis:
  packs:
  packs-test:
```

</Transform>

<qr-code url="https://github.com/evilmartians/ruby-on-whales/blob/b3edbc5663b460eadfd76d1b91a5e358b0f66884/example/.dockerdev/compose.yml" caption="Ruby on Whales: docker-compose.yml" class="absolute bottom-4 right-4 w-48" />

<!--
Docker Composeの設定はもっと長くなりますが、本当に重要なところだけをけんとうしてみましょう。
-->

---

## Dockerの環境の問題解決: ボリューム

 - Rubyのジェム
 - NPMのパッケージ
 - データベースのファイル
 - キャッシュなど

```yaml
volumes:
  # …
  - bundle:/usr/local/bundle
  - rails_cache:/app/tmp/cache
  - node_modules:/app/node_modules
  - packs:/app/public/packs
  - packs-test:/app/public/packs-test
  # …
```

さらに、コンテナ内の`/tmp`とアプリの`tmp/pids`にtmpfsを利用されています。

---
class: text-lg
---

## Dockerの環境の問題解決: ボリューム

どうしてマウントされたフォルダーじゃない？

- ファイルシステムへのアクセスよりも高速 (特にMacOSの場合)
- 前にインストールされたgemを再利用可能
- ファイル許可に問題はありません
- 掃除が簡単

---
class: annotated-list
---

## Dockerの環境の問題解決: サービス


 - Docker Composeの設定でいろいろなサービスが定義されています。

   `rails server`と`rails console`には異なるサービスが使われています。例えば、サーバーの起動用には、公開するポート番号を定義します。

   サービスはdipの設定では使われています。

 - DRYの為、拡張のサービス定義も使われています。

   `x-app`と`x-backend`

 - ヘルスチェック

   `rails db:migrate`は依存するデータベースサービスの準備が整うまで待機するようにします。

---
class: text-sm
---

## Dockerの環境の問題解決: 拡張のサービス定義

<div class="grid grid-cols-2 gap-2">

```yaml
x-app: &app
  build:
    context: .
    args:
      # Ruby, Node, PostgreSQL versions
  image: example-dev:1.0.0
  environment: &env
    NODE_ENV: ${NODE_ENV:-development}
    RAILS_ENV: ${RAILS_ENV:-development}
  tmpfs:
    - /tmp
    - /app/tmp/pids

x-backend: &backend
  <<: *app
  stdin_open: true
  tty: true
  volumes: # see previous slides
  environment: &backend_environment
    <<: *env
    REDIS_URL: redis://redis:6379/
    DATABASE_URL: postgres://postgres:postgres@postgres:5432
```

```yaml
services:
  rails:
    <<: *backend
    command: bundle exec rails

  web:
    <<: *backend
    command: bundle exec rails server -b 0.0.0.0
    ports:
      - '3000:3000'
    depends_on:
      webpacker:
        condition: service_started

  webpacker:
    <<: *app
    command: bundle exec ./bin/webpack-dev-server
    ports:
      - '3035:3035'
    environment:
      <<: *env
      WEBPACKER_DEV_SERVER_HOST: 0.0.0.0
```

</div>

---

## Dockerの環境の問題解決: 設定

```yaml
# Application configuration to save memory
MALLOC_ARENA_MAX: 2
WEB_CONCURRENCY: ${WEB_CONCURRENCY:-1}
# Store caches in Docker volumes
BOOTSNAP_CACHE_DIR: /usr/local/bundle/_bootsnap
XDG_DATA_HOME: /app/tmp/cache
YARN_CACHE_FOLDER: /app/node_modules/.yarn-cache
# Convenience devtools
HISTFILE: /usr/local/hist/.bash_history
PSQL_HISTFILE: /usr/local/hist/.psql_history
IRB_HISTFILE: /usr/local/hist/.irb_history
```

---

## Dip: 不足しているDXを追加し直す

`dip`はDocker Composeに基づいたコマンドラインツールです。

- 短くて、便利なコマンドを追加できます

  ```sh
  dip rails c # 😎
  # instead of
  docker-compose run --rm app bundle exec rails c # 😫
  ```

- `dip provision`というコマンドで、アプリの環境を再構築できます。

- シェルとの統合

  ```sh
  eval "$(dip console)"
  rails c # Dockerの中で起動します😎
  ```

<qr-code url="https://github.com/bibendi/dip" caption="github.com/bibendi/dip" class="absolute bottom-4 right-4 w-36" />

---
class: text-sm
---

## `dip.yml`

<Transform scale="0.25">

```yaml
version: '7.1'

# Define default environment variables to pass
# to Docker Compose
environment:
  RAILS_ENV: development

compose:
  files:
    - .dockerdev/compose.yml
  project_name: example_demo

interaction:
  # This command spins up a Rails container with the required dependencies (such as databases),
  # and opens a terminal within it.
  runner:
    description: Open a Bash shell within a Rails container (with dependencies up)
    service: rails
    command: /bin/bash

  # Run a Rails container without any dependent services (useful for non-Rails scripts)
  bash:
    description: Run an arbitrary script within a container (or open a shell without deps)
    service: rails
    command: /bin/bash
    compose_run_options: [ no-deps ]

  # A shortcut to run Bundler commands
  bundle:
    description: Run Bundler commands
    service: rails
    command: bundle
    compose_run_options: [ no-deps ]

  # A shortcut to run RSpec (which overrides the RAILS_ENV)
  rspec:
    description: Run RSpec commands
    service: rails
    environment:
      RAILS_ENV: test
    command: bundle exec rspec

  rails:
    description: Run Rails commands
    service: rails
    command: bundle exec rails
    subcommands:
      s:
        description: Run Rails server at http://localhost:3000
        service: web
        compose:
          run_options: [service-ports, use-aliases]

  yarn:
    description: Run Yarn commands
    service: rails
    command: yarn
    compose_run_options: [ no-deps ]

  psql:
    description: Run Postgres psql console
    service: postgres
    default_args: anycasts_dev
    command: psql -h postgres -U postgres

  'redis-cli':
    description: Run Redis console
    service: redis
    command: redis-cli -h redis

provision:
  - dip compose down --volumes
  - dip compose up -d postgres redis
  - dip bash -c bin/setup
```

</Transform>

<qr-code url="https://github.com/evilmartians/ruby-on-whales/blob/b3edbc5663b460eadfd76d1b91a5e358b0f66884/example/dip.yml" caption="Ruby on Whales: DIP config" class="absolute bottom-4 right-4 w-48" />

---
class: text-sm
---

## DIPの設定：コマンド

```yaml
interaction:
  bundle:
    service: rails
    command: bundle
    compose_run_options: [ no-deps ] # Don't launch database for `bundle install`


  rspec:
    service: rails
    environment:
      RAILS_ENV: test # overrides the RAILS_ENV
    command: bundle exec rspec

  rails:
    service: rails
    command: bundle exec rails
    subcommands:
      s:
        service: web
        compose:
          run_options: [service-ports, use-aliases]
```

---

## DIPの設定：プロビジョニング

```yaml
provision:
  - dip compose down --volumes
  - dip compose up -d postgres redis
  - dip bash -c bin/setup
```

`dip provision`の一つのコマンドでは、アプリの開発環境はゼロから再構築されます。
---
class: annotated-list
---

<video autoplay>
  <source src="https://evilmartians.com/static/4dd40639ad48829fc05b9a182dc8bcc1/dip-provision.av1.mp4" type="video/mp4">

  Here is the video from this blog post: https://evilmartians.com/chronicles/ruby-on-whales-docker-for-ruby-rails-development#interactive-provisioning

</video>

---
class: annotated-list
---

# 結果

- プロジェクトの設定は数分で可能

  インターネットの速度によっては、数十分かかることもあります。

- 異なるOSでも同様に動作します

- 開発環境をリセットは簡単

  `dip provision`でデータベースなどの状態をゼロから作り直せます。


<div class="border border-4 border-green-600 rounded-md mt-20 p-4 text-center font-bold text-2xl">
  
誰でも今すぐ開発し始めることができます。

</div>

---
layout: statement
---

## 驚くほど速くアプリを移動できます

Docker開発環境に移動することが速い！

なれたら、ローカルOSにすべてをインストールするより、<br>Dockerを使って開発環境を構築するのほうが速くなります。

```sh
bundle exec rails app:template \
LOCATION='https://railsbytes.com/script/z5OsoB'
```

---

# 試してみよう！

必要な既にインストールされているソフトウェア

 - OS
 - Docker と Docker Compose
 - Ruby (サポートされているバージョン)
 - [dip](https://github.com/bibendi/dip) ジェム

```sh
bundle exec rails app:template \
LOCATION='https://railsbytes.com/script/z5OsoB'
```

<qr-code url="https://github.com/evilmartians/ruby-on-whales" caption="ruby-on-whales repo" class="absolute bottom-4 right-4 w-36" />

---

<a href="https://evilmartians.com/chronicles/ruby-on-whales-docker-for-ruby-rails-development"><img alt="Ruby on Whales blog post cover" src="/images/ruby-on-whales-docker-for-ruby-rails-development.jpg" class="text-center mx-auto w-75%"></a>

原本の記事: [Ruby on Whales: Dockerizing Ruby and Rails development](https://evilmartians.com/chronicles/ruby-on-whales-docker-for-ruby-rails-development)

日本語翻訳: [クジラに乗ったRuby: Evil Martians流Docker+Ruby/Rails開発環境構築](https://techracho.bpsinc.jp/hachi8833/2022_04_07/116843)

<div class="absolute bottom-4 right-4"> 
<qr-code url="https://evilmartians.com/chronicles/ruby-on-whales-docker-for-ruby-rails-development" caption="原本" class="w-32 mt-2" />

<qr-code url="https://techracho.bpsinc.jp/hachi8833/2022_04_07/116843" caption="日本語翻訳" class="w-32 mt-2" />
</div>

---

# ありがとうございました！

<div class="grid grid-cols-[8rem_3fr_4fr] mt-12 gap-2">

<div class="justify-self-start">
<img alt="Andrey Novikov" src="https://secure.gravatar.com/avatar/d0e95abdd0aed671ebd0920c16d393d4?s=512" class="w-32 h-32 scaled-image" />
</div>

<ul class="list-none">
<li><a href="https://github.com/Envek"><logos-github-icon class="dark:invert" /> @Envek</a></li>
<li><a href="https://twitter.com/Envek"><logos-twitter /> @Envek</a></li>
<li><a href="https://facebook.com/Envek"><logos-facebook /> @Envek</a></li>
<li><a href="https://t.me/envek"><logos-telegram /> @Envek</a></li>
</ul>

<div>
<qr-code url="https://github.com/Envek" caption="github.com/Envek" class="w-32 mt-2" />
</div>

<div class="justify-self-start">
<a href="https://evilmartians.com/"><img alt="Evil Martians" src="/images/01_Evil-Martians_Logo_v2.1_RGB.svg" class="w-32 h-32 scaled-image block dark:hidden" /><img alt="Evil Martians" src="/images/02_Evil-Martians_Logo_v2.1_RGB_for-Dark-BG.svg" class="w-32 h-32 scaled-image hidden dark:block" /></a>
</div>

<div>

- <logos-github-icon class="dark:invert" /> [@evilmartians](https://github.com/evilmartians?utm_source=izumorb&utm_medium=slides&utm_campaign=kujira-ni-notta-ruby)
- <logos-twitter /> [@evilmartians](https://twitter.com/evilmartians/?utm_source=izumorb&utm_medium=slides&utm_campaign=kujira-ni-notta-ruby)
- <logos-twitter /> [@evilmartians_jp](https://twitter.com/evilmartians_jp/?utm_source=izumorb&utm_medium=slides&utm_campaign=kujira-ni-notta-ruby) <small>(日本語)</small>
- <logos-instagram-icon class="dark:invert" /> [@evil.martians](https://www.instagram.com/evil.martians/?utm_source=izumorb&utm_medium=slides&utm_campaign=kujira-ni-notta-ruby)
</div>

<div>
<qr-code url="https://evilmartians.jp/" caption="evilmartians.jp" class="w-32 mt-2" />
</div>

<div class="col-span-3">

 我々のヤバいブログ: [evilmartians.com/chronicles](https://evilmartians.com/chronicles/?utm_source=izumorb&utm_medium=slides&utm_campaign=kujira-ni-notta-ruby)!{class="text-lg"}

[@hachi8833](https://twitter.com/hachi8833)さんが作ってくれた日本語の翻訳があります！

<p class="text-sm">このスライドは次のアドレスでご覧できます: <a href="https://envek.github.io/izumorb-kujira-ni-notta-ruby/">envek.github.io/izumorb-kujira-ni-notta-ruby</a></p>

</div>
</div>

<style>
  ul a { border-bottom: none !important; }
  ul { list-style-type: none !important; }
  ul li { margin-left: 0; padding-left: 0; }
</style>

<!--

皆さん、最後までご視聴してくださって、ありがとうございました！

我が社のブログでは、RubyとRailsについての記事がたくさんありますので、ぜひお読みください！日本語の翻訳もあります。

以上です。
-->
