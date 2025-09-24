---
marp: true
theme: ../../themes/3shake-theme.css
paginate: true
math: mathjax
mermaid: true
style: |
  :root {
    --logo-url: url("../../assets/images/3shake-cover.png");
    --mini-font-size: 20px;
    --header-footer-height: 50px;
    --black: #333;
  }
  /* Add highlight-red class */
  .highlight-red {
    color: rgb(224, 64, 64);
  }
  /* 通常ページの左下にロゴを表示（より左下に押し込む） */
  section:not(.title)::before {
    content: "";
    position: absolute;
    left: 15px;  /* より左に */
    bottom: 15px;  /* より下に */
    width: 60px !important;  /* ロゴサイズを調整 */
    height: 60px !important;  /* ロゴサイズを調整 */
    background-image: var(--logo-url);
    background-size: contain;
    background-repeat: no-repeat;
    background-position: center;
    opacity: 0.9;
    z-index: 100;
  }
  /* 背景色付きスライドではロゴ色を反転 */
  section[data-background-color="dark"]::before {
    filter: brightness(0) invert(1);
  }
  /* タイトルページのページ番号を非表示 */
  section.title::after {
    display: none;
  }
  /* タイトルスライドのロゴスタイル */
  .title-logo {
    position: fixed;
    top: 5px;
    left: 5px;
    width: 240px !important;
    height: auto;
    z-index: 9999;
  }
  /* すべての画像サイズを上書き - 3shake-logo.pngのみに適用 */
  img[src*="3shake-logo.png"] {
    max-width: 240px !important;
    width: 240px !important;
  }
  /* タイトルとサブタイトルのサイズ調整 */
  .title h1 {
    font-size: 2.4em !important;
    margin-bottom: 0.1em !important;
  }
  .title h3 {
    font-size: 1.1em !important;
    margin-top: 0.1em !important;
  }
  /* 作者情報のスタイル */
  .author-info {
    position: absolute !important;
    bottom: 40px !important;
    left: 100px !important;
    padding-left: 0 !important;
    text-indent: 0 !important;
    font-size: 0.9em !important;
    color: white !important;
    font-weight: bold !important;
  }
  /* スライドタイトル（h2）のスタイル */
  section h2 {
    font-size: 1.8em !important;
    margin-top: -25px !important;
    padding-top: 0 !important;
    margin-bottom: 10px !important;
    color: black !important;
    border-bottom: 1px solid #dadce0 !important;
  }
  /* コンテンツエリアの上部マージンを調整 */
  section > *:not(h2):not(header):not(footer) {
    margin-top: 1.2em !important;
  }
  /* 引用（参考文献）のスタイル */
  blockquote {
    border-top: 0.1em dashed var(--black);
    font-size: var(--mini-font-size);
    width: 100%;
    position: absolute;
    bottom: var(--header-footer-height);
    left: 0;
    padding: 10px 20px;
    margin: 0;
    box-sizing: border-box;
  }
  /* カスタムテーマにMermaidのスタイル設定を追加 */
  mermaid {
    display: block;
    margin: 0 auto;
  }
  /* 参考文献用のスタイル */
  .reference-right {
    font-size: 0.4em;
    text-align: right;
    margin-right: 20px;
    margin-top: -15px;
    display: block;
    width: 30%;
    margin-left: 70%;
  }
---

<!-- 
_backgroundColor: #0a1929
_color: white
_class: title dark
-->

![bg](../../assets/images/3shake-background-full.png)

<img src="../../assets/images/3shake-logo.png" alt="3-SHAKE logo" style="position: absolute !important; top: 100px !important; left: 100px !important; width: 240px !important; height: auto !important; z-index: 9999 !important;">

<div class="title" style="text-align: left; margin-top: 100px; margin-left: 20px; padding-left: 0; max-width: 70%;">

# <span style="font-size: 1.2em;">バイブコーディングとは？</span>


</div>

<div class="author-info" style="text-align: left; padding-left: 0; text-indent: 0;">
2025/09/24 CCoE Meetup #7 </br>
バイブコーディング with Google Cloud</br>
@nwiizo 20min
</div>

---

<!-- _backgroundColor: white -->

![bg left:30% fit](../../assets/images/nwiizo_icon.jpg)
## nwiizo

<div class="info-box">
株式会社スリーシェイクで
プロのソフトウェアエンジニアをやっているものです
格闘技、読書、グラビアが趣味でよく本を紹介しています
</div>

<p style="margin-top: 30px !important;">人生を通して"<strong>運動、睡眠、読書</strong>"をきちんとやりたい</p>

---

## about 3-shake

<div style="text-align: center; margin-top: 30px;">
  <img src="../../assets/images/3shake-about.png" alt="3-shake about" style="width: 80%; margin-top: 10px;">
</div>

---

## We are Hiring!!

<div style="text-align: center; margin-top: 30px;">

