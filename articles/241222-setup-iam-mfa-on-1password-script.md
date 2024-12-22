---
title: "IAMユーザーのMFAを1Passwordに登録するシェルスクリプトを作ってみた"
emoji: "📱"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["AWS", "IAM", "1Password", "Tech"]
published: true
---

どうも、あべたく（[@east-takumi](https://x.com/east_takumi)）です。


この記事は[KDDIアジャイル開発センター（KAG） Advent Calendar 2024](https://qiita.com/advent-calendar/2024/kag)の22日目の記事です。
https://qiita.com/advent-calendar/2024/kag

# 概要
AWS IAMユーザーのMFAの登録、有効化をシェルスクリプトで実行します。
これによりスマホでQRコードを読み込むなどをしなくてもMFA有効化までを目指します。
登録先のデバイスには1Passwordを使用します。

## 1Password CLIについて
パスワードマネージャーとして1Passwordを知っている方は多いと思います。
開発者向けにCLIやSDKが提供されており、今回は1Password CLIを用いて開発を進めます。

※ 1Password CLIのセットアップなどは本ブログでは取り扱いません。

詳しい使い方は下記ブログやWEBページを参照ください👀
https://qiita.com/tonnsama/items/46e560415a55a0b7a842

1Password CLI Docs
https://developer.1password.com/docs/cli


# 作ってみた

実際のコードはこちらをご参照ください👀
https://github.com/east-takumi/aws-iam-mfa-to-1pass

## IAMのMFA
IAMユーザーのMFA利用にあたって、2ステップあり、それぞれAWS CLIのコマンドが別れているので注意して下さい。

AWS CLI、またはAWS APIでMFAデバイスを割り当てるAWS Docs
https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/id_credentials_mfa_enable_cliapi.html

### 1.新しい仮想MFAデバイスを作成する
まず、新規で仮想MFAデバイスを作成します。
この時点で仮想MFAデバイスのシリアルナンバー（ARN）が発行され、OTP(One Time Password)を登録するためのQRコードをダウンロードすることになります。
マネジメントコンソールだとでいうとQRコードを表示するところまでになります。

https://docs.aws.amazon.com/cli/latest/reference/iam/create-virtual-mfa-device.html

今回はQRコードを発行する形を取っていますが、Base32の文字列を発行するという手もあります。
otpauthの形式をプログラムの中で組み合わせたい！という方はそちらをおすすめします。

### 2.MFAデバイスを有効にし、指定したIAMユーザーに紐づける
1の仮想MFAデバイス作成が完了したら、それをIAMユーザーに紐づけていきます。
マネジメントコンソールだと、OTPを2個入力してMFA登録完了するところまでになります。

https://docs.aws.amazon.com/cli/latest/reference/iam/enable-mfa-device.html

ここであまり引っかかるところはありませんが、一つIAMユーザー名が必須となっているのでその部分だけ注意が必要です。

## 1Password CLIを用いて、登録する
今回は1Passwordにすでに登録してあるアイテムに対して、OTPを登録していきます。

今回注意する点として、OTPの登録方法です。
`op item create`でも`op item edit`でもOTPを登録する専用のオプションはないため、「custom field」を使う必要があります。

https://developer.1password.com/docs/cli/item-create/#with-assignment-statements

## 検証不十分なところ
サラッとスルーしていましたが、OTPを登録するには形式がちゃんとあって、「otpauth URI」というフォーマットがあります。
普段読み込んでいるQRコードにはこの「otpauth URI」が埋め込まれている形になります。

### QRコード読み取りするZBar
上記でも示しましたが、今回はQRコードをダウンロードしてきて、そこに登録されているotpauth URIを読み込む必要がありました。

そこで今回「ZBAR BAR CODE READER」を使いました。
コマンドラインでQRコードを読み取りを行いました。

https://github.com/mchehab/zbar

で、このZBarを初めて使ったのもあり、「検証不十分」と記載させていただきました🙏
もし、ここ怖いな〜ということであれば、先述にもあるようにotpauth形式を自身で組み合わせる処理を追加することをおすすめします。

# まとめ

実際使ってみましたが、1分ほどで利用でMFA有効化までできるようになり、ログインできました！
前段階でIAMユーザーやAWS CLIの準備などが必要ですが、一度準備してしまえばという感じです。
AWS側に登録されている仮想MFAデバイスの情報を無効化/削除を行えば、再度実行→利用可能になるので、有効期限による切り替えなどがちょこっと楽になる位の認識でいて下さい💦
（すでに登録されている場合などを考慮すると複雑化するため、今回はスコープ外としました。）

1Password CLIも初めて使ってみましたが、オプションなくてやばいかも💦とか最初思っていましたが、必要なところに手が届くので、パスワード管理やMFAの管理などで自動化/効率化したい！という方は一度ためしてみてほしいな（というかあべたくはもっと使ってみよう！と意気込んでいる）と思いました👀

# 余談
正直、どこでこれ使うねん！って方もいらっしゃるかと思いますが、先日AWS マネジメントコンソール画面のUIのアプデ？と考えられますが、マネジメントコンソール画面の表示ができないUI部品がでて、ちょうどMFAもその範囲ぽい！！どうやってQRコード表示すりゃええねん！となったのが、今回のブログの発端です💦
使ってたPCのOSがかなり古い&&それによるブラウザーの対応バージョンが限界を迎えたのではないか。。。と推測してます💦
→PC OSの問題が大きいと思ったので、この問題に深堀りはしません！

