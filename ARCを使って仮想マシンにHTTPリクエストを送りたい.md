# 

## やりたいこと
http://192.168.33.10/api/sirencontrol
にHTTPリクエストを送信し、サイレン制御時に返ってくるjsonの中身を確認したい
※jsonの中身＝送信したデータ＋array("result" => 1)

## 現状
ARCを使ってHTTPリクエストを送ると404エラーが出る。
どのIPアドレスをホスト部に入れたら届くかわからない

## やっていること（確認したこと）
- https://kekaku.addisteria.com/wp/20181228112237 の手順でインストール
- wikiの「構築手順」を行う（6.cron設定を除く）
  - 未達成：4.httpd.conf 追加
    - パーミッション書き換え
    - 当該ファイルを更新しようとすると以下のメッセージが出る
      - scp: /etc/httpd/conf/httpd.conf: set times: Operation not permitted
      - しかしファイルを見ると書き換えはできている
    - リロード `systemctl reload httpd`
- 仮想マシン側のApatchサーバーのrunning
- 仮想マシン側のmysqlサーバーのrunning
- java SystemManager の実行

## できました
- ```
  {"submergealert":[{"observPointCode":"2320201700010","observTime":"2020\/10\/28 10:00","sirenManual":1,"result":1}]}
  ```
- ```
  [vagrant@localhost sasatest]$ java SystemManager
  2020/12/22 11:58:27     --------------------------------
  2020/12/22 11:58:27     system start
  2020/12/22 11:58:27     read system_property start
  2020/12/22 11:58:27     read system_property end
  2020/12/22 11:58:32     <tx_res>OK

  ```
- 直接的な原因：

## やりたいこと
- 下記の手順をやる？（Host-only ネットワーク3/1追記欄）
  - http://iwsttty.hatenablog.com/entry/2013/12/08/194430
  - virtualBox設定変更
- VirtualBoxのポートフォワーディングを使う？
  - https://qiita.com/unau/items/0a4cec8a4f9abaca0b31
- 直接的な原因：「4.httpd.conf 追加」がちゃんと完了していなかった
  - 学び：詰まったところはちゃんと記録しておこう！
    - というかSlackでサクッと聞こう！
  - チケットのないタスクもちゃんと作業ログを残そう！
  
## わからない要素
- httpなのか、httpsなのか
- ~~IPは結局どれなのか
- 追加で作業がいるのか
- BOUSAI-118 送信データをソートする　とどっちを優先するか

## 今考えていること
- URIはhttp://192.168.33.10/api/sirencontrol で間違いない
- 内部の何が原因？
  - ポートフォワーディングが必要？

## やったこと
- いろいろなIPにヘッダとボディをつけて送信
  - どれもうまくいかず
  - 調べたところゲストOSへのIPはvagrantfileで設定した192.168.33.10に固定される
  - https://ja.stackoverflow.com/questions/54642/vagrant%E3%81%A7%E3%83%9B%E3%82%B9%E3%83%88%E3%81%8B%E3%82%89%E3%82%B2%E3%82%B9%E3%83%88%E3%81%AB%E9%80%9A%E4%BF%A1%E3%81%8C%E5%87%BA%E6%9D%A5%E3%81%AA%E3%81%84
  - 牧野さんもこれでやってる
- https://192.168.33.10/api/sirencontrolにヘッダもボディもつけずに送信
  - 0 The service refused to connect.
- http://192.168.33.10にヘッダもボディもつけずに送信
  - 404
- http://192.168.33.10/api/sirencontrolにヘッダもボディもつけずに送信

- http://localhost:8080/api/sirencontrolにヘッダもボディもつけずに送信
  - 200 OK
  - ```
    {"submergealert":[{"observPointCode":"0000000000000","observTime":"2020\/12\/21 08:58:50","sirenManual":0,"result":0,"errorMessage":"Syntax error"}]}
    ```
  - やはりIPの問題？
- Apatchの電源つけた
  - systemctl restart httpd
  

