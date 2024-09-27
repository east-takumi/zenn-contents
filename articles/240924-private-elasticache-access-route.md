---
title: "Amazon ElastiCacheのクラスターへのアクセスルートについて調べてみた👀"
emoji: "🚪"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["AWS", "AmazonElastiCache", "Tech"]
published: false
---

※ 2024-09-24時点の筆者が確認している情報/画面の状態です。

たまたま業務でAmazon ElastiCacheを使う機会があり、個人的にMomentoなども使い始めたこともあったので、キャッシュサービスについてもう少し調べてみようと思った備忘録です。

# Amazon ElastiCacheについて

Amazon ElastiCacheは、AWSが提供するフルマネージド型のインメモリキャッシングサービスです。インメモリキャッシュエンジンとして、RedisとMemcachedをサポートしています。キャッシュサービスなので、セッションストアやリアルタイム分析などにも用いられます。

https://aws.amazon.com/jp/elasticache/

# クラスターへのアクセスについて

ElastiCacheインスタンスはAWSないからのみアクセスが前提とされているようです。
EC2インスタンスは同一のVPC内でも異なるVPCからでもアクセス可能ですが、異なるVPCの場合はVPCピアリングの使用が求められます。

https://docs.aws.amazon.com/ja_jp/AmazonElastiCache/latest/mem-ug/accessing-elasticache.html

## EC2からアクセス

VPC内にクラスタ-を起動した場合は、同じVPC内のEC2インスタンスからのみクラスターに接続できます。その際、セキュリティブループでクラスタへのネットワークのインバウンドアクセスを許可する必要があります。

![](/images/240924-private-elasticache-access-route/ec2.drawio.png)

### IaCで書いてみる
マネコンで検証中のため後日追記

## 外部からElastiCacheリソースにアクセス

先述にもありますが、ElastiCacheはAWS内からのみアクセスすることが前提となっています。
ただし、クラスターがVPC内にある場合、NATインスタンスを経由することで外部アクセスができます。

![](/images/240924-private-elasticache-access-route/nat_instance.drawio.png)

そして、AWSドキュメントにもはっきり記載されていますが、下記理由により、本番運用ではこの方法は推奨されていません。
ElastiCache特有というより、NATインスタンスを使うからのこその影響というかんじですね👀

- NATインスタンスを経由するので、クラスターのパフォーマンスに影響がでる（アクセス数が増えると影響も拡大）
- クライアント←→NATインスタンス間のトラフィックは暗号化されていない
- NATインスタンスを維持するための管理負担が増加
- NATインスタンスがSPOFになる

### IaCで書いてみる
マネコンで検証中のため後日追記

# 他の方法がないかを考えてみた

ElastiCacheのクラスターはPrivate Subnetに配置するを前提の上で、Momento Cacheみたくサクッと作ってアクセスできないかな〜と考えてみました。

今回は下記条件を前提とします。
- 使用するのはMemcached
- VPC内にクラスターを起動すること
- クラスターはPrivate Subnetに配置する

## VPC Lambda経由でアクセスする

API Gateway → VPC Lambda → ElastiCache

![](/images/240924-private-elasticache-access-route/vpc_lambda.drawio.png)

VPC Lambdaの向き先を指定のサブネットにすることで、APIのようにElastiCacheのクラスターにキャッシュの登録や読み込みができるかなと考えて、試してみます。
ただし、この方法にはいくつか問題点があります。

- キャッシュの登録などの操作は結局のところVPC Lambdaが行うことになるため、メンテナンス範囲が広がる
- レスポンス速度にかなり影響がでる
  - 同一のリクエストが大量に発生した場合は処理がスタックする可能性が高い

### IaCで書いてみる
マネコンで検証中のため後日追記

# まとめ

今回Momento Cacheのようにサクッとキャッシュ機能をテストできたり、外付け追加できたりしないかな？と思って、調べてみた&ほかの構成についても考えてみました。
そもそもインメモリキャッシュというところもありますので、めちゃくちゃこれができないと不便というより、パフォーマンス影響出ちゃうよねってことが多かったですね💦
そう思うとMomento Cacheでサクッとテストできるのはめちゃくちゃありがたいな〜と思いました^^;
もちろんですがセキュリティ要件（Ex. 登録するデータに個人情報がないか、管理サーバーをVPCに閉じたい etc.）次第なところもあるので、それぞれ使い所なところが多いと思います。
ということで、今回の備忘録は以上となります。





