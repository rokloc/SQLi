# SQLi
SQL injectionに関する学習記録

SQLiとは
攻撃者がDBに対するクエリを妨害する脆弱性
フレームワークの普及により落ち着いていたが、AIコードサービスによりSQLiリスクが社会的に再燃しつつある


SQLiの例その１
https://sample-website.com/products?category=Giftsというリクエストが飛んだ時
SELECT * FROM products WHERE category = 'Gifts' AND released = 1というクエリがDBに飛ぶとする released == 0は未発表商品と考えられる
その際攻撃者がURLをhttps://sample-website.com/products?category=Gifts'--と変更すると
SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1というクエリになりreleased == 1がコメントアウトされ未発表商品まで公開されてしまうという脆弱性



SQLiを防ぐ方法



注意喚起
