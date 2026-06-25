# CLAUDE.md

3-SHAKE Inc. 向けMarpプレゼンテーションテンプレート。

このファイルで定義した運用は `AGENTS.md` からも参照され、Codex でも同じルールとして扱う。

## ディレクトリ構成

```
templates/       # ベーステンプレート
themes/          # CSSテーマ
assets/images/   # テンプレート共通画像
examples/        # サンプルスライド
```

講演資料、講演固有画像、公開PDFは `nwiizo/slides` で管理する。このリポジトリは制作物を持たない。

## ビルド・プレビュー

`/build <ファイル名>` を使用。ビルド後に毎回 `open` しない。

## レビューコマンド

| コマンド | 用途 |
|---------|------|
| `/review-slide` | 総合レビュー |
| `/check-structure` | 構成チェック |
| `/fact-check` | ファクトチェック |

## 自動適用ルール

- 利用先の `slides/**/*.md` 編集時 → `.claude/rules/slide-writing.md`（記法・構成・分割基準・引用）
- `themes/*.css` / `.marprc.yml` 編集時 → `.claude/rules/marp-theme-build.md`（テーマ登録・ビルド・PDF・CSSスコーピング）
- レビュー順序 → `.claude/rules/review-workflow.md`

## 利用先との契約

- 利用先はテーマと共通画像を `brands/3shake/` のようなブランドパックとして独立所有し、Marpの実行時パスを自己完結させる。
- 利用先のスライドは、CSSファイルパスではなく登録済みテーマ名を使う。
- submodule内のパスをスライド本文へ直接埋め込まない。
- `themes/` または共通画像を変更したら、`nwiizo/slides` 側が必要な差分だけをブランドパックへ取り込み、2025年・2026年の代表資料で検証する。
- HTMLは検証用、PDFは利用先で公開する成果物とする。
- 標準スターターは `templates/3shake-presentation.md` の1つに絞り、追加パターンは `examples/` へ置く。
- すべてのMarkdownはCSSパスではなく `.marprc.yml` に登録されたテーマ名を使う。
- 構造とMarp CLIで解決できる処理に補助スクリプトを追加しない。専用ツールが継続的に必要な場合はRust CLIとして実装する。