3-shakeは一緒にSRE界隈を盛り上げてくれる<strong>仲間を大募集中</strong>です！
Mobility、FinTech、通信など大規模SREを存分に経験できます
（最近社内はGenAI / GPU / Kubernetesが盛り上がってます）
是非、カジュアル面談しましょう！！！！

  <img src="../../assets/images/3shake-hiring.png" alt="3-shake about" style="width: 80%; margin-top: 10px;">
</div>

---

## Vibe Codingの本質と哲学

### 自然言語対話による新しい開発パラダイム

<div style="font-size: 0.75em;">

**Andrej Karpathy氏が2025年2月に提唱**

「もはやコーディングとは言えない。画面を見て、指示を出して、実行して、コピペするだけ」

<div style="display: flex; gap: 20px; margin-top: 15px;">
<div style="flex: 1; padding: 12px; background-color: #f0f0f0; border-radius: 5px;">

**従来: HOW中心**
- 実装詳細を記述
- 手続き的思考
- コード行数が価値

</div>
<div style="flex: 1; padding: 12px; background-color: #e3f2fd; border-radius: 5px;">

**Vibe Coding: WHAT中心**
- 意図を伝える
- 目的志向的思考
- 成果が価値

</div>
</div>

<div style="margin-top: 15px; padding: 10px; background-color: #fff3cd; border-radius: 5px; text-align: center;">
💡 <strong>パラダイムシフト</strong>: 創造性と生産性の新たなバランスを実現
</div>

</div>

---

## 意図によるプログラミング

### Programming with Intent - 高レベルの抽象化

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 20px;">
<div style="flex: 1;">

**🔄 従来のコーディング**
```javascript
function calculateMonthlyPayment(principal, rate, term) {
  const monthlyRate = rate / 12 / 100;
  const numPayments = term * 12;
  return principal * monthlyRate /
    (1 - Math.pow(1 + monthlyRate, -numPayments));
}
```
詳細なアルゴリズムを実装

</div>
<div style="flex: 1;">

**🎯 意図ベースのコーディング**
```text
住宅ローン計算機を作成:
- 元本、利率、期間を入力
- 月々の支払額を計算
- 0%金利も処理
- 総支払額と利息も表示
```
ビジネス要件を伝える

</div>
</div>

<div style="margin-top: 15px; text-align: center; padding: 10px; background-color: #e3f2fd; border-radius: 5px;">
💡 開発者は<strong>アーキテクトとエディター</strong>の役割にシフト
</div>

</div>

---

## AI Codingの範囲

### Vibe CodingとAI-Assisted Engineeringの連続体

<div style="display: flex; gap: 30px; align-items: flex-start;">
<div style="flex: 1;">

**🎨 Vibe Coding**
- AIと会話しながらコード生成
- 「こんな機能がほしい」で実装
- 細かい作成はAIに任せる
- プロトタイプ開発に最適

<div style="font-size: 0.75em; margin-top: 10px; color: #666;">
作業効率：従来よりも圧倒的に高速
</div>

</div>
<div style="flex: 1;">

**🏗️ AI-Assisted Engineering**
- 計画先行型の開発
- 明確な意図と制約を定義
- テストとレビューを重視
- プロダクション環境向け

<div style="font-size: 0.75em; margin-top: 10px; color: #666;">
Karpathy氏も本番環境では全く異なるアプローチを採用
</div>

</div>
</div>

<div style="text-align: center; margin-top: 20px; font-size: 0.8em;">
💡 短期的な速度 vs 持続的な信頼性の最適化
</div>

---

## AIが得意とする具体的な領域

### 定型作業を高速で処理

<div style="font-size: 0.7em;">

<div style="display: flex; gap: 15px;">
<div style="flex: 1;">

**🚀 高速プロトタイピング**
- MVP作成: 数週間→数時間
- UI/UX実装の即座の具現化
- データベーススキーマ自動設計

**📊 データ処理**
- ETLパイプライン構築
- データ変換・正規化
- レポート生成ロジック

</div>
<div style="flex: 1;">

**🔧 統合・連携**
- サードパーティAPI統合
- Webhook処理
- 認証フロー実装

**📝 ドキュメント生成**
- API仕様書作成
- テストケース生成
- コメント・説明の自動付与

</div>
</div>

<div style="margin-top: 10px; padding: 8px; background-color: #e3f2fd; border-radius: 5px; text-align: center;">
💡 主要フレームワーク（React、Vue、Django、Rails、Flutter）の知識を即座に活用
</div>

</div>

---

## AIと人間の役割分担

### AIが得意な領域 vs 人間が必須の領域

<div style="font-size: 0.65em;">

<div style="display: flex; gap: 15px;">
<div style="flex: 1; background-color: #e8f5e9; padding: 10px; border-radius: 5px;">

