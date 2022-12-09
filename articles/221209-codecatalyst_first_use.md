---
title: "Amazon Code Catalystを触ってみた"
emoji: ""
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["AWS", "CodeCatalyst", "感想", "Tech"]
published: false
---

この記事は[CAMPFIRE Advent Calendar 2022](https://qiita.com/advent-calendar/2022/campfire)の9日目の記事です。

今年はAWS re:Invent 2022で発表されたAmazon CodeCatalystの触ってみたブログです！

# Amazon Code Catalystについて
Amazon Code Catalystは


# スペースを構築
1. Amazon CodeCatalystのページにアクセスして、「Sign up」or「Sing in」をクリック
  実はAWS マネジメントコンソールとは全く別ページのようなので注意。
  ![]()

2. AWS Builder IDでログイン
  AWS Builder IDなるものを使うのだが、これはAWSアカウント（マネジメントコンソールにアクセスするもの）とは全く別物なので作成した記憶がない人はここで作成しましょう。

3. AWSアカウントとの紐付け
  ここで紐付けたAWSアカウントにCodeCatalystで発生した請求を送ったり、後述するAWSサービスにはこのAWSアカウントでアクセスする。

    1. 紐付けするAWSアカウントのマネジメントコンソールからはAWSアカウントIDをコピーする。

    2. コピーしたIDを入力、「Verify」をクリックすると、マネジメントコンソールに遷移する。

    3. 「Verify space」をクリックし、紐付け成功。

    4. CodeCatalyst用のスタンダードのIAMを作る。

4. スペースを作成
  3まで終了してCodeCatalystのページに戻って、「Create Space」をクリックしてスペースの作成は完了。

# スペース管理ページを見ていく

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
  

# プロジェクトを作成
1. 「Create Project」をクリックする

2. プロジェクトの作成の仕方を選択する
    1. blueprintからテンプレートを選択するかどうかを選択
      今回はblueprintを使う

    2. 一覧から作成するblueprintを選択する
        - Webアプリケーションやサーバレスアプリケーション，ビデオオンデマンド構成などが準備されている
        - 選択するとblueprintで定義されている構成の説明やデプロイで使うIAMロールの内容などを見ることができる
        
3. プロジェクトの名前やプロジェクトをどのAWSアカウントのリソースで作成するかなどを設定して、
  - Lambda構成がある場合、どの言語で構成するかなども選択する

4. 3まで完了したら「Create Project」をクリックして、プロジェクトの作成は完了


# プロジェクトの中身を見ていく

## Overview

## Issues

## Code

## CI/CD

## Reports

## ProjectSetting

# Issueを使ってみる

# IDEの接続を使ってみる

# コードを変更~デプロイまでしてみる

# Slack連携してみる

# 使ってみての感想
私が今までで触ってみての感想になります。

## いいなーと思った点
- 


## もうちょっとと思う点
- 

## まとめ


# 参考記事
- 

