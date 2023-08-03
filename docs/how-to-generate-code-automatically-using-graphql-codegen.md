## graphql-codegen を使ったコードの自動生成方法について

### 0. graphql-codegen の設定について

graphql-codegen の設定は `frontend/codegen.yml` に記載されています。  
インストールしたプラグインは下記の通りです。

- `typescript`（GraphQL のスキーマに基づいて、ベースとなる TypeScript の型を生成する。）
- `typescript-operations`（GraphQL のスキーマと GraphQL の操作とフラグメントに基づいて TypeScript の型を生成する。具体的には Query、Mutation、Subscription、Fragment である。）
- `typed-document-node`（GraphQL のオペレーション（クエリやミューテーション等）から TypeScript の型を生成する。）

### 1. クエリ or ミューテーションの追加

TypeScript の型を生成したい Query、もしくは Mutation を追加してください。

例. `frontend/src/graphql/queries/users.graphql` を作成した場合

```graphql
query SelectUniqueUserByEmail($email: String!) {
  users(where: { email: { _eq: $email } }) {
    id
    name
    email
  }
}
```

### 2. graphql-codegen を使ったコードの自動生成について

下記コマンドを実行すると、先ほど追加した Query、もしくは Mutation に対する TypeScript の型が自動で生成されます。  
カレントディレクトリが `bined` であることを想定しています。

```bash
docker-compose -f docker-compose-frontend.yml run --rm frontend npm run generate
```