**✅ AIが得意とする領域**
- **定型処理**: CRUD操作、ボイラープレート
- **フレームワーク実装**: React/Vue/Express標準パターン
- **統合作業**: API接続、データ変換
- **反復コード**: テストケース、類似構造の大量生成

</div>
<div style="flex: 1; background-color: #ffebee; padding: 10px; border-radius: 5px;">

**🔥 人間が担う領域**
- **アーキテクチャ設計**: スケーラビリティ、保守性
- **複雑な問題解決**: 新規アルゴリズム、最適化
- **ドメイン知識**: ビジネスロジック、規制要件
- **品質保証**: セキュリティ監査、エッジケース

</div>
</div>

<div style="margin-top: 10px; padding: 8px; background-color: #fff3cd; border-radius: 5px; text-align: center;">
⚠️ Google Chrome UX責任者: 「仕事は速くなったが、アプリの質は向上していない」
</div>

</div>

---

## 適切な適応範囲

### 場面に応じた使い分け

<div style="display: flex; gap: 20px; font-size: 0.75em;">
<div style="flex: 1;">

**🚀 素早く試す開発**

<div style="font-size: 0.85em; margin-bottom: 8px;">
新機能を作り始めるとき<br>
AIと対話しながらアイデアを形に<br>
デモ版は素早く作って見せる<br>
</div>

```
例：「日付を見やすく表示する関数を作って」
→ AIがサッと生成
→ 確認・修正して組み込み
→ 設計重視の開発に戻る
```

</div>
<div style="flex: 1;">

**🎯 きちんと作る開発**

<div style="font-size: 0.85em; margin-bottom: 8px;">
コードが複雑になってきたら<br>
設計をしっかり考える開発に切り替え<br>
本番システムは品質重視で作り直し<br>
</div>

```
例：システム全体設計
→ アーキテクチャを設計
→ 部分的にAIを活用
→ テストとレビューを徹底
```

</div>
</div>

<div style="text-align: center; margin-top: 15px; font-size: 0.8em;">
💡 AIはツール。場面に応じて賢く使い分けるのがプロ
</div>

---

## シニア開発者の役割進化

### アーキテクトとエディターへの変革

<div style="font-size: 0.7em;">

<div style="display: flex; gap: 15px;">
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 5px;">

**🏗️ アーキテクチャ設計**
- 全体設計と品質管理
- 技術選定とトレードオフ
- システム境界の定義

**👥 メンタリング強化**
- AIツールの効果的な使い方
- プロンプトパターンの指導
- コードレビュー基準の設定

</div>
<div style="flex: 1; background-color: #e3f2fd; padding: 10px; border-radius: 5px;">

**🎯 ドメイン専門性**
- 業界知識と技術的洞察
- ビジネス要件の翻訳
- 長期的視点での意思決定

**🗣️ ソフトスキル重視**
- ステークホルダーコミュニケーション
- 戦略的意思決定
- チームビルディング

</div>
</div>

<div style="margin-top: 10px; padding: 8px; background-color: #fff3cd; border-radius: 5px; text-align: center;">
🎯 <strong>シニアの本質</strong>: AIオーケストラの指揮者として全体を統括
</div>

</div>

---

## ミドル開発者の適応戦略

### 🌳 AIと共に成長する中堅エンジニアの道

<div style="font-size: 0.75em;">

<div style="background-color: #e8f5e9; padding: 15px; border-radius: 8px; margin-top: 15px;">

ミドル開発者にとってAI時代は、単なる脅威ではなく飛躍のチャンスです。

システム統合の専門家として、API設計やマイクロサービス間の通信設計など、複数のコンポーネントを調整する高度な技術が求められます。AIはコードの生成はできても、システム全体の整合性を保つ判断はまだ人間の領域です。

</div>

<div style="margin-top: 15px;">

パフォーマンス最適化においても、データベースの最適化戦略やキャッシング設計、フロントエンドの高速化など、実際のボトルネックを見極めて適切な対策を選択する能力が重要になります。

DevOps文化の推進者として、監視・可観測性の設計やCI/CDパイプラインの改善を通じて、開発プロセス全体の効率化を主導します。AIツールを活用しながらも、チーム全体の生産性を向上させる仕組みづくりが求められます。

</div>

<div style="margin-top: 15px; padding: 12px; background-color: #fff3cd; border-radius: 8px;">
💡 <strong>ミドル開発者の強み</strong>: ビジネス要件を技術に翻訳し、技術的負債を管理しながら、AIと人間の橋渡し役として価値を発揮
</div>

</div>

---

## ジュニア開発者の成長戦略

### 🌱 AI時代でも必要な基礎力の構築

<div style="font-size: 0.75em;">

<div style="background-color: #fff3e0; padding: 15px; border-radius: 8px; margin-top: 15px;">

ジュニア開発者にとって最も重要なのは、AIに依存しすぎない基礎力の構築です。

