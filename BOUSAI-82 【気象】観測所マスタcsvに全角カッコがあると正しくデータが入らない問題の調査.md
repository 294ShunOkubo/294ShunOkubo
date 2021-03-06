# BOUSAI-82 【気象】観測所マスタcsvに全角カッコがあると正しくデータが入らない問題の調査

## 概要
SendRiverWaterLevelTest/m_observ_point.csv
において、観測所名カラムに全角カッコがあるとデータが正しく入らない問題があった。
（具体的な状況はコメントにて後述）
これの原因を調査・特定し、場合によっては修正する。

## 完成要件
- MySQLにおいてPDOを使って（要確認）csvでインポートする際に、全角カッコがあると正しく入らない理由を明らかにする
- 以上の情報を使って当該症状を直せそうであれば、dataUploadObservPoint()の修正

## 提出物
- 報告（口頭 or backlogコメント）
- （必要があれば）各テストクラス

## 計画
各工程ごとにやることを分割

### 計画・シミュレーション
- 手順レベルまでタスクを噛み砕く

### 状況再現
- [x] 河川アクションを単体テスト仕様にする
- [x] m_observ_point.csvの観測所名のカッコを全角にする
- [x] 実行し、phpMyAdminで確認する
- [x] テーブルをスクショし、backlogのコメントに添付

### 調査
- [ ] dataUploadObservPoint()の中身を確認
  - キーワード
    - fgetcsv()
    - bindParam()
    - PDO
- [ ] 「MySQL　全角カッコ」などでググる
- [ ] 「ロケール」について深く知る必要

### 実装・修正
- [ ] 調査のまとめを書く
  - 原因
  - 対処
- [ ] 必要があれば次の処置を検討
  - dataUploadObservPoint()の修正
  - MySQLの設定の変更
  
### 目視確認
- 調査のまとめに漏れはないか？

### 単体テスト（テスト準備～テスト実行～テスト後始末）

### 最終確認

### 提出

## メモ
- S-JISでfgetcsv()すると文字化けとか起きるらしい
- $line を var_dump()してみた
  - 参考：http://piyopiyocs.blog115.fc2.com/blog-entry-513.html
  - 実行結果：
    [4] =>
    string(41) "伊賀町（愛宕橋）"
    2320200400002""
  - つまりここの改行が「改行コード」であっても「データの区切り」とは認識されていないのが問題
- 環境変数LANGの設定してみた
  - https://www.softel.co.jp/blogs/tech/archives/2331
  - https://qiita.com/reika_i/items/cd11ffdf23a3f68b5ebc
  - 変更してOS再起動
  - 何も変わらない
- php.iniでauto_detect_line_endingsを有効にしてみた
  - https://teratail.com/questions/2050
  - 特に変わらず
- 藤原さんに相談
  - fgetcsv()はロケール設定によって挙動が変わる
- ロケール設定の中に文字関係の設定がある？
- 現状どういう環境で動いてる？：
  ```
  LC_COLLATE=C;
  LC_CTYPE=Japanese_Japan.932;
  LC_MONETARY=C;
  LC_NUMERIC=C;
  LC_TIME=C
  ```
- この中で今回の件で関係しそうなのは？：LC_CTYPE

- setlocale(LC_ALL, "C");してみた
  - https://iamapen.hatenablog.com/entry/2017/08/10/134224
  - ズレもなく余計なダブルクオーテーションもない
  - setlocale(LC_CTYPE, "C");でも正しく動く
    - やはりLC_CTYPEがうまく設定されてなかったことが根本原因
    
- MS932とSHIFT- JISの違い？

- とりあえずcommit
- このあと：河川以外の他4機能の横展開
