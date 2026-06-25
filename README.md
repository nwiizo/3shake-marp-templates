# 3shake-marp-templates

3-SHAKEの講演資料をMarpで作るための公開テンプレートです。テーマ、ブランド画像、スターター、例、執筆・レビュー規約を一つにまとめています。

実際の講演Markdown、講演固有画像、公開PDFは [`nwiizo/slides`](https://github.com/nwiizo/slides) で管理します。このリポジトリは制作物を持たず、再利用可能なテンプレートだけを保ちます。

## Quick start

GitHubの **Use this template** から新しいリポジトリを作るか、このリポジトリをcloneします。

```sh
git clone https://github.com/nwiizo/3shake-marp-templates.git my-presentation
cd my-presentation
npm install
mkdir -p slides/2026
cp templates/3shake-presentation.md slides/2026/my-presentation.md
```

HTMLを生成します。

```sh
npx marp slides/2026/my-presentation.md \
  --html --allow-local-files \
  -o slides/2026/my-presentation.html
```

PDFを生成します。

```sh
npx marp slides/2026/my-presentation.md \
  --pdf --allow-local-files \
  -o slides/2026/my-presentation.pdf
```

ローカル画像を使うため、ビルドでは常に `--allow-local-files` を付けます。ビルド後に自動でファイルを開く処理は行いません。

## Contents

```text
templates/       # コピーして使うスターター
themes/          # MarpテーマCSS
assets/images/   # テンプレート共通のブランド画像
examples/        # 目的別の短い記法例
.claude/rules/   # 執筆・テーマ・レビュールール
.claude/skills/  # ビルドと基本レビューのワークフロー
```

## Starter

[`templates/3shake-presentation.md`](templates/3shake-presentation.md) が唯一の標準スターターです。タイトル、問題提起、判断軸、具体例、トランジション、まとめ、終了の最小構成を含みます。

テンプレートを増やす代わりに、再利用できる表現パターンは `examples/` へ追加します。これにより、どのスターターを選ぶべきか迷わず、共通部分を一箇所で保守できます。

## Themes

| テーマ名 | 用途 |
|---|---|
| `3shake-2026-presentation` | 2026年以降の標準プレゼンテーション |
| `3shake-theme` | 2025年以前の資料との互換 |
| `3shake-standard-theme` | 軽量な標準スライド |
| `title-theme` | タイトル表現の例 |
| `custom` | カスタマイズ例 |

フロントマターではCSSパスではなくテーマ名を指定します。

```yaml
---
marp: true
theme: 3shake-2026-presentation
paginate: true
---
```

`.marprc.yml` の `themeSet: themes/` がテーマ名をCSSへ解決します。Marp CLIは公式に、カスタムテーマをtheme setへ登録し、Markdownからテーマ名で選択する方法を提供しています。

## Images

テンプレート共通画像は `assets/images/` に置きます。スターターを `slides/{year}/` へコピーした場合は、次のように参照します。

```markdown
![bg](../../assets/images/3shake-background-full.png)

<img src="../../assets/images/3shake-logo.png" alt="3-SHAKE logo">
```

講演固有画像はテンプレートへ追加せず、利用先リポジトリの `assets/images/{year}/{presentation}/` で管理します。

## Examples

- [`examples/3shake-standard-slides.md`](examples/3shake-standard-slides.md): 標準レイアウト
- [`examples/mermaid-examples.md`](examples/mermaid-examples.md): Mermaid図
- [`examples/sample-presentation.md`](examples/sample-presentation.md): Marp基本記法
- [`examples/title-slide.md`](examples/title-slide.md): タイトル表現

例は特定の講演内容を持たず、コピー可能な一つの目的に集中させます。

## Rules and review

- `CLAUDE.md`: リポジトリの役割と構成
- `AGENTS.md`: Codexを含むエージェント運用
- `.claude/rules/slide-writing.md`: スライド記法と構成
- `.claude/rules/marp-theme-build.md`: テーマとビルド
- `.claude/rules/review-workflow.md`: レビュー順序

基本ワークフロー:

```text
/build <file>
/review-slide <file>
/check-structure <file>
/fact-check <file>
```

Codexでは同名の入力を、`AGENTS.md` に定義した同等ワークフローとして実行します。

より深い論理、主張、物語、冗長性、機知のレビューは `nwiizo/slides` の公開スキルを使います。

## Consumer contract

`nwiizo/slides` はこのリポジトリをGit submoduleで更新元として固定し、公開済み資料のテーマとブランド画像は `brands/3shake/` で独立して所有します。講演スライドはsubmoduleを直接参照しません。

この分離には次の利点があります。

- clone直後でも、スライド側に固定されたブランドパックだけで再ビルドできる
- テンプレート更新を明示的に取り込める
- 将来別ブランドを追加しても、過去の3-SHAKE資料を壊さない

テーマまたは共通画像を変更した場合は、利用先が差分を選択してブランドパックへ取り込み、2025年・2026年の代表資料をHTMLとPDFで検証します。

## Validation

変更内容に応じて次を確認します。

```sh
# スターター
npx marp templates/3shake-presentation.md \
  --html --allow-local-files \
  -o /tmp/3shake-presentation.html

# 例
npx marp examples/3shake-standard-slides.md \
  --html --allow-local-files \
  -o /tmp/3shake-standard-slides.html
```

- テーマ名が解決されている
- ロゴと背景画像が欠落していない
- `--allow-local-files` が付いている
- テンプレートに講演固有の内容や年度画像が混入していない