データ構造とアルゴリズムの理解、非同期処理のメカニズムなど、プログラミングの本質的な概念を身につけることが、AIを効果的に活用する土台となります。AIが生成したコードが正しいかを判断し、必要に応じて修正できる力が求められます。

</div>

<div style="margin-top: 15px;">

意図的な「AIデトックス期間」を設けることも重要です。週に一度は手動でコーディングし、デバッグ手法を習得することで、AIがない環境でも問題解決できる能力を維持します。

品質への意識を高めるために、テスト駆動開発を実践し、コードレビューから積極的に学びます。AIが生成したコードであっても、なぜそのようなコードになっているのかを理解し、改善点を見つける目を養います。

</div>

<div style="margin-top: 15px; padding: 12px; background-color: #e3f2fd; border-radius: 8px;">
💡 <strong>長期的視点</strong>: 可読性を重視したコーディングスタイルを身につけ、OSSへの貢献を通じて実践的なスキルを磨く
</div>

</div>

---

## 実践的ワークフローパターン

### 3つの成功モデルの詳細

<div style="font-size: 0.65em;">

<div style="display: flex; gap: 10px;">
<div style="flex: 1; background-color: #f0f8ff; padding: 8px; border-radius: 5px;">

**🎨 AIファーストドラフター**

初期実装をAIが高速生成
- 要件定義 → AI生成
- 精査とリファクタリング
- モジュール化、エラー処理
- テスト追加

</div>
<div style="flex: 1; background-color: #f0fff0; padding: 8px; border-radius: 5px;">

**👥 AIペアプログラマー**

リアルタイム対話開発
- 新セッション/タスク
- 頻繁なコミット&レビュー
- タイトなフィードバック
- 即座の改善

</div>
<div style="flex: 1; background-color: #fff0f5; padding: 8px; border-radius: 5px;">

**✅ AI検証者**

人間のコードをAIがレビュー
- セキュリティ脆弱性検出
- パフォーマンス改善
- テストケース自動生成
- ベストプラクティス提案

</div>
</div>

<div style="margin-top: 10px; padding: 8px; background-color: #e3f2fd; border-radius: 5px; text-align: center;">
🎯 <strong>成功の鍵</strong>: 人間が主導権を持ち、AIを強力なツールとして活用
</div>

</div>

---

## Vibe Codingのゴールデンルール

### 成功するための12の原則

<div style="font-size: 0.7em;">

<div style="display: flex; gap: 20px;">
<div style="flex: 1;">

**🎯 コミュニケーション**
1. 要求を明確に伝える
2. AIの出力を必ず検証
3. AIを監督下で活用

**🤝 チームワーク**
4. 事前にチーム調整
5. AI使用を共有
6. プロンプトを再利用

</div>
<div style="flex: 1;">

**💻 品質管理**
7. AI変更を分離
8. 全コードレビュー
9. 理解してマージ
10. ドキュメント重視

**🔄 継続的改善**
11. 能力を拡張
12. 定期的な振り返り

</div>
</div>

<div style="text-align: center; margin-top: 20px; padding: 10px; background-color: #e3f2fd; border-radius: 5px;">
💡 <strong>AIはツール。主導権は常に人間が持つ</strong>
</div>

</div>

---

## 人間の専門性が不可欠な領域

### 創造性と判断力が求められる本質的な仕事

<div style="font-size: 0.7em;">

<div style="display: flex; gap: 20px;">
<div style="flex: 1;">

```javascript
// AIが生成する表層的な実装
function processPayment(amount) {
  return chargeCard(amount);
}

// 人間が考慮する深層的な実装
function processPayment(amount, context) {
  // ビジネスルールの複雑な組み合わせ
  // 規制要件とコンプライアンス
  // 例外処理の優先順位付け
  // 監査証跡の設計
}
```

</div>
<div style="flex: 1; padding: 15px; background-color: #fff3cd; border-radius: 5px;">

**人間の創造性が生み出す価値**

人間だけが持つ能力は、単なるコード生成を超えた次元に存在します。

まだ形になっていない問題を予見し、複雑な制約条件の中で最適解を見出し、技術とビジネスの境界を越えて価値を創造する。

これこそが、人間にしかできない創造的な仕事なのです。

</div>
</div>

</div>

---

## プロンプトエンジニアリング基本原則①

### 🎯 具体性と明確性の追求

<div style="font-size: 0.75em;">

<div style="text-align: center; margin-top: 20px;">

曖昧な指示は曖昧な結果を生む

<div style="display: flex; gap: 20px; margin-top: 20px;">
<div style="flex: 1; background-color: #ffebee; padding: 15px; border-radius: 8px;">

<strong>❌ 悪い例</strong>

```text
「JSで書いて」
「DB処理を作って」
「ユーザー登録機能」
「速くして」
```

</div>
<div style="flex: 1; background-color: #e8f5e9; padding: 15px; border-radius: 8px;">

