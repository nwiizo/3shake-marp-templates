# CLAUDE.md

このファイルは、Claude Code (claude.ai/code) がこのリポジトリのコードを扱う際のガイダンスを提供します。

## リポジトリ概要

3-SHAKE Inc. 向けのMarpベースのプレゼンテーションテンプレートリポジトリです。Markdownを使用して一貫性のあるプロフェッショナルなプレゼンテーションを作成するためのブランドテンプレート、テーマ、ツールを提供します。

## よく使う開発コマンド

### 開発サーバー
```bash
# ライブリロード付きローカルサーバー起動（デフォルトポート8080）
npm start

# プレビューモードで起動
npm run preview
```

### プレゼンテーションのビルド
```bash
# PDFに変換（marpがローカルにインストールされている場合）
marp slides/[presentation].md --pdf --allow-local-files

# PDFに変換（npxを使用）
npx @marp-team/marp-cli@latest slides/[presentation].md --pdf --allow-local-files

# PowerPointに変換
marp slides/[presentation].md --pptx --allow-local-files

# HTMLに変換
marp slides/[presentation].md --html --allow-local-files
```

### ウォッチモード（ライブリロード）
```bash
# ブラウザ自動起動付きウォッチモード
marp slides/[presentation].md --html --allow-local-files --watch & open slides/[presentation].html

# 特定プレゼンテーションの例
marp slides/2025/claude-code-beyond.md --html --allow-local-files --watch & open slides/2025/claude-code-beyond.html
```

### Neovim連携（marp.nvim）
```bash
# marp.nvimプラグインのセットアップ（lazy.nvim）
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

# Neovim内の主要コマンド:
# :MarpWatch - ライブプレビュー開始
# :MarpStop - ウォッチ停止
# :MarpExport [format] - 各種フォーマットにエクスポート
# :MarpTheme [theme] - テーマ切り替え
```

### Mermaidダイアグラムのテスト
```bash
# Mermaidサポート付きでプレビュー
npm start -- --html
```

## アーキテクチャ概要

### ディレクトリ構成の目的
- **`templates/`**: 新しいプレゼンテーション作成時にコピーするベーステンプレート
- **`themes/`**: ビジュアルスタイルとレイアウトルールを定義するCSSテーマ
- **`slides/`**: 実際のプレゼンテーション（年別に整理）
- **`assets/images/`**: 共有画像（年別・プレゼンテーション別に整理）

### テンプレート概要

| テンプレート | 用途 | 機能 |
|----------|---------|----------|
| **3shake-standard-template.md** | フル機能 | 自己紹介、会社紹介、採用情報、レイアウト例、コードブロック |
| **basic.md** | シンプル | アジェンダ、セクション、まとめ - 基本構造 |
| **with-logo.md** | スタンダード + 画像レイアウト | 左右画像配置、2カラムレイアウト |
| **background-template.md** | 最小限 | クイックスタート用の最小構造 |

### 共通テンプレート機能

すべてのテンプレートに含まれる機能:
- **フロントマター**: `math: mathjax`, `mermaid: true` 有効
- **自動ロゴ表示**: コンテンツスライドの左下にロゴを自動配置
- **タイトル/終了スライド**: 統一されたダークテーマデザイン
- **CSSクラス**:
  - `.info-box` - 情報ボックスのスタイル
  - `.highlight-blue/.highlight-green/.highlight-yellow` - テキストハイライト
  - `.reference-right` - 右寄せ参照スタイル
  - `.hidden` - 非表示要素
  - `.author-info` - 著者情報スタイル

### テーマシステム
テーマシステムはCSSカスタムプロパティとクラスベースのスタイルを使用:
- `3shake-theme.css`: ブランドカラー（#4AADDD, #0a1929, #ECBE30）を使用したプライマリテーマ
- `section:not(.title)` に対してCSS `::before` 疑似要素で会社ロゴを自動挿入
- 異なるスライドレイアウト用のカスタムクラス（`title`, `dark` など）をサポート

