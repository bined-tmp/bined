# bined

## 環境構築

下記の通りコマンドを実行してください。

### 注意点

- ディレクトリの移動についてもコマンドに含まれています。
- `.env` ファイルに適切な環境変数を設定してください。

### 1. 本レポジトリのクローン

```bash
git clone git@github.com:bined-tmp/bined.git
```

### 2. サブモジュールの更新

```bash
cd bined
```

```bash
git submodule update --init --recursive
```

### 3. ブランチの更新

#### Backend

```bash
cd backend && git switch main && git pull origin main && cd ../
```

#### Frontend

```bash
cd frontend && git switch main && git pull origin main && cd ../
```

#### Hasura

```bash
cd hasura && git switch main && git pull origin main && cd ../
```

### 4. env ファイルのコピー

#### Backend

```bash
cp ./backend/.env.example ./backend/.env
```

#### Frontend

```bash
cp ./frontend/.env.example ./frontend/.env
```

### 5. パッケージのインストール

#### Backend

```bash
docker-compose -f docker-compose-backend.yml run --rm backend npm ci
```

#### Frontend

```bash
docker-compose -f docker-compose-frontend.yml run --rm frontend npm ci
```

### 6. PostgreSQL・Hasura GraphQL Engine の立ち上げ

```bash
docker-compose -f docker-compose-db-hasura.yml up
```

### 7. TypeORM によるマイグレーション

```bash
docker-compose -f docker-compose-backend.yml run --rm backend npm run migration:run
```

### 8. Hasura の metadata の apply

```bash
cd hasura && hasura metadata apply --admin-secret=password && cd ../
```

### 9. 関数を作成する SQL 文の実行

`http://localhost:8080/console/data/sql` にアクセスして、下記の SQL 文を実行する。

```sql
CREATE OR REPLACE FUNCTION update_email_verified()
RETURNS TRIGGER AS $$
BEGIN
  RAISE NOTICE 'NEW record: %', NEW;
  UPDATE users
  SET "emailVerified" = NOW()
  WHERE id = NEW."userId";

  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trigger_update_email_verified
AFTER INSERT ON accounts
FOR EACH ROW EXECUTE PROCEDURE update_email_verified();
```

### 10. アプリケーションの立ち上げ

```bash
docker-compose -f docker-compose-backend.yml -f docker-compose-frontend.yml up
```