<strong>✅ 良い例</strong>

```text
「TS 5.0、React 18で」
「PostgreSQL 15トランザクション」
「メール認証付きAPI」
「100ms以下に最適化」
```

</div>
</div>

<div style="margin-top: 20px; padding: 10px; background-color: #e3f2fd; border-radius: 8px;">
💡 <strong>ポイント</strong>: バージョン明記、制約条件の列挙、期待する出力形式
</div>

</div>

</div>

---

## プロンプトエンジニアリング基本原則②

### 🔄 反復的改善プロセス

<div style="font-size: 0.75em;">

<div style="text-align: center; margin-top: 20px;">

一発で完璧なプロンプトは書けない

<div style="margin-top: 20px; padding: 15px; background-color: #f0f8ff; border-radius: 8px;">

```text
初回プロンプト → 出力評価 → 不足点特定
       ↓                    ↓
  再生成・比較   ←   プロンプト改良
```

</div>

<div style="display: flex; gap: 15px; margin-top: 20px;">
<div style="flex: 1; background-color: #fff3e0; padding: 12px; border-radius: 8px;">

<strong>改善ポイント</strong>

曖昧な表現を具体化し、必要な前提条件を追加。除外すべき事項も明確に記載する。

</div>
<div style="flex: 1; background-color: #f0fff0; padding: 12px; border-radius: 8px;">

<strong>成功パターンの蓄積</strong>

効果的なプロンプトを保存してチームで共有。再利用可能なライブラリを構築する。

</div>
</div>

</div>

</div>

---

## プロンプトエンジニアリング基本原則③

### 📚 適切なコンテキスト提供

<div style="font-size: 0.75em;">

<div style="text-align: center; margin-top: 15px;">

背景情報がAIの理解度を決める

<div style="display: flex; gap: 10px; margin-top: 20px;">
<div style="flex: 1; background-color: #e8f5e9; padding: 12px; border-radius: 8px;">

<strong>🏗️ システム</strong>
- アーキテクチャ
- API仕様
- DBスキーマ
- コード構造

</div>
<div style="flex: 1; background-color: #fff0f5; padding: 12px; border-radius: 8px;">

<strong>💼 ビジネス</strong>
- ユーザーストーリー
- 受入条件
- 非機能要件
- 制約事項

</div>
<div style="flex: 1; background-color: #f0f8ff; padding: 12px; border-radius: 8px;">

<strong>⚙️ 技術</strong>
- 技術スタック
- 性能目標
- セキュリティ
- 保守性

</div>
</div>

<div style="margin-top: 20px; padding: 10px; background-color: #e3f2fd; border-radius: 8px;">
💡 適切なコンテキスト = より精度の高い出力
</div>

</div>

</div>

---

## プロンプトエンジニアリング基本原則④

### 🎯 期待する出力形式の明確化

<div style="font-size: 0.75em;">

<div style="text-align: center; margin-top: 20px;">

出力形式を明示して構造化された結果を得る

<div style="display: flex; gap: 15px; margin-top: 20px;">
<div style="flex: 1; background-color: #f0f8ff; padding: 12px; border-radius: 8px;">

<strong>📝 フォーマット</strong>
```text
「JSON形式」
「Markdown表」
「箇条書き」
「コード+コメント」
```

</div>
<div style="flex: 1; background-color: #fff0f5; padding: 12px; border-radius: 8px;">

<strong>📊 構造定義</strong>
```text
「3観点評価」
「メリット/デメリット」
「5ステップ手順」
「優先度順」
```

</div>
</div>

<div style="margin-top: 20px; padding: 10px; background-color: #e3f2fd; border-radius: 8px;">
💡 明確な形式指定 = 一貫性のある使いやすい出力
</div>

</div>

</div>

---

## 高度なプロンプトテクニック①

### 🧠 Chain-of-Thought (CoT) - 段階的思考の誘導

<div style="font-size: 0.7em;">

<div style="background-color: #fff0f5; padding: 12px; border-radius: 8px; margin-top: 10px;">

```text
「以下のステップで考えてください：
1. 要件を整理
2. 制約条件を確認
3. 解決策を検討
4. 実装方針を決定
5. コードを生成」
```

</div>

<div style="margin-top: 15px;">
<strong>複雑な問題を段階的に分解してAIに考えさせる</strong>
</div>

<div style="margin-top: 15px; padding: 10px; background-color: #e3f2fd; border-radius: 8px;">
<strong>効果的な場面</strong>: アルゴリズム設計、アーキテクチャ検討、デバッグ作業<br>
<strong>メリット</strong>: 推論過程の可視化、論理的飛躍の防止、修正ポイントの明確化
</div>

</div>

---

## 高度なプロンプトテクニック②

### 📚 Few-Shot Learning - 例示による学習

<div style="font-size: 0.7em;">

