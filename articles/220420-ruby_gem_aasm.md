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

### Callbacks

### Guard

### Binding event

### Auto-generated Status Constants

### Bang events

### Timestamps

### Transaction support

### 

### ライフサイクル


# AASMの挙動例


# まとめ



# 参考資料
- https://github.com/aasm/aasm
- https://chietech.com/2019/02/02/rails-aasm/