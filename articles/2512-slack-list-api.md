---
title: "Slackリスト機能をAPIから叩いてみた"
emoji: "📕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Slack","API"]
published: true
---

どうも、あべたく（[@east-takumi](https://x.com/east_takumi)）です。

# Slackのリスト機能とは？

昨年リリースされたSlackのリスト機能。
ちょっとした管理ごとで私も使っていました。

https://slack.com/intl/ja-jp/help/articles/27452748828179-Slack-%E3%81%A7%E3%83%AA%E3%82%B9%E3%83%88%E3%82%92%E4%BD%BF%E3%81%86

ただ、ちょっと癖があるのと、自動化して扱うためにはAPIがなかったりと少し使いにくさがありましたが、今年の秋にAPIがリリースされていましたので、それを試してみようという回です。

# Slackリスト機能をAPI Docsから見る

APIのネームスペースはslackListsとなってます。
ここではslackListsで現在使えるメソッドを種類ごとに区分けして紹介します。

## リスト自体を扱うメソッド

これらはリスト（枠）自体を作成/更新などを行うメソッドたちで、スキーマを指定したりなど、まずリスト機能を使う上で必要な操作を司るものになります。

### slackLists.create

ここで面白いのは下記二つの入力パラメータでした。
リストを複製するのにかなり柔軟に対応できます。

* copy_from_list_id: 作成済みでコピーしたいスキーマ構造のリストIDを指定できる
* include_copied_list_records: リストをコピーするときに元リストのアイテム（行データ）もコピーするか

todo_modeをtrueにすると、「担当者」「期限日」が追加されるようです。
（trueで作った後、アップデートでfalseにする挙動については未検証）

https://docs.slack.dev/reference/methods/slackLists.create

### slackLists.update

リストIDを指定して、対象リストのプロパティを変更しますが、現時点ではスキーマの変更（追加/削除/変更）はできないようです。

https://docs.slack.dev/reference/methods/slackLists.update

## アクセス権限を扱うメソッド

Slack Listsはチャンネルとは独立したオブジェクトのため、独自のアクセス権限の仕組みとなります。
これにより最小権限の運用（必要な人にのみ割り当て）ができるとのことです。

### slackLists.access.set

userやchannelに対してリストへのアクセス権限付与/更新ができます。
userやchannelは配列で複数指定できますが、どちらか（user or channel）のみ指定できるようです。
（両方指定する場合は2度APIを叩く必要がある）

https://docs.slack.dev/reference/methods/slackLists.access.set

### slackLists.access.delete

指定したid(user or channel)のアクセス権限を削除します。
`slackLists.access.set`と同じでuserとchannelの同時指定はできないようです。

https://docs.slack.dev/reference/methods/slackLists.access.delete

## アイテムを操作するメソッド

ここがこのAPIの価値に直結する箇所かなと思います。
これらは格納するデータ（アイテム、行データ）を操作するメソッドたちです。

### slackLists.items.create

createでは単にアイテムを追加するだけでなく、サブタスクの作成（`parent_item_id`）や、既存アイテムから複製（`duplicated_item_id`）などもできます。

https://docs.slack.dev/reference/methods/slackLists.items.create

### slackLists.items.update

updateでは既存アイテムのIDを指定して、指定したアイテムの特定のセルの値（`cells`）を更新します。
`cells`では行列どちらのIDも指定し、1つの値を指定する必要があります。

https://docs.slack.dev/reference/methods/slackLists.items.update

### slackLists.items.delete

アイテムを削除するメソッドですが、こちらは指定したIDを一つのみ削除するものです。

https://docs.slack.dev/reference/methods/slackLists.items.delete

### slackLists.items.deleteMultiple

`slackLists.items.delete`とほぼ同じですが、こちらは指定するIDを配列で複数選択できます。

https://docs.slack.dev/reference/methods/slackLists.items.deleteMultiple

## アイテム情報を取得するメソッド

これらのメソッドはアイテムの情報を取得するものになります。

### slackLists.items.list

ここではリスト内アイテムの「一覧」を取得します。
デフォルトでは100件までですが、ページネーションで分割取得もできます。

しかし、ちょっと使いにくいのが、「アイテムの特定のカラムがこういう値の場合」という検索の仕方はできないようです。
Ex. UI上ではフィルターでのデータの抽出に近いことです

https://docs.slack.dev/reference/methods/slackLists.items.list

### lackLists.items.info

ここではアイテムIDを一つ指定して、「詳細」情報を取得するものになります。

https://docs.slack.dev/reference/methods/slackLists.items.info

## リストの情報をエクスポートするメソッド

大量のアイテムが登録されているリストで、取得したいアイテムのデータが数千件など膨大なときに使うメソッドです。
面白いのはダウンロードの開始と分かれていること。
startをしたのちに少し時間をおいて、getを叩く必要がありました。
（非同期になるようにその仕組みをとってるっぽい？

### slackLists.download.start

https://docs.slack.dev/reference/methods/slackLists.download.start

### slackLists.download.get

https://docs.slack.dev/reference/methods/slackLists.download.get

# SlackリストAPIを使ってみた

では、実際にSlackリストAPIを使ってみて、めちゃ引っかかったことがあったので、ここで共有。

今回、tokenをbot_tokenを使用しまして、`slackLists.create`を実行し、 `"ok": true` が返ってきましたが、Slack上では一向に表示されない、、、、
なぜ、、、、、

とここで思い出す。

> Slack Listsはチャンネルとは独立したオブジェクトのため、独自のアクセス権限の仕組みとなります。

はい、作成したBotくん以外見れない設定となってました💦

なので、 `slackLists.access.set` を使って、Botくん以外にも閲覧権限を付与したところ、無事Slack上に表示されました👏

コマンドとしては下記のように作成します。
今回未検証ですが、user_tokenを利用すればaccess.setは必要ないと思われます。

``` bash
$ curl -X POST "https://slack.com/api/slackLists.create" \
-H "Authorization: Bearer <bot_token>" \
-H "Content-type: application/json; charset=utf-8" \
-d '{
  "name": "APIから作成したタスクリスト",
  "todo_mode": true
}'

{"ok":true,"list_id":"<list_id>","list_metadata":{"schema":[{"key":"name","name":"Name","is_primary_column":true,"type":"text","id":"xxx"},{"key":"todo_completed","name":"\u5b8c\u4e86\u6e08\u307f","is_primary_column":false,"type":"todo_completed","id":"xxx"},{"key":"todo_assignee","name":"\u62c5\u5f53\u8005","is_primary_column":false,"type":"todo_assignee","options":{"format":"multi_entity","default_value":null,"show_member_name":true},"id":"xxx"},{"key":"todo_due_date","name":"\u671f\u9650\u65e5","is_primary_column":false,"type":"todo_due_date","id":"xxx"}],"subtask_schema":[{"key":"name","name":"Name","is_primary_column":true,"type":"text","id":"xxx"},{"key":"todo_completed","name":"\u5b8c\u4e86\u6e08\u307f","is_primary_column":false,"type":"todo_completed","id":"xxx"},{"key":"todo_assignee","name":"\u62c5\u5f53\u8005","is_primary_column":false,"type":"todo_assignee","options":{"format":"multi_entity","default_value":null,"show_member_name":true},"id":"xxx"},{"key":"todo_due_date","name":"\u671f\u9650\u65e5","is_primary_column":false,"type":"todo_due_date","id":"xxx"}]}}%


$ curl -X POST "https://slack.com/api/slackLists.access.set" \
-H "Authorization: Bearer <bot_token>" \
-H "Content-type: application/json; charset=utf-8" \
-d '{
  "list_id": "<list_id>",
  "access_level": "read",
  "user_ids": ["<閲覧したいuserのuser_id>"]
}'

{"ok":true}%
```

# まとめ

今回はSlackリストAPIを触ってみたブログでした。
Slack APIのトークンの持ち方は元々ちょっとクセがあるので、そこさえクリアになればアイテムの複製は全然問題なさそうですね👀
ただ、アイテムの属性を見にいくことが現状できない（フィルターの動きの検索がAPI単位でできない）ことから、属性を見ようと思ったら、`slackLists.download`や`slackLists.items.list`などでリストの全情報を取得して、ローカル内で検索してみるみたいな処理が必要そうです。
ここら辺についても実装できるか次回ブログなどで試してみようと思います。