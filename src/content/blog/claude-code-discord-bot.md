---
title: 'Claude Code でデスクトップを操作する Discord ボットを作ってみた'
description: 'Mac の Mail.app やブラウザを音声・テキストから操作したくて、OpenClaw 経由で Claude Code に Discord ボットの役を任せたら、思っていた 3 倍は便利だった話。'
pubDate: 'May 08 2026'
---

最近、Discord 越しに Mac を操作するボットを書いた。といっても自分が書いたコードは大したことなくて、本体は OpenClaw 経由で動く Claude Code (Opus 4.7) に任せている。

## 動機

ブラウザで Gmail を開く、デスクトップのファイルを ZIP にしてメールで送る、おみくじアプリを実装してテストまで通す ── こういう「日常の小さな自動化」を、ターミナルを開かずに Discord でメンションして済ませたかった。

## 仕組み

> 「Discord メッセージ → MCP プラグイン → Claude Code → AppleScript / Bash / ブラウザ操作」というだけのシンプルな構成。

Mac の Mail.app は AppleScript から自由に叩けるので、Gmail API キーを発行しなくても下書き作成と送信ができる。下のスニペットは実際に下書きを作るコード:

```applescript
tell application "Mail"
    set newMsg to make new outgoing message with properties ¬
        {subject:"テスト", content:"...", sender:"tsu.310@gmail.com"}
    tell newMsg
        make new to recipient at end of to recipients ¬
            with properties {address:"tsu.310@gmail.com"}
        save
    end tell
end tell
```

## 気づき

LLM に「やって」と頼むときの粒度が、人間に依頼するときと近い。「下書きに保存」「送信」「削除」という名詞の使い分けを Claude が拾ってくれるので、コマンドのドキュメントを覚えなくていい。
