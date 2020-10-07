# 【PHP】定数の扱い：define()とconstの違いと使い分け

## define()

#### 定義のしかた
```define('CONSTRUCT_NAME', value);```

#### 分類
組み込み関数

#### スコープ
クラス変数定義ゾーン（クラス定義内のメソッド外）以外のすべて

#### 呼び出し方
`CONSTRUCT_NAME`


## const

#### 定義のしかた
```const CONSTRUCT_NAME = value;```

#### 分類
キーワード

#### スコープ
当該クラス内

#### 呼び出し方
`self::CONSTRUCT_NAME`

## 使い分け

#### define()の使い時

- 特に制約がないとき
- 定義する定数が少ないとき；関数は処理が重いため

#### constの使い時

- メンバ定数（クラス定数）にしたいとき
- define()が使えない定数を定義したいとき
- 定義する定数が多いとき；キーワードは処理が軽いため

## 参考文献

PHP マニュアル > 言語リファレンス > 定数

https://www.php.net/manual/ja/language.constants.php

PHP マニュアル > 言語リファレンス > 定数 > 構文

https://www.php.net/manual/ja/language.constants.syntax.php

PHPマニュアル > 言語リファレンス > クラスとオブジェクト > オブジェクト定数

https://www.php.net/manual/ja/language.oop5.constants.php

PHPで定数定義するときはdefine()よりもconstを使うべき - Qiita

https://qiita.com/Hiraku/items/bb0cb665d830f7cd37ff
