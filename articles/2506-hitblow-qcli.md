---
title: "Amazon Q Developer CLIでHit&Blowゲームを作る"
emoji: "0️⃣"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["AWS","CLI","Amaxon Q","Tech"]
published: true
---

どうも、あべたく（[@east-takumi](https://x.com/east_takumi)）です。

今回はAmazon Q Developer CLIを使って、Hit&Blowのゲームを作ってみました！
(Hit&Blowについては下記を参照)
https://bubudoufu.com/webapp/hit-and-blow/

個人的にQ Chatをめちゃくちゃ使っていたのですが、「Amazon Q CLI でゲームを作ろう」キャンペーン（下記リンク参照）も出ていたので、やってみるか〜くらいの気持ちで始めました。
SNS等に他の方々が作ってみた作品などもあるので、ぜひみてみてください〜
（そしてよければご自身でも作ってみてください〜）
https://aws.amazon.com/jp/blogs/news/build-games-with-amazon-q-cli-and-score-a-t-shirt/


# 完成品はこちら
完成品を見る方が早いと思うので、こちらから遊んでみてください〜
加えてソースコードもこちらに
https://east-takumi.github.io/anzennuma/
https://github.com/east-takumi/anzennuma

# やったこと
やったことをつらつらと書いていきます👀

## コマンドライン用Amazon Qのインスール
私自身はもともとAmazon Qをターミナルの補完機能としても使っていたため、インストール済みでしたが、気になる方は下記リンクよりセッティングしてください。
https://docs.aws.amazon.com/ja_jp/amazonq/latest/qdeveloper-ug/command-line-installing.html

## 利用するモデルの変更
この作成に着手する直前で、Claude-4-Sonetに対応したという話が出ていたのもあり、せっかくだしと思って利用してみることにしました。
q chatの中で`model` コマンドを実行するとclaudeのモデルを選択することができ、デフォルトでは3.7が選択されてます。

```bash
> /model

? Select a model for this chat session ›
  claude-4-sonnet
❯ claude-3.7-sonnet (active)
  claude-3.5-sonnet

```

## 実際のプロンプト
実際にq chatに投げたプロンプトは下記になります。

```bash
このディレクトリでブラウザで動くHit&Blowのゲームを作ってください。
UIは日本語と英語をユーザーによって切り替えることができ、全てが一致した時におめでとうを伝えるアニメーションも欲しいです。
数字の入力はキーボードからでもUIのボタンでもどちらでも構いません。
そして、のちにこれをGitHubPagesで公開したいので、静的に作ってください。
```

はい、、、これだけです←ｵｲｱﾍﾞ
「Hit&Blowとは？」なども全く指令を出しませんでしたが、ゲームの根幹をなす基本的な機能が全て実装されていました。（すごい、おそらく頑張って検索してきたんだろうな、、、←有名なゲームだろLLMが知ってるっぽい）
ちゃんとクリアした際の演出も追加されていたし、入力もボタンとキーボードどちらでもできるようになってます。
このように指示を具体的に出すと実現できる範囲がどんどん広がってるんだなと実感しますね👀

# まとめ
実際に使ってみて、ついにここまできたか、、、と思う方も多いはずです。
今回は静的サイトでの公開を前提にプロンプトを書きましたが、「Pythonを使って〜」も裏で試してみて、
そうすると、Pythonの環境（バージョン管理ツールからのinstallや仮想環境の作成など）を作るところから作業してくれたり、、、、
いや〜〜本当にすごい（語彙力）
今回はmodelの比較について詳しくは話しませんでしたが、いつも使っているのは3.7だったので、claude-4-sonetもすげ、、、、ってなってました。
ぜひみなさん一度触ってみながら、こんなこともできるんだ〜とか体験してみてください〜