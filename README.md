# 3shake-marp-templates

Collection of Marp-based presentation templates for [3-SHAKE](https://3-shake.com/). Features standardized designs, citation styles, Mermaid diagram support, and custom layouts for cloud-native technology and enterprise solution presentations, ensuring consistent branding and high-quality slide creation.

**Note: This is an unofficial repository. Users are encouraged to fork it for their own use. Content may be modified without prior notice.**

## 概要

このリポジトリは[株式会社スリーシェイク](https://3-shake.com/)のプレゼンテーション資料作成のための[Marp](https://marp.app/)テンプレート集です。社内外向けのプレゼンテーションで一貫したブランディングと高品質なスライド作成を支援します。

## 開発環境のセットアップ

```bash
# リポジトリをクローン
git clone https://github.com/nwiizo/3shake-marp-templates.git
cd 3shake-marp-templates

# 依存関係をインストール
npm install

# 開発サーバーを起動（http://localhost:8080）
npm start

# プレビューモードで起動
npm run preview
```

## テンプレート一覧

### メインテンプレート（templates/）

| テンプレート | 用途 | 特徴 |
|-------------|------|------|
| **3shake-standard-template.md** | フル機能版 | 自己紹介、会社紹介、Hiring、各種レイアウト例、コードブロック含む |
| **basic.md** | シンプル版 | アジェンダ、セクション、まとめの基本構成 |
| **with-logo.md** | 標準版 + 画像レイアウト例 | 右/左配置、2カラムレイアウトの例を含む |
| **background-template.md** | ミニマル版 | 最小限の構成で素早く開始可能 |

### テンプレートの共通機能

すべてのテンプレートには以下の機能が含まれています：

- **フロントマター設定**: `math: mathjax`, `mermaid: true` 対応
- **自動ロゴ表示**: 通常スライドの左下に自動配置
- **タイトル/終了スライド**: 統一されたダークテーマデザイン
- **CSSクラス**:
  - `.info-box` - 情報ボックス
  - `.highlight-blue/.highlight-green/.highlight-yellow` - テキストハイライト
  - `.reference-right` - 参考文献用右寄せスタイル
  - `.hidden` - 非表示要素
  - `.author-info` - 作者情報スタイル

### サンプル・例（examples/）
- **3shake-presentation-nwiizo-edition.md** - @nwiizo カスタムテンプレート
- **3shake-standard-slides.md** - 拡張版プレゼンテーション例
- **mermaid-examples.md** - Mermaid図表の包括的な使用例
- **sample-presentation.md** - サンプルプレゼンテーション
- **title-slide.md** - タイトルスライド専用例

## テーマ

- **themes/3shake-theme.css** - 基本テーマ（推奨）
- **themes/3shake-standard-theme.css** - 標準テーマ
- **themes/title-theme.css** - タイトルスライド用テーマ
- **themes/custom.css** - カスタムテーマ

## 使い方

### 新しいプレゼンテーションの作成

1. このリポジトリをフォークして自分のリポジトリとして使用することを推奨します
2. 目的に合ったテンプレートファイル（`templates/`）をコピーして新しいファイルを作成
   ```bash
   # 例：標準テンプレートをベースに新しいプレゼンテーションを作成
   cp templates/3shake-standard-template.md slides/2025/my-presentation.md
   ```
3. テンプレートの内容を編集してプレゼンテーションを作成
4. 画像用ディレクトリを作成（必要に応じて）
   ```bash
   mkdir -p assets/images/2025/my-presentation
   ```
5. 開発サーバーでプレビューしながら編集
   ```bash
   npm start
   # ブラウザで http://localhost:8080 にアクセス
   ```

### テンプレートのカスタマイズ

テンプレート使用時に編集が必要な箇所：

1. **タイトルスライド**
   - プレゼンテーションタイトル
   - サブタイトル
   - イベント名・日付
   - 発表者名・発表時間

2. **自己紹介スライド**（標準/with-logoテンプレート）
   - プロフィール画像のパス
   - 自己紹介文
   - 趣味・専門分野

3. **コンテンツスライド**
   - セクションタイトル
   - 本文内容
   - 画像パス

4. **終了スライド**
   - 発表者名
   - 連絡先

### ファイル出力

[Marp CLI](https://github.com/marp-team/marp-cli)、[Marp for VS Code](https://marketplace.visualstudio.com/items?itemName=marp-team.marp-vscode)、または [marp.nvim](https://github.com/nwiizo/marp.nvim) を使用してPDFやPPTXに変換できます。

```bash
# PDFへの変換（ローカルにmarpがインストールされている場合）
marp slides/my-presentation.md --pdf --allow-local-files

# PDFへの変換（npxを使用）
npx @marp-team/marp-cli@latest slides/my-presentation.md --pdf --allow-local-files

# PowerPointへの変換
marp slides/my-presentation.md --pptx --allow-local-files

# HTMLへの変換
marp slides/my-presentation.md --html --allow-local-files
```

### ライブプレビュー（Watchモード）

ファイルの変更を監視して自動的に再生成する機能：

```bash
# Watchモードでブラウザ自動起動
marp slides/my-presentation.md --html --allow-local-files --watch & open slides/my-presentation.html

# 具体例
marp slides/2025/claude-code-beyond.md --html --allow-local-files --watch & open slides/2025/claude-code-beyond.html
```

### Neovim ユーザー向け (marp.nvim)

[marp.nvim](https://github.com/nwiizo/marp.nvim) プラグインを使用することで、Neovim内でMarpプレゼンテーションを効率的に作成・編集できます。

**インストール (lazy.nvim):**
```lua
{
  'nwiizo/marp.nvim',
  ft = "markdown",
  config = function()
    require("marp").setup {
      marp_command = "/path/to/marp",
      server_mode = false
    }
  end,
}
```

**主要コマンド:**
- `:MarpWatch` - ライブプレビュー開始
- `:MarpStop` - 監視停止
- `:MarpExport [format]` - プレゼンテーション出力（HTML、PDF、PPTX、PNG、JPEG対応）
- `:MarpTheme [theme]` - テーマ切り替え

**特徴:**
- ライブプレビュー（自動リフレッシュ）
- 自動クリーンアップ（バッファクローズ時）
- マルチフォーマット出力対応
- 簡単なテーマ切り替え
- プレゼンテーション要素用スニペット

## 高度な機能

### スライドレイアウトパターン

#### タイトル/終了スライド
```markdown
<!--
_backgroundColor: #0a1929
_color: white
_class: title dark
-->

![bg](../assets/images/3shake-background-full.png)

<img src="../assets/images/3shake-logo.png" alt="3-SHAKE logo" style="position: absolute !important; top: 100px !important; left: 100px !important; width: 240px !important; height: auto !important; z-index: 9999 !important;">

<div class="title" style="text-align: left; margin-top: 100px; margin-left: 20px; padding-left: 0; max-width: 70%;">

# タイトル

### サブタイトル

</div>

<div class="author-info" style="text-align: left; padding-left: 0; text-indent: 0;">
2025/XX/XX イベント名</br>
@your_name 発表時間
</div>
```

#### 2カラムレイアウト
```markdown
## セクションタイトル

<div style="display: flex; gap: 40px;">
<div style="flex: 1;">

**左カラム**
- 項目 1
- 項目 2

</div>
<div style="flex: 1;">

**右カラム**
- 項目 A
- 項目 B

</div>
</div>
```

#### 画像 + テキストレイアウト
```markdown
## タイトル

<div style="display: flex; gap: 40px;">
<div style="width: 35%;">
<img src="path/to/image.png" alt="説明" style="width: 100%; height: fit-content;">
<div style="font-size: 0.7em; text-align: left; margin-top: 5px;">
画像の説明やソース
</div>
</div>

<div style="flex: 1;">
メインコンテンツ</br></br>

1. **ポイント 1**
2. **ポイント 2**
</div>
</div>
```

#### 背景画像（右/左）
```markdown
<!-- 左側に背景画像 -->
![bg left:30% fit](path/to/image.jpg)
## タイトル

コンテンツ

<!-- 右側に背景画像 -->
![bg right:30% 80%](path/to/image.png)
## タイトル

コンテンツ
```

### Mermaid図表サポート

このテンプレートは包括的なMermaid図表サポートを提供します。詳細な例は `examples/mermaid-examples.md` を参照してください。

そこまで良いものではないので画像を使用することを推奨します。

利用可能なサイズクラス：
- `.mermaid-xs` - 200px（超小）
- `.mermaid-sm` - 300px（小）
- `.mermaid-md` - 500px（中）
- `.mermaid-lg` - 700px（大）
- `.mermaid-xl` - 900px（超大）

### 引用スタイル

引用が必要な場合は、上付き文字記法を使用できます：

```markdown
これはサンプルテキストです^[1]^。さらに別の引用もあります^[2]^。

> [1] https://example.com
> [2] https://sub.example.com
```

### 参考文献の右寄せ表示

```markdown
<div class="reference-right">
参考：ソース名やURL
</div>
```

## 資料作成のTips

### 効果的なプレゼンテーション作成のポイント

#### コンテンツ構成
- **ストーリー性を意識**: 導入 → 課題 → 解決策 → 結論の流れを明確に
- **1スライド1メッセージ**: 各スライドで伝えたいことを1つに絞る
- **数字と具体例**: 抽象的な表現より具体的なデータや事例を使用
- **聴衆に合わせた内容**: 技術レベルや関心事に応じて内容を調整

#### 視覚的デザイン
- **コントラストを活用**: 重要な情報は色や大きさで強調
- **余白を効果的に使用**: 情報を詰め込みすぎず、読みやすさを重視
- **一貫性のあるスタイル**: フォント、色、レイアウトを統一
- **画像の活用**: 文字だけでなく図表や画像で視覚的に訴求

#### ハイライトの使い分け
```markdown
<span class="highlight-blue">重要なキーワード</span>
<span class="highlight-green">ポジティブな内容</span>
<span class="highlight-yellow">注意点・警告</span>
```

#### 時間管理
- **1スライド1-2分**: 発表時間から逆算してスライド数を決定
- **余裕を持った構成**: 質疑応答の時間も考慮
- **練習で時間測定**: 実際に発表練習して調整

#### 発表のコツ
- **キーメッセージの繰り返し**: 重要なポイントは複数回伝える
- **インタラクティブな要素**: 質問や確認を挟んで聴衆の注意を引く
- **バックアップスライド**: 質疑応答用の補足資料を準備

### パフォーマンス最適化
- **画像サイズの最適化**: 必要以上に大きな画像は避ける
- **ファイル構成の整理**: assets/images/年度/プレゼン名/ の構造を維持
- **テーマの選択**: 用途に応じて適切なテーマを選択
- **HTMLとCSSの活用**: Marpでは直接HTMLが使用可能、柔軟なレイアウトを実現

## Claude Code 連携

このリポジトリは [Claude Code](https://claude.ai/code) との連携機能を提供しています。

### スラッシュコマンド (.claude/commands/)

Claude Code で以下のコマンドが使用できます：

| コマンド | 用途 |
|---------|------|
| `/review-slide` | スライドの総合レビュー（構成、容量、配色、ファクトチェック、表現） |
| `/check-structure` | プレゼンテーション構成の専門チェック |
| `/fact-check` | 事実関係の検証（数値、引用、技術仕様、年号など） |

**使用例:**
```
/review-slide slides/2025/my-presentation.md
```

### サブエージェント (.claude/agents/)

専門的な視点でレビューを行うサブエージェントが利用可能です：

| エージェント | 役割 | 専門分野 |
|-------------|------|----------|
| **slide-reviewer** | 山本真理 | スライド構成、視覚的デザイン、メッセージの明確化 |
| **audience-simulator** | 鈴木康太 | 聴衆心理の分析、初心者/中級者/専門家の視点 |
| **fact-checker** | 村上健一 | 情報の検証、出典確認、数値データの妥当性 |

### レビューのワークフロー

1. スライド作成後、`/review-slide` で総合レビュー
2. 構成に問題がある場合は `/check-structure` で詳細確認
3. 事実関係の確認は `/fact-check` を使用
4. 必要に応じてサブエージェントで専門的なレビューを依頼

## カスタマイズ

テーマファイル（CSSファイル）を編集して、必要に応じてスタイルをカスタマイズできます。

## 貢献方法

1. このリポジトリをフォークします
2. 新しいブランチを作成します
3. 変更を加えてコミットします
4. プルリクエストを送信します

## ライセンス

社内用テンプレートのため、ライセンスは適用されません。このリポジトリは非公式であり、予告なく変更される可能性があります。

## 連絡先

質問や提案がある場合は、社内のコミュニケーションチャネルでお問い合わせください。
