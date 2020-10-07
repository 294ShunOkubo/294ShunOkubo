## 1.引数を保持する関数の作成

```
    public function idProvider()
    {
        return [
            [1],
        ];
    }
```
入れたい引数を配列に格納し、更に全体を配列に格納する。

## 2. 

```
     /**
      * @dataProvider idProvider
      */
    public function testCase001($id)
    {
```
引数を使いたいテストケースの直上に、アノテーションを書く。
テストケースの関数の引数欄に引数を保持する変数を書く。

## 3.
```
$_SERVER['argv'][2] = $id;
```

引数を保持する変数を、`$_SERVER['argv'][2]`に代入する。

```
$argv = $_SERVER['argv'];
```

これによりAction側で指定した引数がコマンドライン引数と同じところに入るので、

通常通り使える。

## 補足：複数の引数を入れたいとき/複数の引数パターンを入れたいとき
```
    public function idProvider()
    {
        return [
            [1,2,3],
            [4,5,6],
        ];
    }
```
一回の実行で引数を複数入れたいときは、狭い配列に複数の値を並べればよい。

一つのテストケースで複数の引数パターンを試したい場合は、狭い配列をパターンの数だけ並べる。

テストケースごとに引数を帰る場合は、dataProviderを使わず直接メソッド内に定義したほうが正確。

## 参考文献
PHPUnit マニュアル　2. PHPUnit 用のテストの書き方　データプロバイダ
https://phpunit.de/manual/5.5/ja/writing-tests-for-phpunit.html#writing-tests-for-phpunit.data-providers
