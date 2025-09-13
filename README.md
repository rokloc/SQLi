# SQLi
SQL injectionに関する学習記録

SQLiとは
攻撃者がDBに対するクエリを妨害する脆弱性
フレームワークの普及により落ち着いていたが、AIコードサービスによりSQLiリスクが社会的に再燃しつつある


SQLi
例１ 隠されたデータの取得
https://sample-website.com/products?category=Giftsというリクエストが飛んだ時
SELECT * FROM products WHERE category = 'Gifts' AND released = 1というクエリがDBに飛ぶとする released == 0は未発表商品と考えられる
その際攻撃者がURLをhttps://sample-website.com/products?category=Gifts'--と変更すると
SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1というクエリになりreleased == 1がコメントアウトされ未発表商品まで公開されてしまうという脆弱性

例2  アプリケーションロジックの破壊
ユーザーがユーザー名wienerとパスワードを送信するとbluecheese
SELECT * FROM users WHERE username = 'wiener' AND password = 'bluecheese'
admin'--と入力すると
SELECT * FROM users WHERE username = 'administrator'--' AND password = ''というクエリが飛び
adminユーザーとしてパスワードなしでログインできてしまう

例3  他のデータベーステーブルからデータを取得する（あとでまとめる）
UNION攻撃

例4  ブラインドSQLインジェクションの脆弱性

例5  二次SQLインジェクション

例6  データベースの調査

例7  さまざまなコンテキストでのSQLインジェクション



SQLiを防ぐ方法



注意喚起
