## Hasura の metadata のエクスポート方法について

### 0. metadata とは

Hasura サーバのコンフィギュレーションを metadata と呼びます。  
具体的な情報は次の通りです。

1. データベース接続
2. データソースとテーブル/ビュー/関数
3. リレーション
4. パーミッション
5. リモートスキーマとアクション
6. イベントトリガー

上記の内容を変更すると Hasura の metadata が更新されます。

参考.

> https://hasura.io/docs/latest/migrations-metadata-seeds/manage-metadata/

### 1. metadata のエクスポート方法

Hasura サーバに何らかの変更を加えた場合、下記のコマンドを実行して変更内容をファイルに反映させてください。  
カレントディレクトリが `bined` であることを想定しています。

```bash
cd hasura && hasura metadata export --admin-secret=password && cd ../
```
