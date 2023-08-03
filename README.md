# bined

## 環境構築

### 本レポジトリのクローン

```bash
git clone git@github.com:bined-tmp/bined.git
```

### サブモジュールの更新

```bash
cd bined
```

```bash
git submodule update --init --recursive
```

### ブランチの更新

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

### env ファイルのコピー

#### Backend

```bash
cp ./backend/.env.example ./backend/.env
```

#### Frontend

```bash
cp ./frontend/.env.example ./frontend/.env
```

### パッケージのインストール

#### Backend

```bash
docker-compose -f docker-compose-backend.yml run --rm backend npm ci
```

#### Frontend

```
docker-compose -f docker-compose-frontend.yml run --rm frontend npm ci
```

### PostgreSQL・Hasura GraphQL Engine の立ち上げ

```bash
docker-compose -f docker-compose-db-hasura.yml up
```

### TypeORM によるマイグレーション

```bash
docker-compose -f docker-compose-backend.yml run --rm backend npm run migration:run
```

### Hasura の metadata を apply する

```bash
cd hasura && hasura metadata apply --admin-secret=password && cd ../
```

### アプリケーションの立ち上げ

```bash
docker-compose -f docker-compose-backend.yml -f docker-compose-frontend.yml up
```
