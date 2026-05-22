# Laravel Docker Environment

Laravel の開発環境です。  
Docker を利用して以下の構成で動作します。

## 構成

- PHP
- Apache
- PostgreSQL
- Redis
- pgAdmin
- Adminer

---

## 使用ポート

| Service | URL |
|----------|------|
| Laravel | http://localhost:8080 |
| pgAdmin | http://localhost:5050 |
| Adminer | http://localhost:8082 |
| PostgreSQL | localhost:5432 |
| Redis | localhost:6379 |

---

## 初回セットアップ

### 1. リポジトリをクローン

```bash
git clone <repository-url>
cd <project-name>
```

### 2. 環境変数ファイル作成

`.env.example` をコピーして `.env` を作成します。

```bash
cp .env.example .env
```

---

### 3. APP_KEY生成

コンテナ起動前はLaravelが動かないため、一旦コンテナを立ち上げます。

```bash
docker compose up -d
```

その後 APP_KEY を生成

```bash
docker compose exec php php artisan key:generate
```

---

### 4. Composerインストール

vendor がない場合のみ実行

```bash
docker compose exec php composer install
```

---

### 5. マイグレーション実行

```bash
docker compose exec php php artisan migrate
```

Seederを使用する場合:

```bash
docker compose exec php php artisan migrate --seed
```

---

## 起動

```bash
docker compose up -d
```

確認:

```bash
docker compose ps
```

---

## 停止

```bash
docker compose down
```

ボリュームも削除:

```bash
docker compose down -v
```

---

## コンテナへ入る

PHPコンテナ:

```bash
docker compose exec php bash
```

PostgreSQL:

```bash
docker compose exec database bash
```

DB接続:

```bash
psql -U postgres -d laravel_sample
```

---

## Laravelコマンド

キャッシュ削除

```bash
docker compose exec php php artisan optimize:clear
```

Queue起動

```bash
docker compose exec php php artisan queue:work
```

テスト

```bash
docker compose exec php php artisan test
```

---

## DB接続情報

### PostgreSQL

```txt
Host: database
Port: 5432
Database: laravel_sample
User: postgres
Password: 
```

### pgAdminログイン

```txt
Email: admin@example.com
Password: 
```

pgAdminへログイン後、新規サーバ登録:

```txt
Host: database
Port: 5432
User: postgres
Password: 
```

---

## ディレクトリ構成

```txt
.
├── apache/
├── docker-compose.yml
├── Dockerfile
├── .env.example
├── app
├── routes
├── resources
└── ...
```