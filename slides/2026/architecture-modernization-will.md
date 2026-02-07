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
  .highlight-red {
    color: rgb(224, 64, 64);
  }
  section:not(.title)::before {
    content: "";
    position: absolute;
    left: 15px;
    bottom: 15px;
    width: 60px !important;
    height: 60px !important;
    background-image: var(--logo-url);
    background-size: contain;
    background-repeat: no-repeat;
    background-position: center;
    opacity: 0.9;
    z-index: 100;
  }
  section[data-background-color="dark"]::before {
    filter: brightness(0) invert(1);
  }
  section.title::after {
    display: none;
  }
  .title-logo {
    position: fixed;
    top: 5px;
    left: 5px;
    width: 240px !important;
    height: auto;
    z-index: 9999;
  }
  img[src*="3shake-logo.png"] {
    max-width: 240px !important;
    width: 240px !important;
  }
  .title h1 {
    font-size: 2.4em !important;
    margin-bottom: 0.1em !important;
  }
  .title h3 {
    font-size: 1.1em !important;
    margin-top: 0.1em !important;
  }
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
  section h2 {
    font-size: 1.8em !important;
    margin-top: -25px !important;
    padding-top: 0 !important;
    margin-bottom: 10px !important;
    color: black !important;
    border-bottom: 1px solid #dadce0 !important;
  }
  section > *:not(h2):not(header):not(footer) {
    margin-top: 1.2em !important;
  }
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
  mermaid {
    display: block;
    margin: 0 auto;
  }
  .reference-right {
    font-size: 0.4em;
    text-align: right;
    margin-right: 20px;
    margin-top: -15px;
    display: block;
    width: 30%;
    margin-left: 70%;
  }
  table {
    font-size: 0.65em !important;
  }
  table th, table td {
    padding: 4px 6px !important;
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

# <span style="font-size: 0.9em;">意志を実装する</br><span class="highlight-yellow">アーキテクチャ</span></br><span class="highlight-yellow">モダナイゼーション</span></span>

### レガシーを競争優位性へ転換する</br>ソシオテクニカルなアプローチ

</div>

<div class="author-info" style="text-align: left; padding-left: 0; text-indent: 0;">
2026/02/19 Developers Summit 2026</br>
@nwiizo 40min（12:40〜13:20）
</div>

---

<!-- _backgroundColor: white -->

![bg left:30% fit](../../assets/images/nwiizo_icon.jpg)
## nwiizo

<div class="info-box">
株式会社スリーシェイクで</br>プロのソフトウェアエンジニアをやっているものです</br>
格闘技、読書、グラビアが趣味でよく本を紹介してます</br>
</div>

<p style="margin-top: 30px !important;">人生を通して"<strong>運動、睡眠、読書</strong>"をちゃんとやりたい</p>

---

## about 3-shake

<div style="text-align: center; margin-top: 30px;">
  <img src="../../assets/images/3shake-about.png" alt="3-shake about" style="width: 80%; margin-top: 10px;">
</div>

---

## Sreakeのお仕事

<div style="font-size: 0.68em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

**クラウドネイティブなアプローチで、お客様の事業をより安全に、競争力のあるサービスへ**

</div>

### 提供サービス

<div style="display: flex; gap: 8px; flex-wrap: wrap; align-items: center;">
<div style="flex: 1; min-width: 180px; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

**SRE/DevOps支援**

- Kubernetes構築・運用
- クラウドネイティブ化推進
- Observability導入

</div>
<div style="flex: 1; min-width: 180px; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

**アーキテクチャモダナイゼーション**

- 現状分析・戦略策定
- 段階的な移行支援
- 内製化・伴走支援

</div>
<div style="flex: 1; min-width: 180px; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

**データ活用支援**

- データ基盤構築
- BigQuery/Snowflake
- 分析基盤最適化

</div>
</div>

<div style="margin-top: 12px; padding: 12px; background-color: #e0e0e0; border-radius: 8px; text-align: center;">
<span style="color: #e65100; font-weight: bold; font-size: 1.1em;">ご依頼・ご相談お待ちしております</span></br>
<span style="font-size: 0.9em;">https://sreake.com/</span>
</div>

</div>

---

## We are Hiring!!

<div style="text-align: center; margin-top: 30px;">

3-shakeは一緒にSRE界隈を盛り上げてくれる**仲間を大募集中**です！
Mobility、FinTech、通信など大規模SREを存分に経験できます
是非、カジュアル面談しましょう！

  <img src="../../assets/images/3shake-hiring.png" alt="3-shake hiring" style="width: 80%; margin-top: 10px;">
</div>

---

## 本日のアジェンダ

1. AI時代に私たちの仕事はどう変わるのか
2. ソフトウェアと組織を同時に設計する
3. ビジネスを可視化する協働的手法
4. ドメイン駆動設計とチーム設計の統合
5. レガシーを競争優位性へ転換する
6. 意志を実装するということ

---

## この発表で解決できること

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 20px; margin-top: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**こんな悩みを持っていませんか？**

レガシーシステムの改善が進まない。技術的な改善と組織の問題が絡み合っている。AIでコードは書けるが、**設計の判断ができない**。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**この発表で持ち帰れるもの**

アーキテクチャモダナイゼーションの全体像、ソシオテクニカルな視点での設計アプローチ、そして**協働的な設計手法**（イベントストーミング等）。

</div>
</div>

<div style="margin-top: 15px; padding: 12px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>目標：「どんな未来を描くか」を考えるための武器を持ち帰る</strong>
</div>

</div>

---

## この発表の限界

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

本日紹介する考え方は、**すべての現場に当てはまるわけではありません。**

</div>

<div style="display: flex; gap: 20px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**文脈によって最適解は変わる**

スタートアップかエンタープライズか、B2CかB2Bか、規制産業かどうか——これらによって正解は異なる。「うちには当てはまらない」と思う部分が必ずある。それは正しい反応。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**想定される反論に先に答えておく**

「そんな余裕はない」→ 余裕がないからこそ構造を見直す価値がある。「うちは小さいから関係ない」→ 小さいうちに考えておく方が楽。「理想論だ」→ 理想を知った上で、現実とすり合わせるのが意志の実装。

</div>
</div>

<div style="margin-top: 15px; padding: 12px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>正解は存在しない。だが「考え方の型」は存在する。それを持ち帰ってほしい</strong>
</div>

</div>

---

## 始める前に

<div style="font-size: 0.8em;">

<div style="background-color: #f5f5f5; padding: 25px; border-radius: 8px; margin-top: 30px;">

<div style="text-align: center; font-size: 1.1em; line-height: 1.8;">

**事情でレガシーシステムと呼ばれ**
**敵にされる全てのシステムに敬意を表します。**

</div>

<div style="margin-top: 20px; font-size: 0.9em; color: #666;">

それらのシステムは、かつて誰かが全力で作り、ビジネスを支え、価値を生み出してきた。

**敬意の次に必要なのは責任。** 今日書くコードも、いつかレガシーになる。その時に「あの時代の制約の中で、よくここまで作った」と言われるか、「なぜこんな設計にしたのか」と言われるか。それを決めるのは今日の選択。

</div>

</div>

</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: transition
-->

<div style="display: flex; flex-direction: column; justify-content: center; align-items: center; height: 80%; text-align: center;">

<div style="font-size: 1.5em; font-weight: bold;">

# 1. AI時代に私たちの仕事は</br>どう変わるのか

</div>

</div>

---

## AIがコードを生成する時代

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 20px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**AIにできること**

ボイラープレートの生成、パターンに従った実装、既知の問題の解決。**再現可能な作業**はAIが得意とする領域。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**AIにできないこと（まだ）**

「何を作るべきか」の判断、ビジネス固有の文脈理解、組織や人の問題への対処。**正解のない問い**には人間が必要。

</div>
</div>

<div style="margin-top: 20px; padding: 12px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>「書けるコード」の価値はゼロに近づく。「書くべきコード」を見極める力だけが残る。</strong>
</div>

</div>

---

## なぜ「決める」ことが難しいのか

<div style="font-size: 0.7em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

**「人間が決めるべき」は正論。だが現場では決められていない。**

</div>

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**組織構造の問題**

プロダクトオーナーは「要件をまとめる人」、エンジニアは「言われたことを作る人」。**「なぜ作るか」を問う役割が不在**。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**心理的な問題**

決めると責任が発生する。「正解がわからない」から決められない。**上位者の承認を待つ癖**がついている。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**情報の問題**

ビジネス側と技術側で情報が分断。「何が可能か」と「何が必要か」が噛み合わない。**判断材料が揃う頃には手遅れ**。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>AIがコードを書いても、この構造的問題は解決しない。むしろ露わになる。</strong>
</div>

</div>

---

## AIは責任を取ることができない

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**AIは問いを立てることもできる。答えを出すこともできる。**

でも、その結果に**責任を取ることはできない**。

</div>

<div style="display: flex; gap: 20px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**AIができること**

選択肢を提示し、トレードオフを分析し、過去の事例を参照する。**判断材料を揃えること**はできる。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**人間にしかできないこと**

「これでいく」と決め、結果を引き受け、ステークホルダーに説明する。**最終的な責任を負うこと**は人間だけの仕事。

</div>
</div>

<div style="margin-top: 15px; padding: 12px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>AIは答えを出せる。だが責任は取れない。</strong>
</div>

</div>

---

## 生成AIとの付き合い方

<div style="font-size: 0.7em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

<strong>生成AIは「鏡」であり「拡声器」である</strong>

あなたの理解が深ければ、AIは優れた出力を返す。理解が浅ければ、もっともらしいが間違った出力を返す。AIは理解の深さを増幅する装置であって、理解を代替する装置ではない。

</div>

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**うまくいくパターン**

ドメイン知識を持つ人間が、AIに具体的な文脈を与えて壁打ちする。「注文処理のBounded Contextで、在庫引当のイベントが先か決済確認が先か」——**問いの解像度が高いとき**、AIは強力な思考パートナーになる。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**危険なパターン**

理解が曖昧なまま「いい感じにして」と丸投げする。出力はそれっぽく見えるが、ドメインの文脈を欠いている。**もっともらしさと正しさは別物**。レビューできる知識がなければ、間違いに気づけない。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>AIは鏡だ。あなたの理解が浅ければ、もっともらしく浅い答えが返る</strong>
</div>

</div>

---

## AIエージェントという新しい存在

<div style="font-size: 0.7em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

<strong>AIは「対話相手」から「自律的な実行者」へ変わりつつある</strong>

コード生成、テスト実行、デプロイ、障害対応——AIエージェントが自律的にタスクを遂行する時代が来ている。ただ、これは新しい話ではない。CI/CDパイプラインも、自動スケーリングも、広い意味では「自律的な実行者」だった。

</div>

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**AIエージェントが変えること**

タスクの粒度が変わる。「関数を書いて」から「この機能を実装して」へ。**委任の単位が大きくなる**ほど、委任する側に求められる判断力も高くなる。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**AIエージェントが変えないこと**

「何を作るべきか」の判断、組織間の合意形成、ビジネスの文脈理解、そして**失敗したときに顧客の前に立つ覚悟**。これらは依然として人間の領域。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>エージェントが賢くなるほど、委任する側の判断力が問われる</strong>
</div>

</div>

---

## AIエージェントとの信頼関係を設計する

<div style="font-size: 0.7em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

<strong>AIエージェントは「わかったふりをする優秀な新人」に似ている</strong>

新人に「全権委任」はしない。小さなタスクから任せ、成果を確認し、徐々に委任範囲を広げる。AIエージェントも同じ。自信満々で間違えるから、確認の仕組みが余計に重要になる。

</div>

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**委ねてよいもの**

定型的なコード生成、テストの実行と結果の解析、ドキュメントの下書き、既知パターンの適用。**正解が検証可能な領域**。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**委ねてはいけないもの**

ドメイン境界の決定、チーム構造の設計、技術的負債をどこまで許容するかの判断、ステークホルダーへの説明。**正解が文脈に依存する領域**。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>新人に全権委任しないのと同じ。信頼は実績で積む。AIも例外ではない。</strong>
</div>

</div>

---

## AIが変えるもの、変えないもの

<div style="font-size: 0.7em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

**ここで一歩引いて考えたい。AIの進化で、本当に変わるものは何か。**

</div>

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**変わるもの**

実装の速度、コード生成のコスト、情報収集の効率、プロトタイピングの手軽さ。要するに**「How」のコストが劇的に下がる**。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**変わらないもの**

コンウェイの法則、認知負荷の限界、組織の慣性、人間の感情、信頼の構築に時間がかかること。要するに**「人間の性質」は変わらない**。

</div>
</div>

だからこそ、ソシオテクニカルなアプローチの価値は**AIが発達するほど高まる**。技術側のコストが下がれば下がるほど、ボトルネックは「人間の調整コスト」に移動する。AIエージェントが100倍速くコードを書いても、チーム間の合意形成が3ヶ月かかるなら、全体のリードタイムは3ヶ月のまま。

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">AIは「How」を加速する。だが「What」と「Why」は加速しない。ここが今日の出発点。</span>
</div>

</div>

---

## 歴史は繰り返す——「作れる」と「運用できる」

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

<strong>新しい技術が出るたびに、同じサイクルが回る</strong>

「魔法のような技術」に触れる → 「これなら自分たちで作れる」 → 内製する → 運用が回らなくなる → 結局外部に移行する。クラウド黎明期にもコンテナブームでも、そして今のAIでも、同じことが起きている。

</div>

技術の進歩は「作る」コストを劇的に下げる。AIが書いたコードは動く。だが、そのコードを3年間運用し、法改正に追従し、セキュリティパッチを当て続け、担当者が退職しても回し続けるコストは下がらない。<strong>ソフトウェアの価値の大部分は初期構築ではなく、変化への継続的適応にある。</strong>

</div>

---

## なぜ同じ罠に繰り返しはまるのか

<div style="font-size: 0.75em;">

歴史的教訓を持たない世代が、新技術に触れるたびに同じ興奮を覚える。それ自体は健全な反応だが、「作れた」の興奮と「運用し続けられる」の地味な現実との落差に、多くのプロジェクトが沈む。

組織・法制度・業務フローは常に変化するため、<strong>システムは「完成」しない。作った瞬間から陳腐化が始まる。</strong>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>「作れること」と「運用できること」の間には、技術では埋まらない溝がある</strong>
</div>

</div>

---

## エンジニアの仕事の質的シフト

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 15px; font-size: 0.9em;">

AIが「作る」コストを下げ、歴史が「運用は別物」だと教えている。ならばエンジニアの仕事はどこへ向かうのか。

</div>

<div style="display: flex; gap: 30px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 20px; border-radius: 8px;">

**以前**

コードを書き、技術を選び、機能を実装し、バグを直す。**実装そのものが仕事**だった。

</div>
<div style="flex: 1; background-color: #e0e0e0; padding: 20px; border-radius: 8px;">

**これから**

設計を決め、価値を定義し、未来を描き、システムを進化させる。**意思決定と方向性の定義**が仕事になる。

</div>
</div>

<div style="margin-top: 15px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>「作れる人」は余る。「何を作り、何を作らないか決められる人」が足りなくなる</strong>
</div>

</div>

---

## 意志を実装するとは

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

技術の選択、アーキテクチャの設計、プロダクトがもたらす価値への責任を負うこと。

</div>

### 具体的に何を「意志」と呼ぶのか

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**ビジネスへの意志**

どんな価値を届けるか、誰の問題を解決するか、どの市場で勝つか。**プロダクトの方向性**を決める。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**アーキテクチャへの意志**

どこに境界を引くか、何を内製し何を調達するか、どこで技術的負債を許容するか。**システムの形**を決める。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**組織への意志**

チームをどう分けるか、責任をどう分配するか、どんな文化を醸成するか。**人の配置**を決める。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">これらを「誰かに決めてもらう」のではなく「自ら決める」こと。それが意志の実装。</span>
</div>

</div>

---

## 技術が楽になると、人間が見える

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**開発コストの低下がもたらす逆説**

コーディングが楽になる → 開発のボトルネックが移動する → 「人間の問題」が前面に出てくる

</div>

<div style="display: flex; gap: 20px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**以前は隠れていた**

「技術的負債のせいで遅い」「レガシーが複雑すぎる」「リソースが足りない」——**技術的な言い訳**ができた。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**今は露わになる**

「何を作るか合意できない」「チーム間の調整が進まない」「誰も決められない」——**人間の問題**が見える。

</div>
</div>

<div style="margin-top: 15px; padding: 12px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>コードが楽に書ける時代、言い訳はもう使えない。本当のボトルネックと向き合え。</strong>
</div>

</div>

---

## これは「面倒」ではなく「本題」

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**制約理論の考え方**

ボトルネックを1つ解消すると、次のボトルネックが見えてくる。コーディングがボトルネックでなくなった今、次は「人間の調整」がボトルネックになる。

</div>

### 向き合うべき「人間の問題」

<div style="display: flex; gap: 20px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

- 何を作るべきかの合意形成
- チーム間の責任境界の設計
- 組織文化の変革

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

- ステークホルダーとの期待値調整
- 知識の共有と継承
- 変化への抵抗の克服

</div>
</div>

<div style="margin-top: 15px;">

これらは「面倒な雑務」ではない。**ソフトウェア開発の本質的な難しさ**であり、AIが発達してもなくならない。むしろ、これこそが人間のエンジニアが取り組むべき本題。

</div>

</div>

---

## では、どう向き合えばいいのか？

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**「人間の問題に向き合う」とは、具体的に何をすることか？**

抽象的な理解だけでは動けない。私たちには実践的なフレームワークと手法が必要。

</div>

<div style="display: flex; gap: 20px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**必要なもの**

- ソフトウェアと組織を同時に設計する方法
- ビジネスを協働で可視化する手法
- チームの自律性を確保するパターン

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**これから紹介する内容**

- ソシオテクニカルアーキテクチャ
- イベントストーミング
- DDD × Team Topologies

</div>
</div>

<div style="margin-top: 15px; padding: 12px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>私がこれから紹介するのは、その「面倒な本題」に向き合うための道具。</strong>
</div>

</div>

---

## 「Architecture Modernization」という書籍

<div style="display: flex; gap: 40px; align-items: center;">
<div style="width: 35%;">
<img src="../../assets/images/2025/forkwell-library-pek/book_architecture_modernization.png" alt="Architecture Modernization" style="width: 80%; height: fit-content;">
</div>

<div style="flex: 1; font-size: 0.75em;">

### 書籍の概要

- **著者**: Nick Tune, Jean-Georges Perrin
- レガシーシステムを競争優位性に転換する包括的ガイド
- ソシオテクニカルな視点を中心に据えた実践書

### 本書のアプローチ

1. モダナイゼーションの「Why」を理解する
2. モダンアーキテクチャを「Design」する
3. 変革を「Execute」する

</div>
</div>

---

## 日本語版が出ました

<div style="display: flex; gap: 40px; align-items: center;">
<div style="width: 30%;">
<img src="../../assets/images/2026/アーキテクチャモダナイゼーション.jpg" alt="アーキテクチャモダナイゼーション" style="width: 100%; height: fit-content;">
</div>

<div style="flex: 1; font-size: 0.75em;">

### アーキテクチャモダナイゼーション

*「何度書き直しても、また遅くなる。」* — 本書冒頭より

- **翻訳**: 株式会社スリーシェイク（5名で翻訳しました）

翻訳作業は、扱う用語の範囲が広く、適切な日本語がまだ定まっていない用語も多くて大変でしたが、やりがいのある仕事でした。

<div style="margin-top: 10px; padding: 10px; background-color: #f5f5f5; border-radius: 8px;">

**Developer Summit 2026でサイン会があります！**

</div>


</div>
</div>

---

## 書籍の構成（17章）

<div style="font-size: 0.7em;">

<div style="display: flex; gap: 20px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**Part 1: Why（理解）**
- Ch.1 アーキテクチャモダナイゼーションとは
- Ch.2 旅への準備
- Ch.3 ビジネス目標

**Part 2: Discovery（発見）**
- Ch.4 リスニング＆マッピングツアー
- Ch.5 Wardley Mapping
- Ch.6 プロダクトタクソノミー

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**Part 3: Design（設計）**
- Ch.7 イベントストーミング
- Ch.8-9 ドメイン境界の特定

**Part 4: Organization（組織）**
- Ch.10 戦略的ITポートフォリオ
- Ch.11 Team Topologies

**Part 5: Technical（技術）**
- Ch.12-14 疎結合アーキテクチャ、IDP、Data Mesh

</div>
</div>

**Part 6: Execute（実行）** — Ch.15-17 AMET、戦略とロードマップ、学習

</div>

---

## 現代システムの「三体問題」

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**「技術」「組織」「戦略」の3つが相互に影響し合う**

3つの要素が互いに引力を及ぼし合い、予測不可能な振る舞いをする。どれか1つだけを変えても、残り2つが引き戻す。

</div>

<div style="display: flex; gap: 20px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px; text-align: center;">

**技術**

アーキテクチャ、コード
インフラ、ツール

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px; text-align: center;">

**組織**

チーム構造、文化
スキル、コミュニケーション

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px; text-align: center;">

**戦略**

ビジネス目標、優先度
投資判断、ロードマップ

</div>
</div>

<div style="margin-top: 15px; padding: 12px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">3つの変数に解析解はない。正解を計算するな。小さく動いて観測せよ。</span>
</div>

</div>

---

## なぜモダナイゼーションが必要なのか

<div style="font-size: 0.8em;">

<div style="background-color: #e0e0e0; padding: 20px; border-radius: 8px; margin-bottom: 20px;">

**問い：今のシステムを維持し続けて、5年後も競争できるか？**

</div>

<div style="background-color: #f5f5f5; padding: 20px; border-radius: 8px; margin-bottom: 20px;">

**私がこの問いを突きつけられたのは、翻訳作業の途中だった。**

今のシステムを維持し続けて、5年後も競争できるか？「改善したい」という意志はある。だが改善の速度が競合に追い越される速度に負けたら、意志は何の意味も持たない。

</div>

<div style="margin-top: 20px; padding: 15px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold; font-size: 1.1em;">優秀なエンジニアは時間の42%を技術的負債に奪われている</span></br>
<span style="font-size: 0.8em;">（Stripe調査, 2018）——この数字は6年経った今も改善していない</span>
</div>

</div>

---

## モダナイゼーションの4つの動機

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 15px; flex-wrap: wrap; align-items: center;">
<div style="flex: 1; min-width: 45%; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**競争力の維持**

市場の変化に追従できない = 淘汰される。競合が3日で出す機能を、3ヶ月かけていては勝てない。

</div>
<div style="flex: 1; min-width: 45%; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**技術的負債の返済**

負債は複利で増える。今日の「後で直す」は、来年の「直せない」になる。

</div>
</div>

<div style="display: flex; gap: 15px; flex-wrap: wrap; margin-top: 15px; align-items: center;">
<div style="flex: 1; min-width: 45%; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**開発者体験の向上**

優秀なエンジニアは「成長できない環境」から離れる。レガシーと戦うだけの日々は、誰も望まない。

</div>
<div style="flex: 1; min-width: 45%; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**ビジネスアジリティ**

新機能のリードタイムが長すぎると、市場機会を逃す。「技術的に難しい」が口癖になっていないか。

</div>
</div>

<div style="margin-top: 15px; padding: 12px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>あなたの組織の痛みはどれか。まず自社の入り口に名前をつけることから始めよ</strong>
</div>

</div>

---

## 技術的負債とは何か

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**Ward Cunninghamが1992年に提唱した比喩**

将来の変更を困難にする、過去の技術的決定の蓄積。金融の負債と同様に「利子」がつく。ただし金融の負債と違い、技術的負債は意図せず発生し、残高が不明確なことが多い。

</div>

### 技術的負債の種類

| 種類 | 説明 | 例 | 対処法 |
|-----|------|-----|-------|
| 意図的 | 短期目標のため意識的に受け入れる | MVP優先、後でリファクタ | 返済計画を立てて追跡 |
| 無意識 | 知識不足から生じる | 設計ミス、ベストプラクティス無視 | コードレビュー、学習投資 |
| 環境的 | 外部要因から生じる | フレームワークの陳腐化 | 定期的な依存関係更新 |

</div>

---

## 技術的負債の複利効果

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 20px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**負債が増える理由**

「後で直す」が永遠に来ず、負債の上に負債を重ねる。ドキュメントは陳腐化し、知識を持つ人は離職する。**負債は複利で増える。** しかも組織・法制度・業務フローは常に変化するため、何もしなくても負債は増え続ける。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**負債の影響**

新機能開発が遅延し、バグ修正が困難になり、オンボーディングコストが増加し、モチベーションが低下する。**全てが悪循環に入る。** AIで新規コードを高速に書いても、既存の負債が消えるわけではない——むしろ負債の上に新しいコードが積み上がり、問題を深くする。

</div>
</div>

<div style="margin-top: 15px; padding: 12px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>システムは作られた瞬間から老化が始まる。投資し続けなければ、新規開発すらレガシーになる。</strong>
</div>

</div>

---

## モダナイゼーションの銀の弾丸はない

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**「でも、マイクロサービスにすれば解決するのでは？」——この問いに正面から答えたい。**

モダナイゼーションは成果が見えにくい。「マイクロサービス」と言えば経営層にも伝わるし、予算が通る。だがそれは「ダイエットしたい→サプリを買う」と同じ構造。本当に必要なのは生活習慣の変革であり、サプリは補助にすぎない。

</div>

<div style="display: flex; gap: 20px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**失敗パターン**

「マイクロサービスにすれば解決」「クラウドに移行すれば解決」「AIで全部自動化」——**手段が目的化**している。そもそも「レガシー」「モダン」という主語が大きすぎる。決済システムのレガシーと社内ツールのレガシーでは、対処法がまったく違う。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**成功のアプローチ**

ビジネス目標との整合、段階的な改善、組織と技術の同時変革。**目的から逆算する**姿勢が必要。バズワード的な二項対立（「〇〇 is Dead」「内製 vs 外注」）は、多様な領域を一括りにすることで**議論の粒度を下げ、本質を見失わせる**。

</div>
</div>

<div style="margin-top: 10px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">説明しやすい嘘より、説明しにくい真実を選べるか。それがリーダーの仕事。</span>
</div>

</div>

---

## 「説明しにくい真実」とは何か

<div style="font-size: 0.7em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

**経営層に通りにくいが、正しい判断**

</div>

<div style="font-size: 0.85em;">

| 説明しやすい嘘 | 説明しにくい真実 |
|--------------|----------------|
| 「マイクロサービス化に2年かかります」 | 「まず半年かけて現状を理解します」 |
| 「新しいフレームワークで生産性2倍」 | 「今のコードを理解できる人を増やします」 |
| 「クラウド移行でコスト30%削減」 | 「運用を見直さないと移行しても変わりません」 |

</div>

<div style="margin-top: 12px; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**なぜ「説明しにくい真実」を通すのが難しいか**

成果が出るまでに時間がかかり、その間「何をしているか」が外から見えにくい。さらに「やらなかった場合の損失」は証明できず、競合が「銀の弾丸」で成功したように見えてしまう。

</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>「半年後に失敗するプロジェクト」より「半年かけて基盤を作る」を選べるか</strong>
</div>

</div>

---

## 見えないコストを見る力

<div style="font-size: 0.7em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

**人は「何ができるか」に注目し、「何を防いでいるか」を軽視する**

機能的価値（新機能、画面、API）は目に見える。非機能要件（セキュリティ、コンプライアンス、可用性、属人化リスク）は目に見えない。だが、システムの長期的な存続を決めるのは後者のほう。

</div>

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**外部ベンダーへの支払いの正体**

「自分たちで作れるのに、なぜ外部に払うのか？」という問いが出たとき、見落とされているものがある。外部への支払いには機能だけでなく、**リスクと責任の移転**という対価が含まれている。セキュリティアップデート、法令対応、24/365の運用——これらを自前で持つコストは、ライセンス費の何倍にもなりうる。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**属人化という静かな爆弾**

内製システム最大のリスクは属人化。「あの人がいないと動かない」状態は、人の流動性がある組織では時限爆弾になる。ドキュメントを書いても読まれない。知識が1人の頭に閉じている限り、その人の退職はシステム障害と同義。**持続可能性の観点を欠いた内製は、最も高コストな選択**になりうる。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>「いくらかかるか」ではなく「何のリスクを誰が引き受けるか」で判断する</strong>
</div>

</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: transition
-->

<div style="display: flex; flex-direction: column; justify-content: center; align-items: center; height: 80%; text-align: center;">

<div style="font-size: 1.2em; font-weight: bold; color: #fff; font-style: italic;">

"Success breeds inertia, and the greater the past success, the greater the inertia"

— Simon Wardley

</div>

<div style="font-size: 0.9em; margin-top: 30px; color: #aaa;">

成功は惰性を生む。過去の成功が大きいほど、惰性も大きくなる

</div>

</div>

---

## では、判断はどんな情報に基づくべきか？

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**ここまでの話**

AI時代には「何を作るか」「なぜ作るか」を決める力が重要になる。技術が楽になると人間の問題がボトルネックとして露わになり、技術的負債は複利で増えるが銀の弾丸はない。

</div>

<div style="background-color: #e0e0e0; padding: 15px; border-radius: 8px;">

**だが、問いが残る**

「判断する」と言っても、判断には情報が要る。技術だけ見ていても全体像は見えない。組織を無視して技術を変えても、組織が引き戻す。

答えは「技術と組織を同時に見ること」。次のセクションでは、その視点——**ソシオテクニカル**——を紹介する。

</div>

</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: transition
-->

<div style="display: flex; flex-direction: column; justify-content: center; align-items: center; height: 80%; text-align: center;">

<div style="font-size: 1.5em; font-weight: bold;">

# 2. ソフトウェアと組織を</br>同時に設計する

</div>

</div>

---

## ソシオテクニカルとは何か

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 20px;">

**Socio（社会的・組織的）+ Technical（技術的）= ソシオテクニカル**

1960年代にタビストック研究所で生まれた概念。ソフトウェアアーキテクチャと組織構造は切り離せない。どちらか片方だけを変えても、真のモダナイゼーションは実現できない。

</div>

<div style="display: flex; gap: 20px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**技術だけ変えても**

組織の壁でデプロイが止まる

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**組織だけ変えても**

システムの依存関係でチームが自律できない

</div>
</div>

</div>

---

## なぜ同時設計ができないのか

<div style="font-size: 0.7em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

**「技術と組織を同時に設計すべき」は正しい。だが現実にはできていない。**

</div>

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**縦割りの壁**

技術はエンジニア、組織は人事・経営の領域。「アーキテクチャの議論に人事を呼ぶ」という**発想自体がない**。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**時間軸の違い**

技術変更は数週間〜数ヶ月、組織変更は数ヶ月〜数年。McKinsey（2023）の調査では、デジタル変革の70%が目標未達であり、最大の原因は「技術と組織の変革速度の不一致」と報告されている。**同期が取れない**ため、どちらかが置いていかれる。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**政治的コスト**

組織変更は人の感情が絡み、「あなたのチームを解体します」は言いにくい。**技術だけ変える方が楽に見える**。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>この壁を越える権限がないなら、その権限を獲りに行け</strong>
</div>

</div>

---

## なぜソシオテクニカルが重要なのか

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

レガシーの問題は、技術だけでなく**組織・文化・プロセス**に根ざしている。

</div>

### 関連する概念

| 概念 | 説明 |
|-----|------|
| **コンウェイの法則** | 組織構造がシステム設計に反映される |
| **逆コンウェイ戦略** | 望ましいアーキテクチャに合わせて組織を設計する |

<div style="margin-top: 15px; padding: 12px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>技術だけ変えても組織が追いつかない。組織だけ変えても技術が足を引っ張る</strong>
</div>

</div>

---

## 翻訳で体験したソシオテクニカル

<div style="font-size: 0.7em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

**この本を5人で翻訳した経験は、まさにソシオテクニカルの実験場だった。**

</div>

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**技術的な課題**

翻訳ツール、用語集の管理、Gitでの原稿管理。200以上のコミットを重ねて品質を磨いた。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**組織的な課題**

5人の「解釈」の違い。同じ英語を違う日本語に訳す。章をまたぐと整合性が崩れる。**暗黙の前提が異なっていた。**

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**結果として起きたこと**

ツールだけ揃えても訳文は揃わない。「同じ言葉を同じ意味で使う」という組織的合意——まさにユビキタス言語——が必要だった。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>5人のチームでも起きる。あなたのチームでも必ず起きている。見えていないだけだ</strong>
</div>

</div>

---

## コンウェイの法則

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 45%;">
<img src="../../assets/images/2026/figure-2-1-conway-law.png" alt="Conway's Law" style="width: 100%; height: fit-content;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Figure 2.1 Conway's Law より引用</div>
</div>

<div style="flex: 1; font-size: 0.68em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px; font-style: italic;">

"Organizations which design systems are constrained to produce designs which are copies of the communication structures of these organizations" — Melvin Conway (1968)

システムを設計する組織は、その組織のコミュニケーション構造をコピーした設計を生み出すことを余儀なくされる。

</div>

**疎結合なドメイン境界が、疎結合なアーキテクチャを可能にする**

- チーム構造を変えずにアーキテクチャだけ変えても失敗する
- 「逆コンウェイ戦略」: 望むアーキテクチャに合わせてチームを編成する

<div style="margin-top: 10px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">組織の形がシステムの形になる。これは好むと好まざるとに関わらず起きる。ならば利用せよ。</span>
</div>

</div>
</div>

---

## コンウェイの法則が働く理由

<div style="font-size: 0.7em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

**「組織を変えずにアーキテクチャだけ変えればいい」——これが最もよく聞く反論。だが、なぜそれが通用しないのか。**

</div>

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**コミュニケーションコスト**

チーム内の会話は低コストだが、チーム間は調整・承認・ドキュメントが必要で高コスト。結果として、**高コストな境界でモジュールが切れる**。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**責任の分界点**

「ここから先は別チームの責任」という境界が**インターフェース境界**になる。責任が曖昧だと**密結合の温床**になる。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**暗黙知の共有範囲**

チーム内では文脈が共有されるが、チーム外には明示的なAPIが必要。**共有文脈の境界がモジュール境界**になる。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>コンウェイの法則は「選択肢」ではなく「物理法則」。抗うより利用せよ</strong>
</div>

</div>

---

## コンウェイの法則を味方にした先に何がある？

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**組織とアーキテクチャを揃えたとして、その成功をどう測るか？**

「デプロイ頻度が上がった」だけで満足していいのか。速度だけを追うと、品質・安全性・チームの持続可能性が犠牲になりかねない。モダナイゼーションの成果には、**多面的な物差し**が必要。

</div>

<div style="background-color: #e0e0e0; padding: 15px; border-radius: 8px;">

→ Jon Smartが提唱した **BVSSH（Better Value Sooner Safer Happier）** フレームワークで、モダナイゼーションの「本当の成功」を測る

</div>

</div>

---

## BVSSH: モダナイゼーションの成果指標

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 40%;">
<img src="../../assets/images/2026/architecture-modernization-bvssh.png" alt="Better Value Sooner Safer Happier" style="width: 100%; height: fit-content;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Figure 1.3 Better Value Sooner Safer Happier (Source: Smart et al., Sooner Safer Happier [IT Revolution, 2020]) より引用</div>
</div>

<div style="flex: 1; font-size: 0.7em;">

**Jon Smartが提唱したBVSSHフレームワーク**

<div style="display: flex; gap: 10px; margin-top: 10px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

**Better** — 品質向上、手戻り削減

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

**Value** — ビジネス成果

</div>
</div>

<div style="display: flex; gap: 10px; margin-top: 8px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

**Sooner** — 時間短縮

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

**Safer** — セキュリティ

</div>
</div>

<div style="background-color: #f5f5f5; padding: 10px; border-radius: 8px; margin-top: 8px; text-align: center;">

**Happier** — 従業員とステークホルダーの満足度

</div>

</div>
</div>

---

## BVSSHの5つの指標

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

<strong>「速くなった」だけでは不十分。</strong> Soonerだけ伸ばした結果、品質低下・インシデント3倍・離職率上昇という事例がある。

</div>

| 指標 | 危険な兆候 |
|-----|----------|
| <strong>Better</strong> | 速くなったが品質が下がった |
| <strong>Value</strong> | 機能は増えたが使われない |
| <strong>Sooner</strong> | 頻度は上がったがバグも増えた |
| <strong>Safer</strong> | 速度優先でセキュリティ軽視 |
| <strong>Happier</strong> | 速くなったが燃え尽きた |

5つの指標は互いにトレードオフの関係になり得る。1つだけ伸ばしても、他が崩れれば全体としては後退する。

</div>

---

## Goodhartの法則に注意する

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

「測定指標が目標になると、良い測定指標でなくなる」

</div>

デプロイ頻度は空デプロイで、欠陥数は報告しないことで、数字だけ改善できてしまう。指標をKPIにした瞬間、人は指標を最適化し始める。

顧客のビジネス成果、チームの自己評価、定性的なフィードバックなど<strong>数字の裏にある文脈</strong>を見る指標こそ重要。数字は「会話のきっかけ」であって「結論」ではない。

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>数字が目標になった瞬間、数字は信用できなくなる。指標は「会話の入口」であって「結論」ではない</strong>
</div>

</div>

---

## 正しく測る先にあるもの

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**BVSSHで「何を測るか」はわかった。では「正しい値を出せる組織の形」とは？**

5つの指標すべてを健全に保つには、チームが自律的にデリバリーできる構造が必要。1つの変更に5チームの承認が要る状態では、どの指標も改善しない。

</div>

<div style="background-color: #e0e0e0; padding: 15px; border-radius: 8px;">

**答え：Independent Value Streams（独立した価値ストリーム）**

ドメインに整合し、自律的に動け、成果で測られ、疎結合な組織単位。BVSSHのすべての指標を持続的に改善できるのは、この構造を持つチームだけ。

</div>

</div>

---

## 独立した価値ストリーム (IVS)

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 40%;">
<img src="../../assets/images/2026/ivs-four-characteristics.png" alt="IVS Four Characteristics" style="width: 100%; height: fit-content;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Figure 1.5 The four key characteristics of an independent value stream より引用</div>
</div>

<div style="flex: 1; font-size: 0.7em;">

モダンアーキテクチャの基本単位「Independent Value Stream」

**4つの特性**

1. **ドメイン整合**: 特定のビジネス領域に焦点
2. **チーム自律**: 製品・技術・デリバリーの決定権
3. **成果志向**: 機能数ではなくビジネス成果で測定
4. **疎結合**: 独立してデプロイ可能

<div style="margin-top: 10px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="font-size: 0.85em;">"Teams aligned to domains become owners of their product's destiny"</span></br>
<strong>ドメインに整合したチームは、プロダクトの運命の所有者になる</strong>
</div>

</div>
</div>

---

## IVSの4つの特性を深堀り

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**ドメイン整合**

チームが担当する範囲がビジネスドメインと一致している。「決済チーム」は決済のすべてを知っている。

→ 認知負荷の軽減、専門性の深化

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**チーム自律**

技術選定、リリース判断、優先順位付けをチームが決められる。上位承認を待たない。

→ <span style="font-size: 0.85em;">"You build it, you run it"</span> — 責任が信頼性を生む

</div>
</div>

<div style="display: flex; gap: 15px; margin-top: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**成果志向**

「機能を何個作ったか」ではなく「顧客にどんな価値を届けたか」で測る。

→ 正しいものを作る動機づけ

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**疎結合**

他チームに依頼しなくてもデプロイできる。共有データベースや共有ライブラリへの依存が少ない。

→ デリバリー速度の向上

</div>
</div>

</div>

---

## IVSを実現するための条件

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**IVSは「宣言するだけ」では実現しない**

4つの特性を満たすには、技術的・組織的な前提条件がある。

</div>

### 前提条件のチェックリスト

| 特性 | 前提条件 | よくある障害 | 対処法 |
|-----|---------|------------|-------|
| ドメイン整合 | ドメイン境界が明確 | 曖昧な責任分担 | イベントストーミングで可視化 |
| チーム自律 | 権限委譲の文化 | マイクロマネジメント | 段階的に権限を委譲、成功体験を積む |
| 成果志向 | ビジネス指標の可視化 | 技術指標だけで評価 | OKRでビジネス成果に紐付け |
| 疎結合 | APIファースト、CI/CD | 共有DB、モノリス | Strangler Figで段階的に分離 |

<div style="margin-top: 10px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>「IVSにしたい」の前に「IVSにできる状態か」を確認する</strong>
</div>

</div>

---

## リスニングツアー

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 40%;">
<img src="../../assets/images/2026/figure-4-4-listening-tour.png" alt="Listening Tour" style="width: 100%; height: fit-content;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Figure 4.4 A visual summary from a listening tour より引用</div>
</div>

<div style="flex: 1; font-size: 0.68em;">

**組織全体からインサイトを収集する構造化されたアプローチ**

モダナイゼーションを始める前に、現状を深く理解することが重要。

### 実施内容

- **対象者**: 開発者、PM、ビジネスステークホルダー、運用チーム
- **質問例**: 最も痛みを感じている部分は？なぜ変更が難しいのか？

### 効果

- 問題の全体像を把握
- 組織全体のバイイン（賛同）を獲得
- 優先順位付けの材料を収集

</div>
</div>

---

## リスニングツアーで役割別に質問する

<div style="font-size: 0.7em;">

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**開発者への質問**

- デプロイ頻度は？ブロッカーは何？
- 最も触りたくないコードは？
- チーム間の依存で困ることは？

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**PMへの質問**

- 機能リリースのリードタイムは？
- 優先順位の変更にどう対応している？
- ステークホルダーからの不満は？

</div>
</div>

<div style="display: flex; gap: 15px; margin-top: 10px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**ビジネス側への質問**

- 競合に勝てないと感じる領域は？
- 「もっと早くできないの？」と思うのは？
- 技術チームへの期待と現実のギャップは？

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**運用チームへの質問**

- 最も障害が多いシステムは？
- 夜間対応が必要になる原因は？
- 「直したい」と思っても直せないものは？

</div>
</div>

</div>

---

## リスニングツアーで収集した情報を分析する

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**「聞くこと」自体が変化の始まり**

リスニングツアーは情報収集だけでなく、「経営層が現場の声を聞いている」というメッセージになる。これがモダナイゼーションへのバイイン（賛同）を生む。

</div>

### 収集した情報の整理

| 分類 | 内容 | 次のアクション |
|-----|------|--------------|
| **痛み** | 現場が困っていること | 優先度の判断材料 |
| **願望** | 「こうなったらいい」 | ビジョンの素材 |
| **政治** | 誰が何を守りたいか | 障害の予測 |
| **知恵** | 現場ならではの知識 | 設計への反映 |

<div style="margin-top: 10px;">

聞いた内容は必ず**匿名化して共有**する。「誰が言ったか」ではなく「何が問題か」に焦点を当てる。

</div>

</div>

---

## マッピングツアー

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**システムとビジネスの関係を可視化**

リスニングツアーで得た情報を整理し、マップとして表現する。

</div>

### マッピングの対象

<div style="display: flex; gap: 20px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**技術的マッピング**

- システム間の依存関係
- データフロー
- 技術スタック

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**組織的マッピング**

- チーム構造
- 責任範囲
- コミュニケーションパス

</div>
</div>

<div style="margin-top: 10px;">

これらのマップが、コンウェイの法則を可視化し、逆コンウェイ戦略の立案に使える。

</div>

</div>

---

## マッピングツアーで依存関係を可視化する

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**「誰が誰に依存しているか」を明らかにする**

依存関係を可視化すると、ボトルネックと変更の影響範囲が見えてくる。

</div>

### 描くべきマップ

| マップ | 内容 | 発見できること |
|-------|------|--------------|
| **システム依存図** | どのシステムがどのAPIを呼んでいるか | 変更の影響範囲 |
| **データフロー図** | データがどこからどこへ流れるか | ボトルネック、単一障害点 |
| **チーム責任マップ** | どのチームがどのシステムを担当するか | オーナーシップの曖昧さ |

### ポイント

- 完璧を目指さない。80%正しければ十分
- 「これは誰に聞けばいいの？」という質問自体が価値ある発見

</div>

---

## マッピングツアーでギャップを発見する

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**技術マップと組織マップを重ねると、ギャップが見える**

</div>

<div style="display: flex; gap: 20px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**よくあるギャップ**

- 1つのシステムを複数チームが触る
- 1つのチームが関連のない複数システムを担当
- 依存先チームとのコミュニケーションパスがない
- 「誰がオーナーかわからない」システム

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**ギャップが示すこと**

- コンウェイの法則が働いていない箇所
- 逆コンウェイ戦略で改善できる箇所
- モダナイゼーションの優先度が高い領域

</div>
</div>

<div style="margin-top: 10px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">

<strong>完璧なマップは要らない。来週、ホワイトボードに依存関係を1つ描け。会話が始まる</strong>

</div>

</div>

---

## プロダクトタクソノミー

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 40%;">
<img src="../../assets/images/2026/figure-6-6-product-taxonomy.png" alt="Product Taxonomy" style="width: 100%; height: fit-content;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Figure 6.6 A product taxonomy より引用</div>
</div>

<div style="flex: 1; font-size: 0.68em;">

**製品とサービスを体系的に分類・整理する手法**

モダナイゼーションの対象を明確にするために、組織が持つ製品・サービスを整理。

### 分類の軸

<div style="display: flex; gap: 10px;">
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

**ビジネス軸**

顧客セグメント、収益モデル、市場での位置づけ

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

**技術軸**

技術スタック、アーキテクチャパターン、依存関係

</div>
</div>

</div>
</div>

---

## なぜプロダクトタクソノミーが必要か

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**「うちの製品は何個あるのか」に即答できるか？**

多くの組織で、製品・サービスの全体像は誰の頭の中にもない。属人的な知識が散在し、重複や抜け漏れが発生する。

</div>

### よくある問題

| 問題 | 症状 | 結果 |
|-----|------|------|
| 製品の重複 | 似た機能を複数チームが別々に開発 | リソースの無駄 |
| 境界の曖昧さ | 「これは誰の製品？」 | 責任の押し付け合い |
| 全体像の欠如 | 経営判断の材料がない | 投資の偏り |

<div style="margin-top: 10px;">

プロダクトタクソノミーは「新しいことをする」のではなく、**暗黙的にやっていることを明示化**する作業。

</div>

</div>

---

## プロダクトタクソノミーの実践的なアプローチ

<div style="font-size: 0.7em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 15px;">

**完璧な分類より、「使える分類」を目指す**

</div>

### ステップ

1. **棚卸し**: 現在提供している製品・サービスをリストアップ
2. **グルーピング**: 顧客・ドメイン・技術などの軸で分類
3. **関係性の整理**: 製品間の依存関係を可視化
4. **オーナーの明確化**: 各製品の責任者を決める

### 分類の粒度

| 粒度 | 例 | 用途 |
|-----|-----|------|
| **大分類** | 「決済」「物流」「顧客管理」 | 経営層向け、投資判断 |
| **中分類** | 「クレジット決済」「銀行振込」 | チーム設計の単位 |
| **小分類** | 「与信チェックAPI」 | 技術的な境界 |

<div style="margin-top: 10px;">

分類は「正解」を求めるものではない。**「議論の土台」を作るもの**と割り切る。

</div>

</div>

---

## 地味な仕事こそモダナイゼーションの土台

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

<strong>リスニングツアー、マッピングツアー、プロダクトタクソノミー</strong>

これらはSNSでバズらない。カンファレンスのトレンドにもならない。「うちもやってます」とは言いにくい地味な作業。<strong>そして四半期の評価に書きにくい。</strong>

</div>

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>派手な施策</strong>

「マイクロサービス導入」「Kubernetes移行」「新フレームワーク採用」——<strong>発表しやすい、評価されやすい</strong>。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>地味だけど重要な作業</strong>

現場の声を丁寧に聞く、依存関係を可視化する、製品を整理・分類する——<strong>発表しにくいが、土台になる</strong>。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>あなたの四半期レビューに「リスニングツアー実施」と書く覚悟はあるか。その覚悟が第一歩。</strong>
</div>

</div>

---

## 見えない仕事をどう評価するか

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**「四半期の成果」に書きにくい仕事をどう評価するか**

「リスニングツアーを実施しました」は、「マイクロサービス化を完了しました」より弱く見える。でも、前者がなければ後者は失敗する。

</div>

<div style="display: flex; gap: 20px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**評価されにくい理由**

成果物が「ドキュメント」や「合意」で、効果が出るのは数四半期後。「やらなかった場合の損失」は**見えない**。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**組織に必要なこと**

成熟した評価文化、長期的な視点を持つリーダー、「土台づくり」の価値を理解する人。**短期成果だけで評価しない**姿勢。

</div>
</div>

<div style="margin-top: 15px; padding: 12px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>「リリース数」は誰でも数えられる。「避けた障害の数」を評価できるかが、組織の成熟度を決める</strong>
</div>

</div>

---

## 逆コンウェイ戦略の実践

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**望ましいアーキテクチャに合わせて組織を設計する**

コンウェイの法則を逆手に取る戦略。

</div>

### ステップ

1. **目標アーキテクチャの定義**: 疎結合で独立デプロイ可能な構造
2. **必要なチーム構造の設計**: 目標に合わせたチーム境界
3. **段階的な移行**: 一度にすべてを変えない
4. **継続的な調整**: フィードバックに基づく改善

<div style="margin-top: 10px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">権限なき責任は拷問。チームに責任を求めるなら、権限もセットで渡せ。</span></br>
<span style="font-size: 0.8em;">責任だけ押し付けて権限を握り続けるリーダーは、無意識にモダナイゼーションを妨害している</span>
</div>

</div>

---

## 構造とプロセスの誤謬

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 22%;">
<img src="../../assets/images/2026/企業変革のジレンマ.jpg" alt="企業変革のジレンマ" style="width: 100%; height: fit-content;">
<span style="font-size: 0.5em; color: #666;">出典: 日経BP 日本経済新聞出版</span>
</div>

<div style="flex: 1; font-size: 0.68em;">

<div style="background-color: #f5f5f5; padding: 10px; border-radius: 8px; margin-bottom: 10px;">

**逆コンウェイ戦略は強力だが、落とし穴がある。組織図を書き換えるだけでは何も変わらない。**

</div>

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

**よくある失敗**

組織図を変えただけ、新しいプロセスを導入しただけ、ツールを変えただけ——**形だけ変えて満足**する。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

**本当に必要なこと**

文化の変革、スキルの向上、技術的な改善、継続的な実践。**構造と中身の両方**を変える必要がある。

</div>
</div>

<div style="margin-top: 10px; padding: 8px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>箱を入れ替えても、中身が同じなら結果は同じ。形だけの変革は変革ではない</strong>
</div>

</div>
</div>

---

## なぜ「構造を変えるだけ」では失敗するのか

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 22%;">
<img src="../../assets/images/2026/企業変革のジレンマ.jpg" alt="企業変革のジレンマ" style="width: 100%; height: fit-content;">
<span style="font-size: 0.5em; color: #666;">宇田川元一著（2024）</span>
</div>

<div style="flex: 1; font-size: 0.68em;">

<div style="background-color: #f5f5f5; padding: 10px; border-radius: 8px; margin-bottom: 10px;">

**「構造的無能化」（宇田川元一）** — 組織の断片化が進み、思考の幅と質が制約され、目先の問題解決を繰り返して疲弊していく現象。成功した大企業ほど陥りやすい。

</div>

### 3つの症状とモダナイゼーションへの影響

| 症状 | 説明 | モダナイゼーションへの影響 |
|-----|------|------------------------|
| **断片化** | 分業化しすぎて縦割りに | チーム間の壁、サイロ化 |
| **不全化** | 変化を察知して自ら動けない | 「誰かが決めてくれる」待ち |
| **表層化** | 場当たり的な対応しか取れない | 銀の弾丸への期待 |

<div style="margin-top: 10px; padding: 8px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>成功した組織ほどこの罠にはまる。だからこそ、構造を意識的に壊し続ける必要がある</strong>
</div>

</div>
</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: transition
-->

<div style="display: flex; flex-direction: column; justify-content: center; align-items: center; height: 80%; text-align: center;">

<div style="font-size: 1.2em; font-weight: bold; color: #fff; font-style: italic;">

"Well-designed domain boundaries reduce dependencies and maximize team autonomy"

</div>

<div style="font-size: 0.9em; margin-top: 30px; color: #aaa;">

適切に設計されたドメイン境界は</br>依存関係を減らし、チームの自律性を最大化する

</div>

</div>

---

## 同時設計をどう実現するか？

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**ここまでの話でわかったこと**

組織とアーキテクチャは同時に設計しなければ、片方が足を引っ張る。コンウェイの法則は物理法則のように避けられず、独立した価値ストリーム（IVS）が目指すべき姿である。

</div>

<div style="background-color: #e0e0e0; padding: 15px; border-radius: 8px;">

**だが、暗黙の前提が邪魔をする**

組織の中には「言語化されていない前提」が無数にある。アーキテクトが一人で設計書を書いても、現場の暗黙知は反映されない。ビジネス側に聞いても技術的制約が見えない。

この暗黙知を可視化する手法がある。それが**イベントストーミング**。

</div>

</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: transition
-->

<div style="display: flex; flex-direction: column; justify-content: center; align-items: center; height: 80%; text-align: center;">

<div style="font-size: 1.5em; font-weight: bold;">

# 3. ビジネスを可視化する</br>協働的手法

</div>

</div>

---

## イベントストーミングとは

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 45%;">
<img src="../../assets/images/2026/event-storming-timeline.png" alt="Event Storming Timeline" style="width: 100%; height: fit-content;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Figure 7.1 EventStorming timeline より引用</div>
</div>

<div style="flex: 1; font-size: 0.7em;">

Alberto Brandoliniが考案した、ビジネスドメインを探索するためのワークショップ手法。**「正確さより発見を優先する」** という哲学が特徴。

### 基本的な要素

| 付箋の色 | 意味 |
|---------|------|
| オレンジ | ドメインイベント（過去形） |
| 青 | コマンド（アクション） |
| 黄色 | アクター/ユーザー |
| ピンク | 外部システム |
| 濃いピンク | ホットスポット（問題） |

</div>
</div>

---

## イベントストーミングの記法

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 50%;">
<img src="../../assets/images/2026/event-storming-notation.png" alt="Event Storming Notation" style="width: 100%; height: fit-content;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Figure 7.3 People, systems and hotspots より引用</div>
</div>

<div style="flex: 1; font-size: 0.7em;">

### 記法の意味

- **オレンジの付箋**: 過去形で書く「〜された」
- **青の付箋**: 命令形で書く「〜する」
- **矢印**: イベントの流れ、因果関係
- **ピンクのホットスポット**: 曖昧さや問題点をマーク

### ポイント

最初は「正しさ」を気にせず、とにかく貼り出す。**発見は混乱の中から生まれる**。

</div>
</div>

---

## イベントストーミングの進め方

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 20px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**Phase 1: カオス**

参加者全員がドメインイベントを時系列で貼り出す。この段階では議論せず、とにかく可視化。

<span style="color: #666; font-size: 0.85em;">"Chaos is a feature, not a bug"</span>

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**Phase 2: タイムライン**

イベントを時間軸で整理。矛盾や抜けを発見していく。

</div>
</div>

<div style="display: flex; gap: 20px; margin-top: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**Phase 3: 問題の発見**

ホットスポット（問題点、曖昧な箇所）をマーク。ここが改善のチャンス。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**Phase 4: 境界の特定**

自然なドメイン境界を見つけ、サブドメインを定義する。

</div>
</div>

</div>

---

## イベントストーミングの価値

<div style="font-size: 0.75em;">

### なぜトップダウンの設計より効果的なのか

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-top: 15px;">

- **多様な視点の統合**: 開発者、ドメインエキスパート、UXデザイナーが同じ場に
- **暗黙知の可視化**: ドキュメントには書かれていない知識が浮かび上がる
- **共通理解の形成**: 認識のズレがその場で解消される
- **設計の民主化**: 「アーキテクトだけが知っている」状態を脱却

</div>

<div style="margin-top: 15px;">

従来の「アーキテクトが設計して渡す」モデルでは、現場の知識が設計に反映されにくい。イベントストーミングは、**設計プロセス自体を協働的にする**という発想の転換。

</div>

</div>

---

## イベントストーミングでよくある失敗と対策

<div style="font-size: 0.7em;">

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**失敗パターン**

「正しい」イベントを探そうとして議論が止まる、技術者だけで実施してビジネスが抜ける、ファシリテーター不在で発散しすぎる、1回やって満足し継続しない。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**対策**

**「発見優先」**の姿勢を冒頭で共有し、必ずドメインエキスパートを招く。事前にファシリテーターを決めてタイムボックスを設定し、**定期的に再実施**して理解を更新する。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>「忙しくてワークショップの時間がない」——その忙しさの原因が、暗黙知の不共有ではないか？</strong>

</div>

</div>

---

## 翻訳チームで起きたイベントストーミング

<div style="font-size: 0.7em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

**翻訳チームで起きた問題は、イベントストーミングで解決される典型的パターンだった。**

</div>

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**暗黙の前提がズレていた**

5人が同じ英語を違う日本語に訳す。「これはこういう意味だと思っていた」——その前提が**暗黙のまま**放置されていた。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**可視化で解決した**

用語集を作り、各章担当者が集まって認識合わせ。「この文脈でのこの用語の意味」を**全員で可視化**した。イベントストーミングと同じ原理。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**これはBounded Contextの実践**

「全体で統一」ではなく「境界内で一貫」。章ごとに文脈が違うことを認め、境界を明示した。次のDDDセクションで詳しく説明する。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>翻訳で最も難しかったのは英語ではない。著者の思考を追体験することだった。</strong>
</div>

</div>

---

## 「行間」を翻訳する——暗黙知の発見

<div style="font-size: 0.7em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

<strong>ドキュメントに書いてあることと、著者が言いたいことは違う。</strong> 原著の「行間」を読む必要があった。文脈を知らないと誤訳する。<strong>「なぜこの章がこの順番なのか」</strong>を理解して初めて訳せた。

</div>

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**翻訳の現場**

著者Nick Tuneが「なぜBounded Contextの章をイベントストーミングの後に置いたのか」——その意図を理解しないと、章の繋ぎが訳せない。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**ソフトウェア開発の現場**

仕様書に書いてあることと、顧客が欲しいものは違う。既存コードを読むだけでは設計意図がわからない。**「なぜ」が共有できているか**が品質を決める。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>暗黙知を可視化する——翻訳でもソフトウェアでも、イベントストーミングの原理は同じ</strong>
</div>

</div>

---

## イベントストーミングの成果を設計に活かす

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**イベントストーミングで得られたもの**

- ビジネスフローが可視化された
- ホットスポット（問題箇所）が特定された
- チーム間の暗黙知が共有された

</div>

<div style="background-color: #e0e0e0; padding: 15px; border-radius: 8px;">

**次の問い**

「この可視化されたビジネスフローを、どうシステム設計に落とし込むか？」

イベントストーミングで発見した「自然な境界」を、**ドメイン駆動設計（DDD）の概念**で整理する。

→ Bounded Context、コンテキストマッピングへ（型で守る方法も紹介）

</div>

</div>

---

## ドメイン境界を発見する

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 40%;">
<img src="../../assets/images/2026/domain-boundaries.png" alt="Well-designed domain boundaries" style="width: 100%; height: fit-content;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Figure 9.1 Well-designed domain boundaries reduce dependencies より引用</div>
</div>

<div style="flex: 1; font-size: 0.7em;">

### 境界を定義する原則

**「正しい」唯一の境界は存在しない。** 境界は組織の目標に奉仕するために存在する。

**一緒に変わるものは、一緒に置く。** 変更の波及範囲が境界の外に漏れるなら、境界の引き方が間違っている。

### 依存関係のコスト評価

**Pain = Strength × Volatility × Distance**

- **Strength**: 依存の強さ
- **Volatility**: 変更頻度
- **Distance**: チーム間の距離

強く依存し、よく変わり、遠いチームとの関係が最もコストが高い。

</div>
</div>

---

## Bounded Contextとは何か

<div style="font-size: 0.75em;">

<div style="background-color: #e0e0e0; padding: 12px; border-radius: 8px; margin-bottom: 15px;">

**問い：システム全体で「顧客」の定義を統一すべきか？**

</div>

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**Bounded Context = 同じ言葉が同じ意味を持つ範囲**

Eric Evansが2003年の「Domain-Driven Design」で提唱したDDDの中核概念。

「境界づけられたコンテキスト」と訳されるが、要するに**この範囲内では、この言葉はこの意味**という明確な境界線のこと。モデルの適用範囲を限定することで、矛盾なくドメインを表現できる。

なぜ重要か？ → 同じ言葉でも、文脈によって意味が異なるから。

</div>

</div>

---

## なぜ「統一」ではなく「境界」なのか

<div style="font-size: 0.7em;">

### 例：「顧客」という言葉

| コンテキスト | 「顧客」の意味 |
|------------|--------------|
| 営業 | 商談中の見込み客（連絡先、商談ステージ）|
| サポート | 問い合わせをしてきた人（チケット履歴）|
| 請求 | 支払い義務のある法人/個人（請求先住所）|
| マーケティング | セグメント、ペルソナ（行動履歴）|

<div style="margin-top: 15px; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

これを無視して「統一顧客マスタ」を作ると → **あらゆる属性を持つ巨大テーブル**になり、変更のたびに全部門に影響。

</div>

</div>

---

## Bounded Contextが解決する問題

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 20px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**統一モデルの問題**

全員の要求を満たそうとして肥大化し、1箇所の変更が全体に波及。「誰のためのモデルか」が曖昧になり、**チーム間の調整コストが爆発**する。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**Bounded Contextのアプローチ**

各コンテキストは必要なモデルだけ持ち、変更が局所化される。「このコンテキストの責任者」が明確で、**チームが自律的に動ける**。

</div>
</div>

<div style="margin-top: 15px; padding: 12px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>Bounded Contextは「分断」ではなく「明確化」。境界を認めることで、かえって連携しやすくなる。</strong>
</div>

</div>

---

## 翻訳で証明されたBounded Context

<div style="font-size: 0.7em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

**翻訳チームで起きた用語論争は、Bounded Contextの必要性を身をもって証明した。**

</div>

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**例：「Bounded Context」の訳語**

「境界づけられたコンテキスト」で統一するか、初出のみ日本語を添えてあとは英語表記にするか。5人の翻訳者で議論が割れた。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**解決策：用語集で「境界」を引いた**

19の中核用語について「この表記、この意味」を明文化した。まさに**翻訳チーム自体がBounded Contextを実践**していた。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>Bounded Contextは理論ではない。翻訳者5人の現場で必然的に生まれた解決策だった。</strong>
</div>

</div>

---

## コンテキストマッピング

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 40%;">
<img src="../../assets/images/2026/figure-9-12-context-map.png" alt="Context Mapping" style="width: 100%; height: fit-content;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Figure 9.12 A context map より引用</div>
</div>

<div style="flex: 1; font-size: 0.7em;">

**Bounded Context同士の連携パターン**を可視化する手法（Eric Evans, DDD）

Bounded Contextは独立して存在するわけではない。他のコンテキストと**どう連携するか**を明示的に設計する必要がある。

Team Topologiesのインタラクションモードと対応する概念であり、チーム間の関係性を技術的に表現したもの。

</div>
</div>

---

## コンテキストマッピングの代表的なパターン

<div style="font-size: 0.58em;">

<div style="display: flex; gap: 10px;">
<div style="flex: 1; background-color: #f5f5f5; padding: 8px; border-radius: 8px;">

**パートナーシップ（Partnership）**

両チームが対等な立場で協力し、互いの成功が相手の成功に依存する関係。共同で開発計画を立て、API変更は両者で合意する。

<span style="color: #666;">**特徴**: 調整コストは高いが柔軟性も高い。片方だけが恩恵を受ける関係では機能しない。</span>

<span style="color: #666;">**例**: 決済チームと注文チームが共同で新機能を開発</span>

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 8px; border-radius: 8px;">

**カスタマー・サプライヤー（Customer-Supplier）**

上流チーム（サプライヤー）が下流チーム（カスタマー）のニーズに対応する。下流の要望が上流の優先度に影響を与える。

<span style="color: #666;">**特徴**: 上下関係が明確だが、下流の声が届く仕組みがある。上流が下流を無視し始めると崩壊する。</span>

<span style="color: #666;">**例**: 基盤チーム（上流）と機能チーム（下流）</span>

</div>
</div>

<div style="display: flex; gap: 10px; margin-top: 8px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 8px; border-radius: 8px;">

**順応者（Conformist）**

下流が上流のモデルをそのまま受け入れ、上流に影響を与える力がない場合に採用。翻訳コストをかける余裕がない、または上流のモデルが十分に良い場合。

<span style="color: #666;">**特徴**: 実装は簡単だが、上流の変更に振り回される。戦略的に選択すべき。</span>

<span style="color: #666;">**例**: 外部決済API（Stripe等）をそのまま使う</span>

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 8px; border-radius: 8px;">

**腐敗防止層（Anti-Corruption Layer）**

外部システムの影響から自チームを守る翻訳層。自分たちのドメインモデルを汚染させないための防御壁。

<span style="color: #666;">**特徴**: 実装コストはかかるが、自律性を確保できる。レガシーシステムとの統合に有効。</span>

<span style="color: #666;">**例**: 古い基幹システムのデータを変換するアダプター</span>

</div>
</div>

</div>

---

## コンテキストマッピングで関係性を設計する

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**「繋がり方」を設計しないと、最悪の形で繋がる**

Bounded Contextを発見しただけでは不十分。コンテキスト間の**関係性**を意図的に設計しなければ、暗黙の依存関係が蔓延する。

</div>

### パターンの選択基準

| パターン | 採用する状況 | 避けるべき状況 |
|---------|------------|--------------|
| **パートナーシップ** | 両チームの成功が相互依存 | 片方だけが恩恵を受ける |
| **カスタマー・サプライヤー** | 明確な上流/下流がある | 双方向の影響がある |
| **腐敗防止層（ACL）** | 外部システムから保護したい | 自チームの負担が大きすぎる |

<div style="margin-top: 10px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">境界は自然発生しない。アーキテクトが意志を持って「引く」ものだ</span>
</div>

</div>

---

## コンテキストマッピングでよくある失敗

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 20px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**失敗パターン1: 順応者パターンの濫用**

「上流に合わせる」が楽だからと、すべて順応者にする。結果として、上流の変更に振り回され続ける。

→ **自チームの自律性を守るべき場所には腐敗防止層を**

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**失敗パターン2: 暗黙のパートナーシップ**

明示的な合意なく「なんとなく協力」。責任の押し付け合いが発生。

→ **関係性は文書化し、定期的に見直す**

</div>
</div>

<div style="margin-top: 15px; padding: 12px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>コンテキストマップは「描いて終わり」ではなく、チーム間の「契約書」として運用する</strong>
</div>

</div>

---

## 境界を守る方法の一つ——型システム

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**ここまでの話**

イベントストーミングでドメインを発見し、Bounded Contextで境界を定義し、コンテキストマッピングで関係性を設計した。

</div>

<div style="background-color: #e0e0e0; padding: 15px; border-radius: 8px;">

**余談：境界をコードでも守る方法がある**

ドキュメントに書いても破られる。コードレビューで指摘しても漏れる。そこで**コンパイラに守らせる**という選択肢がある。

→ 関数型ドメインモデリング——型システムで境界を強制するアプローチの紹介

</div>

</div>

---

## 関数型ドメインモデリング

<div style="display: flex; gap: 30px; align-items: center;">
<div style="width: 25%;">
<img src="../../assets/images/2026/functional-domain-modeling.webp" alt="関数型ドメインモデリング" style="width: 100%; height: fit-content;">
</div>

<div style="flex: 1; font-size: 0.7em;">

### DDDを「型」で実装する

**著者**: Scott Wlaschin（日本語版 2025年発売、KADOKAWA）

DDDの概念（Aggregate、Entity、Value Object）を**型システム**で表現する実践書。F#を使い、「経験則」を「コンパイラが検証可能な制約」に昇華させる。

### 核心的なアイデア

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

**Make Illegal States Unrepresentable**

不正な状態を型レベルで表現不可能にする

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

**変えるな。作れ。**

状態を変更せず、新しい値を生成する

</div>
</div>

</div>
</div>

---

## 型で境界を守るという選択肢

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

<strong>「状態は例外的な存在であり、ほとんどの処理は状態を使うことなく記述できる」</strong>

ビジネスロジックの大半は「入力を受け取り、変換し、出力を返す」純粋な関数として設計できる。すべてのプロジェクトに適用すべきとは限らないが、知っておくと武器になる。

</div>

<div style="display: flex; gap: 20px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>型による制約の例</strong>

- `NonEmptyList<OrderLine>` → 空の注文を表現不可能に
- `VerifiedEmailAddress` → 未検証メールの操作を不可能に
- `ValidatedOrder` → 検証前の注文を処理に渡せない

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>なぜ型なのか</strong>

コメントは嘘をつく。ドキュメントは古くなる。テストは書き忘れる。だが型制約は<strong>コンパイラが強制する壁</strong>。人間の注意力に依存しない。

</div>
</div>

</div>

---

## 型がもたらす設計上の恩恵

<div style="font-size: 0.75em;">

「不正な状態を表現できない」設計は、バグを減らすだけではない。<strong>チーム間のコミュニケーションコスト</strong>を劇的に下げる。

型定義がそのままAPIの仕様書になり、コンパイルが通る＝仕様を満たしている、という状態を作れる。Bounded Contextの境界を人間の約束ではなく、機械が検証可能な制約として実装できる。

<div style="margin-top: 15px; padding: 12px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>境界は「約束」で守るな。「型」で守れ。</strong>
</div>

</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: transition
-->

<div style="display: flex; flex-direction: column; justify-content: center; align-items: center; height: 80%; text-align: center;">

<div style="font-size: 1.2em; font-weight: bold; color: #fff; font-style: italic;">

"Loosely coupled domain boundaries are not optional—they are essential"

</div>

<div style="font-size: 0.9em; margin-top: 30px; color: #aaa;">

疎結合なドメイン境界はオプションではない。それは必須である

</div>

</div>

---

## 境界をシステムとチームに実装する

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**境界が見えた。だが見えただけでは意味がない。**

イベントストーミングで得られたもの——ビジネスの全体像、ドメイン境界の候補（Bounded Context）、チーム間の暗黙知、型で境界を守る選択肢。

</div>

<div style="background-color: #e0e0e0; padding: 15px; border-radius: 8px;">

**問いが変わる**

「境界に合わせてチームをどう設計するのか？」

Bounded ContextとTeam Topologiesを対応させ、コンテキストマッピングとインタラクションモードを揃える。DDDとTeam Topologiesを**統合的に活用**する。

</div>

</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: transition
-->

<div style="display: flex; flex-direction: column; justify-content: center; align-items: center; height: 80%; text-align: center;">

<div style="font-size: 1.5em; font-weight: bold;">

# 4. ドメイン駆動設計と</br>チーム設計の統合

</div>

</div>

---

## DDDとTeam Topologiesの対応関係

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**なぜ2つを統合的に使うのか？**

DDDはソフトウェアの境界を定義し、Team Topologiesはチームの境界を定義する。両者を一致させることで、コンウェイの法則が味方になる。

</div>

| DDD概念 | Team Topologies対応 |
|---------|---------------------|
| **Bounded Context** | Stream-aligned Team の責任範囲 |
| **Core Domain** | 最も重要なStream-aligned Team |
| **Supporting Subdomain** | Enabling Team / Complicated Subsystem Team |
| **Generic Subdomain** | Platform Team / 外部調達 |
| **Context Mapping** | Team Interaction Modes |

<div style="margin-top: 10px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>「きれいに分離できない」という反論が出る。完全な分離は幻想。依存の方向と強さを制御することが本質</strong>
</div>

</div>

---

## Core Domain Chart

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 35%;">
<img src="../../assets/images/2026/core-domain-chart.png" alt="Core Domain Chart" style="width: 100%; height: fit-content;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Figure 10.5 The Core Domain Chart より引用</div>
</div>

<div style="flex: 1; font-size: 0.7em;">

**問い：すべてのドメインに同じリソースを投入すべきか？**

**答え：No。戦略的に投資先を選ぶ必要がある。**

| 分類 | 差別化 | 複雑度 | 投資戦略 |
|-----|-------|-------|---------|
| **Core** | 高 | 高 | 重点投資、内製 |
| **Supporting** | 低 | 高 | 効率重視、外部委託も |
| **Generic** | 低 | 低 | 購入/SaaS |

X軸: ビジネス差別化、Y軸: モデル複雑度

</div>
</div>

---

## Team Topologies とは

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 35%;">
<img src="../../assets/images/2026/team-topologies-four-types.png" alt="The four team types" style="width: 100%; height: fit-content;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Figure 11.3 The four team types in Team Topologies より引用</div>
</div>

<div style="flex: 1; font-size: 0.7em;">

Matthew SkeltonとManuel Paisが提唱。「高速なフローを持続可能に実現する」ためのチーム設計フレームワーク。

### 4つのチームタイプ

<div style="display: flex; gap: 10px; margin-top: 10px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

**Stream-aligned** — 価値ストリームをEnd-to-Endで担当

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

**Platform** — 共通基盤を提供

</div>
</div>

<div style="display: flex; gap: 10px; margin-top: 10px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

**Complicated Subsystem**
高度な専門知識が必要な領域を担当

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**Enabling** — 他チームのスキル向上を支援

</div>
</div>

</div>
</div>

---

## Stream-alignedチームの設計原則

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**Stream-alignedチームが「主役」**

Team Topologiesの核心は、Stream-alignedチームが価値を届けることに集中できるよう、他の3タイプがサポートする構造。

</div>

### Stream-alignedチームに必要な条件

| 条件 | 説明 | なぜ重要か |
|-----|------|----------|
| **End-to-End責任** | 設計からデプロイまで | 他チームへの依存を減らす |
| **ビジネス成果へのオーナーシップ** | 機能数ではなく価値 | 正しいものを作る動機 |
| **認知負荷の限界内** | 担当範囲が大きすぎない | 持続可能なペース |

<div style="margin-top: 10px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>来週、1つの機能リリースに必要な調整先を数えてみろ。その数があなたの組織の現在地だ</strong>
</div>

</div>

---

## 3つのインタラクションモード

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 40%;">
<img src="../../assets/images/2026/figure-11-10-team-interactions.png" alt="Team Interactions" style="width: 100%; height: fit-content;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Figure 11.5 The three interaction modes in Team Topologies より引用</div>
</div>

<div style="flex: 1; font-size: 0.68em;">

チーム間の関わり方を明確に定義する

<div style="display: flex; gap: 10px; margin-top: 10px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

**Collaboration**

高帯域幅での協働。コスト高だが発見多い。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

**X-as-a-Service**

セルフサービスAPI経由での利用。

</div>
</div>

<div style="background-color: #f5f5f5; padding: 10px; border-radius: 8px; margin-top: 10px;">

**Facilitating** — Enablingチームが他チームの能力向上を支援

</div>

</div>
</div>

</div>

---

## インタラクションモードの選択

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**「どのモードで関わるか」は状況によって変わる**

同じチーム同士でも、プロジェクトフェーズや課題によって最適なモードは変化する。

</div>

### 状況別の選択指針

| 状況 | 推奨モード | 理由 |
|-----|----------|------|
| **新しい境界の発見期** | Collaboration | 曖昧さを解消するには密な対話が必要 |
| **成熟したサービス利用** | X-as-a-Service | 低オーバーヘッドで利用可能 |
| **新技術の導入期** | Facilitating | スキル移転が目的 |

<div style="margin-top: 10px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>Collaborationは「高コスト・高リターン」。常態化させず、必要な期間に限定する</strong>
</div>

</div>

---

## インタラクションモードの進化

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**典型的な進化パターン**

</div>

```
[発見期]           [移行期]           [成熟期]
Collaboration  →  Facilitating   →  X-as-a-Service
  密な協働          スキル移転         セルフサービス
  高コスト          中コスト           低コスト
```

### なぜ進化が重要か

**Collaborationは持続不可能**——2チームが常に密に連携すると両方の速度が落ちる。**X-as-a-Serviceは最初から無理**——境界が不明確な段階でAPIを固定すると後で苦しむ。**Facilitatingは橋渡し**——知識を移転し、自律性を高める。

<div style="margin-top: 10px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>チーム間の関わり方に「卒業条件」を設定せよ。期限なきCollaborationは共依存になる</strong>
</div>

</div>

---

## DDDとTeam Topologiesの統合

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**ドメイン境界 = チーム境界**

DDDで発見したサブドメインの境界と、Team Topologiesで設計するチーム境界を一致させる。これが「逆コンウェイ戦略」の実践。

</div>

### 統合の効果

認知負荷を最小化し、チームは自分のドメインに集中できる。コンテキストマップでチーム間関係が明確になり、ドメイン内の変更は同じチーム内で完結する。ビジネスの成長に合わせて**スケーラブルにチームを増やせる**。

</div>

---

## 概念の関係性マップ

<div style="font-size: 0.7em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**本日紹介した概念はすべて繋がっている**

</div>

```
イベントストーミング ──→ Bounded Context の発見
                              ↓
               ┌──────────────┴──────────────┐
               ↓                              ↓
    Core Domain Chart              Team Topologies
   (どこに投資すべきか)            (チーム境界の設計)
               ↓                              ↓
       Wardley Mapping                  IVS設計
   (進化段階で投資判断)        (独立した価値ストリーム)
               ↓                              ↓
               └───────────→ 逆コンウェイ戦略 ←───┘
                            (アーキテクチャ×組織の整合)
```

<div style="margin-top: 10px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>イベントストーミングで発見し、DDDで分析し、Team Topologiesで組織化する</strong>
</div>

</div>

---

## なぜチーム境界を「小さく」保つのか？

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**ここまでの話の核心**

- ドメイン境界＝チーム境界にすることで、自律性と疎結合を実現する
- でも、チームが担当する範囲が広すぎたら？

</div>

<div style="background-color: #e0e0e0; padding: 15px; border-radius: 8px;">

**問い：1チームでどこまでの範囲を担当できるか？**

答えは「認知負荷の限界内」まで。これを超えると、チームは機能しなくなる。

→ Team Topologiesが「認知負荷」を重視する理由を理解する

</div>

</div>

---

## 認知負荷（Cognitive Load）とは

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**人間の脳が同時に処理できる情報量には限界がある**

John Swellerが1988年に提唱した認知負荷理論に基づく概念。Team Topologiesでは、チームが担当する範囲を「認知負荷の限界内」に収めることを重視する。人間のワーキングメモリには限界があり、それを超えるとパフォーマンスが急激に低下する。

<div style="margin-top: 10px; padding: 8px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="font-size: 0.85em; font-style: italic;">"Cognitive load is the primary constraint"</span></br>
<strong>認知負荷こそがチーム設計における最大の制約</strong>
</div>

</div>

### 認知負荷の3種類

| 種類 | 説明 | 対策 |
|-----|------|------|
| **Intrinsic（内在性）** | 問題固有の複雑さ | 削減困難。受け入れる |
| **Extraneous（外因性）** | 環境・ツールの複雑さ | **削減すべき** |
| **Germane（関連性）** | 学習のための負荷 | 適度に維持 |

</div>

---

## 認知負荷を管理する

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 20px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**プラットフォームチームの役割**

開発者体験を向上させ、「舗装された道」を提供することで、Stream-alignedチームの認知負荷を軽減する。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**チーム設計の原則**

1チームが担当する範囲を限定し、依存関係を最小化。**自律的に動ける単位**にする。

</div>
</div>

<div style="margin-top: 15px; padding: 12px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>チームに「何が一番面倒？」と聞け。その答えがExtraneous Loadであり、最初に潰すべき標的だ</strong>
</div>

</div>

---

## 持続可能な高速フロー

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**Team Topologiesの核心目標**

速度だけでなく、品質を維持しながら何年も続けられる持続可能なフロー。

</div>

### 高速フローを阻害するもの

<div style="display: flex; gap: 20px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**組織的な障壁**

過剰な承認プロセス、チーム間の依存関係、不明確な責任範囲、サイロ化された知識。**人の問題**。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**技術的な障壁**

モノリシックなコードベース、共有データベース、手動デプロイプロセス、テスト不足。**技術の問題**。

</div>
</div>

<div style="margin-top: 10px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="font-size: 0.85em;">Accelerate（Forsgren et al., 2018）の研究が示した事実：チームの自律性とデリバリー性能は強く相関する</span></br>
<strong>高速フローは組織の健全性を測る物差し——ベロシティ指標ではなく</strong>
</div>

</div>

---

## 小さく長寿命のチーム

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**5-9人のチームサイズが最適**

ダンバー数に基づく認知限界を考慮。

</div>

### なぜ小さく長寿命か

<div style="display: flex; gap: 20px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**小さいチームの利点**

コミュニケーションコストを最小化し、意思決定が迅速。全員が全体を把握可能で、**信頼関係が構築**できる。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**長寿命チームの利点**

ドメイン知識が蓄積し、コードベースに習熟。チームとして成熟し、**予測可能なベロシティ**が得られる。

</div>
</div>

チームをプロジェクトごとに解散・再編成するのではなく、製品に長期的に紐づける。DORA State of DevOps Report（2023）は、安定したチーム構成がデリバリー性能の予測因子であることを繰り返し報告している。

</div>

---

## なぜチームか

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**問い：優秀な個人を集めれば、優秀なチームになるか？**

答えは No。個人のスキルの総和がチームの能力ではない。チームには独自のダイナミクスがあり、それが成果を左右する。

</div>

### 従来のアプローチの問題

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**リソース思考**

「この人をあのプロジェクトに」——スキルセットだけで編成し、個人の専門性に依存する。**人を部品として扱う**発想。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**なぜ問題か**

チームの文脈が失われ、暗黙知が継承されず、信頼関係を毎回再構築。**毎回ゼロからスタート**になる。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">人を「リソース」と呼ぶ組織は、チームを育てられない。名前で呼べ。</span>
</div>

</div>

---

## チームファーストの実践

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 15px;">

**チームを設計の基本単位として扱う**

システムを設計するとき、「どのチームが担当するか」を最初に考える。個人ではなく、チームの認知負荷・スキル・成長を考慮する。

</div>

### 具体的なアプローチ

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**設計時の考慮**

チームの認知負荷を超えない範囲に留め、依存関係を最小化し、**自律的に意思決定できる単位**にする。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**運用時の考慮**

チームの学習と成長を支援し、長寿命チームを維持。**チーム間コラボレーション**も意図的に設計する。

</div>
</div>

### 「意志を実装する」との関係

チームが自律的に動けなければ、意志は実装できない。**チームの自律性を確保することが、意志を実装する前提条件**。

</div>

---

## 「チームの力で組織を動かす」

<div style="display: flex; gap: 30px; align-items: center;">
<div style="width: 22%;">
<img src="../../assets/images/2026/team-power-organization.jpg" alt="チームの力で組織を動かす" style="width: 100%; height: fit-content;">
</div>

<div style="flex: 1; font-size: 0.68em;">

**著者**: 松本成幸（技術評論社, 2025）

Team Topologiesを日本の現場に落とし込んだ実践書。**16のアンチパターン**、**6つの原則**、**7つのガイドライン**を収録。

### なぜアンチパターンを学ぶのか

<div style="background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

アンチパターンは「絶対に避けるべきもの」ではない。**一時的に取らざるを得ないこともある**。

重要なのは「これがアンチパターンである」と認識すること。言語化できれば、**将来どんな問題を引き起こすか想像でき、戦略的な意思決定ができる**。

</div>

<div style="margin-top: 10px; padding: 8px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>10人のスターを集めても機能しないチームがある。5人の凡人でも機能するチームがある</strong>
</div>

</div>
</div>

---

## チームの自律性を支える基盤

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**ここまでの話**

Stream-alignedチームが価値を届けることに集中するには、認知負荷を軽減する必要がある。では、誰がその負荷を引き受けるのか？

</div>

<div style="background-color: #e0e0e0; padding: 15px; border-radius: 8px;">

**答え：プラットフォームチームとIDP（Internal Developer Platform）**

各チームがインフラ、CI/CD、監視をゼロから作る必要はない。共通基盤として「舗装された道」を提供すれば、チームはビジネスロジックに集中できる。

→ これがTeam Topologiesにおける**Platform Team**の役割

</div>

</div>

---

## Internal Developer Platform (IDP)

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 45%;">
<img src="../../assets/images/2026/idp-paved-road.png" alt="IDP Paved Road" style="width: 100%; height: fit-content;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Figure 13.1 Golden paths (paved roads) より引用</div>
</div>

<div style="flex: 1; font-size: 0.65em;">

**問い：各チームがインフラ、CI/CD、監視を個別に作るべきか？**

**答え：No。共通基盤として「舗装された道（Paved Road）」を提供する。** Netflixが提唱した概念で、強制ではなく「この道を通ると楽」という設計思想。

<div style="display: flex; gap: 10px; margin-top: 10px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

**セルフサービス機能**

環境プロビジョニング、CI/CDパイプライン、モニタリング・ログ。**必要なものを自分で取得**できる。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

**ゴールデンパス**

推奨されるやり方、テンプレート提供、ガードレール。**迷わず正解にたどり着ける**道。

</div>
</div>

開発者は「どうやるか」を考えずに「何を作るか」に集中できる。

</div>
</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: transition
-->

<div style="display: flex; flex-direction: column; justify-content: center; align-items: center; height: 80%; text-align: center;">

<div style="font-size: 1.2em; font-weight: bold; color: #fff; font-style: italic;">

"Stream-aligned teams can deliver independently when domains are properly decoupled"

</div>

<div style="font-size: 0.9em; margin-top: 30px; color: #aaa;">

ドメインが適切に分離されていれば</br>Stream-alignedチームは独立してデリバリーできる

</div>

</div>

---

## 現実のレガシーとどう向き合うか

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 15px;">

**設計手法は揃った。しかし現実のレガシーシステムは理論通りにはいかない。**

| フェーズ | 手法 | 得られるもの |
|---------|------|-------------|
| 現状理解 | リスニング/マッピングツアー | 痛み・願望・政治 |
| 可視化 | イベントストーミング | ビジネスフロー、ホットスポット |
| 境界定義 | DDD（Bounded Context） | 責任範囲、連携パターン |
| チーム設計 | Team Topologies | 自律的なチーム構造 |
| 境界の強制 | 型システム（関数型DDD） | AIが破れない壁 |

</div>

<div style="background-color: #e0e0e0; padding: 12px; border-radius: 8px;">

**残る問い**

道具は揃った。だが目の前には、10年以上動き続けてきたレガシーシステムがある。これを理論通りにきれいに移行できるのか？

→ **段階的・継続的な進化**のアプローチへ

</div>

</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: transition
-->

<div style="display: flex; flex-direction: column; justify-content: center; align-items: center; height: 80%; text-align: center;">

<div style="font-size: 1.2em; font-weight: bold; color: #fff; font-style: italic;">

"Legacy architecture is a business liability, not merely a technical problem"

</div>

<div style="font-size: 0.9em; margin-top: 30px; color: #aaa;">

レガシーアーキテクチャは単なる技術的問題ではない</br>**ビジネス上の負債**である

</div>

</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: transition
-->

<div style="display: flex; flex-direction: column; justify-content: center; align-items: center; height: 80%; text-align: center;">

<div style="font-size: 1.5em; font-weight: bold;">

# 5. レガシーを</br>競争優位性へ転換する

</div>

</div>

---

## レガシーの本当の問題は現場で起きている

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 15px;">

**「古いから問題」ではない。「変化に対応できないから問題」**

レガシーシステム自体は悪ではない。問題は、ビジネスが求める速度で変化できないこと。ソフトウェアの価値は初期構築にではなく、変化への継続的適応にある。だからシステムは「完成」しない——作った瞬間から適応の旅が始まる。

</div>

### あなたの現場で、こんなことが起きていないか？

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**開発者が感じる痛み**

- 小さな変更に数週間かかる
- 「ここを触ると何が壊れるかわからない」
- テストがないので怖くて触れない
- 新しい技術を試す余裕がない

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**ビジネスが感じる痛み**

- 競合は3日で出す機能が、うちは3ヶ月
- 「技術的に難しい」と言われ続ける
- 見積もりが常に大きく外れる
- 優秀なエンジニアが辞めていく

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">モグラ叩きをやめろ。構造を変えない限り、問題は永遠に湧き出る。</span></br>
<span style="font-size: 0.8em;">「構造を変える余裕がない」——それは構造を変えないことのコストを見えていないだけ</span>
</div>

</div>

---

## レガシーの本当の問題は負のサイクルにある

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 40%;">
<img src="../../assets/images/2026/figure-1-3-negative-cycle.png" alt="Negative Cycle" style="width: 100%; height: fit-content;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Figure 1.1 The negative cycle of declining architecture health より引用</div>
</div>

<div style="flex: 1; font-size: 0.68em;">

**なぜ問題は悪化し続けるのか？**

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 10px;">

1. **変更が困難** → デリバリーが遅れる
2. **デリバリーが遅い** → ビジネスからの信頼低下
3. **信頼低下** → 投資が減少（「どうせ遅いから」）
4. **投資減少** → 技術的負債がさらに蓄積
5. **負債の蓄積** → さらに変更が困難に（最初に戻る）

</div>

このサイクルは**自己強化**する。放置すればするほど、抜け出すのが難しくなる。

<div style="margin-top: 10px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>サイクルを断ち切るには、どこかに「くさび」を打つ必要がある</strong>
</div>

</div>
</div>

---

## 負のサイクルを断ち切る「くさび」

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**5つのポイントのどこかに介入する**

サイクル全体を一度に変えることはできない。1箇所に集中して「くさび」を打つ。

</div>

<div style="font-size: 0.85em;">

### 介入ポイントと戦略

| 介入点 | 具体的な戦略 | 難易度 |
|-------|------------|-------|
| **変更の困難さ** | モジュール化、テスト追加 | 高（時間がかかる） |
| **デリバリー速度** | CI/CD、小さなリリース | 中（ツールで支援可能） |
| **信頼低下** | 可視化、小さな成功の積み重ね | 中（コミュニケーション） |
| **投資減少** | ビジネス価値の可視化 | 高（経営層の理解が必要） |
| **負債の蓄積** | 技術負債の定量化、優先順位付け | 中 |

</div>

<div style="margin-top: 10px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">最初の1勝が、組織を動かす。小さく勝て。</span>
</div>

</div>

---

## 小さな成功が次を呼ぶ

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**負のサイクルを「正のサイクル」に転換する**

</div>

```
[負のサイクル]                    [正のサイクル]
変更困難 → 遅延 → 不信          小さな成功 → 信頼回復 → 投資増加
    ↑          ↓                      ↑              ↓
  負債蓄積 ← 投資減少            能力向上 ←───── 改善加速
```

### 最初の「小さな成功」を選ぶ基準

1. **ビジネスに見える成果**: 技術者だけが喜ぶ改善は避ける
2. **3ヶ月以内に達成可能**: 長すぎると信頼を失う
3. **再現可能な方法**: 一度きりの英雄的行為ではなく、プロセスとして

<div style="margin-top: 10px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>最初の成功は「証明」のため。2回目以降は「拡大」のため。</strong>
</div>

</div>

---

## 翻訳者が見てきた「繰り返される失敗」

<div style="font-size: 0.7em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

**システムを一新しても、問題を生み出した構造がそのままだと、数年後にまた同じ問題にぶつかる。** 「今度こそ」と始めたリプレースが、また新しいレガシーになる。なぜか。コードは新しくなっても、意思決定の構造、チーム間の力学、承認フローが同じだから。

</div>

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**よくあるパターン**

「技術を一新すれば解決する」と信じてフルリプレース。2年かけて新システムを構築。だが組織構造もプロセスも同じまま。3年後、新システムも同じ問題を抱え始める。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**なぜ繰り返すのか**

問題を生み出した**構造**——組織のサイロ、承認フロー、責任の曖昧さ——が変わっていないから。技術は箱を取り替えただけ。中身を詰める人間と仕組みが同じなら、同じ結果になる。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">技術の刷新は必要条件であって十分条件ではない。構造を変えなければ、歴史は繰り返す。</span>
</div>

</div>

---

## 段階的な進化のアプローチ

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 45%;">
<img src="../../assets/images/2026/architecture-modernization-parallel-streams.png" alt="Parallel Streams of Work" style="width: 100%; height: fit-content;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Figure 16.2 Parallel streams of work より引用</div>
</div>

<div style="flex: 1; font-size: 0.7em;">

**モダナイゼーションを「プロジェクト」と呼んだ瞬間、失敗が始まる。** プロジェクトには終わりがある。だが組織の進化に終わりはない。

### Portfolio-Driven Evolution

- 発見 → 設計 → デリバリー → 学習
- これらが**並行して継続的に**進む
- ビッグバンリプレースは失敗する

### 早期の価値提供

- 小さな改善を積み重ね、継続的に価値を届ける
- 学習をフィードバックして計画を調整

</div>
</div>

---

## 並行ストリームの実践

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**4つのストリームを同時に回す**

発見・設計・デリバリー・学習——これらは順番にやるものではなく、常に並行して走らせる。

</div>

### 各ストリームの活動

| ストリーム | 活動 | 成果物 |
|----------|------|-------|
| **Discovery** | リスニングツアー、マッピング | 現状の可視化、問題の特定 |
| **Design** | イベントストーミング、境界設計 | 目標アーキテクチャ |
| **Delivery** | 段階的な移行、機能開発 | 動くソフトウェア |
| **Learning** | 振り返り、メトリクス分析 | 次のイテレーションへの入力 |

<div style="margin-top: 10px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>「設計が終わってから実装」ではない。学びながら設計し、設計しながら実装する。</strong>
</div>

</div>

---

## 車輪を再発明しない勇気

<div style="font-size: 0.7em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

**すべてを自分たちで作る必要はない——「巨人の肩に乗る」原則**

多くの業務には業界横断的なベストプラクティスが既に存在する。認証、決済処理、ログ集約、監視——これらを個別に再発明することは、エンジニアリングリソースの浪費であるだけでなく、セキュリティやコンプライアンスのリスクを自ら抱え込むことでもある。

</div>

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**競争優位を生まない領域**

認証基盤、メール配信、ログ管理、CI/CDインフラ——これらは差別化の源泉ではない。ここに投資しても顧客は1円も多く払わない。**標準に乗って、浮いたリソースを差別化領域に振り向ける**のが合理的な判断。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**差別化すべき領域**

自社のドメイン知識が詰まったロジック、顧客にしか見えない独自の業務フロー、競合が真似できない判断エンジン。**Core Domainに全力を注ぐために、Generic Subdomainは外部に任せる**。これはCore Domain Chartの実践そのもの。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>「作れる」から作るのではない。「作るべきか」を問い、作らない勇気を持つ</strong>
</div>

</div>

---

## Newmanが語る「進化的アーキテクト」

<div style="font-size: 0.68em;">

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 18%;">
<img src="../../assets/images/2026/building-microservices-2nd-edition.jpeg" alt="Building Microservices, 2nd Edition" style="width: 100%; border-radius: 8px;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Sam Newman, O'Reilly, 2021</div>
</div>
<div style="flex: 1;">

Sam Newmanは **Building Microservices** の最終章（Ch.16 "The Evolutionary Architect"）で、アーキテクトの役割を再定義している。

<div style="background-color: #e0e0e0; padding: 10px; border-radius: 8px; margin-bottom: 8px; text-align: center; font-size: 1.1em;">
<strong>アーキテクトは「完璧な設計図を描く人」ではなく「変化を可能にする構造を設計する人」</strong>
</div>

<div style="background-color: #f5f5f5; padding: 10px; border-radius: 8px; margin-bottom: 8px;">

Newmanの主張は明快。**独立デプロイ可能性**こそがマイクロサービスの唯一の本質的価値であり、それが不要ならモノリスでいい。情報隠蔽と低結合を徹底し、「知りすぎない」境界を引くことが設計者の最も重要な仕事。そして最大のスケーリングコストは技術ではなく**チーム間の調整コスト**。

</div>

これは本書Architecture Modernizationの「意志を実装する」と同じ構図。Newman自身もConway's Lawを取り上げ、**組織がアーキテクチャを形作る**ことを前提にチーム設計を語っている。

<div style="margin-top: 8px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center; font-size: 1.1em;">
<span style="color: #e65100; font-weight: bold;">「進化的アーキテクト」とは「意志を持って構造を選ぶ人」のこと。</span>
<div style="font-size: 0.75em; margin-top: 4px;"><strong>完璧な設計は存在しない。変化に耐える設計だけが生き残る。それを選ぶのがアーキテクトの意志。</strong></div>
</div>

</div>
</div>

</div>

---

## 「作らない」の先に何を作るか

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

車輪の再発明をやめてリソースが浮いた。そのリソースをどこに向けるか？

**「現状維持でよい」は選択肢にない。** 効率化には理論的上限があり、効率化だけを価値とするプロダクトは、より安い代替に置き換えられる運命にある。次の問いは「何に進化させるか」。

</div>

<div style="background-color: #e0e0e0; padding: 15px; border-radius: 8px;">

→ **記録するだけの仕組み**を、**洞察を生む仕組み**に進化させること。それがモダナイゼーションの真の目的地。

</div>

</div>

---

## 記録のシステムから洞察のシステムへ

<div style="font-size: 0.7em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

**「代替されない」ことは「現状維持でよい」ことを意味しない**

効率化には理論的上限がある。入力を受けて記録するだけの System of Record（記録のシステム）は、いずれコモディティ化する。効率化だけを価値とするプロダクトは、より安いプロダクトに置き換えられる運命にある。

</div>

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**System of Record**

データを正確に記録・管理する。ERPや基幹システムの多くがここに位置する。**必要不可欠だが、差別化にはならない。**

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**System of Insight**

蓄積されたデータから未来を予測し、意思決定を支援する。「何が起きたか」ではなく「**次に何をすべきか**」を提示する。ここに事業の持続的成長がある。

</div>
</div>

モダナイゼーションの真の目的地は、単にレガシーを新しくすることではない。記録するだけのシステムを、**洞察を生むシステムに進化**させること。それは技術の入れ替えでは達成できず、ドメイン理解の深さによって決まる。

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>事業の持続的成長は技術要素ではなく、提供価値の深さによって決まる</strong>
</div>

</div>

---

## なぜ「意志」が必要なのか

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**モダナイゼーションは「正解を実行する」作業ではない**

最適なアーキテクチャ、最適なチーム構造、最適な技術選定——そんなものは存在しない。あるのは「トレードオフ」と「制約」と「不確実性」だけ。

</div>

<div style="display: flex; gap: 20px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**誰かに決めてもらうことの危険**

ベンダーの「ベストプラクティス」、コンサルタントの「業界標準」、本に書いてある「推奨アプローチ」——**自社の文脈を無視した解は機能しない**。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**意志を持つとは**

自社の強み・弱みを理解した上で選び、トレードオフを引き受ける覚悟を持ち、選択の結果に責任を持つ。**文脈を理解した者だけが正しく選べる**。そして「今のままで十分」は意志ではない。現状維持は緩やかな後退に過ぎない。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>「今のままでいい」は意志ではない。緩やかな後退に過ぎない。</strong>
</div>

</div>

---

## 失敗パターンと成功パターン

<div style="font-size: 0.7em;">

<div style="display: flex; gap: 20px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**失敗パターン**

技術だけに注力し組織を無視、ビッグバンリプレース、現場を巻き込まないトップダウン、銀の弾丸への期待、短期的な成果を求めすぎる、学習投資の軽視。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**成功パターン**

ソシオテクニカルなアプローチ、段階的・継続的な改善、協働的な設計プロセス、ビジネス成果との明確な紐づけ、長期的なコミットメント、学習文化の醸成。

</div>
</div>

<div style="margin-top: 15px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="font-size: 0.85em;">"If leaders are still looking for quick fixes, they don't understand the problem"</span></br>
<strong>リーダーがまだ銀の弾丸を探しているなら、問題を理解していない</strong>
</div>

</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: transition
-->

<div style="display: flex; flex-direction: column; justify-content: center; align-items: center; height: 80%; text-align: center;">

<div style="font-size: 1.2em; font-weight: bold; color: #fff; font-style: italic;">

"Modularization is not just about code—it's about enabling business agility"

</div>

<div style="font-size: 0.9em; margin-top: 30px; color: #aaa;">

モジュール化は単なるコードの話ではない</br>ビジネスのアジリティを実現することだ

</div>

</div>

---

## 旅を振り返る

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**たどってきた道 — 「意志を実装する」ための5つのステップ**

| ステップ | 内容 | 意志との関係 |
|---------|------|-------------|
| 1. AI時代 | AIが「How」を加速、人間は「What」「Why」へ | **意志の必要性**を理解 |
| 2. ソシオテクニカル | 技術と組織を同時に設計 | **意志の対象**を理解 |
| 3. イベントストーミング | 協働でドメインを発見 | **意志を言語化**する |
| 4. DDD × TT | 境界を揃えて自律的チームへ | **意志を構造化**する |
| 5. レガシー脱却 | 段階的・継続的に進化 | **意志を実行**する |

</div>

<div style="margin-top: 10px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">道具は揃っている。足りないのは「この組織をどうしたいか」という意志だけ。</span>
</div>

</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: transition
-->

<div style="display: flex; flex-direction: column; justify-content: center; align-items: center; height: 80%; text-align: center;">

<div style="font-size: 1.5em; font-weight: bold;">

# 6. 意志を実装するということ

</div>

</div>

---

## 翻訳者として伝えたいこと

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 20px; border-radius: 8px; margin-bottom: 15px;">

この本を訳しながら、私はずっと考えていました。なぜ同じ失敗が繰り返されるのか。なぜ「正しいこと」を知っている人がたくさんいるのに、組織は変われないのか。

</div>

<div style="background-color: #f5f5f5; padding: 20px; border-radius: 8px;">

**私の結論はこうです。モダナイゼーションの本質は、完成形を目指すことではない。変化し続ける能力を組織に埋め込むこと。**

技術を一新しても、変化に対応し続ける文化がなければ、またレガシーに戻る。逆に、変化し続ける構造があれば、技術は後からついてくる。問題は技術ではなく、組織が変化を受け入れる仕組みを持っているかどうか。

</div>

</div>

---

## 本日のまとめ

<div style="font-size: 0.72em;">

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**今日の話の核心**

AIがHowのコストを下げた結果、ボトルネックは人間の調整に移った。だからソフトウェアと組織を同時に設計する必要がある。イベントストーミングで協働的にドメインを発見し、DDDとTeam Topologiesで境界を一致させる。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**持ち帰ってほしいこと**

モダナイゼーションは「負債を返す」作業ではない。**変化し続ける能力を組織に埋め込む**作業。技術を新しくしても、構造が同じなら同じ問題が再発する。変えるべきは技術だけでなく、意思決定の構造そのもの。

</div>
</div>

<div style="margin-top: 12px; padding: 12px; background-color: #e0e0e0; border-radius: 8px; text-align: center; font-size: 1.1em;">
<span style="color: #e65100; font-weight: bold;">持ち帰るべきは手法ではない。「自分の組織をどうしたいか」という問いだ。</span>
</div>

</div>

---

## 「意志を実装する」とは

<div style="font-size: 0.7em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

冒頭で「3つの意志」を定義した。旅を終えた今、それぞれに何が見えたか。

</div>

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**ビジネスへの意志**

「何を作り、何を作らないか」を決める。Core Domainに集中し、Generic Subdomainは外部に任せる。System of Recordを超え、System of Insightへ進化させる。

→ イベントストーミング、Core Domain Chart

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**アーキテクチャへの意志**

どこに境界を引き、どう疎結合を実現するか。Bounded Contextで境界を定義し、コンテキストマッピングで関係を設計する。

→ DDD、Strangler Fig、IDP

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**組織への意志**

チームをどう分け、責任をどう渡すか。逆コンウェイ戦略でドメイン境界＝チーム境界にし、認知負荷の限界内で設計する。

→ Team Topologies、IVS

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>この3つは互いに依存している。どれか1つだけ変えても、残りが引き戻す。だから同時に設計する必要がある。</strong>
</div>

</div>

---

## AI時代に「意志」はどう変わるか

<div style="font-size: 0.7em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

**「AIがいずれWhatやWhyも判断するのでは？」——最もよく受ける質問に答えたい。**

仮にそうなったとしても、その判断を**採用する責任**は人間にしかない。AIが「このドメインは外部に委託すべき」と提案しても、その結果として起きるレイオフの責任をAIは取らない。意志とは判断することではなく、**判断の結果を引き受ける覚悟**のこと。

</div>

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**AIエージェントがいる世界の落とし穴**

「作れるから作る」が加速する。コストが低いから、考えずに試す。結果、**方向性なき大量生産**に陥る。技術的負債も倍速で溜まる。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**だからこそ「意志」が要る**

何を作らないかを決める力、どこに境界を引くかの判断、組織の方向性を定める覚悟。**AIが速く走れるからこそ、人間がハンドルを握る意味がある。**

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>速く走れるマシンほど、操縦者の判断が生死を分ける</strong>
</div>

</div>

---

## 今日から始められること

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>来週やること（30分で終わる）</strong>

チームの定例で「最も変更が怖いコード」を1つ挙げる。怖い理由を付箋に書き出す。技術の問題（テストがない、依存が複雑）か、組織の問題（担当者が不明、承認が必要）かを仕分ける。**仕分け結果がモダナイゼーションの出発点になる。**

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>来月やること（意思決定の棚卸し）</strong>

「誰かが決めてくれるのを待っている」案件を1つ特定する。「なぜ決まらないのか」を3つの観点で書き出す——権限がない、情報がない、合意できない。そのうち**自分で動かせるものから着手する**。全部は無理でも、1つ動かすだけで景色が変わる。

</div>
</div>

</div>

---

## AIと一緒にやること

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

試しにAIエージェントに「このシステムをどうモダナイズすべきか」と聞いてみる。出力はもっともらしい。だが「うちの組織では無理だな」と思う箇所が必ずある。**その「無理だな」が、あなただけが持っている文脈知。**

</div>

AIは選択肢を高速に列挙する。だが「この組織で、この時期に、このチームで、どれを選ぶか」は人間にしか決められない。AIの出力と自分の判断の差分を観察すると、自分が無意識に持っている暗黙知——組織の力学、チームの疲弊度、ステークホルダーの温度感——が言語化できる。それが意思決定の出発点になる。

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">AIは100の選択肢を並べる。あなたは1つを選び、結果を引き受ける。それが意志の実装。</span>
</div>

</div>

---

## 本日、夕方からイベントがあります

<div style="display: flex; gap: 40px; align-items: center;">
<div style="width: 40%;">
<img src="../../assets/images/2026/event_アーキテクチャモダナイゼーション.png" alt="イベント" style="width: 100%; height: fit-content;">
</div>

<div style="flex: 1; font-size: 0.7em;">

### 3-shake SRE Tech Talk 特別回

**アーキテクチャモダナイゼーション**

レガシーシステムを競争優位性に転換するための包括的ガイド。ソシオテクニカルな視点、イベントストーミング、DDD、Team Topologiesなど、本セッションで紹介した内容をより深く学べます。

https://3-shake.connpass.com/event/382086/

</div>
</div>

---

## 参考資料

<div style="font-size: 0.65em;">

- [Architecture Modernization](https://www.manning.com/books/architecture-modernization) - Nick Tune, Jean-Georges Perrin（Manning, 2024）
- [アーキテクチャモダナイゼーション](https://www.oreilly.co.jp/) - 日本語版、株式会社スリーシェイク訳
- [Team Topologies](https://teamtopologies.com/) - Matthew Skelton, Manuel Pais（IT Revolution, 2019）
- [チームの力で組織を動かす](https://gihyo.jp/book/2025/978-4-297-15064-8) - 松本成幸（技術評論社, 2025）
- [企業変革のジレンマ](https://bookplus.nikkei.com/atcl/catalog/24/06/14/01289/) - 宇田川元一（日経BP, 2024）
- [Building Microservices, 2nd Edition](https://www.oreilly.com/library/view/building-microservices-2nd/9781492034018/) - Sam Newman（O'Reilly, 2021）
- [Domain-Driven Design](https://www.domainlanguage.com/ddd/) - Eric Evans（Addison-Wesley, 2003）
- [関数型ドメインモデリング](https://asciidwango.jp/post/769073029498839040/) - Scott Wlaschin（KADOKAWA, 2025）
- [EventStorming](https://www.eventstorming.com/) - Alberto Brandolini
- [Sooner Safer Happier](https://www.soonersaferhappier.com/) - Jon Smart（IT Revolution, 2020）
- [現代システムの三体問題](https://syu-m-5151.hatenablog.com/entry/2025/01/21/124130) - Architecture Modernization 読書感想文
- [関数型ドメインモデリング 読書感想文](https://syu-m-5151.hatenablog.com/entry/2026/01/22/094654)

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
