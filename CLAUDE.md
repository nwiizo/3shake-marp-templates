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

## 自動適用ルール

- `slides/**/*.md` 編集時 → `rules/slide-writing.md`（記法・構成・分割基準・引用）
- `themes/*.css` / `.marprc.yml` 編集時 → `rules/marp-theme-build.md`（テーマ登録・ビルド・PDF・CSSスコーピング）
- レビュー順序 → `rules/review-workflow.md`
