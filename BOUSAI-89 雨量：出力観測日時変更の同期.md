BOUSAI-89 雨量：出力観測日時変更の同期
===

修正検討
---

### 結論
修正点なし

### 着目点
- [x] 貯留池の時点で見つかった修正点（横展開）
  - [x] 【チェック済】観測日時
  - [x] 【チェック済】欠測書き換え
  - [x] 【チェック済】二重作成バグ
- [x] 貯留池になくて雨量にあるもの
  - [x] データを（観測所ごとに）積算する処理
  - [x] データを数えて足りてなければカットする処理
    - unset()により添字番号がずれがち（飛びがち）だが、大丈夫？
      - ここからarray_merge()するまでapi_dataの添字使った処理がないので大丈夫
  - [x] jsonフォーマット準備
    - 大差はない
    - itemcode, value, statusをDBには文字列で送る必要性？
      - ありません（貯留池で分けてる意味なさそう）
  - [x] 欠測再取得時の書き換え判定対象
    - 大差はない
    - 貯留池も$api_data_missing_processedがあるかを判定基準にしたほうがいいと思う
  - [x] selectMissing()に引数DATA_TYPEがない
    - それが必要なのは水位だけ
  - [x] getTimes()に引数$duration_by_itemcodeがある
    - データの作り方が10分と1時間で違うため
  - [x] 正時のデータ作りがコピペではなく取得し直している
    - データの作り方が10分と1時間で違うため
  - [x] 固有名詞が違う（rainとwaterlevelなどの違い）
    - 自明

### おまけ：リファクタリングしたい点

- selectMissing()とselectNotSent, 将来のrainfallテーブル仕様変更に備えて引数DATA_TYPE持たす？
- selectWithObservpoint()かselectByTimeAndObservpointかに統一したい
- 欠測再取得データを$api_data_missingに入れるか判定、if (count($api_data_missing_processed) > 0)に統一したほうがいいと思う
  - db時代にデータがあってもprocessForAPIで消える可能性があるため
  - 検討：$db_dataにデータがないとprocessForAPIでエラー吐く？
    - 吐くならprocessForAPI呼び出しをif (count($db_data_missing) > 0)で囲む必要
- ifと()の間にスペース足りてない
- jsonフォーマット統一、$master_for_jsonをいじること
　- もはや$api_data_masterをいじればいい？
- 貯留池アクションL238の&いらない
- 雨量のdata_counterを消す処理、$is_missing = false;の後に移動してforeachを一つ減らす

テストケース.xlxs修正
---

- 欠測再取得＆未送信かぶり対策、ケースに落とし込み済（case009）
- 欠測再取得２／３で挟む（◯×◯）をcase011に追加

## メモ
- 「観測所マスタに存在しない観測所コードを持ったデータがDBから取得された場合」は考慮すべき？
  - 特に何も起こらない（欠測判定は観測所マスタにある該当観測所コード全てのデータを産ませるためのもののため）
