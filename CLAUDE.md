# CLAUDE.md

3-SHAKE Inc. 向けMarpプレゼンテーションテンプレート。

## ディレクトリ構成

```
templates/       # ベーステンプレート
themes/          # CSSテーマ
slides/{year}/   # プレゼンテーション
assets/images/   # 画像
examples/        # サンプルスライド
```

## ビルド・プレビュー

`/build <ファイル名>` を使用。ビルド後に毎回 `open` しない。

## レビューコマンド

| コマンド | 用途 |
|---------|------|
| `/review-slide` | 総合レビュー |
| `/check-structure` | 構成チェック |
| `/fact-check` | ファクトチェック |
| `/depth-check` | 主張の深さチェック |
| `/logical-flow-check` | 論理の流れチェック |
| `/narrative-empathy-review` | 物語・共感設計 |
| `/redundancy-check` | 冗長性チェック |

## 自動適用ルール

- `slides/**/*.md` 編集時 → `rules/slide-writing.md` が自動適用
- レビュー順序 → `rules/review-workflow.md` を参照

## テーマ

- 2026年スライドは `theme: 3shake-2026-presentation` を使用（フロントマターにテーマ名を指定、パス不要）
- ビルド: `npx @marp-team/marp-cli <file>.md --theme themes/3shake-2026-presentation.css -o <file>.html`

## 学び

### Marp CSS スコーピングの罠

Marpテーマ内のCSSセレクタは `div#... > svg > foreignObject > section` でスコープされる。

- `.title h1` → `section.title h1` に変換される（h1がsection直下にある場合のみマッチ）
- 実際のDOM: `section.title > div.title > h1`（divを挟む）
- **正解**: `section .title h1` → `section .title :is(h1, marp-h1)` に変換され、子孫セレクタとして正しくマッチ

インラインCSS（`style:` ブロック）はスコープされないため、テーマCSSに移行する際にこの差異に注意。

### 書籍画像の引用

- キャプションは書籍の正式なFigureタイトルと一致させる（自分で要約しない）
- 画像の命名: `figure-[章]-[番号]-[説明].png`（例: `figure-1-10-modernization-overview.png`）
- `assets/images/2026/architecture-modernization/fig[章]-[番号].png` も同書籍の図（別解像度）
