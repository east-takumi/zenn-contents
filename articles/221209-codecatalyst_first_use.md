---
title: "Amazon Code Catalystを触ってみた"
emoji: "🏃🏻‍♂️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["AWS", "CodeCatalyst", "感想", "Tech"]
published: true
---

この記事は[CAMPFIRE Advent Calendar 2022](https://qiita.com/advent-calendar/2022/campfire)の9日目の記事です。

今年はAWS re:Invent 2022で発表されたAmazon CodeCatalystの触ってみたブログです！

:::message
2022/12/09時点での操作ログになります
この時点ではプレビュー版でオレゴンリージョンのみで提供されています
:::

# Amazon Code Catalystについて
> ソフトウェア開発および配信の統合サービスである Amazon CodeCatalyst を使用すると、ソフトウェア開発チームは AWS 上でアプリケーションを迅速かつ簡単に計画、開発、コラボレーション、構築、および配信できるため、開発ライフサイクル全体でフリクションが軽減されます。

※ AWSさんブログから引用

うむ、なんかすごそう（語彙力）

# とりあえず使ってみる

## スペースを構築
1. Amazon CodeCatalystのページにアクセスして、「Sign up」or「Sing in」をクリック
  実はAWS マネジメントコンソールとは全く別ページのようなので注意。
  ![](https://storage.googleapis.com/zenn-user-upload/53c20646de43-20221209.png)
2. AWS Builder IDでログイン
  AWS Builder IDなるものを使うのだが、これはAWSアカウント（マネジメントコンソールにアクセスするもの）とは全く別物なので作成した記憶がない人はここで作成しましょう。
  ![](https://storage.googleapis.com/zenn-user-upload/6553ca7360ed-20221209.png)
3. AWSアカウントとの紐付け
  ここで紐付けたAWSアカウントにCodeCatalystで発生した請求を送ったり、後述するAWSサービスにはこのAWSアカウントでアクセスする。
  ![](https://storage.googleapis.com/zenn-user-upload/9ed8251129a6-20221209.png)
    1. 紐付けするAWSアカウントのマネジメントコンソールからはAWSアカウントIDをコピー
    ![](https://storage.googleapis.com/zenn-user-upload/208465e4d719-20221209.png)
    2. コピーしたIDを入力、「Verify」をクリックすると、マネジメントコンソールに遷移する
    3. 「Verify space」をクリックし、紐付け成功
    ![](https://storage.googleapis.com/zenn-user-upload/23190c5e879b-20221209.png)

    ![](https://storage.googleapis.com/zenn-user-upload/1737bbcb787f-20221209.png)
    
    4. マネジメントコンソールでCodeCatalyst用のスタンダードのIAMを作成
    ![](https://storage.googleapis.com/zenn-user-upload/b26da23adb92-20221209.png)

    ![](https://storage.googleapis.com/zenn-user-upload/d953073faab3-20221209.png)

4. スペースを作成
  3まで終了してCodeCatalystのページに戻って、「Create Space」をクリックしてスペースの作成は完了。
  ![](https://storage.googleapis.com/zenn-user-upload/d7ef0b9cecd9-20221209.png)

## スペース管理ページを見ていく

- Projects
  プロジェクト（作り方については後述）の一覧がここで見れる。
  ![](https://storage.googleapis.com/zenn-user-upload/37f63681576d-20221209.png)

- Activity
  スペース内での行動（誰が何のアクションをしたのか）が記録されていて、ここで見ることができる。
  アクションごとのフィルタリングできる。
  ![](https://storage.googleapis.com/zenn-user-upload/0aab019c0a91-20221209.png)

- Members
  スペースのメンバー管理がここででき、メンバーの招待もできる。
  ![](https://storage.googleapis.com/zenn-user-upload/e64f50db9a3a-20221209.png)

- Installed extensions
  ほかサービスとの接続のための拡張機能を管理できる。
  Ex. GitHubのリポジトリとの接続 etc.
  ![](https://storage.googleapis.com/zenn-user-upload/b1c5ea746b82-20221209.png)

- AWS accounts
  紐付けるAWSアカウントの管理ができる。
  ![](https://storage.googleapis.com/zenn-user-upload/1f89eca7b7da-20221209.png)

- Space settings
  スペース自体の名前の変更などができる。
  ![](https://storage.googleapis.com/zenn-user-upload/7b55db6da5d6-20221209.png)

- Billings
  CodeCatalystの利用状況などを確認できる。
  ![](https://storage.googleapis.com/zenn-user-upload/9dc724b66305-20221209.png)

## プロジェクトを作成
1. 「Create Project」をクリック
2. プロジェクトの作成の仕方を選択
    1. blueprintからテンプレートを選択するかどうかを選択
      今回はblueprintを使う
      ![](https://storage.googleapis.com/zenn-user-upload/11dbb39143f3-20221209.png)

    2. 一覧から作成するblueprintを選択
        - Webアプリケーションやサーバレスアプリケーションなどが準備されている
        ![](https://storage.googleapis.com/zenn-user-upload/d63ffc72d7a5-20221209.png)

        - 選択するとblueprintで定義されている構成の説明やデプロイで使うIAMロールの内容などを見ることができる
        ![](https://storage.googleapis.com/zenn-user-upload/b34e1123037b-20221209.png)
        
3. プロジェクトの名前やプロジェクトを紐付けるAWSアカウントやIAMなどを設定
  ![](https://storage.googleapis.com/zenn-user-upload/5e87faf6ceb2-20221209.png)

  - Lambda構成がある場合、どの言語で構成するかも選択する必要がある
  ![](https://storage.googleapis.com/zenn-user-upload/cd91ca653254-20221209.png)


4. 3まで完了したら「Create Project」をクリックして、プロジェクトの作成は完了
  ![](https://storage.googleapis.com/zenn-user-upload/5be9ade75680-20221209.png)

## プロジェクトの中身を見ていく

### Overview
  プロジェクトの概要などを確認できる。
  ![](https://storage.googleapis.com/zenn-user-upload/baeacb7423c2-20221209.png)
  
### Issues
  Issueの管理ができる。使い方については後述。
  ![](https://storage.googleapis.com/zenn-user-upload/2975c63ef0e3-20221209.png)

### Code
  リポジトリやPull Request、開発環境の管理ができる。
  ![](https://storage.googleapis.com/zenn-user-upload/89f94cc27c56-20221209.png)

### CI/CD
  CI/CDのワークフローの作成や実行状況の管理ができる。
  ![](https://storage.googleapis.com/zenn-user-upload/365391a1e0fc-20221209.png)

### Reports
  テストのカバレッジなどが確認できる。
  ![](https://storage.googleapis.com/zenn-user-upload/f399d7a75993-20221209.png)

  ![](https://storage.googleapis.com/zenn-user-upload/acec043c8700-20221209.png)

### ProjectSetting
  メンバーの管理やCI/CD実行結果の通知設定などができる。
  ![](https://storage.googleapis.com/zenn-user-upload/c516a0f92fbb-20221209.png)


## Issueを使ってみる
- Issueを作成
  Issueの内容もMarkdownで記載できるし、進行度や優先度、カスタムフィールドの作成もできる
  ![](https://storage.googleapis.com/zenn-user-upload/2c25c5d72363-20221209.png)

  ![](https://storage.googleapis.com/zenn-user-upload/2dd7150a573f-20221209.png)

- Issueの管理
  ボード形式で進行度の変更も直感的にできる
  ![](https://storage.googleapis.com/zenn-user-upload/d51f3ff077df-20221209.png)


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

1. 「Create Dev Enviroment」をクリックすると選択できるIDEが表示されるので、VScodeを選択
  ![](https://storage.googleapis.com/zenn-user-upload/3149fd9368ef-20221209.png)

2. どのブランチでつかうかなどを選択して「Create」をクリック
  ![](https://storage.googleapis.com/zenn-user-upload/31f483939e4b-20221209.png)

3. ローカルのVScodeが起動するのでプロジェクトを確認
  AWS Tool Kitがインストールされてない場合は、事前にインストールしておく。

4. 使い終わったあと
  CodeCatalystのCode→Dev Enviromentから対象の開発環境設定を選択して、Actions→Stopをクリックする。
  :::message
  この開発環境設定をどのくらいの時間使っているかが無料利用枠だと制限があるため
  どのくらい使用したかはSpaceのBillingsから確認できる
  :::

5. もしIDEを変更したい場合、CodeCatalystのCode→Dev Enviromentから、対象の開発環境設定を削除してからIDEの選択をし直す事が可能

![](https://storage.googleapis.com/zenn-user-upload/7f04b805988e-20221209.png)

## コードを変更~デプロイまでしてみる
今回は出力する文章を変更してみる。

1. 作業ブランチを作成
    1. 「Source Reqositories」→「Action」→「Create branch」をクリックする
      ![](https://storage.googleapis.com/zenn-user-upload/6a1f011b997f-20221209.png)

    2. ブランチの名前とどのブランチから派生させるかを入力して「Create」をクリックしたら作成完了
      ![](https://storage.googleapis.com/zenn-user-upload/d12de4e944dc-20221209.png)

      ![](https://storage.googleapis.com/zenn-user-upload/b177a2390a1b-20221209.png)

2. コードの変更とPushまで
  IDEで出力文章を変更して，IDEのターミナルからリモートにPushする。（今回は省略）

3. Pull Requestの作成
    1. 「Pull requests」→「Create pull request」をクリックする
    2. 情報を入力して、「Create」をクリックする
        ```
        Source branch: マージ元のブランチ（作業ブランチ）
        Destination branch: マージ先のブランチ（main）
        Pull request title: Pull Requestのタイトル
        Pull request description: Pull Requestの説明（任意項目）
        ```
    ![](https://storage.googleapis.com/zenn-user-upload/40c4c53845da-20221209.png)

    3. 作成が完了すると、「Changes」で差分を確認できる
      ![](https://storage.googleapis.com/zenn-user-upload/31c1ff832c4d-20221209.png)

4. mainブランチにマージ
  対象のPull Requestで「Merge」をクリックし，マージの方式を選択したらマージ完了
  ![](https://storage.googleapis.com/zenn-user-upload/c022a51cb20a-20221209.png)

5. デプロイワークフロー実行状況を確認
  現状何故か落ちてる...(なんで！？😇)
  調査中！
  ![](https://storage.googleapis.com/zenn-user-upload/fb9b2f7ad464-20221209.png)

6. 動作確認
  続報待ち！！！！

# 使ってみての感想
私が今までで触ってみての感想になります。

## いいなーと思った点
- あらゆる機能をオールインワンしてる感じ
  - 今までであれば，VScode，Asana，Github etc. のように様々なサービスを組み合わせて運用する形でやっていたことが，CodeCatalyst一つで完結する方向性に進んでいる印象
  - それぞれの機能がスペシャリストレベル（コード管理ならGitHubとか）とまではなくとも、4名くらいまでの小規模&&ハッカソンなど数日で完結するようなプロジェクトであれば、積極的に使っていきたいと思った
- IDEを自前で持っているものを使え、ローカルマシンで環境構築などをしなくてよいからとても楽
  - IDEからターミナルを見てみるとAmazon Linuxが動いてた👀
  - （未検証）ローカルマシン由来のエラーとか回避できる？
    - Ex.チーム開発してると，ある人は起動できてある人は起動できない→原因ローカルマシンのセッティングの問題のようなもの

## もうちょっとと思う点
出たばかりのサービスなので、まだまだこれからのアップデートに大きな期待をしていることを前提でこうなったらいいなぐらいで捉えてください🙏

- 一部ラジオボタンとかが使いにくかった
  - blueprintの選択のところなどはラジオボタンのところをクリックしないとだめだったので、そのへんの小さな部分は今後ちょっとずつ変更してくれると信じてる
- IAM絡みの権限エラーなどがぱっとみわからない
  - デプロイがエラーしててAWSリソース絡みだと全くその内容が見えないようで、見れたらうれしい

## まとめ
昨年のAmplify Studioといい、アプリケーション層に向けた新サービスが年々出てきていますが、CodeCatalystはまさか！こんなものがでてくるなんて！という感じです。

商用で使うかどうかはまだプレビューということもあり、まだ様子見かなと。
今後使い所になるかな？と思ってるのは、
- これから新規で始めるプロジェクト（人数も数人規模）
- 前述にもあるハッカソン
- プロジェクトの関係でローカルマシンに1から環境構築する必要はあるがHTMLや文章の修正ぐらいのPushしかしないなどの方
とかですかね

すでにプロジェクトがあり、サポート体制などが十分であるというチームからすると、まだ物足りなく感じると思いますが、今後CodeCatalystに乗り代わってくる未来は十分考えれると思ってます。

個人的に使用感としては結構いいなーと思っていて、今後のアップデート注目していきます。

# 参考記事
- https://aws.amazon.com/jp/blogs/aws/announcing-amazon-codecatalyst-preview-a-unified-software-development-service/
- https://aws.amazon.com/jp/blogs/news/announcing-amazon-codecatalyst-preview-a-unified-software-development-service/

