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
