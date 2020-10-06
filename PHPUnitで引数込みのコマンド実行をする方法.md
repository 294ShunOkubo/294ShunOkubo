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

複数入れたい場合は
```
    public function idProvider()
    {
        return [
            [1,2,3],
            [4,5,6],
        ];
    }
```

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
