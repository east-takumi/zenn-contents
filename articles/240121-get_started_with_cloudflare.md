---
title: "Cloudflareアカウント作成してみる！"
emoji: "🚪"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Cloudflare", "Tech"]
published: true
---

※ 2024-01-21時点の筆者が確認している情報/画面の状態です。

[Cloudflare](https://www.cloudflare.com/ja-jp/)のアカウント登録手順についてまとめました！

![](https://storage.googleapis.com/zenn-user-upload/7650d71a7ba0-20240121.png)

# 必要なもの
- インターネットに接続できるブラウザ
- アカウント登録に使うメールアドレスとパスワード
- 2要素認証に使うモバイル端末の認証アプリ
  - Ex. Google Authenticator etc.

# 手順

## 1. アカウント登録

Cloudflare トップ画面にアクセスして、右上の「ログイン」をクリック。

https://www.cloudflare.com/ja-jp/

![](https://storage.googleapis.com/zenn-user-upload/0cae9b01465a-20240121.png)

遷移した画面（下図）がアカウント登録後にログインする際に使う画面です。
今回はアカウント登録を行うので「サインアップ」をクリックします。

![](https://storage.googleapis.com/zenn-user-upload/6e4e43fc8fcf-20240121.png)

遷移した画面（下図）がアカウント登録の画面になります。
自身のアカウントにログインするための、「メールアドレス」と「パスワード」を設定していきます。

![](https://storage.googleapis.com/zenn-user-upload/536f1b34072b-20240121.png)

パスワード要件をすべて満たす必要があるのを注意してください。
下記のようにパスワード要件が満たされた && ロボットでないとこが確認されている状態までなれば準備完了です。
「サインアップ」をクリックします。

![](https://storage.googleapis.com/zenn-user-upload/6ca9c7d21d74-20240121.png)

下図のような画面に遷移すればアカウント登録完了です！
念のため、右上の人のマークをクリックして、「マイ プロフィール」をクリックします。

![](https://storage.googleapis.com/zenn-user-upload/2414ac17e23e-20240121.png)

![](https://storage.googleapis.com/zenn-user-upload/4657be0c7b8f-20240121.png)

遷移した画面のメールアドレスの部分を確認します。
先程設定したメールアドレス設定されいる && メールアドレスのあとに「(確認済み)」と表記があるかを確認にします。

![](https://storage.googleapis.com/zenn-user-upload/9d3100ea3d58-20240121.png)

「(確認済み)」と表記がない場合は登録したメールアドレス宛に下図のようなメールが届いているので、メールボックスを確認します。
Cloudflareからのメールアドレス認証の旨のメールになりますので、必ず確認します。
メールに記載されているURLをクリックして、メールアドレス認証を行います。

![](https://storage.googleapis.com/zenn-user-upload/7ff69b12b727-20240121.png)

ここまで完了すれば、アカウント登録は完了です。

## 2. 2要素認証の設定

セキュリティのために2要素認証を設定しておきます。 

マイプロフィール画面（メールアドレスを確認した画面）の左のサイドバーをみます。
「マイプロフィール」が一番上にあり、その下にある「認証」をクリックします。

![](https://storage.googleapis.com/zenn-user-upload/4b72afc57a56-20240121.png)

遷移した画面で「2要素認証」が「無効」と表示されていることを確認し、「設定」をクリックします。

![](https://storage.googleapis.com/zenn-user-upload/e1998dabe78d-20240121.png)

遷移した画面で「セキュリティ キー認証」と「モバイル アプリの認証」のどちらかを選択し、2要素認証の設定をしていきます。
今回は「モバイル アプリの認証」を選択し、設定していきます。
「追加」をクリックします。

![](https://storage.googleapis.com/zenn-user-upload/61fce5a99339-20240121.png)

下図のような画面に遷移していれば問題ありません。
画面の指示に従って設定していきます。

![](https://storage.googleapis.com/zenn-user-upload/fa88fbc7fc32-20240121.png)

「モバイル端末に認証アプリ」については、筆者は「Google Authenticator」を使っています。
「Google Authenticator」アプリを自身のスマホでダウンロードして使用します。

iPhone
https://apps.apple.com/jp/app/google-authenticator/id388497605

Android
https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2&hl=ja&gl=US

次に遷移した画面で2要素認証の復旧コードをダウンロードします。
アカウントログインに使う、パスワードを入力します。

![](https://storage.googleapis.com/zenn-user-upload/ac3aef606fac-20240121.png)

パスワードを入力すると、下図のように復旧コードが表示されるので、ダウンロードするなどして、保管しておきます。
保管が完了したら、「次へ」をクリックします。

![](https://storage.googleapis.com/zenn-user-upload/5f4296f386e8-20240121.png)

下図のような画面に遷移し「次へ」をクリックします。

![](https://storage.googleapis.com/zenn-user-upload/733ef87df0da-20240121.png)

認証についての画面に戻ってきていること && 「2要素認証」の部分が「有効」になっていれば、2要素認証の設定は完了です！

![](https://storage.googleapis.com/zenn-user-upload/818e79d749d3-20240121.png)
