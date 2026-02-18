---
marp: true
theme: 3shake-2026-presentation
paginate: true
---

<!--
_backgroundColor: #0a1929
_color: white
_class: title dark
-->

![bg](../../assets/images/3shake-background-full.png)

<img src="../../assets/images/3shake-logo.png" alt="3-SHAKE logo" style="position: absolute !important; top: 100px !important; left: 100px !important; width: 240px !important; height: auto !important; z-index: 9999 !important;">

<div class="title" style="text-align: left; margin-top: 100px; margin-left: 20px; padding-left: 0; max-width: 70%;">

# <span style="font-size: 1.0em;">アーキテクチャ</br>モダナイゼーション</br>とは何か</span>

### 翻訳者が全17章を訳して見えた本質

</div>

<div class="author-info" style="text-align: left; padding-left: 0; text-indent: 0;">
2026/04/10 設計ナイト 2026 武蔵野公会堂ホール</br>
@nwiizo 20min（19:10〜19:30）
</div>

---

<!-- _backgroundColor: white -->

![bg left:30% fit](../../assets/images/nwiizo_icon.jpg)
## nwiizo

<div style="font-size: 0.75em;">

株式会社スリーシェイクでプロのソフトウェアエンジニアをやっているものです。「アーキテクチャモダナイゼーション」（Manning, 2024）の翻訳者。

技術書翻訳を手がけるたび、わかることが1つ増えるのと引き換えに、わからないことが3つ増えていく。全17章を訳した体験から見えた本質をお伝えします。

インターネット上では **nwiizo** を名乗り、ブログ「**じゃあ、おうちで学べる**」を運営しています。X / GitHub もこのIDでやっています。

</div>

---

## アーキテクチャモダナイゼーションとは何か

<div style="font-size: 0.75em;">

<div style="background-color: #e0e0e0; padding: 20px; border-radius: 8px; margin-bottom: 15px; text-align: center;">
<span style="color: #e65100; font-weight: bold; font-size: 1.3em;">技術・事業・組織を同時に進化させ、</br>変化し続ける能力を組織に埋め込むこと。</span>
</div>

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

マイクロサービス化でもクラウド移行でもない。リプレースでもリライトでもない。

**ソシオテクニカル** — 技術システムと社会システム（組織・チーム・文化）を統合的に設計する思想。Sam Newmanが言うように「**ソフトウェアを変えるだけでは足りない。組織も同時に変えなければ、同じ問題が形を変えて再現される**」。

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

技術で大事なこと

</div>
</div>

---

## 疎結合 — 独立デプロイ可能性が唯一の本質的価値

<div style="font-size: 0.72em;">

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 35%;">
<img src="../../assets/images/2026/figure-12-1-loose-coupling.png" alt="疎結合アーキテクチャ" style="width: 100%; height: fit-content;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Figure 12.1 Loosely coupled architecture より引用</div>
</div>

<div style="flex: 1;">

**Newman: 「独立デプロイ可能でなければ、マイクロサービスにする意味がない」**

疎結合の本質は技術的な美しさではなく、**チームが他のチームに依存せずに変更をリリースできること**。

<div style="display: flex; gap: 12px; align-items: center; margin-top: 10px;">
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

**4パターン**: 非同期メッセージング、APIゲートウェイ、イベント駆動、データ所有権分離

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

**問い**: 「このサービスを独立してデプロイできるか？」

</div>
</div>

</div>
</div>

</div>

---

## Bounded Context — 境界を正しく引く

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 35%;">
<img src="../../assets/images/2026/figure-9-2-bounded-context.png" alt="Bounded Context" style="width: 100%; height: fit-content;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Figure 9.2 Bounded Context and its relationships より引用</div>
</div>

<div style="flex: 1; font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

**翻訳チームでの証明体験**

全17章の翻訳をチームで分担した。章ごとに用語の意味が微妙に異なることに気づいた。「Context」という言葉ひとつとっても、技術文脈・ビジネス文脈・組織文脈で指すものが違う。

</div>

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**Bounded Contextは「翻訳の境界」**

同じ言葉が異なる意味を持つ境界こそが、システムを分割すべき場所。書籍のCh.9では、ドメイン境界を見つける**9つのヒューリスティック**が紹介されている。

</div>

</div>
</div>

---

## 関数型ドメインモデリング — 型で境界を守る

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**ドキュメントに書いても破られる。コードレビューで指摘しても漏れる。**

イベントストーミングでドメインを発見し、Bounded Contextで境界を定義した。だが、その境界をどう守るか？ 答えの一つが**コンパイラに守らせる**こと。

</div>

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

Scott Wlaschin「関数型ドメインモデリング」が提唱する **Make Illegal States Unrepresentable** —— 不正な状態を型レベルで表現不可能にするアプローチ。境界を越えるデータは必ず型変換を通る。型が合わなければコンパイルが通らない。

</div>

