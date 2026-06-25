# スライドリポジトリ分離設計

## 目的

公開講演資料を `nwiizo/slides` で管理し、`nwiizo/3shake-marp-templates` は再利用可能なMarpテンプレートへ特化する。転職などでブランドが変わっても、過去資料の見た目とPDFを再現できる構造にする。

## リポジトリ境界

### `nwiizo/3shake-marp-templates`

テンプレートとして再利用するものだけを所有する。

```text
templates/       # 唯一の標準スターター
themes/          # Marpテーマ
assets/images/   # テンプレート共通のブランド画像
examples/        # 目的別の短い例
.claude/rules/   # 執筆・ビルド・レビュー規約
.claude/skills/  # 基本ワークフロー
```

- 講演Markdown、年度別画像、公開PDFを持たない。
- 標準スターターは `templates/3shake-presentation.md` に一本化する。
- MarkdownはCSSパスではなく、`.marprc.yml` に登録されたテーマ名を使う。
- GitHubのTemplate repositoryとして公開する。

### `nwiizo/slides`

公開講演を再現するために必要なものを所有する。

```text
slides/{year}/                 # Marp Markdownと公開PDF
assets/images/{year}/          # 講演固有画像
assets/shared/                 # 発表者共通画像
brands/3shake/themes/          # 過去資料用テーマの固定実体
brands/3shake/assets/images/   # 過去資料用ブランド画像の固定実体
.claude/skills/                # 公開レビュースキル
vendor/3shake-marp-templates/  # 上流参照用submodule
```

- HTMLはローカル検証物として追跡しない。
- PDFは公開物としてMarkdownと同じディレクトリで追跡する。
- スライド本文は `vendor/` を参照しない。
- `brands/3shake/` は上流のキャッシュではなく、過去資料を再現する固定資産としてslides側が所有する。

## 親子関係

`nwiizo/slides` は `vendor/3shake-marp-templates` にGit submoduleを置き、利用した上流revisionを明示する。ただしsubmoduleはMarpの実行時依存にしない。

上流更新は自動同期しない。差分を確認し、必要なテーマ・画像だけをブランドパックへ選択的に取り込む。これにより、テンプレート更新が公開済みPDFの再現性を暗黙に壊すことを防ぐ。

将来のブランドは `brands/{brand}/` に追加し、`brands/3shake/` を上書きしない。

## Marpの参照規約

- 2026年資料: `theme: 3shake-2026-presentation`
- 2025年互換資料: `theme: 3shake-theme`
- `.marprc.yml`: `themeSet: brands/3shake/themes/`
- 3-SHAKE画像: `../../brands/3shake/assets/images/...`
- 発表者共通画像: `../../assets/shared/...`
- 講演固有画像: `../../assets/images/{year}/...`
- ビルド: リポジトリ直下から `--allow-local-files --no-stdin` 付きで実行

## 公開レビュースキル

個人講演の内容に依存する高度なレビューはslides側へ置く。

- `review-slide-flow`
- `deepen-slide-claims`
- `review-slide-narrative`
- `trim-slide-redundancy`
- `review-slide-wit`
- `review-slide-suite`
- `prepare-slide-release`

スキルは架空ペルソナや根拠のない平均スコアに頼らず、証拠、聴衆への影響、最小修正、コスト、保持すべき強みを示す。

## 補助ツールの原則

構造、安定したパス、Marp CLI、VCS差分で解決する。自動同期や包括チェックスクリプトは置かない。

専用ツールを継続的に保守する必要が生じた場合だけRust CLIとして設計する。Marp CLIのような公式ツールは公式実装を利用する。

## 移行手順

1. Git履歴から `slides/` と年度別画像の履歴を抽出する。
2. 未コミットのMarkdown、画像、PDFを新しいローカルslidesリポジトリへ重ねる。
3. ブランド画像、テーマ、発表者画像を所有境界へ配置し、参照パスを変更する。
4. 高度なレビュースキルを公開名で再設計してslides側へ移す。
5. テンプレート側から講演資料、年度別画像、個人講演由来の巨大例、重複テンプレートを除く。
6. テンプレート側を検証・公開し、そのrevisionをslides側submoduleに固定する。
7. 2025年・2026年・新規資料をHTMLへビルドし、公開対象はPDFも再生成する。
8. `nwiizo/slides` をPublicリポジトリとして作成・pushする。
9. clean cloneで、未初期化のsubmoduleに依存せず代表資料を再ビルドする。

## 既知のlegacy資料

次の資料は、元リポジトリに参照画像が一度もコミットされていない。

- `slides/2025/career-change-to-aws-mcp-server.md`
- `slides/2025/getting-started-rust-observability.md`

履歴とMarkdownは保持するが、元画像の回収または自作図への置換が終わるまで新しい公開PDFを生成しない。欠落を別画像で黙って置き換えない。

## 完了条件

- テンプレート側に講演Markdown、年度画像、公開PDF、重複スターターがない。
- slides側に24件のMarkdown、年度画像、ブランドパック、発表者画像、既存公開PDFがある。
- 3-SHAKE画像とCSSが `vendor/` なしで解決する。
- 2025年・2026年・新規資料の代表HTMLがビルドできる。
- 公開対象PDFがREADMEから参照できる。
- 7スキルが構造検証と独立実行評価を通る。
- 両リポジトリのAGENTS、CLAUDE、READMEが同じ所有境界を説明する。
- Publicな `nwiizo/slides` とTemplate repository設定済みの `nwiizo/3shake-marp-templates` がGitHubに存在する。