<div style="background-color: #f5f0ff; padding: 12px; border-radius: 8px; margin-top: 10px;">

```text
「以下の変換例を参考に：
例1: getUserName() → get_user_name
例2: calcTotal() → calc_total
例3: validateEmail() → validate_email
同様に変換：checkUserPermission() → ?"
```

</div>

<div style="margin-top: 15px;">
<strong>具体例を示してパターンを学習させる</strong>
</div>

<div style="margin-top: 15px; padding: 10px; background-color: #e3f2fd; border-radius: 8px;">
<strong>使用例</strong>: コーディング規約、データ変換パターン、プロジェクト慣習<br>
<strong>ポイント</strong>: 3-5例が最適、エッジケースを含める、失敗例との対比も効果的
</div>

</div>

---

## 高度なプロンプトテクニック③

### 🎭 Role Prompting - 専門家視点の活用

<div style="font-size: 0.7em;">

<div style="background-color: #f0f8ff; padding: 12px; border-radius: 8px; margin-top: 10px;">

```text
「あなたはセキュリティエンジニアです。
OWASP Top 10を考慮して、
このAPIの脆弱性を指摘してください。」
```

</div>

<div style="margin-top: 15px;">
<strong>AIに特定の役割を与えて専門的な視点を引き出す</strong>
</div>

<div style="margin-top: 15px; padding: 10px; background-color: #e3f2fd; border-radius: 8px;">
<strong>アーキテクト</strong>: 全体設計、スケーラビリティ検討<br>
<strong>レビュアー</strong>: 品質チェック、改善提案<br>
<strong>メンター</strong>: 教育的説明、ベストプラクティス
</div>

</div>

---

## 高度なプロンプトテクニック④

### 🔧 メタプロンプティング - 出力構造の制御

<div style="font-size: 0.7em;">

<div style="background-color: #fff3e0; padding: 10px; border-radius: 8px; margin-top: 10px; font-size: 0.85em;">

```text
「以下の形式で出力してください：

## 概要
[簡潔な説明]

## 実装
[実装コード]

## テスト
[テストケース]

## 注意事項
[制限事項]」
```

</div>

<div style="margin-top: 15px;">
<strong>テンプレートで出力を完全にコントロール</strong>
</div>

<div style="margin-top: 15px; padding: 10px; background-color: #e3f2fd; border-radius: 8px;">
<strong>活用場面</strong>: ドキュメント生成、レビュー結果、仕様書作成<br>
<strong>形式例</strong>: Markdown、JSON/XML、カスタムテンプレート
</div>

</div>

---

## 高度なプロンプトテクニック⑤

### 🔄 Progressive Refinement - 反復的詳細化

<div style="font-size: 0.7em;">

<div style="background-color: #f0fff0; padding: 12px; border-radius: 8px; margin-top: 10px;">

```text
Step1: 「認証システムの概要設計」
  ↓
Step2: 「JWT認証フローの詳細」
  ↓
Step3: 「リフレッシュトークン実装」
```

</div>

<div style="margin-top: 15px;">
<strong>段階的に詳細度を上げて精度を高める</strong>
</div>

<div style="margin-top: 15px; padding: 10px; background-color: #e3f2fd; border-radius: 8px;">
<strong>メリット</strong>: 段階的理解、修正が容易、文脈の保持<br>
<strong>適用例</strong>: 複雑な設計、大規模リファクタリング、新技術の学習
</div>

</div>

---

## 実装から本番環境への移行

### デモ品質の罠を避ける

<div style="font-size: 0.7em;">

<div style="display: flex; gap: 20px;">
<div style="flex: 1;">

**❌ デモ品質**
```javascript
function save(data) {
  localStorage.set('user',
    JSON.stringify(data));
}
```
動作確認優先

</div>
<div style="flex: 1;">

**✅ 本番品質**
```javascript
function save(data) {
  // バリデーション
  // エラー処理
  // セキュリティ
  // リトライ
}
```
信頼性確保

</div>
</div>

<div style="margin-top: 20px; padding: 15px; background-color: #e3f2fd; border-radius: 8px;">

**🛡️ 品質保証の3つの柱**

<div style="display: flex; gap: 10px; margin-top: 10px;">
<div style="flex: 1;">
• <strong>レビュー</strong>: AI生成コードの精査
</div>
<div style="flex: 1;">
• <strong>テスト</strong>: 網羅的なカバレッジ
</div>
<div style="flex: 1;">
• <strong>監査</strong>: セキュリティ確認
</div>
</div>

</div>

</div>

---

## 組織的な導入戦略①

### 👥 チームでの実践

<div style="font-size: 0.75em;">

<div style="background-color: #e8f5e9; padding: 15px; border-radius: 8px; margin-top: 15px;">

**事前調整が成功の鍵**

