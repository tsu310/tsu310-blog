---
title: 'Python でおみくじ CLI を書きながら East Asian Width に殴られた'
description: '罫線文字で枠を書こうとしたら、CJK 文字との混在で右端がガタつく。unicodedata.east_asian_width() の Ambiguous をどう解釈するかが鍵だった。'
pubDate: 'May 08 2026'
---

ターミナルで罫線を引きたい。たとえばこういう感じ:

```
╔════════╗
║ おみくじ ║
╚════════╝
```

問題は、罫線文字 (`║`, `═` ...) と日本語が混在したときに右端がズレることだ。

## 原因

`unicodedata.east_asian_width()` は文字を `F / W / H / Na / N / A` に分類する。
罫線は `A` (Ambiguous) ── つまり「環境次第で 1 にも 2 にもなる」。多くのターミナルでは半角扱いだが、それを前提にしないとレイアウトが破綻する。

## 解決

```python
def visual_width(text: str) -> int:
    width = 0
    for ch in text:
        ea = unicodedata.east_asian_width(ch)
        width += 2 if ea in ("W", "F") else 1
    return width
```

Ambiguous を 1 として扱うのが一番安全だった。CJK 設定の terminal で見たら違う結果になるが、いまの想定ユーザーには十分。

> 「美しさは右端が揃うことで生まれる」と祖母が言っていた（言ってない）。