### Mermaid連携
カスタムCSSクラスによるレスポンシブサイズ調整でMermaidダイアグラムをサポート:
- `.mermaid-xs`: 超小型ダイアグラム（最大高さ200px）
- `.mermaid-sm`: 小型ダイアグラム（最大高さ300px）
- `.mermaid-md`: 中型ダイアグラム（最大高さ500px）
- `.mermaid-lg`: 大型ダイアグラム（最大高さ700px）
- `.mermaid-xl`: 超大型ダイアグラム（最大高さ900px）

### 引用システム
上付き文字表記で自動フォーマット:
```markdown
重要な主張^[1]^
```
学術的/プロフェッショナルな引用用の適切なスタイルでレンダリングされます。

## 主要な技術詳細

### Marp設定
- **HTML有効**: Mermaidダイアグラムと高度なフォーマットに必須
- **ローカルファイル許可**: テーマCSSと画像へのアクセスに必要
- **テーマ継承**: カスタムテーマはMarpのデフォルトテーマを拡張

### エディタ連携
このリポジトリは複数のエディタをサポート:

**VS Code連携:**
- MarkdownプレビューでHTMLを有効化
- Marpプレビュー設定を構成
- 適切なファイル関連付けを設定

**Neovim連携（marp.nvim）:**
- `:MarpWatch` でライブプレビュー
- `:MarpExport [format]` でエクスポート機能
- `:MarpTheme [theme]` でテーマ切り替え
- バッファクローズ時の自動クリーンアップ
- HTML, PDF, PPTX, PNG, JPEG形式をサポート

### アセット管理
画像は明確性を保つため年別・プレゼンテーション別に整理:
```
assets/images/2025/[presentation-name]/[image-name].png
```

### 画像の貼り方（Marp）

Marpで画像を表示する際、`<div>` タグの中にMarkdown形式の画像（`![alt](path)`）を入れると表示されないことがある。**HTMLの `<img>` タグを使用する**。

**推奨: HTMLのimgタグを使用**
```markdown
<div style="text-align: center;">
<img src="../../assets/images/2025/presentation-name/image.png" alt="説明" style="max-width: 80%; height: auto;">
</div>
```

**NG: div内でMarkdown画像記法**
```markdown
<div style="background-color: #f5f5f5; padding: 15px;">

![画像](../../assets/images/2025/presentation-name/image.png)

</div>
```

**画像配置の手順:**
1. 画像ディレクトリを作成: `mkdir -p assets/images/2025/[presentation-name]/`
2. 画像を配置: `mv /path/to/image.png assets/images/2025/[presentation-name]/`
3. スライドで参照: `<img src="../../assets/images/2025/[presentation-name]/image.png" ...>`

## 開発パターン

### 新しいプレゼンテーションの作成
1. `templates/` から適切なテンプレートをコピー
2. `slides/[year]/[presentation-name].md` に配置
3. 必要に応じて対応する画像ディレクトリを作成: `assets/images/[year]/[presentation-name]/`
4. フロントマターのテーマパスを更新: `theme: ../../themes/3shake-theme.css`
5. 必要なセクションをカスタマイズ:
   - タイトルスライド（タイトル、サブタイトル、イベント、著者）
   - 自己紹介（standard/with-logoテンプレート使用時）
   - コンテンツスライド
   - 終了スライド（著者、連絡先）

### テーマのカスタマイズ
テーマ修正時:
- ブランドカラーは `:root` 内のCSS変数で定義
- ロゴ配置は `section:not(.title)::before` で処理
- カスタムスライドレイアウトはクラスベースのセレクタを使用
- タイトルスライドは `_class: title dark` ディレクティブを使用

### 標準スライドレイアウト

#### タイトル/終了スライドパターン
```markdown
<!--
_backgroundColor: #0a1929
_color: white
_class: title dark
-->

![bg](../../assets/images/3shake-background-full.png)

<img src="../../assets/images/3shake-logo.png" alt="3-SHAKE logo" style="position: absolute !important; top: 100px !important; left: 100px !important; width: 240px !important; height: auto !important; z-index: 9999 !important;">

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
- 項目1
- 項目2

</div>
<div style="flex: 1;">

**右カラム**
- 項目A
- 項目B

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
画像の説明または出典
</div>
</div>

<div style="flex: 1;">
メインコンテンツ</br></br>

1. **ポイント1**
2. **ポイント2**
</div>
</div>
```