チームでAIを活用する際、最も重要なのは事前の調整です。まずAI生成コードの品質基準を明確にし、レビューポイントを統一することで、チーム全体の認識を揃えます。

効果的なプロンプトパターンを共有し、チーム共通のテンプレートを作成することで、個人の属人的なスキルに依存しない仕組みを構築できます。

さらに、プロンプトのリポジトリ化やベストプラクティスの文書化を通じて、成功事例と失敗事例を蓄積し、組織全体の学習を促進します。

</div>

<div style="margin-top: 15px; padding: 12px; background-color: #e3f2fd; border-radius: 8px; text-align: center;">
💡 <strong>AI活用のナレッジを組織の競争力に変える</strong>
</div>

</div>

---

## 組織的な導入戦略②

### ⚠️ リスク管理

<div style="font-size: 0.75em;">

<div style="background-color: #ffebee; padding: 15px; border-radius: 8px; margin-top: 15px;">

**AIに振り回されないための対策**

AI活用の裏側には様々なリスクが潜んでいます。まず懸念されるのは、AIへの過度な依存による開発者のスキル退化です。これを防ぐため、定期的な手動コーディング練習や「AIデトックス」期間の設定が必要です。

基礎スキルの維持・向上プログラムを実施し、コードリーディング能力を継続的に訓練することで、AIを使いこなす側の立場を保持できます。

セキュリティ面では、AI生成コードの脆弱性チェック体制を整備し、著作権・ライセンスへの配慮、機密情報の取り扱いガイドラインを策定することが不可欠です。

</div>

<div style="margin-top: 15px; padding: 12px; background-color: #e3f2fd; border-radius: 8px; text-align: center;">
🎯 <strong>着実な文化醸成とリスク管理の両立</strong>
</div>

</div>

---

## AIコーディングツールの選択

### まだまだ戦国時代

<div style="font-size: 0.85em;">

<div style="text-align: center; margin-top: 15px;">

**🤖 AIツール市場は群雄割拠**

<div style="margin: 10px 0;">
GitHub Copilot、Claude、ChatGPT、Cursor、Gemini、Codeium...
</div>

毎月新しいツールが登場する戦国時代

</div>

<div style="margin-top: 20px; padding: 15px; background-color: #e3f2fd; border-radius: 8px;">

<div style="font-size: 1.05em; margin-bottom: 10px;">
💡 <strong>ツールは手段。重要なのは「どう使いこなすか」</strong>
</div>

<strong>成功の鍵：</strong>
明確な意図 × 適切な判断 × 創造性

</div>

<div style="margin-top: 15px; font-size: 0.85em; color: #666; text-align: center;">
変わらないのは、それを使いこなす人間のスキル
</div>

</div>

---

## 将来展望と自律型エージェント

### 開発者の役割のさらなる進化

<div style="font-size: 0.7em;">

<div style="display: flex; gap: 15px;">
<div style="flex: 1;">

**🤖 自律型AIエージェント**
- 完全な開発ワークフローを独立実行
- 要件からデプロイまで自動化
- 継続的な最適化と保守

**🔮 今後の技術進化**
- マルチモーダルAI
- リアルタイムペアプログラミング
- 自律的バグ修正

</div>
<div style="flex: 1; background-color: #e3f2fd; padding: 10px; border-radius: 5px;">

**🎯 開発者の新たな役割**

**アーキテクチャ監督**
- システム全体の方向性
- 技術選定と最適化

**戦略的意思決定**
- ビジネス要件の解釈
- 長期的視点での設計

</div>
</div>

<div style="margin-top: 10px; padding: 8px; background-color: #fff3cd; border-radius: 5px; text-align: center;">
💡 「今日知られているプログラミングの終わり」だが「プログラミング自体の終わり」ではない
</div>

</div>

---

## 核心的なメッセージ

### AIは開発者を置き換えるのではなく、能力を増幅する

<div style="font-size: 0.75em;">

**🎯 Vibe Codingの本質**
- 「HOW」から「WHAT」への根本的転換
- 自然言語対話による開発パラダイム
- 創造性と生産性の新たなバランス

**🔑 成功の鍵**

<div style="display: flex; gap: 15px; margin-top: 10px;">
<div style="flex: 1; background-color: #e8f5e9; padding: 10px; border-radius: 5px;">

**役割分担ルール**
- AI: ルーチンワーク
- 人間: 創造的業務

</div>
<div style="flex: 1; background-color: #e3f2fd; padding: 10px; border-radius: 5px;">

**品質と速度**
- 迅速なプロトタイピング
- 高品質な本番実装

</div>
</div>

<div style="margin-top: 15px; padding: 10px; background-color: #fff3cd; border-radius: 5px; text-align: center;">
💡 <strong>AIの速度と人間の判断力を組み合わせ、
迅速な開発と高品質なソフトウェアを両立</strong>
</div>

</div>


---

## AIで浮いた時間で何をする？

