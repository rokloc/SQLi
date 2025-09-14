```markdown
# SQLi (SQL Injection) 学習記録

SQLiは、攻撃者がデータベースへのクエリを操作できる脆弱性です。フレームワークの普及により落ち着いていましたが、AIコード生成サービスの普及によりリスクが再燃しています。

## 1. SQLiの基本
攻撃者は入力値を操作して、DBクエリを意図せず変更させることができます。これにより、未公開データの取得や管理者権限の取得などのリスクが発生します。

## 2. SQLiの具体例

### 例1: 隠されたデータの取得
通常リクエスト:
https://sample-website.com/products?category=Gifts

生成されるクエリ:
SELECT * FROM products WHERE category = 'Gifts' AND released = 1;

攻撃者による変更:
https://sample-website.com/products?category=Gifts'--

生成されるクエリ:
SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1;

→ `released = 1` がコメントアウトされ、未発表商品まで取得可能。

### 例2: アプリケーションロジックの破壊
通常ログイン:
SELECT * FROM users WHERE username = 'wiener' AND password = 'bluecheese';

攻撃者が `username=admin'--` を入力すると:
SELECT * FROM users WHERE username = 'administrator'--' AND password = '';

→ パスワードなしで管理者アカウントにログイン可能。

### 例3: 他テーブルからのデータ取得（UNION攻撃）
- 後でまとめ予定

### 例4: ブラインドSQLインジェクション
- [Blind SQLiサンプル](https://github.com/rokloc/blind-SQLi)

### 例5: 二次SQLインジェクション
- 後で追記予定

### 例6: データベース調査
- テーブル構造やデータの調査に利用

### 例7: 様々なコンテキストでのSQLi
- Webアプリの様々な箇所で発生する可能性あり

## 3. SQLi対策

### Spring Boot + JPA の活用
Spring Data JPA / CrudRepository を使うと、多くの場合SQLiは自動的に防止されます。

Repository定義:
@Repository
public interface ItemRepository extends JpaRepository<Item, Long> {
    List<Item> findByCategory(String category);
}

呼び出し側:
List<Item> items = itemRepository.findByCategory(category);

- 内部でパラメータ化クエリが使われているため安全
- 生SQLを自作するより簡単で安全

### その他のSQLi防止策
- 入力値のバリデーション・サニタイズ
- PreparedStatementの利用（生SQLが必要な場合）
- エラーメッセージを詳細に返さない
- 権限管理と最小権限の徹底

## 4. 注意喚起
- SQLiは未だに頻発する脆弱性
- Webアプリだけでなく、AIコード生成サービス由来のコードにも注意
- 学習目的だけでなく、実務でも防御策を理解しておくことが重要
