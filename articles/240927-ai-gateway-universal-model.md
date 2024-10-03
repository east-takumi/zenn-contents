---
title: "Cloudflare AI GatewayのUniversalモデルを使ってみた"
emoji: ""
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Cloudflare", "AIGateway", "生成AI", "Tech"]
published: true
---

今回はCloudflareのAI GatewayのUniversalモデルをを触ってみたブログです👀
Cloudflare Meet-up Oita Vol.2でハンズオンを行ってみました！
https://cfm-cts.connpass.com/event/326062/

※ 2024-09-27時点の筆者が確認している情報/画面の状態です。

# AI Gatewayについて

LLMサービス（Ex. ChatGPT, Workers AI etc.）のプロキシーとして配置し、下記などの機能を提供しています。
- 各サービスへのエンドポイントを統一
- トークン状況をモニタリング
- Req/Res状態の可視化、分析
- キャッシュやRate Limit機能

![](/images/240927-ai-gateway-universal-model/ai_gateway_overview.png)

# Universalモデルについて

上記で「各サービスへのエンドポイントを統一」と示しましたが、それの延長のお話です。
まず、AI Gatewayでは各LLMサービスへのエンドポイントを提供しており、Cloudflareのマネジメント画面からcurlのサンプルなども提示してくれます。
必要情報（APIキーなど）をHeaderで登録する必要があります。

そのうえで、Universalモデルというのは、一つのエンドポイントで複数のLLMサービスのゲートウェイとして機能させるというものです。
例えば、OpenAIへのリクエストにトークン枯渇などで失敗したときに、Workers AIへリクエストを送り直すみたいないめーじです👀

![](/images/240927-ai-gateway-universal-model/ai_gateway_universal_model.png)

# Universalモデルをつかってみる

TBD