### 生産性向上の先にある3つの道

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 15px;">
<div style="flex: 1; background-color: #e8f5e9; padding: 12px; border-radius: 8px;">

**📚 学習への投資**

AIをより使いこなすために
自分の専門性を深める

- 隣接領域の技術習得
- AIの仕組みを理解
- 体系的な知識の構築

</div>
<div style="flex: 1; background-color: #e3f2fd; padding: 12px; border-radius: 8px;">

**🤔 問いの深化**

作るから考えるへ
本質的な価値を追求

- ユーザーの真の課題
- なぜ作るのかを問う
- ビルドトラップ回避

</div>
<div style="flex: 1; background-color: #fff3e0; padding: 12px; border-radius: 8px;">

**🧘 余白の活用**

加速する時代だからこそ
立ち止まって考える

- 思考の速度を下げる
- クリエイティブな発想
- 心と感性を磨く

</div>
</div>

<div style="margin-top: 15px; padding: 10px; background-color: #f0f0f0; border-radius: 5px; text-align: center;">
💡 <strong>AIが作り出した時間を、人間としての成長と創造性に再投資する</strong>
</div>

</div>

---

## 今日から始めるVibe Coding

### 実践的なアクションプラン

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 15px;">
<div style="flex: 1;">

**🚀 今すぐできること**
- 小さなタスクでAIツールを試す
- 生成されたコードを必ず理解する
- プロンプトのパターンを記録する

**📅 次のステップ**
- 日常業務の一部にAI活用を組み込む
- チームメンバーと経験を共有
- 効果的なプロンプトを蓄積

</div>
<div style="flex: 1;">

**🎯 将来の目標**
- 開発速度の向上を実感
- ベストプラクティスの確立
- より創造的な仕事への時間配分

**💡 成功の秘訣**
- 完璧を求めすぎない
- 失敗から学ぶ姿勢
- チームで知見を共有

</div>
</div>

<div style="margin-top: 10px; padding: 8px; background-color: #fff3cd; border-radius: 5px; text-align: center;">
⚡ 小さな一歩から始めて、徐々に活用範囲を広げていく
</div>

</div>

---

## まとめ

### Vibe Coding時代の開発者へ

<div style="font-size: 0.75em;">

**🌟 今日学んだこと**

<div style="display: flex; gap: 15px;">
<div style="flex: 1; background-color: #f0f8ff; padding: 10px; border-radius: 5px;">

**パラダイムシフト**
- HOWからWHATへ
- 意図でプログラミング
- 創造性への集中

</div>
<div style="flex: 1; background-color: #f0fff0; padding: 10px; border-radius: 5px;">

**役割分担**
- AI: 定型作業
- 人間: 創造的作業
- 適切な役割分担

</div>
<div style="flex: 1; background-color: #fff0f5; padding: 10px; border-radius: 5px;">

**成功の鍵**
- 理解してから使う
- 主導権を保つ
- 継続的な学習

</div>
</div>

<div style="margin-top: 15px; padding: 10px; background-color: #e3f2fd; border-radius: 5px; text-align: center;">
💡 <strong>AIはツール。主役は今もこれからも、人間の開発者</strong>
</div>

</div>

---



---

## 参考資料

### 深く学ぶためのリソース

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 15px;">
<div style="flex: 1;">

**📚 必読記事・論文**
- Andrej Karpathy - "Vibe Coding"
- Google Chrome UX - "The 70% Problem"
- mizchi - "AIコーディングの現実と未来"

**🔧 実践ツール**
- GitHub Copilot Workspace
- Claude Projects
- Cursor IDE
- Continue.dev (OSS)

</div>
<div style="flex: 1;">

**🌐 コミュニティ**
- r/VibeCoding (Reddit)
- AI-Assisted Development Japan
- Prompt Engineering Guide

**📖 書籍**
- 『バイブコーディング』
- 『AI時代のソフトウェア開発』
- 『プロンプトエンジニアリング』

</div>
</div>

<div style="margin-top: 15px; padding: 10px; background-color: #e3f2fd; border-radius: 5px; text-align: center;">
💡 継続的な学習とコミュニティでの情報交換が成功の鍵
</div>

</div>

---

<!-- 
_backgroundColor: #0a1929
_color: white
_class: title dark
-->

![bg](../../assets/images/3shake-background-full.png)



<!-- タイトルページ左上に大きなロゴを表示 -->
<div style="position: absolute !important; top: 5px !important; left: 5px !important; z-index: 9999 !important; margin: 0 !important; padding: 0 !important;">
  <img src="../../assets/images/3shake-logo.png" style="width: 240px !important; height: auto !important; display: block !important;">
</div>

<div style="text-align: center; margin-top: 200px;">

# ありがとう<span class="highlight-yellow">ございました</span>

### ご質問・ご相談はお気軽にお問い合わせください

@nwiizo | https://3-shake.com
</div>
