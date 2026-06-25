# AGENTS.md

このリポジトリでは、**Claude Code 向けに定義された運用ルールを Codex でも正として扱う**。
Codex は `CLAUDE.md` と `.claude/` 配下のルール・レビュー定義を参照し、同じ期待値で作業すること。

Codex 固有の補足は `.claude/` 配下に追記せず、この `AGENTS.md` に集約すること。

## 正式な参照元

- `CLAUDE.md`
- `.claude/rules/slide-writing.md`
- `.claude/rules/marp-theme-build.md`
- `.claude/rules/review-workflow.md`
- `.claude/skills/review-slide/SKILL.md`
- `.claude/skills/check-structure/SKILL.md`
- `.claude/skills/fact-check/SKILL.md`
- `.claude/skills/build/SKILL.md`

## 自動適用ルール

- 利用先の `slides/**/*.md` を編集する前に `.claude/rules/slide-writing.md` を読む
- `themes/*.css` または `.marprc.yml` を編集する前に `.claude/rules/marp-theme-build.md` を読む
- スライドレビュー依頼では `.claude/rules/review-workflow.md` の順序を基準に進める

## Claude Code ワークフローの Codex 対応

- `/build <file>` 相当:
  リポジトリ直下で `npx marp` を実行してビルドする。`--allow-local-files` を必ず付け、必要な形式だけ出力する。ビルド後に自動で `open` しない
- `/review-slide <file>` 相当:
  `.claude/skills/review-slide/SKILL.md` を主軸に総合レビューを行う。聴衆起点・構成・容量・配色・ファクト・表現の6観点を押さえ、指摘は重要度順に並べて総評より先に置く
- `/check-structure <file>` 相当:
  `.claude/skills/check-structure/SKILL.md` に従い、構成と論理展開を優先してレビューする。`slide-writing.md` と `review-workflow.md` を必ず参照する。過密スライドの分割が妥当か、薄い単独スライドを増やしていないかも確認する
- `/fact-check <file>` 相当:
  `.claude/skills/fact-check/SKILL.md` に従い、一次情報・公式情報を優先して検証する。Codex のブラウズ機能を使い、確認に使った URL と未検証項目を明記する
- 高度なレビュー相当:
  `nwiizo/slides` の `$review-slide-suite` を入口に、`$review-slide-flow`、`$deepen-slide-claims`、`$review-slide-narrative`、`$trim-slide-redundancy`、`$review-slide-wit` を必要に応じて利用する

Claude Code のスラッシュコマンド名がそのままユーザー入力に現れた場合、Codex では「未対応コマンド」として扱わず、上記の同等ワークフローとして解釈して実行すること。

## スライド改善時の追加観点

- 導入で立てた問いや約束は、本編とまとめで必ず回収する
- セクションは概念の列挙ではなく、因果や判断の流れが見える一本の筋でつなぐ
- 過密スライドを分割するときは、論点が切り替わる場所で切る。補足だけの薄い単独スライドは増やさない
- パンチラインは一般論で終わらせず、構造・因果・トレードオフ・境界条件が見える文へ寄せる
- 技術や手法は名前の紹介で終わらせず、「何のために必要か」「何を防ぐか」「どのコストを受け入れるか」まで言語化する
- 対立する性質は、好みや流行ではなく「何を優先するか」の違いとして説明する
- 強い主張を書くときは、適用範囲と反例の有無を意識し、断定の強さを調整する
- まとめでは新しい情報を足さず、冒頭の問い・本編の判断軸・持ち帰りだけを短く回収する

## リポジトリ境界と利用先との契約

- このリポジトリはテンプレート、テーマ、共通画像、共通ルールだけを管理する。講演Markdown、講演固有画像、公開PDFは `nwiizo/slides` で管理する。
- submoduleを置くだけではMarpの画像・CSSパスは解決しない。利用先は `brands/3shake/` のようなブランドパックとしてテーマと共通画像の実体を独立して保持する。
- 利用先のスライド本文からsubmodule内のパスを直接参照させない。テーマ名とブランドパックの安定パスを使う。
- テーマまたは共通画像を変更したら、利用先は必要な差分だけをブランドパックへ取り込み、2025年・2026年の代表資料で画像とCSSを検証する。
- HTMLは検証用生成物、PDFは利用先リポジトリで公開する成果物として扱う。

## ツール対応方針

- Claude の `Bash` 相当は Codex の端末実行で代替する
- Claude の `WebSearch` / `WebFetch` 相当は Codex のブラウズ機能で代替する
- Claude のサブエージェント相当は、Codex 側で明示的な委譲依頼がある場合、または適用スキルが独立評価を要求する場合のみ利用する
- ツール名が異なっても、**ルールで要求される作業の意図を優先**して遂行する

## 補助ツールの方針

- まずリポジトリ構造、安定した相対パス、Marp CLIの標準機能で解決する
- 一度限りの変換や単純な確認のために補助スクリプトを追加しない
- 専用ツールを継続保守する必要が生じた場合はRust CLIを第一選択とする
- Marp CLIのような公式ツールは公式のNode.js実装をそのまま利用する

## リポジトリ固有メモ

- 2026年スライドでは `theme: 3shake-2026-presentation` を使う
- 2025年以前の互換資料では `theme: 3shake-theme` を使う
- ビルド時は `--allow-local-files` を必須とする
