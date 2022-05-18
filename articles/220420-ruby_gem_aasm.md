---
title: ""
emoji: "🐼"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Ruby", "Gem", "AASM", "Tech"]
published: false
---

# AASMってなんぞ？
- Rubyで状態遷移（有限オートマトン）を簡単に実装することができるGem
- ActiveRecordモデルのみを対象としない、汎用的なライブラリ

## 定義時の構成要素
- state（状態）
- event（イベント）
  - 実行不可の場合，AASM::InvalidTransitionを発生させる
- transitions（遷移，更新内容）

``` ruby
aasm do
    state :sleeping

    event :run do
      transitions :from => :sleeping, :to => :running
    end
  end
```

## 利用場面
-
-
-

# AASMの機能
- 

### may_<***>
- readmeに一切記載がなく，筆者ははじめてみたときはまった
- いわゆるdry_runで，<***>の部分で指定したイベントの実行可否をbooleanで返してくれる

``` ruby

```

### Callbacks
- 特定の状態に遷移するなどの基準に合致する場合にメソッドやクラスが呼び出される
#### ライフサイクル


``` ruby
begin
  event           before_all_events
  event           before
  event           guards
  transition      guards
  old_state       before_exit
  old_state       exit
                  after_all_transitions
  transition      after
  new_state       before_enter
  new_state       enter
  ...update state...
  event           before_success      # if persist successful
  transition      success             # if persist successful, database update not guaranteed
  event           success             # if persist successful, database update not guaranteed
  old_state       after_exit
  new_state       after_enter
  event           after
  event           after_all_events
rescue
  event           error
  event           error_on_all_events
ensure
  event           ensure
  event           ensure_on_all_events
end
```

### Guard
- 定義した条件に合致する場合に特定の遷移を**許可**する
- 

### Binding event

### Auto-generated Status Constants

### Bang events

### Timestamps

### Transaction support


# AASMの挙動例


# まとめ



# 参考資料
- https://github.com/aasm/aasm
- https://chietech.com/2019/02/02/rails-aasm/