# ts's notebook

[tsu310.com](https://tsu310.com) で公開中の個人ブログ。[Astro](https://astro.build) 製、Cloudflare Pages でホスティング。

## 開発

```sh
npm install
npm run dev     # http://localhost:4321
npm run build   # ./dist に静的ファイル生成
```

## 記事を書く

`src/content/blog/*.md` に Markdown ファイルを追加するだけ。

```markdown
---
title: '記事タイトル'
description: 'メタディスクリプション'
pubDate: 'May 08 2026'
---

本文...
```

## デプロイ

main ブランチに push → Cloudflare Pages が自動ビルド＆デプロイ。

## Credit

Astro の [Bear Blog template](https://github.com/HermanMartinus/bearblog/) をベースに作成。