#### 背景画像（左/右）
```markdown
<!-- 左側背景画像 -->
![bg left:30% fit](path/to/image.jpg)
## タイトル

コンテンツ

<!-- 右側背景画像 -->
![bg right:30% 80%](path/to/image.png)
## タイトル

コンテンツ
```

#### 右寄せ参照
```markdown
<div class="reference-right">
参照: 出典名またはURL
</div>
```

#### 参考URL（スライド右下隅）
スライド内で参考URLを表示する場合は、右下隅に小さく配置する：
```markdown
<div style="position: absolute; bottom: 30px; right: 80px; font-size: 0.5em; color: #666;">
<a href="https://example.com/article">参考: 記事タイトル</a>
</div>
```
- `bottom: 30px` でスライド下端から適切な距離
- `right: 80px` でロゴと重ならない位置
- `font-size: 0.5em` で控えめなサイズ
- `color: #666` でグレー表示

### Mermaidダイアグラムのベストプラクティス
- ダイアグラムは常にサイズ指定クラス付きのHTML divでラップする
- HTMLコメントを使用してMarpディレクティブをMermaidから隠す
- ビルド前にプレビューモードでダイアグラムをテストする
- 複雑なダイアグラムは画像使用を検討（より高品質）

## プレゼンテーション作成のベストプラクティス

### コンテンツ戦略
- **ストーリーアークに従う**: 導入 → 問題 → 解決策 → 結論
- **1スライド1メッセージ**: 各スライドは1つの概念に集中
- **具体例を使う**: 抽象的な概念を具体的なデータや実例に置き換える
- **聴衆に適したコンテンツ**: 聴衆の専門知識に応じて技術的な深さを調整

### ビジュアルデザインの原則
- **コントラストを活用**: 色、サイズ、位置で重要な情報を強調
- **余白を大切に**: コンテンツを詰め込みすぎず、読みやすさを優先
- **一貫性を保つ**: フォント、色、レイアウトを全体で統一
- **戦略的な画像使用**: テキストと関連するビジュアルを組み合わせて記憶に残す

### スライドボックスの配色
カラフルな背景色は安っぽく見えるため、シンプルな階層構成を使用する：

**3段階の強調レベル:**
- **通常**: `#f5f5f5`（薄いグレー）- 通常のボックス、セクション背景
- **強（スライド内で強い）**: `#e0e0e0`（濃いグレー）- 💡ヒントボックスなど
- **最強（本当に強いメッセージ）**: `#e0e0e0` + `color: #e65100`（オレンジ文字）+ `font-size: 1.3em`

```markdown
<!-- 通常のボックス -->
<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px;">
内容
</div>

<!-- 強（ヒントボックス等） -->
<div style="background-color: #e0e0e0; padding: 10px; border-radius: 5px; text-align: center;">
💡 <strong>スライド内で強調したいメッセージ</strong>
</div>

<!-- 最強（パンチライン） -->
<div style="background-color: #e0e0e0; padding: 15px; border-radius: 8px; text-align: center; font-size: 1.3em;">
<span style="color: #e65100; font-weight: bold;">最も伝えたいメッセージ</span>
</div>
```

**避けるべき色**:
- `#ffebee` (ピンク), `#e8f5e9` (緑), `#fff3e0` (オレンジ背景), `#fff0f5` (ラベンダー)
- `#e3f2fd` (薄い青), `#fff3cd` (黄色) などの複数色の組み合わせ
- 背景色で強調するのではなく、テキスト色で強調する

### ハイライトの使用
```markdown
<span class="highlight-blue">重要なキーワード</span>
<span class="highlight-green">ポジティブなコンテンツ</span>
<span class="highlight-yellow">警告/注意</span>
```

