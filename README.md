# wp-docker-compose

docker compose | WordPress6.0.3 PHP8.1 MySQLl8.0 Apache 環境

## 要件

- [Dockerクライアント](https://www.docker.com/products/docker-desktop/)

## 起動

```sh
# 1, .env を作成し、必要に応じて環境変数を変更します（各値は他のプロジェクと重複しないようしてください）
cp ./.env-example ./.env

# 2, WP 起動
docker compose up -d
```

上記手順で起動後 <http://localhost:8001/>（8001は環境変数`ADMINER_PORT`の値）を開き初期設定を実行する

### WP-CLIの実行

```sh
# 例
docker compose run --rm cli bash -c "wp --version"
```

### データベースのインポート

<http://localhost:8001/>（8001は環境変数`ADMINER_PORT`の値）を開き、adminerにログイン(サーバー:`db`それ以外は全て`wordpress`) データベースのインポートなどができます。

wp-cliでインポートする場合

```sh
# 1, shared/ ディレクトリにインポートする sql ファイルを配置

# 2, 既存のDBをリセットし、インポート
docker compose run --rm cli bash -c "wp db reset --yes && wp db import shared/database.sql"


```

### テーマやプラグインの追加

docker compose 起動後に生成される`wp-content/`以下に直接コピーするか、<http://localhost:8000/wp-login.php/>（8000は環境変数`ADMINER_PORT`の値）からログインして管理画面からインストール。

## 削除

```sh
# 終了/ボリューム削除（DBやメディアなどは削除されます）
docker compose down -v

# 未使用のコンテナ、イメージ・ボリューム・ネットワークなどを完全に削除
# [!]注意：他プロジェクトで停止中の分もすべて削除されます
docker system prune --volumes
```

> 削除方法の詳細は[公式文書](https://docs.docker.jp/config/pruning.html)など参照
