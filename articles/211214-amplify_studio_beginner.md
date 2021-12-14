---
title: "話題のAmplify Studioを触ってみた&感想"
emoji: "🐼"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["AWS", "Amplify", "amplifystudio", "Tech"]
published: true
---

この記事は[CAMPFIRE Advent Calendar 2021](https://qiita.com/advent-calendar/2021/campfire)の14日目の記事です。

今回はAWS re:Inventをみてて、個人的に「おぉぉぉぉぉ！！！！！」と思ったサービス、AWS Amplify Studioを触ったのでそのログも兼ねて書きました。

# AWS Amplify Studioについて
AWS Amplify StudioはAWS Amplifyの特性を活かしつつ、ローコードで爆速なUI開発ができるGUI開発環境のことです。
最大の特徴としてデザインツールの「Figma」と接続、Figmaで作成したインターフェースをReactのUIコンポーネントに変換できます。

一つ気になるであろうAmplify Admin UIとの関係性ですが、
**Amplify Studioの中にAmplify Admin UIも統合されている**というのが適切らしいです。

# とりあえず、一つプロジェクトを立ててみる
1. Amplifyのトップページの下の方に下図のようなコンテンツがあるので、「使用を開始する」をクリックし、プロジェクト名など入力して、数分待つ
  ![](https://storage.googleapis.com/zenn-user-upload/7d8ba11b68bc-20211214.png)

2. 作成されたプロジェクトのBackend Enviromentに遷移すると「Studioを起動する」とあるので、クリックするとAmplify Studio用のページに遷移する
  ![](https://storage.googleapis.com/zenn-user-upload/41c856e813d6-20211214.png)

サイドバーにもAdmin UIからあるデータやGraphQL APIのコンテンツがありますね。

## データモデルの作成
データはAmplify Admin UIからの引き継ぎだから詳細は省きます。
重要なのは ``UIコンポーネントとデータをバインドするために、Amplify上でデータのモデルを作成する``ということ
今回は下図のように作成しました。
Enumを設定もできるし、神か？と思いました。

![](https://storage.googleapis.com/zenn-user-upload/2364790ca3d4-20211214.png)

## FigmaからのUIコンポーネント読み込み
ざっくりした流れは
1. 「Get started」からスタート
2. ②のテキストボックスにFigmaのURLを入力
3. 「Continue」をクリックして、URL先でデザインされたコンポーネントを読み込む

という感じです。

![](https://storage.googleapis.com/zenn-user-upload/e09cc3a4d0a9-20211214.png)

もうちょっと詳細を詰めると

- ①からAmplify側で用意してくれてるテンプレートをDepulicateできる（下図参照）
  ![](https://storage.googleapis.com/zenn-user-upload/47f7b69fd9df-20211214.png)

- もし自分で作ったデザインを使用する場合、取り込みたいものは「Create Component」でコンポーネント化の必要があります
![](https://storage.googleapis.com/zenn-user-upload/75bb1287b365-20211214.png)

この記事では自分が過去に作ったデザインを使ってみました。
（これいつ作ったものかを思い出すのに5分かかった）
![](https://storage.googleapis.com/zenn-user-upload/a55e2470211a-20211214.png)

※ 入力するURLはファイル単位ではなく、Figmaで作成したコンポーネントやコンポーネントを含むフレームなどでもできるようです

## 読み込んだUIコンポーネントにデータを紐付ける
前述で作成下データモデルのデプロイが完了していたら、次はデータの紐付けです。

1. 前述で作成したコンポーネントを選択して、「Configure」をクリック
  ![](https://storage.googleapis.com/zenn-user-upload/252522da4f37-20211214.png)
2. Element TreeにFigmaで作成したときの要素が表示されていて、データの紐付けをしたい要素をクリック、プロパティを設定

  設定する項目の内容は下記を参考
  - Component properties
    - Name → 任意のプロパティ名
    - Type → 紐付けたいデータモデル
  - Child properties
    - Prop → 値を代入したいHTML属性
    - Value → 紐付けたいデータモデルのカラム

![](https://storage.googleapis.com/zenn-user-upload/779acb987a52-20211214.png)

右上の「Create Collection」をクリックすると、作業しているコンポーネントのコレクションも作ってくれます。
今回だとカードだが、カード同士の感覚や縦/横ならびの選択、1行/列に配置する数なども調整ができた。
![](https://storage.googleapis.com/zenn-user-upload/30daa76b6ad7-20211214.png)

今回は表示が間に合わなかったので割愛します。


## 作成したUIコンポーネントをReact UIコンポーネントとして取り込む
では、実際にReactのプロジェクトを立ち上げて、作成したUIコンポーネントを実装してみる！

1. Reactプロジェクト作成
ターミナルにてReactプロジェクトを事前に作成しておきます。

``` shell
$ npx create-react-app .
$ npm install aws-amplify @aws-amplify/ui-react
```

2. amplifyからソースを取得
前述で作成したプロジェクトにAmplify CLIをインストール、CLIを使って、これまででセッティングしたコンポーネントをReactプロジェクトに取り込みます。

``` shell
# Amplify CLI
$ npm install -g @aws-amplify/cli
# Amplify Studioページ右上の「Local setup instrucitons」から引っ張ってくる
$ amplify pull --appId ***** --envName staging
```

  確認ダイアログが出るので「Yes」をクリック→ターミナルに戻ります

3. コンソールでセッテイング
色々聞かれますが、今回はエンターを押し続けるでやりました。
今回はyarnでの起動を選択しています。

``` shell
$ amplify pull --appId *** --envName staging
Opening link: https://****.amplifyapp.com/admin/****/staging/verify/
✔ Successfully received Amplify Studio tokens.
Amplify AppID found: ***. Amplify App name is: ****
Backend environment staging found in Amplify Console app: ***
? Choose your default editor: Visual Studio Code
? Choose the type of app that you're building javascript
Please tell us about your project
? What javascript framework are you using react
? Source Directory Path:  src
? Distribution Directory Path: build
? Build Command:  yarn build
? Start Command: yarn start
✔ Synced UI components.
GraphQL schema compiled successfully.
...
Successfully pulled backend environment staging from the cloud.
Run 'amplify pull' to sync future upstream changes.
```

4. 作成したUIコンポーネントが`ui-components`配下にあることを確認
コンポーネントが下図のようにとりこまれます。
ここでは書きませんでしたが、`src/models`には前述でセットしたモデルのスキーマファイルなどが入ってます。

![](https://storage.googleapis.com/zenn-user-upload/8c5c70aa7cae-20211214.png)


## 作成したコンポーネントをローカル環境で表示する
`App.js`にて、表示するデータをpropsで入れて表示してみます。

※`image_url`は実験的に入れていて、データ紐付けのときに割り当ててないので、表示には使われてないです。

``` js
import './App.css';
import { Poll } from "./ui-components";

function App() {
  const item = {
    title: "もちつき",
    group_name: "家族",
    image_url: "test"
  }
  return (
    <div className="App">
      <Poll PollCard={item} />
    </div>
  );
}

export default App;
```

ちゃんと入力したデータが出てました（拍手）
ただ、このテキスト位置のズレは何なんだ....とおもったら余白設定がかなり甘々になってる...？
ここの修正がFigmaかUI側かは試し中です。

![](https://storage.googleapis.com/zenn-user-upload/42bf39cecb0b-20211214.png)


あとはPollのCollection調整をカチャカチャ...(データが入らず間に合わなかった)
Graph QLをカチャカチャ...
contentをいれてカチャカチャ...
file storageを設定して（そのまえにAuthenticationを設定して）カチャカチャ...

を進行中
また後日お見せできたらと思います！！！

# 使ってみての感想
私が今までで触ってみての感想になります。

### プラスな点
- 噂に違わず、爆速な開発ができる
  - レイアウト調整の時間が大幅に短縮できる（CSS苦手）
  - 今後ハッカソンなどで多用できる
  - モックを手早く作りたいなどのシチュエーションだと最適解と思った
- フロントエンドはデータバインドとコンポーネントの組み立てに専念できる
- Figmaからのコンポーネントとして持ってこれるは今までになかった気がして、面白い！
  - 個人的な感想であるが、AWSがここまでデザイナー寄りのシステム出したのは初めてな気がする
  - デザイナーができる範囲が広がり、エンジニアとの認識合わせなどが容易になると思う


### うーんどうしたものかと思ってる点
- Studioで生成したコードに対してロジック追加ができないらしい
  - 保守を想定するとちょっと使いづらいのかもといった印象
- Webフォントを指定できない
- Figmaで作成したコンポーネントにWarningがあると、UIをただしくはいてくれない事がある？
  - 前述のWebフォントと少しかぶるが、Studioでの作成時点でFigmaで使用がストップしてるWebフォントを使っているとpropsで値を入れてもテキストを表示してくれなかった（そもそも今回使ったデザインの作成時期がかなり前なのもあるかも）
    - まだ調査中なので分かり次第、editします
- テキストエリアの大きさをよしなにできない
  - 今回だとタイトルの部分が4文字以上になると改行されてしまうため、多分widthか左右の余白をを固定されてるっぽい？
  - Reactにはき直した際にそのへんの調整がまだ必要？
   - もし解決方法知ってる人いたら教えて下さい

Figmaできちんと作らないとコンポーネントが正しくはかれないというのはありそう....
いかんせん、Admin UIを詳しく触らずきてる部分があるので、そこからもうちょっと見直したいと思います。

# 参考記事
- https://aws.amazon.com/jp/blogs/news/aws-amplify-studio-figma-to-fullstack-react-app-with-minimal-programming/
- https://dev.classmethod.jp/articles/new-aws-amplify-studio/
- https://zenn.dev/qnaiv/articles/4d5a5526aa403d
- https://zenn.dev/yoshii0110/articles/9f900b11203834qii