### パフォーマンスと整理
- **画像サイズを最適化**: Web配信に適したサイズの画像を使用
- **ファイル構造を維持**: `assets/images/year/presentation-name/` 規則に従う
- **適切なテーマを選択**: プレゼンテーションのコンテキストに合ったテーマを選択
- **各フォーマットでテスト**: HTML、PDF、PPTX出力での見た目を確認
- **HTML/CSSの柔軟性**: 標準Markdownを超えた高度なレイアウトには直接HTMLとCSSを活用

### 時間管理ガイドライン
- **1スライド1〜2分**: 発表時間に基づいて総スライド数を計算
- **バッファ時間を含める**: Q&Aと潜在的な技術的問題を考慮
- **タイミングを練習**: リハーサルでペースを確認

### エンゲージメント戦略
- **キーメッセージを繰り返す**: 重要なポイントを複数回強調
- **インタラクティブ要素**: 投票、質問、ディスカッションプロンプトを含める
- **バックアップ資料**: 詳細なQ&A対応用の補足スライドを準備

## プレゼンテーションの流れと遷移のベストプラクティス

### セクション間の繋ぎスライドを必ず入れる

プレゼンテーションでセクションが変わる際、唐突に新しいトピックに入らない。**繋ぎスライド**を入れて文脈を繋げる。

**繋ぎスライドの構成要素:**
1. **前セクションの要約/確認**: 「〜はわかった」「〜が決まった」
2. **次セクションへの疑問/課題提起**: 「では、〜は？」
3. **予告**: 次に何を紹介するかを明示

**繋ぎスライドのテンプレート（濃い背景版）:**
```markdown
---

<!--
_backgroundColor: #0a1929
_color: white
-->

<div style="display: flex; flex-direction: column; justify-content: center; align-items: center; height: 80%; text-align: center;">

<div style="font-size: 1.5em; font-weight: bold;">

前セクションの確認。では、次の問い？

</div>

<div style="font-size: 0.9em; margin-top: 30px; color: #aaa;">

補足説明や文脈<br/>
→ 次に紹介する内容の予告

</div>

</div>

---
```

**繋ぎスライドのテンプレート（通常背景版）:**
```markdown
---

## 前セクションの確認、次への問い

### サブタイトル（課題や疑問）

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 20px; margin-top: 15px;">
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**よくある悩み/課題**

- 悩み1
- 悩み2
- 悩み3

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**解決策の予告**

次に紹介する内容の概要

→ 具体的に何を紹介するか

</div>
</div>

</div>

---
```

### 繋ぎが必要な典型的パターン

| パターン | 繋ぎの例 |
|---------|---------|
| Why → What | 「価値はわかった。では、何を？」 |
| What → How | 「ネタは見つかった。では、どう？」 |
| How → Practice | 「書き方はわかった。あとは実践」 |
| 概念説明 → 具体的手法 | 「でも、〜だけじゃダメ」→ 手法の紹介へ |
| 書き方 → 公開の不安 | 「書けた。でも、公開が怖い」 |
| 失敗パターン → ツール活用 | 「〜を使えばいいのでは？」→ 正しい使い方へ |

### 唐突な主張を避ける

**NG例:**
- いきなり「〇〇とは」で始まる（なぜその話をするのか文脈がない）
- 前のスライドと関係なく新しいトピックに入る
- 読者/聴衆が「なぜ今この話？」と思う展開

**OK例:**
- 前スライドで課題を提示 → 「その解決策として〇〇を紹介」
- 「よくある悩み」を先に見せる → 「それを解決する方法」
- 「〜という疑問に答えておく」と前置きしてから本題へ

### 目次（今日お話しすること）との整合性

目次スライドの内容と実際のスライド構成を必ず一致させる。構成を変更したら目次も更新する。

**チェックポイント:**
- 目次の各項目がスライド内で実際にカバーされているか
- スライドの順番と目次の順番が一致しているか
- 繋ぎスライドを追加した場合、目次の表現も適切か

### スライド分割のルール

