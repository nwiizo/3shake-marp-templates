---
paths: themes/*.css, .marprc.yml
---

# Marp テーマ・ビルド規約

## テーマ登録の仕組み（themeSet）

- Marp CLI v4 のフロントマター `theme:` はテーマ名のみ受け付ける（ファイルパス不可）
- テーマCSSに `/* @theme 3shake-2026-presentation */` ディレクティブが必要
- `.marprc.yml` の `themeSet: themes/` でテーマディレクトリを登録し、名前でマッチさせる
- `@theme` ディレクティブを削除するとテーマ登録が壊れるので **削除禁止**
- nvim.marp も `.marprc.yml` を読むため、CLI と nvim.marp の出力は一致する

## ビルド手順

- 2026年スライドはフロントマターに `theme: 3shake-2026-presentation` を必ず記載する（CSSパス指定は不要）
- `.marprc.yml` に `themeSet: themes/` が必須。これがないとテーマCSSが読み込まれない
- ビルドはリポジトリ直下で `npx marp slides/2026/<file>.md --html --allow-local-files -o slides/2026/<file>.html`
- PDF出力: 上記に `--pdf` を追加して `-o slides/2026/<file>.pdf`
- `--allow-local-files` を付けないと表紙画像など相対パスのアセットが読み込めず崩れるので必須
- `slides/` 配下で実行する場合は `--theme ../themes/3shake-2026-presentation.css` のように正しい相対パスを渡す

## PDF出力とタイトルスライドの注意

- PDF出力はChromium/Puppeteerを使うため、HTML表示と微妙に異なる
- タイトルスライドの `margin-left` は `80px` が適切（`20px` だと左に寄りすぎる）
- テーマCSSで `section.title` に `justify-content: center` + `overflow: visible` を設定
- `section.title::before`（ロゴ）は `display: none` で非表示にする
- `section .title h1` の `font-size` は `1.6em`（2.4em以上だとPDFで文字がはみ出す）

## Marp CSS スコーピングの罠

Marpテーマ内のCSSセレクタは `section` でスコープされる。

- `.title h1` → `section.title h1` に変換（section直下のh1のみマッチ）
- 実際のDOM: `section.title > div.title > h1`（divを挟む）
- **正解**: `section .title h1` と書けば子孫セレクタとして正しくマッチ
- インラインCSS（`style:` ブロック）はスコープされないため、テーマCSSへ移行時に注意

## Flexboxレイアウトの高さ揃え

- `display: flex` 内の `background-color` 付きdivは文字数で高さが変わる
- テーマCSSに以下を追加して解決:
  ```css
  section div[style*="display: flex"] > div[style*="background-color"] {
    align-self: stretch !important;
  }
  ```
