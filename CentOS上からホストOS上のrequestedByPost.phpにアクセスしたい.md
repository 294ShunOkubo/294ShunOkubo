# CentOS上からホストOS上のrequestedByPost.phpにアクセスしたい

## 引っかかり
- CentOSから見たホストOSのipがわからない

    
## やったこと
- https://qiita.com/tmiki/items/d786edf221feb4cb1af5 を読んで現状の理解
  - CentOSから見たwin10のipがわかればよいこと
- win側コマンドプロンプトでipconfig実行
　- イーサネットアダプタVirtualBox Host-Only Networkから192.168.56.1と192.168.33.1が振られている＝これのどっちかと思われる
   - たぶん192.168.33.1
    - 他はすぐfalseを吐くのに対しこれだけフリーズする＝処理を待っている
- サーバが立っていない？
  - xampp起動


## これから試すこと
- php -S　コマンドでサーバーを明示的に立てる

## 正解
- 10.0.2.2
  - 前やんなかったっけ？
    - xampp起動してなかったからやろ