1. **溢れたら分割する**（圧縮・削除ではなく）
2. **フォントサイズは0.75emを基準**に（コンパクトな場合は0.7em）
3. **1スライド1メッセージ**を守る
4. **繋ぎスライドは別カウント**として扱う（内容スライドではない）

### スライド1枚あたりの容量制限

Marpのスライドは画面サイズに制限があるため、以下の目安を守る：

**font-size: 0.8em の場合の目安:**

| 要素 | 収まる量 |
|-----|---------|
| 説明テキスト + 3カラムボックス | 各カラム2〜3行程度 |
| 表（テーブル） | 3〜4行程度（ヘッダー含む） |
| 2カラム + 下部ヒント | 各カラム3〜4項目 |
| リスト単独 | 5〜6項目程度 |

**収まらない場合の対処法:**

1. **分割する**: 1スライドを2スライドに分ける（最も推奨）
2. **フォントを小さくする**: 0.75em → 0.7em（限界あり）
3. **余白を減らす**: padding: 15px → 12px、gap: 20px → 15px
4. **項目を減らす**: 本当に必要な情報に絞る

**コンパクト版テンプレート（font-size: 0.8em）:**
```markdown
<div style="font-size: 0.8em;">

<div style="display: flex; gap: 15px;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**見出し**

- 項目1
- 項目2

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**見出し**

- 項目1
- 項目2

</div>
</div>

</div>
```

**溢れやすいパターンと対策:**

| パターン | 問題 | 対策 |
|---------|-----|------|
| 3カラム + 説明文 + ヒント | 縦に長すぎる | 説明文を削除 or 分割 |
| 表が5行以上 | 下が切れる | 3行程度に絞る or 分割 |
| 各カラム5項目以上 | 下が切れる | 2〜3項目に絞る or 分割 |
| 上部説明 + 中央コンテンツ + 下部ヒント | 全部入らない | 上部説明を削除 |

### 「この発表で解決できること」スライドの重要性

プレゼンテーションの冒頭（目次の後、本編の前）に「この発表で解決できること」を入れる。

**含めるべき要素:**
- 対象者の悩み（「こんな悩みを持っていませんか？」）
- 持ち帰れるもの（「この30分で得られること」）
- 明確な目標（「この発表を聞いた人が〜する」）

```markdown
## この発表で解決できること

### サブタイトル

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 20px; margin-top: 15px;">
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**こんな悩みを持っていませんか？**

- 悩み1
- 悩み2
- 悩み3

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**この発表で持ち帰れるもの**

- 得られること1
- 得られること2
- 得られること3

</div>
</div>

<div style="margin-top: 15px; padding: 12px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">目標：〜</span>
</div>

</div>
```

## プレゼンテーション構成の原則

### 全体構成の設計パターン

プレゼンテーションは以下の構造で設計する：

```
1. タイトル
2. 自己紹介（必要に応じて）
3. 目次（今日お話しすること）
4. この発表で解決できること ← 重要
5. 本編（セクション × N）
   - 各セクション間に繋ぎスライドを入れる
6. まとめ
7. 終了スライド
```

### 説得力のある主張の構成

**主張を通すための3ステップ:**

1. **反論を先に潰す**: 聴衆が持ちそうな「でも〜」を先回りして否定
2. **損失を示す**: やらないことのコスト・機会損失を直視させる
3. **利益を示す**: やることで得られるリターンを具体的に提示

**テンプレート例（「なぜ〇〇すべきか」の構成）:**

```markdown
## スライド1: 「〇〇しない理由」を全部潰す

❌「時間がない」 → 反論
❌「難しそう」 → 反論
❌「自分には早い」 → 反論

## スライド2: 「〇〇しない」という選択のコスト

- 失われるもの1（学習効果、機会、etc）
- 失われるもの2
- 失われるもの3
- 失われるもの4

## スライド3: 〇〇がもたらす3つの価値

- 価値1（具体的なリターン）
- 価値2
- 価値3
```

### コンテンツの配置原則

**情報の種類と配置場所:**

