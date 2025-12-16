# CLAUDE.md

このファイルは、Claude Code (claude.ai/code) がこのリポジトリのコードを扱う際のガイダンスを提供します。

## リポジトリ概要

3-SHAKE Inc. 向けのMarpベースのプレゼンテーションテンプレートリポジトリです。Markdownを使用して一貫性のあるプロフェッショナルなプレゼンテーションを作成するためのブランドテンプレート、テーマ、ツールを提供します。

## よく使うコマンド

### ビルドコマンド

HTML出力はプレビュー用、PDF出力は配布用として使い分けます。`--no-stdin` を付けないとstdin待ちでハングすることがあるため、必ず付与してください。

```bash
# HTML出力（プレビュー用）
npx @marp-team/marp-cli@latest slides/[file].md --html --allow-local-files -o slides/[file].html --no-stdin

# PDF出力（配布用）
npx @marp-team/marp-cli@latest slides/[file].md --pdf --allow-local-files -o slides/[file].pdf --no-stdin

# HTMLとPDFを連続生成
npx @marp-team/marp-cli@latest slides/[file].md --html --allow-local-files -o slides/[file].html --no-stdin && \
npx @marp-team/marp-cli@latest slides/[file].md --pdf --allow-local-files -o slides/[file].pdf --no-stdin

# ライブプレビュー（編集しながら確認）
npx @marp-team/marp-cli@latest slides/[file].md --html --allow-local-files --watch
```

### 開発サーバー

`npm start` でライブリロード付きのローカルサーバーがポート8080で起動します。`npm run preview` はプレビューモードです。

## ディレクトリ構成

```
templates/       # ベーステンプレート（コピーして使用）
themes/          # CSSテーマファイル
slides/          # プレゼンテーション（年別に整理）
  └── 2025/
assets/images/   # 画像（年別・プレゼンテーション別に整理）
  └── 2025/
      └── [presentation-name]/
.claude/
  ├── commands/  # スラッシュコマンド
  ├── agents/    # サブエージェント
  └── rules/     # スライド作成ルール
```

## Claude Code 連携機能

| コマンド | 用途 |
|---------|------|
| `/review-slide` | 総合レビュー |
| `/check-structure` | 構成チェック |
| `/fact-check` | ファクトチェック |

サブエージェントも用意しています。まずは `/review-slide` で全体を見てから、必要に応じて使い分けるのがおすすめです。

| エージェント | 専門分野 |
|-------------|----------|
| slide-reviewer | スライド構成、視覚的デザイン |
| audience-simulator | 聴衆心理、3視点分析 |
| fact-checker | 情報検証、出典確認 |