<div style="padding: 12px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">境界は「約束」で守るな。「型」で守れ。</span>
</div>

</div>

---

## 段階的移行 — Strangler Figパターン

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**ビッグバンは負債の再生産**

「一気に作り直す」は最もリスクの高い選択肢。2年間のフルリライトが完了した頃には、ビジネス要件が変わっている。

</div>

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**Strangler Figパターン**

既存システムの周囲に新しいシステムを段階的に構築し、徐々に古いシステムを置き換える。Martin Fowlerが命名したこのパターンは、リスクを最小化しながら進化を可能にする。**各ステップでビジネス価値を提供しながら移行する。**

</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">技術選定の前にドメイン戦略。</span>
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

事業で大事なこと

</div>
</div>

---

## Core Domain — 差別化すべき領域の見極め

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 35%;">
<img src="../../assets/images/2026/core-domain-chart.png" alt="Core Domain Chart" style="width: 100%; height: fit-content;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Core Domain Chart</div>
</div>

<div style="flex: 1; font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

**すべてのドメインが等しく重要ではない**

Eric Evansが提唱したCore Domainの概念。差別化度と複雑性の2軸で全ドメインを整理し、**どこに投資し、どこを手放すか**を判断する。

</div>

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

設計者が知るべきこと——**最も美しい設計が必要なのはCore Domainだけ**。Generic Subdomainに凝った設計を施すのは、差別化に使えるエネルギーの浪費。

</div>

</div>
</div>

---

## なぜプロダクトタクソノミーが必要か

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 35%;">
<img src="../../assets/images/2026/figure-6-6-product-taxonomy.png" alt="Product Taxonomy" style="width: 100%; height: fit-content;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Figure 6.6 A product taxonomy より引用</div>
</div>

<div style="flex: 1; font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

**「うちの製品は何個あるのか」——この問いに即答できる組織は少ない**

プロダクトタクソノミーは、組織が持つ製品・サービスを体系的に分類・整理する手法。モダナイゼーションの対象を明確にする前提条件。

</div>

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**設計者にとっての意味**

システム境界を引く前に、ビジネス上の製品境界を理解する必要がある。技術的なモジュール分割がビジネスの製品境界と一致しなければ、設計は常に摩擦を生む。

</div>

</div>
</div>

---

## Wardley Mapping — 進化段階に応じた戦略

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**すべてのコンポーネントは進化する**

Simon WardleyのWardley Mapは、ビジネスコンポーネントの進化段階（Genesis → Custom → Product → Commodity）を可視化する。進化段階に応じて、**設計方針も変わるべき**。

</div>

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**Genesis/Custom**

不確実性が高い。実験的な設計が求められる。変更しやすさを優先。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

**Product/Commodity**

安定している。標準化と効率性を優先。自前で持つ必要があるか疑う。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">「作れる」から作るのではない。「作るべきか」を問う。</span>
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

組織で大事なこと

</div>
</div>

---

## ソシオテクニカル — 技術と組織は同時に設計する

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 35%;">
<img src="../../assets/images/2026/figure-2-1-sociotechnical.png" alt="Sociotechnical" style="width: 100%; height: fit-content;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Figure 2.1 Sociotechnical systems より引用</div>
</div>

<div style="flex: 1;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

**Sam Newman: 「ソフトウェアシステムとそれを構築・運用する組織は、共進化する単一のシステム」**

</div>

技術システムだけを最適化しても、組織が変わらなければ効果は一時的。組織だけを変えても、技術が追いつかなければ絵に描いた餅。**両方を同時に、整合性を持って動かす**のがソシオテクニカルアプローチ。

</div>
</div>

</div>

---

## コンウェイの法則 — 組織構造がアーキテクチャを決める

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 35%;">
<img src="../../assets/images/2026/figure-2-1-conway-law.png" alt="Conway's Law" style="width: 100%; height: fit-content;">
</div>

<div style="flex: 1;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

**「組織はそのコミュニケーション構造を反映したシステムを設計する」**（Melvin Conway, 1968）

</div>

設計者として知るべきことは2つ。第一に、どれだけ美しい設計を描いても、組織のコミュニケーション構造に反していれば実装されない。第二に、**逆コンウェイ戦略** — 望ましいアーキテクチャに合わせて組織を設計すれば、アーキテクチャは自然とそこに収束する。

</div>
</div>

</div>

---

## Team Topologies — 認知負荷を制約にチームを設計する

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 30%;">
<img src="../../assets/images/2026/team-topologies-four-types.png" alt="Team Topologies" style="width: 100%; height: fit-content;">
</div>

<div style="flex: 1; font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

**チームの認知負荷を超えない範囲にシステムを分割する**

Matthew Skelton & Manuel Paisが提唱した4つのチームタイプと3つのインタラクションモード。設計者にとって重要なのは、**システムの境界 = チームの境界**という原則。

</div>

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