| 情報の種類 | 配置場所 | 理由 |
|-----------|---------|------|
| Why（なぜやるべきか） | 本編の最初 | 動機づけが先 |
| What（何をするか） | Whyの後 | 動機が固まってから具体論 |
| How（どうやるか） | Whatの後 | 何をするか決まってから方法論 |
| 種類・分類 | Howの冒頭 | 方法を学ぶ前に選択肢を知る |
| 具体的テクニック | 種類の後 | 種類を選んでから詳細へ |
| 注意点・落とし穴 | テクニックの後 | 実践前に知っておくべきこと |
| 実践編 | 本編の最後 | 知識が揃ってから行動へ |

**配置を間違えやすいパターン:**

| NG | OK | 理由 |
|----|-----|------|
| Whyの中に種類・分類 | Howの冒頭に種類・分類 | 種類は「どうやるか」の一部 |
| いきなり具体的手法 | 課題提示→解決策として手法 | 文脈なしの手法は唐突 |
| 最後に「なぜやるべきか」 | 最初に「なぜやるべきか」 | 動機づけは最初に必要 |

### 聴衆の心理に寄り添う構成

**聴衆が各段階で持つ疑問:**

1. **冒頭**: 「この発表、自分に関係ある？」→ 解決できることを明示
2. **Why**: 「本当にやる必要ある？」→ 反論を潰し、損失と利益を示す
3. **What**: 「で、具体的に何するの？」→ ネタの見つけ方、発想法
4. **How**: 「どうやればいいの？」→ 種類、型、テンプレート
5. **実践前**: 「失敗したくない」→ 注意点、チェックリスト
6. **最後**: 「で、何から始める？」→ 具体的な次のアクション

**各疑問に答えるスライドを用意する。**

### 外部知識・フレームワークの導入パターン

書籍や理論を紹介する際の構成：

```
1. 課題の提示（「〜だけじゃダメ」「〜が必要」）
2. 解決策の予告（「〇〇というフレームワークを紹介」）
3. 著者/出典の紹介（信頼性の担保）
4. フレームワークの概要
5. 各ステップの詳細（本題への応用を含む）
6. まとめ（本題との関連を再確認）
```

**NG: いきなり「〇〇氏とは」で始める**
**OK: 課題→解決策として〇〇氏の理論を紹介→人物紹介**

### レビュー時のチェックリスト

プレゼンテーション完成後、以下を確認：

**構成チェック:**
- [ ] 目次と実際のスライド構成が一致しているか
- [ ] 各セクション間に繋ぎスライドがあるか
- [ ] 「この発表で解決できること」が冒頭にあるか
- [ ] Why → What → How の順番になっているか
- [ ] 種類・分類はHowセクションに配置されているか

**主張チェック:**
- [ ] 「なぜやるべきか」で反論を潰しているか
- [ ] やらないことのコストを示しているか
- [ ] やることのリターンを示しているか

**流れチェック:**
- [ ] 唐突に新しいトピックに入っている箇所はないか
- [ ] 外部知識の導入前に課題提示があるか
- [ ] 聴衆の疑問に順番に答えているか

**表現チェック:**
- [ ] 年号が古くなっていないか（例: 2024年→2025年）
- [ ] 繋ぎスライドに予告が含まれているか
- [ ] 各スライドが1メッセージになっているか

## ファクトチェック

### ファクトチェックの重要性

プレゼンテーションや技術ブログで誤った情報を発信すると、信頼性を大きく損なう。特に以下の情報は必ず確認する：

- **数値・統計データ**: 出典を明記し、最新のデータか確認
- **引用・フレームワーク**: 原典を確認し、正確に引用
- **技術的事実**: 公式ドキュメントで裏付け
- **人名・書籍名**: 正式名称を確認

### 確認すべき項目

| 項目 | 確認方法 | 注意点 |
|-----|---------|-------|
| 数値・統計 | 一次ソースを確認 | 年度、調査対象を明記 |
| 引用文 | 原典で確認 | 意訳と原文を区別 |
| 技術仕様 | 公式ドキュメント | バージョンを明記 |
| 人名 | 公式プロフィール | 肩書きの時点を確認 |
| 日付・年号 | 複数ソースで確認 | 現在の年号と整合性 |

