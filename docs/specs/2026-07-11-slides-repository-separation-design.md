# スライドリポジトリ分離設計

## 目的

講演資料をテンプレート実装から分離し、公開リポジトリ `nwiizo/slides` で管理する。`nwiizo/3shake-marp-templates` は3-SHAKE向けMarpテンプレートとして維持し、`nwiizo/slides` から交換可能な依存先として参照する。

この分離により、講演資料と講演固有画像の履歴を個人の制作物として管理しつつ、転職などで利用テンプレートが変わった場合にも依存先を明示的に差し替えられるようにする。

## 採用方式

`nwiizo/slides` から `nwiizo/3shake-marp-templates` をGit submoduleとして参照する。

GitHub Template repositoryから生成する方式は採用しない。Template repositoryは作成時のコピーには適するが、作成後の更新を継続的に取り込む依存関係を表現しないためである。Git subtreeとnpmパッケージ化も採用しない。subtreeはテンプレート資産を子側へ複製し、npmパッケージ化はMarpのCSS・ローカル画像パスに対して運用が過剰になる。

## リポジトリ境界

### `nwiizo/slides`

個人の講演制作物を所有する。

```text
nwiizo/slides
├── slides/
│   ├── 2025/
│   └── 2026/
├── assets/
│   └── images/
│       └── 2026/
├── vendor/
│   └── 3shake-marp-templates/  # Git submodule
├── .marprc.yml
├── package.json
├── README.md
└── AGENTS.md
```

- `slides/` 配下の全Markdownと補助スクリプトを移す。
- `assets/images/2026/` の講演固有画像を移す。
- スライドの作成、レビュー、ビルドに必要な設定と説明を持つ。
- submoduleは検証済みのテンプレートコミットへ固定する。

### `nwiizo/3shake-marp-templates`

再利用可能なテンプレート資産を所有する。

- `templates/`、`themes/`、`examples/` を残す。
- `assets/images/` 直下のロゴ、背景、会社紹介、発表者アイコンなどの共通画像を残す。
- `.claude/`、`CLAUDE.md`、`AGENTS.md` にテンプレート利用者向けの執筆、レビュー、ビルドルールを残す。
- `slides/` と年度別の講演固有画像は持たない。
- READMEの利用例は、利用者側リポジトリからsubmoduleとして参照する構成に合わせる。

## 依存関係とパス

`nwiizo/slides` は `vendor/3shake-marp-templates` にsubmoduleを配置する。

- 2026年スライドはテーマ名 `3shake-2026-presentation` を維持する。
- `nwiizo/slides/.marprc.yml` の `themeSet` はsubmodule内の `themes/` を指す。
- 2025年スライドのCSSファイル参照はsubmodule内のテーマへ変更する。
- 共通ロゴや背景画像の参照は `vendor/3shake-marp-templates/assets/images/` へ変更する。
- 講演固有画像は `nwiizo/slides/assets/images/{year}/` を参照する。
- ビルドは常に `nwiizo/slides` のルートから `--allow-local-files` 付きで実行する。

既存スライドの内容やページ構成は変更せず、移行に必要なテーマ・画像パスだけを機械的に変更する。

## 移行手順

1. 現在の作業ツリーにある未コミットのスライド、講演固有画像、README、CLAUDEの変更を一覧化する。
2. コミット済み履歴から `slides/` と年度別の講演固有画像に関係する履歴を抽出し、新しいローカルリポジトリの基礎にする。
3. 未コミットのスライドと講演固有画像を新しいローカルリポジトリへ重ね、現状の内容を保全する。
4. `nwiizo/3shake-marp-templates` から `slides/` と年度別画像を削除し、ドキュメントをテンプレート専用構成へ更新する。
5. テンプレート側の変更をローカルで確定する。まだGitHubへはpushしない。
6. `nwiizo/slides` にテンプレートリポジトリをsubmoduleとして追加する。.gitmodulesには公開GitHub URLを記録し、ローカルでは手順5のコミットに固定する。
7. `nwiizo/slides` のMarp設定、パス、README、AGENTSを更新し、ローカルでビルドを検証する。
8. ローカル検証に成功したテンプレート側コミットをGitHubへpushする。
9. GitHubにPublicリポジトリ `nwiizo/slides` を作成してpushする。
10. 一時ディレクトリへのクリーンcloneでsubmodule取得と代表スライドのビルドを検証する。

子側へのコピーとビルド確認が終わるまで、親側の元ファイルは確定・pushしない。移行途中に失敗した場合は、Jujutsuの操作履歴と新規リポジトリ側のコピーから復旧できる状態を保つ。

## 履歴の扱い

新しいリポジトリには、スライドと年度別画像に関係する既存コミット履歴を可能な範囲で引き継ぐ。テンプレート、テーマ、共通画像だけを変更したコミットは含めない。

現在の未コミット変更は、既存履歴の抽出後に移行コミットとして追加する。READMEとCLAUDEの既存未コミット変更はユーザー変更として保全し、テンプレート側の構成更新と衝突する箇所だけ内容を統合する。

## エラー処理と安全策

- `nwiizo/slides` が既に存在する場合は作成を中止し、既存内容を上書きしない。
- submoduleの追加または取得に失敗した場合は、親側の資料削除をpushしない。
- パス変更後に画像またはテーマが解決できないスライドがあれば、移行完了としない。
- 履歴抽出ツールが利用できない場合は、作業を止めず、対象履歴を保持できる別のGit手順を選ぶ。ただし、テンプレートリポジトリの全履歴を無条件に子へ複製しない。
- 既存の未コミット変更を破棄するGit/Jujutsu操作は行わない。
- GitHubへのpush前に、対象リポジトリ、revision、bookmarkを明示的に確認する。

## 検証

### 構造検証

- テンプレート側に `slides/` と年度別講演画像が残っていない。
- スライド側に `slides/`、講演固有画像、submodule、Marp設定が存在する。
- submodule URLが公開リポジトリ `https://github.com/nwiizo/3shake-marp-templates` を指す。
- スライド側にテンプレート共通画像の不要な複製がない。

### ビルド検証

少なくとも以下をHTMLへビルドする。

- 2025年の既存スライド1件。
- 2026年の既存スライド1件。
- 現在未コミットの新規2026年スライド。
- 講演固有画像を多く参照する新規2026年スライド。

すべて `--allow-local-files` を付け、終了コードだけでなく、テーマ未検出やローカル画像未解決の警告がないことを確認する。

### クリーンclone検証

一時ディレクトリへ次の条件でcloneし、代表スライドを再ビルドする。

- `git clone --recurse-submodules` でsubmoduleが取得できる。
- 依存関係をREADMEの手順どおり導入できる。
- ローカルの元リポジトリを参照せずにビルドできる。

## 完了条件

- `nwiizo/slides` がPublicリポジトリとしてGitHubに存在する。
- 全スライドと講演固有画像が新リポジトリに存在し、現在の未コミット変更も含まれる。
- テンプレートリポジトリから講演制作物が除かれている。
- submodule経由で2025年・2026年の代表スライドをビルドできる。
- クリーンcloneから同じビルドを再現できる。
- 両リポジトリのREADMEにclone、submodule更新、ビルド、将来のテンプレート差し替え方法が記載されている。

## 対象外

- スライド本文、構成、デザインの改善。
- 3-SHAKE以外の新しいテーマ作成。
- テンプレートのnpmパッケージ化。
- submodule更新の自動化。
- GitHub PagesやReleaseへの生成物公開。
