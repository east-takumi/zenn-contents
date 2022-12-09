---
title: "Amazon Code Catalystを触ってみた"
emoji: "🏃🏻‍♂️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["AWS", "CodeCatalyst", "感想", "Tech"]
published: false
---

この記事は[CAMPFIRE Advent Calendar 2022](https://qiita.com/advent-calendar/2022/campfire)の9日目の記事です。

今年はAWS re:Invent 2022で発表されたAmazon CodeCatalystの触ってみたブログです！

# Amazon Code Catalystについて
Amazon Code Catalystは

# とりあえず使ってみる

## スペースを構築
1. Amazon CodeCatalystのページにアクセスして、「Sign up」or「Sing in」をクリック
  実はAWS マネジメントコンソールとは全く別ページのようなので注意。
  ![]()

2. AWS Builder IDでログイン
  AWS Builder IDなるものを使うのだが、これはAWSアカウント（マネジメントコンソールにアクセスするもの）とは全く別物なので作成した記憶がない人はここで作成しましょう。

3. AWSアカウントとの紐付け
  ここで紐付けたAWSアカウントにCodeCatalystで発生した請求を送ったり、後述するAWSサービスにはこのAWSアカウントでアクセスする。

    1. 紐付けするAWSアカウントのマネジメントコンソールからはAWSアカウントIDをコピーする

    2. コピーしたIDを入力、「Verify」をクリックすると、マネジメントコンソールに遷移する

    3. 「Verify space」をクリックし、紐付け成功

    4. CodeCatalyst用のスタンダードのIAMを作る

4. スペースを作成
  3まで終了してCodeCatalystのページに戻って、「Create Space」をクリックしてスペースの作成は完了。

## スペース管理ページを見ていく

- Projects
  プロジェクト（作り方については後述）の一覧がここで見れる。

- Activity
  スペース内での行動（誰が何のアクションをしたのか）が記録されていて、ここで見ることができる。
  アクションごとのフィルタリングできる。

- Members
  スペースのメンバー管理がここででき、メンバーの招待もできる。

- Installed extensions
  ほかサービスとの接続のための拡張機能を管理できる。
  Ex. GitHubのリポジトリとの接続（この接続手順は後述） etc.

- AWS accounts
  紐付けるAWSアカウントの管理ができる。

- Space settings
  スペース自体の名前の変更などができる。

- Billings
  

## プロジェクトを作成
1. 「Create Project」をクリックする

2. プロジェクトの作成の仕方を選択する
    1. blueprintからテンプレートを選択するかどうかを選択
      今回はblueprintを使う

    2. 一覧から作成するblueprintを選択する
        - Webアプリケーションやサーバレスアプリケーションなどが準備されている
        - 選択するとblueprintで定義されている構成の説明やデプロイで使うIAMロールの内容などを見ることができる
        
3. プロジェクトの名前やプロジェクトをどのAWSアカウントのリソースで作成するかなどを設定して、
  - Lambda構成がある場合、どの言語で構成するかなども選択する

4. 3まで完了したら「Create Project」をクリックして、プロジェクトの作成は完了


## プロジェクトの中身を見ていく

### Overview
  プロジェクトの概要などを確認できる。
  
### Issues
  Issueの管理ができる。使い方については後述。

### Code
  リポジトリやPull Request、開発環境の管理ができる。

### CI/CD
  CI/CDのワークフローの作成や実行状況の管理ができる。

### Reports
  テストのカバレッジなどが確認できる。

### ProjectSetting
  メンバーの管理や通知設定（Slackへの通知が可能でやり方は後述）などができる。


## Issueを使ってみる
- Issueを作成
  Issueの内容もMarkdownで記載できるし、進行度や優先度、カスタムフィールドの作成もできる

- Issueの管理
  ボード形式で進行度の変更も直感的にできる


## IDEの接続を使ってみる
CodeCatalystではCloud9だけではなく、所有しているIDEを利用することができるので，今回はVScodeでつかってみた。


:::message
- 2022/12/09時点で選択できるIDEはCloud9とVScode、JetBrainの一部IDEのみ
- 実態はAWS Tool Kitなどでsshしてるっぽい👀
  （間違ってたらツッコミお願いします！）
- 開発環境（IDE）の選択は1つ/1ブランチ
  - 付け替えは可能
  - 複数ユーザーで使う場合の挙動は未検証
:::

1. 「Create Dev Enviroment」をクリックすると選択できるIDEが表示されるので、VScodeを選択する
2. どのブランチでつかうかなどを選択して「Create」をクリックする
3. ローカルのVScodeが起動するのでプロジェクトを確認
  AWS Tool Kitがインストールされてない場合は、事前にインストールしておく。
4. 使い終わったあと
  CodeCatalystのCode→Dev Enviromentから対象の開発環境設定を選択して、Actions→Stopをクリックする。
    :::message
    この開発環境設定をどのくらいの時間使っているかが無料利用枠だと制限があるため
    どのくらい使用したかはSpaceのBillingsから確認できる
    :::
5. もしIDEを変更したい場合、CodeCatalystのCode→Dev Enviromentから、対象の開発環境設定を削除してからIDEの選択をし直す事ができる

## コードを変更~デプロイまでしてみる
今回は出力する文章を変更してみる。

1. 作業ブランチを作成
2. コードの変更とPushまで
3. Pull Requestの作成
4. mainブランチにマージ
5. デプロイワークフロー実行状況を確認
6. 動作確認

## Slack連携してみる

# 使ってみての感想
私が今までで触ってみての感想になります。

## いいなーと思った点
- あらゆる機能をオールインワンしてる感じ
  - 今までであれば，VScode，Asana，Github etc. のように様々なサービスを組み合わせて運用する形でやっていたことが，CodeCatalyst一つで完結する方向性に進んでいる印象
  - それぞれの機能がスペシャリストレベル（コード管理ならGitHubとか）とまではなくとも、4名くらいまでの小規模&&ハッカソンなど数日で完結するようなプロジェクトであれば、積極的に使っていきたいと思った
- IDEを自前で持っているものを使えるのと

## もうちょっとと思う点
出たばかりのサービスなので、まだまだこれからのアップデートに大きな期待をしていることを前提でこうなったらいいなぐらいで捉えてください🙏

- 一部ラジオボタンとかが使いにくかった
  - blueprintの選択のところなどはラジオボタンのところをクリックしないとだめだったので、そのへんの小さな部分は今後ちょっとずつ変更してくれると信じてる
- IAM絡みの権限エラーなどがぱっとみわからない
  - デプロイがエラーしててAWSリソース絡みだと全くその内容が見えないようで、できたら見れたらうれしい

## まとめ


# 参考記事
- 