「このシステムをいくつに分割するか」の答えは「チームがいくつあるか」に依存する。**認知負荷** — あるチームが理解・保守できる範囲 — が唯一の制約。

</div>

</div>
</div>

---

## 「組織図を書き換えるだけ」では失敗する

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**構造とプロセスの誤謬（Ch.2）**

組織図を変えれば組織が変わると信じるのは「構造の誤謬」。新しいプロセスを導入すれば行動が変わると信じるのは「プロセスの誤謬」。どちらも、**変化を「仕組み」に外注しようとする**点で同じ間違いを犯している。

</div>

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

実際に必要なのは、チーム間のインタラクションを意図的に設計し、**学習と信頼を積み重ねるプロセス**を組織に埋め込むこと。構造もプロセスも手段であって、目的は「チームが自律的に価値を提供できる状態」。

</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">組織を変えずにアーキテクチャだけ変えたいは幻想。</span>
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

3つを同時に動かすということ

</div>
</div>

---

## 3つの軸は独立していない

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**技術の軸**

疎結合、Bounded Context、型による境界の保護、段階的移行

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**事業の軸**

Core Domain、プロダクトタクソノミー、Wardley Mapping、ビジネスケース

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**組織の軸**

ソシオテクニカル、コンウェイの法則、Team Topologies、学習する組織

</div>
</div>

<div style="margin-top: 15px; background-color: #e0e0e0; padding: 15px; border-radius: 8px; text-align: center;">

技術だけ変えれば？ → 組織のサイロがアーキテクチャに再現される
事業だけ変えれば？ → 技術が追いつかず絵に描いた餅
組織だけ変えれば？ → 何のために変えたのか見失う

<span style="color: #e65100; font-weight: bold; font-size: 1.1em;">3つを同時に動かすのがモダナイゼーション。</span>

</div>

</div>

---

## 翻訳者として伝えたいこと

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**「変化し続ける能力を組織に埋め込む」**

全17章を訳して辿り着いた結論はシンプルだった。モダナイゼーションの目的地は「完成したアーキテクチャ」ではない。**変化し続ける能力そのもの**。

</div>

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

1つ理解するたびに3つの未知が生まれた。DDDを訳せばTeam Topologiesが必要になり、Team Topologiesを訳せばWardley Mappingが必要になり、Wardley Mappingを訳せばビジネス戦略の知識が必要になった。**すべてが繋がっている。だからこそ、同時に動かす必要がある。**

</div>

</div>

---

## 日本の製造業のカイゼンとソフトウェア設計の親和性

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

**訳者まえがきで書いたこと**

日本の製造業が世界に示した「カイゼン」の哲学は、小さな改善を継続的に積み重ね、品質を維持し続けるアプローチ。これはソフトウェアにおけるモダナイゼーションの思想と驚くほど親和性が高い。

</div>

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**革命ではなく進化。** ビッグバンリライトは日本の「一気に変える」文化に合致するように見えて、実はカイゼンの精神に反している。小さく始め、学び、改善する——この繰り返しの中でしか、持続的な変化は生まれない。

</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold; font-size: 1.1em;">技術・事業・組織——3つの軸で、小さくカイゼンし続けよ。</span>
</div>

</div>

---

## 参考資料

<div style="font-size: 0.65em;">

- [Architecture Modernization](https://www.manning.com/books/architecture-modernization) - Nick Tune, Jean-Georges Perrin（Manning, 2024）
- [アーキテクチャモダナイゼーション](https://www.oreilly.co.jp/) - 日本語版、株式会社スリーシェイク訳
- [Team Topologies](https://teamtopologies.com/) - Matthew Skelton, Manuel Pais（IT Revolution, 2019）
- [Building Microservices, 2nd Edition](https://www.oreilly.com/library/view/building-microservices-2nd/9781492034018/) - Sam Newman（O'Reilly, 2021）
- [Domain-Driven Design](https://www.domainlanguage.com/ddd/) - Eric Evans（Addison-Wesley, 2003）
- [関数型ドメインモデリング](https://asciidwango.jp/post/769073029498839040/) - Scott Wlaschin（KADOKAWA, 2025）
- [Wardley Maps](https://learnwardleymapping.com/) - Simon Wardley
- [EventStorming](https://www.eventstorming.com/) - Alberto Brandolini

</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: title dark
-->

![bg](../../assets/images/3shake-background-full.png)

<img src="../../assets/images/3shake-logo.png" alt="3-SHAKE logo" style="position: absolute !important; top: 100px !important; left: 100px !important; width: 240px !important; height: auto !important; z-index: 9999 !important;">

<div class="title" style="text-align: left; margin-top: 150px; margin-left: 20px; padding-left: 0; max-width: 70%;">

# ありがとうございました

### @nwiizo

</div>

<div class="author-info" style="text-align: left; padding-left: 0; text-indent: 0;">
設計ナイト 2026 / 2026-04-10</br>
アーキテクチャモダナイゼーションとは何か
</div>
