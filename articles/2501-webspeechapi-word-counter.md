---
title: "WebSeechAPIを使って文字数カウントしてみる"
emoji: "🔊"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["WebSpeechAPI", "JavaScript", "音声認識"]
published: true
---

どうも、あべたく（[@east-takumi](https://x.com/east_takumi)）です。

# 概要
今回はWebSpeechAPIを使って、音声をテキストに変換し、変換したテキストの文字数をカウントするという内容のコードを書いてみたので、その内容をさっくりまとめてみます。

## WebSpeechAPIとは？
WebSpeechAPIはW3C（World Wide Web Consortium）によって策定された　主に下記の2つをブラウザ上で音声データを取り扱うAPIになります。
- 音声認識（Speech Recognition）: 音声→テキストに変換
- 音声合成（Speech Synthesis）: テキスト→音声に読み上げ

https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API

Web標準APIとして提供されるため、サービスへのAPIコールは必要なく、JavaScriptから簡単に動かすことができます。

# 早速やってみる
今回は画面から録音スタートなども操作したかったので、Reactで書いて見ました。
下記は実際のコードで、重要そうなところを下記で解説します。

:::details 実際のコード
```ts
import React, { useState, useEffect, useRef } from 'react';

const SpeechMonitor = () => {
    const [isListening, setIsListening] = useState(false);
    const [error, setError] = useState('');
    const [transcript, setTranscript] = useState('');
    const [isRecordingComplete, setIsRecordingComplete] = useState(false);
    const [speechData, setSpeechData] = useState({
        totalSpeechLength: 0,
        speechDuration: 0,
        isOverTalkative: false
    });

    const recognitionRef = useRef(null);
    const startTimeRef = useRef(null);
    const durationIntervalRef = useRef(null);

    const RECORDING_DURATION = 20;
    const CHAR_LIMIT = Math.floor((300 / 60) * RECORDING_DURATION);

    useEffect(() => {
        const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;

        if (SpeechRecognition) {
            try {
                // オブジェクトの生成
                recognitionRef.current = new SpeechRecognition();
                const recognition = recognitionRef.current;

                // プロパティの設定
                recognition.continuous = true;
                recognition.interimResults = true;
                recognition.lang = 'ja-JP';

                // 音声入力開始時の処理
                recognition.onstart = () => {
                    setIsListening(true);
                    setError('');
                    setIsRecordingComplete(false);
                };

                // 音声入力時にエラーが発生した際の処理
                recognition.onerror = (event) => {
                    setError(`音声認識エラー: ${event.error}`);
                    setIsListening(false);
                    clearInterval(durationIntervalRef.current);
                };

                // 音声入力終了時の処理
                recognition.onend = () => {
                    setIsListening(false);
                    setIsRecordingComplete(true);
                    clearInterval(durationIntervalRef.current);
                };

                // 音声入力の結果が正しい値として認識&&そのデータを取得
                recognition.onresult = (event) => {
                    const currentTranscript = Array.from(event.results)
                      .map(result => result[0].transcript)
                      .join('');

                    setTranscript(currentTranscript);

                    const charCount = currentTranscript.length;
                    const duration = Math.floor((Date.now() - startTimeRef.current) / 1000);

                    setSpeechData({
                        totalSpeechLength: charCount,
                        speechDuration: duration,
                        isOverTalkative: charCount > CHAR_LIMIT
                    });
                };
            } catch (err) {
                setError(`初期化エラー: ${err.message}`);
            }
        } else {
            setError('このブラウザは音声認識APIに対応していません');
        }

        return () => {
            if (recognitionRef.current) {
                recognitionRef.current.stop();
            }
            if (durationIntervalRef.current) {
                clearInterval(durationIntervalRef.current);
            }
        };
    }, []);

    const startListening = () => {
        try {
            setTranscript('');
            setSpeechData({
                totalSpeechLength: 0,
                speechDuration: 0,
                isOverTalkative: false
            });

            if (recognitionRef.current) {
                startTimeRef.current = Date.now();
                recognitionRef.current.start();

                durationIntervalRef.current = setInterval(() => {
                    const duration = Math.floor((Date.now() - startTimeRef.current) / 1000);
                    setSpeechData(prev => ({
                        ...prev,
                        speechDuration: duration,
                    }));

                    if (duration >= RECORDING_DURATION) {
                        stopListening();
                    }
                }, 1000);
            }
        } catch (err) {
            setError(`開始エラー: ${err.message}`);
        }
    };

    const stopListening = () => {
        try {
            if (recognitionRef.current) {
                recognitionRef.current.stop();
                clearInterval(durationIntervalRef.current);
            }
        } catch (err) {
            setError(`停止エラー: ${err.message}`);
        }
    };

    const formatDuration = (seconds) => {
        const mins = Math.floor(seconds / 60);
        const secs = seconds % 60;
        return `${mins}:${secs.toString().padStart(2, '0')}`;
    };

    return (
      <div className="p-4 bg-gray-100">
          <h1 className="text-2xl mb-4">話し過ぎ検出アプリ</h1>

          {error && (
            <div className="bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded mb-4">
                {error}
            </div>
          )}

          <div className="mb-4">
              <button
                onClick={startListening}
                disabled={isListening}
                className="bg-blue-500 text-white p-2 mr-2 rounded disabled:bg-gray-400"
              >
                  録音開始
              </button>
              <button
                onClick={stopListening}
                disabled={!isListening}
                className="bg-red-500 text-white p-2 rounded disabled:bg-gray-400"
              >
                  録音停止
              </button>
          </div>

          <div className="mt-4 p-4 bg-white rounded shadow">
              <div className="text-xl font-bold mb-2">
                  経過時間: {formatDuration(speechData.speechDuration)} / {formatDuration(RECORDING_DURATION)}
              </div>
              <div className="text-xl font-bold mb-2">
                  文字数: {speechData.totalSpeechLength}文字 / {CHAR_LIMIT}文字
              </div>

              <h2 className="font-bold mb-2">文字起こし:</h2>
              <div className="p-2 bg-gray-50 rounded min-h-[100px] mb-4">
                  {transcript || '音声を入力してください...'}
              </div>

              {isRecordingComplete && speechData.isOverTalkative && (
                <div className="text-red-500 mt-2 font-bold">
                    文字数が多すぎます！もう少しゆっくり話してください。
                </div>
              )}
          </div>
      </div>
    );
};

export default SpeechMonitor;
```
:::

## 文字数カウントする部分について解説
まず、下記のプロパティを設定しておく。
このプロパティを設定することで、音声認識を連続させることができます。
```ts
recognition.continuous = true;
```

連続とはなんじゃろな？と思った方もいると思いますので、サクッと説明します。
私たちが喋る時って言葉の間に少しの間があったりしますよね。
この間がコンピュータとしては「あっ会話終了したな」って認識しちゃうので、一定時間の沈黙があるまでは音声認識を継続させるみたいなイメージです。

上記のプロパティ設定をした上で下記のコードで音声入力の結果を取得してきます(onresult)。

```ts
recognition.onresult = (event) => {
    const currentTranscript = Array.from(event.results)
        .map(result => result[0].transcript)
        .join('');

    const charCount = currentTranscript.length;
}
```

この際、`event.results`は配列構造になっていて、上記のプロパティ設定の影響で、連続する会話を配列構造として持たせています。
例えば「今日は晴れですね。今日の予定はなんですか？」と喋っていた場合、
```
[
    "今日は晴れですね",
    "今日の予定はなんですか"
]
```
のような感じです。（本当にざっくりです）

なので、`event.results`を`.map`で取得してきて、それぞれの文字列を`.join`で結合している。
その結果の`currentTranscript`を`.length`で文字列の長さを計測しているという流れです。

# まとめ
今回はWeb Speech APIで音声認識をしてみたことについてまとめてみました。
Web標準APIとして提供されてるので、諸々インポートなどが必要なく利用できるし、ブラウザベースで簡単に試せるのはとても楽しかったです。
ただ、ブラウザによっては動かない機能があったりするようなので、そこは要確認ですね👀
https://caniuse.com/?search=Speech%20API

ちなみに、昨年末に今回の内容も含む発表をさせていただいたので、よかったご覧ください。
https://speakerdeck.com/east_takumi/241226-tui-sinikixian-warenaitameniwatasitatihananiwosubekika-vol-dot-0

## 参考資料
- https://zenn.dev/micronn/articles/b654ceca1bdf13