### ファクトチェックのプロセス

1. **一次ソースの確認**: 二次情報ではなく、原典や公式ドキュメントを参照
2. **複数ソースでの裏付け**: 重要な主張は複数の信頼できるソースで確認
3. **出典の明記**: 参考URLや参考文献として記載
4. **更新日の確認**: 情報が最新かどうか確認（特に技術情報）

### よくある誤り

**避けるべきパターン:**
- ❌ 「〇〇によると」と書いて出典を示さない
- ❌ 古いデータを最新のように提示
- ❌ 意訳を原文のように引用
- ❌ 伝聞情報を事実として記載
- ❌ バージョンを明記せずに技術仕様を説明

**正しいパターン:**
- ✅ 参考URLまたは参考文献を明記
- ✅ データの年度・調査対象を明記
- ✅ 原文と意訳を区別して記載
- ✅ 「〜とされている」「〜という報告がある」と断定を避ける
- ✅ 「v1.2.3時点」「2025年1月現在」と時点を明記

### プレゼンテーションでの出典表示

**スライド内での表示:**
```markdown
<div style="position: absolute; bottom: 30px; right: 80px; font-size: 0.5em; color: #666;">
<a href="https://example.com/source">出典: 調査名（2025年）</a>
</div>
```

**参考資料スライドでの表示:**
```markdown
## 参考資料

- [公式ドキュメント名](https://example.com/docs) - 2025年1月参照
- [調査レポート名](https://example.com/report) - 2024年版
- 書籍名, 著者名, 出版社, 出版年
```

### レビュー時のファクトチェックリスト

- [ ] 数値データに出典があるか
- [ ] 引用は原典を確認したか
- [ ] 技術仕様は公式ドキュメントで確認したか
- [ ] 年号・日付は現在と整合性があるか
- [ ] 人名・書籍名は正式名称か
- [ ] 参考URLは有効か（リンク切れがないか）

## Claude Code 連携機能

このリポジトリは Claude Code との連携機能を提供しています。

### スラッシュコマンド (.claude/commands/)

| コマンド | 用途 | 実行例 |
|---------|------|--------|
| `/review-slide` | 総合レビュー | `/review-slide slides/2025/my-presentation.md` |
| `/check-structure` | 構成チェック | `/check-structure slides/2025/my-presentation.md` |
| `/fact-check` | ファクトチェック | `/fact-check slides/2025/my-presentation.md` |

### サブエージェント (.claude/agents/)

専門的な視点でレビューを行うサブエージェントです：

| エージェント | 担当者 | 専門分野 |
|-------------|--------|----------|
| **slide-reviewer.md** | 山本真理（42歳） | スライド構成、視覚的デザイン、メッセージの明確化 |
| **audience-simulator.md** | 鈴木康太（32歳） | 聴衆心理、初心者/中級者/専門家の3視点分析 |
| **fact-checker.md** | 村上健一（52歳） | 情報検証、出典確認、数値妥当性評価 |

### レビューワークフロー

**スライド作成後の推奨フロー:**

1. **総合レビュー**: `/review-slide` で全体評価
2. **構成確認**: 構成に問題があれば `/check-structure` で詳細確認
3. **ファクトチェック**: `/fact-check` で事実関係を検証
4. **専門レビュー**: 必要に応じてサブエージェントを使用

**サブエージェントの使い分け:**

| 状況 | 推奨エージェント |
|-----|-----------------|
| スライドが見づらい、情報過多 | slide-reviewer |
| 聴衆に響くか不安 | audience-simulator |
| 数値や引用の正確性を確認したい | fact-checker |

### コマンド・エージェントの追加方法

**コマンドの追加:**
1. `.claude/commands/` に Markdown ファイルを作成
2. ファイル名がコマンド名になる（例: `my-command.md` → `/my-command`）

**エージェントの追加:**
1. `.claude/agents/` に Markdown ファイルを作成
2. ペルソナ（名前、年齢、経歴）を設定
3. 評価観点と出力フォーマットを定義
