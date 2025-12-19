---
title: "Slackリスト機能をAPIから試してみた"
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

### slackLists.create

https://docs.slack.dev/reference/methods/slackLists.create

### slackLists.update

https://docs.slack.dev/reference/methods/slackLists.update

## アクセス権限を扱うメソッド

### slackLists.access.set

https://docs.slack.dev/reference/methods/slackLists.access.set

### slackLists.access.delete

https://docs.slack.dev/reference/methods/slackLists.access.delete

## アイテムを操作するメソッド

### slackLists.items.create

https://docs.slack.dev/reference/methods/slackLists.items.create

### slackLists.items.update

https://docs.slack.dev/reference/methods/slackLists.items.update

### slackLists.items.delete

https://docs.slack.dev/reference/methods/slackLists.items.delete

### slackLists.items.deleteMultiple

https://docs.slack.dev/reference/methods/slackLists.items.deleteMultiple

## アイテム情報を取得するメソッド

### slackLists.items.list

https://docs.slack.dev/reference/methods/slackLists.items.list

### lackLists.items.info

https://docs.slack.dev/reference/methods/slackLists.items.info

## リストの情報をエクスポートするメソッド

### slackLists.download.start

https://docs.slack.dev/reference/methods/slackLists.download.start

### slackLists.download.get

https://docs.slack.dev/reference/methods/slackLists.download.get

# SlackリストAPIを使って