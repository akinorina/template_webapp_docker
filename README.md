# template_webapp_docker

このリポジトリは次のリポジトリと一緒に使用することを前提にしています：

- template_webapp_server
- template_webapp_client

上記２つとともに、次のコマンドを使用することで、Docker を用いた、c/s Web アプリ開発を行うテンプレートとして使用することができます。

## 手順

1. ３つのリポジトリをクローン
   次のように、３つのリポジトリを配置する：

```
.../WEBAPP_PROJECT/
  |
  +-- template_webapp_server/
  +-- template_webapp_client/
  +-- template_webapp_docker/
```

2. ビルド

```
% docker-compose build
```

3. 実行

```
% docker-compose up -d
% docker-compose ps
NAME                COMMAND                  SERVICE             STATUS              PORTS
backend-app         "node dist/main.js"      backend-app         running             0.0.0.0:3000->3000/tcp
db                  "docker-entrypoint.s…"   db                  running             0.0.0.0:3306->3306/tcp
frontend-app        "/docker-entrypoint.…"   frontend-app        running             0.0.0.0:80->80/tcp
phpmyadmin          "/docker-entrypoint.…"   phpmyadmin          running             0.0.0.0:8080->80/tcp
smtp                "MailHog"                mailhog             running             0.0.0.0:1025->1025/tcp, 0.0.0.0:8025->8025/tcp
%
```

4. template_webapp_server コンテナに入り、DB の migration を実行。DB に TBL と初期データを作成する。

```
% docker-compose exec backend-app bash
以下、backend-appコンテナ内：
# alias typeorm="npx ts-node --transpile-only ./node_modules/typeorm/cli.js"
# typeorm migration:run
# exit
%
```

http://localhost:8080 で phpMyAdmin 画面があるので、DB の状況、migration 結果を確認できる。

5. システムを再起動
   次のコマンドで、template_webapp_server / template_webapp_client を再起動：

```
% docker-compose down
% docker-compose up -d
```

http://www.example.com (/etc/hosts 等を 127.0.0.1 設定済みとして)へアクセスすると、template_webapp_client 画面が表示。
c/s 全体として動作する。
