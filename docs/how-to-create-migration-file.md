## マイグレーションファイルの作成方法について

### 0. 準備

`README.md` に記載されている「[6. PostgreSQL・Hasura GraphQL Engine の立ち上げ](https://github.com/bined-tmp/bined#6-postgresqlhasura-graphql-engine-%E3%81%AE%E7%AB%8B%E3%81%A1%E4%B8%8A%E3%81%92)」を完了させてください。

### 1. Entity の定義

テーブルを新たに作成する場合は、`backend/src/database/entity` フォルダに `xxxxx.entity.ts` を作成してください。  
`xxxx` は Entity の名前です。`xxxx` は単数形とします。

`id` は、UUID を採用します。  
また、`createdAt` と `updatedAt` を忘れずに追加してください。

例. `user.entity.ts` の場合

```ts
import { Field, ID, ObjectType } from "@nestjs/graphql";
import {
  Column,
  CreateDateColumn,
  Entity,
  OneToMany,
  PrimaryGeneratedColumn,
  UpdateDateColumn,
} from "typeorm";

@Entity({ name: "users" })
@ObjectType()
export class User {
  @PrimaryGeneratedColumn("uuid")
  @Field(() => ID)
  id: string;

  @Column({ nullable: true })
  @Field()
  name: string;

  @Column({ nullable: true, unique: true })
  @Field()
  email: string;

  @CreateDateColumn()
  @Field(() => Date)
  createdAt: Date;

  @UpdateDateColumn()
  @Field(() => Date)
  updatedAt: Date;
}
```

### 2. マイグレーションファイルの作成

下記コマンドを実行すると、先ほど追加した Entity に対するマイグレーションファイルが生成されます。

```bash
docker-compose -f docker-compose-backend.yml run --rm backend npm run migration:run
```
