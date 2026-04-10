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

<div class="title" style="text-align: left; margin-top: 100px; margin-left: 80px; padding-left: 0; max-width: 70%;">

# <span style="font-size: 1.0em;">アーキテクチャ</br>モダナイゼーション</br>とは何か</span>

### 技術・事業・組織で大事なこと

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

株式会社スリーシェイクでプロのソフトウェアエンジニアをやっているものです。Nick Tune「アーキテクチャモダナイゼーション」（翔泳社, 2026）の翻訳に携わりました。

インターネット上では <strong>nwiizo</strong> を名乗り、ブログ「<strong>じゃあ、おうちで学べる</strong>」を運営しています。X / GitHub もこのIDでやっています。

</div>

---

## この発表の軸になる書籍

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 30px; align-items: center;">
<div style="width: 30%;">
<img src="../../assets/images/2025/forkwell-library-pek/book_architecture_modernization.png" alt="Architecture Modernization" style="width: 100%;">
</div>

<div style="flex: 1;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

本発表の軸になる2冊。翻訳に携わった Nick Tune「<strong>アーキテクチャモダナイゼーション</strong>」（翔泳社, 2026）が<strong>技術・事業・組織の3軸で同時に動かす</strong>全体像を示し、Susanne Kaiser「<strong>Architecture for Flow</strong>」（Addison-Wesley, 2025）が<strong>Wardley Mapping × DDD × Team Topologiesを統合する</strong>実装論を展開する。

</div>

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

翻訳者として原著と向き合ってきた立場から、書籍の紹介ではなく自分の言葉で語る。技術・事業・組織それぞれで「よくある語られ方」がどう構造的に間違っているのか、そしてモダナイゼーションとは何かを伝える。

</div>

</div>
</div>

</div>

---

## 今日話すこと

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 20px; border-radius: 8px; text-align: center;">

<strong>技術</strong>

何を変えるか

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 20px; border-radius: 8px; text-align: center;">

<strong>事業</strong>

なぜ変えるか

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 20px; border-radius: 8px; text-align: center;">

<strong>組織</strong>

誰が変えるか

</div>
</div>

<div style="text-align: center; font-size: 1.5em; margin: 8px 0;">↓</div>

<div style="padding: 15px; background-color: #e0e0e0; border-radius: 8px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">3つを同時に動かすということ</span>
<div style="margin-top: 8px;">1つだけ動かせば、残り2つが設計図を上書きする</div>
</div>

</div>

---

## この発表で持ち帰れること

<div style="font-size: 0.75em;">

<div style="background-color: #e0e0e0; padding: 20px; border-radius: 8px; margin-bottom: 15px; text-align: center;">
<span style="color: #e65100; font-weight: bold; font-size: 1.2em;">「モダナイゼーション」の実体を掴み、</br>雰囲気に流されない設計判断の軸を持ち帰る。</span>
</div>

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

技術・事業・組織の3つを同時に考えなければ、どんな施策も一部分の改善にしかならない。しかし多くの組織では、わかりやすい説明に「なるほど」と頷いた瞬間に、自分たちの現状を正直に見ることを省略している。雰囲気ではなく構造で判断する視点を持ち帰ってほしい。

</div>

</div>

---

## この発表の限界

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

本日紹介する「技術・事業・組織を同時に動かす」という考え方は、<strong>すべての組織にそのまま当てはまるわけではありません。</strong>

</div>

<div style="display: flex; gap: 20px; align-items: stretch;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>文脈によって3軸のバランスは変わる</strong>

スタートアップで技術から始めるか、大企業で組織から始めるか、規制産業で事業制約から始めるか——出発点は組織ごとに違う。「3軸を完璧に同時に」という読み方は誤読。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>想定される反論に先に答えておく</strong>

「3つ同時に動かす余裕がない」→ 余裕がないからこそ、動かした1軸を残り2軸が上書きする構造を知っておく価値がある。「理想論だ」→ 理想型ではなく、自組織の盲点を可視化する装置として使ってほしい。

</div>
</div>

<div style="margin-top: 15px; padding: 12px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>「3軸同時」は完璧解ではない。自組織の盲点を可視化する装置として持ち帰ってほしい</strong>
</div>

</div>

---

## 始める前に

<div style="font-size: 0.8em;">

<div style="background-color: #f5f5f5; padding: 25px; border-radius: 8px; margin-top: 30px;">

<div style="text-align: center; font-size: 1.1em; line-height: 1.8;">

<strong>事情でレガシーシステムと呼ばれ</strong></br>
<strong>敵にされる全てのシステムに敬意を表します。</strong>

</div>

<div style="margin-top: 20px; font-size: 0.9em; color: #666;">

それらのシステムは、かつて誰かが全力で作り、ビジネスを支え、価値を生み出してきた。

<strong>敬意の次に必要なのは責任。</strong> 今日書くコードも、いつかレガシーになる。その時に「あの時代の制約の中で、よくここまで作った」と言われるか、「なぜこんな設計にしたのか」と言われるか。それを決めるのは今日の選択。

</div>

</div>

</div>

---

## 技術の「よくある語られ方」

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

「マイクロサービスにすれば速くなる」「疎結合にしよう」「コンテナ化すれば運用が楽になる」。手段の名前を出した瞬間に、なぜそうすべきかの診断が消える。聴衆が頷いた時点で、思考停止が完成する。「マイクロサービス」という言葉自体が目的化し、なぜその構造が必要かという問いが蒸発した。これは技術の問題ではなく、<strong>思考の構造の問題</strong>。

</div>

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

なぜこうなるのか。技術の名前には「正解感」がある。「マイクロサービス」と言えば先進的に聞こえ、「モノリス」と言えば遅れている印象を与える。しかし実際には、モノリスが正解な文脈は多い。問題は技術そのものではなく、<strong>名前が持つ印象で判断を代替してしまう</strong>こと。名前は思考の入口であって、結論ではない。

</div>

</div>

---

## ベストプラクティスは文脈を剥がした処方箋

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

成功事例には構造的な偏りがある。移行に成功した企業は登壇するが、失敗した企業は沈黙する。聞こえてくるのは常に生き残った側の声だけ。ベンダーも同じで、導入事例に載るのは成功した顧客だけ。<strong>「〇〇社もやっている」は技術判断の根拠にならない。</strong>

</div>

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

文脈とは何か。チームの規模、システムの複雑さ、エンジニアの経験、事業のフェーズ。これらが違えば、同じ技術が毒にもなる。100人で成功した分散アーキテクチャが、10人のチームで同じ効果を出す保証はない。<strong>処方箋を借りるなら、まず自分の症状を診断すべき。</strong>

</div>

<div style="padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">技術の名前が出た瞬間に「なぜ」が消えるなら、それは選定ではなく信仰。</span>
</div>

</div>

---

## 事業の「よくある語られ方」

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

「DXを推進する」「モダンなアーキテクチャへ移行する」「売上20%増」「リリース速度2倍」。これらは目標であって戦略ではない。現状の正直な分析を省略し、数字の裏づけがない標語を掲げる。「悪い戦略」の本質はまさにこの構造にある。診断を省略して目標だけを並べた瞬間に、<strong>戦略ごっこが始まる。</strong>

</div>

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

「売上を上げたい」は誰でも言える。戦略とは「なぜ今の売上がこの水準なのか」を分析し、そこから「何を変えれば動くのか」を特定すること。目標だけを掲げるリーダーは、組織に方向性ではなく<strong>雰囲気</strong>を提供している。そして雰囲気は、最初の困難で蒸発する。

</div>

</div>

---

## 判断基準なき選定は同調圧力にすぎない

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

技術選定の会議で「好き嫌い」の議論が延々と続くのは、判断基準がないから。「うちもAIを入れないと取り残される」「クラウドネイティブにしないと」。市場の進化段階も自社の立ち位置も分析せず、<strong>隣の会社がやっているからという理由で投資判断する組織に、戦略はない。</strong> あるのは同調圧力だけ。

</div>

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

「隣がやっているから」は判断ではなく反射。自社の顧客が何に困っているか、システムのどこが詰まっているか。この問いに答えずに技術を選ぶのは、<strong>症状を見ずに薬を飲むのと同じ</strong>。しかも組織はこの反射を「迅速な意思決定」と呼び替えてしまう。

</div>

<div style="padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">診断を省略した目標設定は、速いのではない。雑なだけ。</span>
</div>

</div>

---

## 組織の「よくある語られ方」

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

「Spotifyモデルを導入した」「Team Topologiesに基づいて再編した」。他社の組織構造をコピーするのは、他社のアーキテクチャをコピーするのと同じ問題がある。その構造が機能した背景——事業フェーズ、人材、技術の成熟度——が違えば、同じ結果にはならない。<strong>組織設計もまた、診断なきコピーは機能しない。</strong>

</div>

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

組織図に描かれているのは報告ラインと責任範囲だけ。実際に仕事を動かしているのは、組織図に載らない非公式なつながり——雑談で生まれる信頼、Slackの横チャンネルで起きる調整、「あの人に聞けばわかる」という暗黙知。<strong>他社の組織図をコピーしても、この非公式な層はついてこない。</strong>

</div>

</div>

---

## 仕組みの導入で人は変わらない

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

「チームを再編しよう」「アジャイルを導入しよう」「心理的安全性を高めよう」。組織変革が語られるとき、ほぼ確実に仕組みの導入として提示される。しかし組織図を書き替えた翌日から変わるのは報告ラインだけで、人と人の間の信頼関係や暗黙の意思決定経路はそのまま残る。スクラムを入れてもレトロスペクティブで本音が出なければ改善は起きない。1on1を制度化しても上司が評価者である限り、部下は防御的になる。

</div>

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

「心理的安全性が大事」と掲げながら失敗を評価で罰する組織は、変革の最も困難な部分——<strong>人間の行動様式が実際に変わること</strong>——を省略している。仕組みは行動を変えない。行動を変えるのは、仕組みの運用に込められた意図と一貫性。制度の文言ではなく、制度をどう使うかの日常が組織文化を作る。

</div>

<div style="padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">組織変革を語るとき、変えようとしているのは箱と線か、人間の行動か。</span>
</div>

</div>

---

## アーキテクチャモダナイゼーションとは何か

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 35%;">
<img src="../../assets/images/2026/figure-1-1-why-modernize.png" alt="Why Modernize" style="width: 100%; height: fit-content;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Figure 1.1 Why modernize より引用</div>
</div>

<div style="flex: 1;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 10px;">

マイクロサービス化でもクラウド移行でもない。リプレースでもリライトでもない。ここまで見てきた「よくある語られ方」には共通の構造がある。技術は手段を目的化し、事業は診断を省略し、組織は仕組みに変化を外注する。いずれも<strong>3つの軸のうち1つだけを語り、残りを無視している</strong>。

</div>

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

では何か。技術・事業・組織の現状をそれぞれ正直に診断し、3つを同時に動かすこと。どの境界でシステムを切るか、どの領域に投資すべきか、どのチームが何を担うか。これらは別々の問いに見えて、実は1つの設計判断の3つの側面。

</div>

</div>
</div>

<div style="margin-top: 12px; background-color: #e0e0e0; padding: 15px; border-radius: 8px; text-align: center;">
<span style="color: #e65100; font-weight: bold; font-size: 1.2em;">ゴールを描くことではない。</br>技術・事業・組織を同時に見直し続ける営みそのもの。</span>
</div>

</div>

---

## なぜ「同時に」でなければならないのか

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

3つの軸は相互に依存している。境界設計（技術）はチーム設計（組織）を規定し、チーム設計はどの領域に投資するか（事業）に制約され、投資判断は境界の引き方に影響する。<strong>円環的な関係にあるから、1つだけ動かしても残り2つが元に引き戻す。</strong>

</div>

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

「同時に」とは「すべてを一度に完璧に変える」という意味ではない。<strong>診断の段階で3つの軸を視野に入れ、変更の影響が他の軸にどう波及するかを意識しながら、小さく動かす</strong>ということ。技術だけの改善計画、事業だけの戦略資料、組織だけの再編提案——これらが別々に作られている組織は、すでに一部分の改善にしかならない構造に嵌まっている。

</div>

</div>

---

## なぜ3つが同時に語られることがないのか

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

同時に動かすべきだとわかっていても、実際にそう語られることはほとんどない。理由は単純で、<strong>3つを同時に扱うのは個別に語るよりはるかに難しい</strong>から。技術の話は技術者が、事業の話は経営者が、組織の話は人事やマネージャーがそれぞれの専門領域として語る。専門が深くなるほど、他の軸は視野の外に出ていく。

</div>

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

これは物理学の三体問題に似ている。2つの天体の運動は解析的に解ける。しかし<strong>3つになった瞬間に、相互作用が複雑になりすぎて一般解が存在しなくなる</strong>。技術と事業の2軸なら語れる。事業と組織の2軸でも語れる。しかし3つが相互に影響し合う現実を、1つの言語体系で記述するのは本質的に難しい。

</div>

<div style="padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">3つを同時に語れる言葉は、まだ誰も持っていない。だから組織は、3つを別々に語る分業を生む。</span>
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

## 疎結合の本質と結合の3次元

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 35%;">
<img src="../../assets/images/2026/figure-12-1-loose-coupling.png" alt="疎結合アーキテクチャ" style="width: 100%; height: fit-content;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Figure 12.1 Loosely coupled architecture より引用</div>
</div>

<div style="flex: 1;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

<strong>マイクロサービスは目標ではなく手段。</strong> 本質は、各部分を他の部分に影響なく変更・デプロイできること。答えがモノリスでも、それが正しいなら正しい。問うべきは<strong>「独立してデプロイ・進化できる単位はどこか？」</strong>。

</div>

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>「疎結合にすれば良い」も思考停止。</strong> 結合には<strong>強度・距離・変動性</strong>の3つの次元がある。APIで分割しても、片方を変更するたびに相手も変更が必要なら、それは距離が遠い密結合。<strong>結合の3次元を見ずに分割すると、複雑さが増すだけ。</strong>

</div>

</div>
</div>

</div>

---

## 同じ言葉が違う意味を持つ場所に境界がある

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 35%;">
<img src="../../assets/images/2026/figure-9-2-bounded-context.png" alt="Bounded Context" style="width: 100%; height: fit-content;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Figure 9.2 Bounded Context and its relationships より引用</div>
</div>

<div style="flex: 1;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 10px;">

「ユーザー」という単語は、認証チームにとってはクレデンシャルの集合であり、課金チームにとっては請求先であり、サポートチームにとっては問い合わせ元。同じ単語が異なる意味を持つ場所に、システムの自然な切れ目がある。この「意味が通じる範囲」を明確に区切る設計単位を、Bounded Contextと呼ぶ。

</div>

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

この「言葉の境界」を無視してシステムを統合すると何が起きるか。全チームが「ユーザー」テーブルを共有し、あるチームの変更が別チームを壊す。データモデルの変更に全員の合意が必要になり、リリースが遅くなる。<strong>共有データベースは、境界を引かなかった代償。</strong>

</div>

</div>
</div>

</div>

---

## 境界は4つ揃って初めて機能する

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

Bounded Contextには4つの側面がある。<strong>言葉の境界</strong>（同じ単語が違う意味を持つ場所）、<strong>意味の境界</strong>（ビジネスルールが異なる場所）、<strong>所有権の境界</strong>（誰がコードを変更できるか）、<strong>物理的な境界</strong>（デプロイの単位）。この4つが揃って初めて、境界が実効性を持つ。

</div>

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

言葉の境界だけ引いても、コードの持ち主が曖昧なら「誰がこれを直すのか」で毎回揉める。サービスを分離しても、チーム構造と合っていなければ境界は形だけになる。全部を一度に直したくなるが、<strong>今いちばん足を引っ張っている境界から着手する</strong>。

</div>

<div style="padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">4つの境界が揃わない分割は、複雑さを分散させただけで消していない。</span>
</div>

</div>

---

## ボトルネック以外の改善は幻想である

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

境界を引いたら、次に問うべきは「どこから手をつけるか」。制約理論（TOC）の考え方が効く。最も遅い工程がライン全体の速度を決める。デプロイパイプラインが詰まっていれば、各チームの開発速度をいくら上げても意味がない。

</div>

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

ではボトルネックをどう見つけるか。Value Stream Mappingで価値の流れを可視化し、<strong>価値を生む活動と待ち時間を分離</strong>する。やってみると、リードタイムの大半が「待ち」であることに気づく。ハンドオフ、承認待ち、環境構築待ち。技術的な複雑さより、<strong>人と人の間の待ち時間</strong>こそが最大の敵であることが多い。

</div>

<div style="padding: 12px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">ボトルネック以外の改善が「成果」に見えるのは、測っていないから。</span>
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

## すべてのドメインに等しく投資してはならない

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 35%;">
<img src="../../assets/images/2026/core-domain-chart.png" alt="Core Domain Chart" style="width: 100%; height: fit-content;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Core Domain Chart</div>
</div>

<div style="flex: 1; font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

ドメインは3つに分類できる。<strong>Core</strong>（差別化の源泉）、<strong>Supporting</strong>（専門的だが差別化にはならない）、<strong>Generic</strong>（認証、メール配信など汎用的なもの）。<strong>最も美しい設計が必要なのはCore Domainだけ。</strong> Genericに凝った設計を施すのは、差別化に使えるエネルギーの浪費。

</div>

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

ただしポートフォリオの見直しは一度きりではない。重要なのは<strong>一過性（Transience）の原則</strong>。市場が変われば昨日のCoreが今日のCommodityになり、規制変更でGenericが突然Core化することもある。<strong>「うちの製品は何個あるのか」「各ドメインは今どの進化段階にあるのか」。この問いに即答できない組織は、投資判断の根拠を持っていない。</strong>

</div>

</div>
</div>

---

## 価値の流れで組織を切る

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 35%;">
<img src="../../assets/images/2026/figure-6-1-value-stream-activities.png" alt="Value Stream Activities" style="width: 100%; height: fit-content;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Figure 6.1 Value stream activities より引用</div>
</div>

<div style="flex: 1;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 10px;">

ドメインを分類したら、次は価値の流れ（バリューストリーム）を設計する。バリューストリームとは、アイデアが顧客に届くまでの一連の活動。独立したバリューストリームが持つべき特性は4つ——<strong>ドメインとの整合、成果への責任、チームへの権限付与、ソフトウェアの分離</strong>。

</div>

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

この4つが揃っていないと何が起きるか。ドメインを跨いだバリューストリームは調整コストが爆発する。成果ではなくアウトプット（コード行数、デプロイ回数）で測るチームは、ビジネス価値と切り離される。権限のないチームは承認待ちで止まる。<strong>高速なフローは、バリューストリームの独立性から生まれる。</strong>

</div>

</div>
</div>

</div>

---

## 進化段階を可視化する

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 40%;">
<img src="../../assets/images/2026/figure-5-7-wardley-map.png" alt="Wardley Map" style="width: 100%; height: fit-content;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Figure 5.7 Wardley map より引用</div>
</div>

<div style="flex: 1;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 10px;">

どの領域に投資すべきかがわかっても、「どう投資すべきか」は進化段階で変わる。Wardley Mapは縦軸にバリューチェーン（顧客に近い→遠い）、横軸に進化段階（発明→汎用化）をとり、ビジネスコンポーネントの現在地を可視化する。

</div>

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

技術選定の会議で「好き嫌い」の議論が延々と続くのは、この地図がないから。コンポーネントの進化段階がわかれば、<strong>「この段階でこの方法論は適切か」</strong>という判断基準が生まれる。<strong>直感ではなく、景観の理解に基づいた意思決定ができるようになる。</strong>

</div>

</div>
</div>

</div>

---

## 進化段階が変われば方法論も変わる

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

重要なのは、<strong>進化段階ごとに適切な方法論が異なる</strong>という点。発明段階のコンポーネントに標準化を求めれば創造性が死に、汎用化した領域に実験を続ければ無駄なコストが膨らむ。同じ組織の中でも、領域によって異なるマインドセットが必要になる。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>発明〜カスタム構築 — 探索者のマインドセット</strong>

まだ何が正解かわからない領域。新規事業や新機能の立ち上げが該当する。<strong>小さく試して、早く失敗して、そこから学ぶ</strong>。この段階で標準化や効率化を求めると、答えが見つかる前に選択肢を潰してしまう。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>製品化〜汎用化 — 都市計画者のマインドセット</strong>

すでに正解がわかっている領域。給与計算や決済処理のように、正確性と安定性が絶対の領域が該当する。<strong>標準化と効率化で品質を上げる</strong>。この段階で「もっと実験しよう」は無駄なコストを生むだけ。

</div>
</div>

</div>

---

## 作れるからといって作るな

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

問うべきは「作れるか」ではない。<strong>「この進化段階で内製し続ける経済合理性があるか」</strong>。人件費、機会コスト、改善コストの3つを見れば、汎用化した領域では外部に任せた方が合理的な構造がある。

</div>

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

マネージドサービスの価値は「今の機能」ではなく<strong>「改善が勝手に続くこと」</strong>。自社で運用すれば改善コストは全部自分持ちだが、外部に任せればコストは利用者全体で分担される。差別化にならない領域は外に出し、Core Domainにエンジニアを集中させる。

</div>

<div style="margin-top: 10px; padding: 8px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">内製の誘惑は「作れる」という自信から来る。しかし作れることと、作り続ける経済合理性は別の問い。</span>
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

## コンウェイの法則は設計図を上書きする

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 35%;">
<img src="../../assets/images/2026/figure-2-1-sociotechnical.png" alt="Sociotechnical" style="width: 100%; height: fit-content;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Figure 2.1 Sociotechnical systems より引用</div>
</div>

<div style="flex: 1;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

<strong>「組織はそのコミュニケーション構造を反映したシステムを設計する」</strong>（Melvin Conway, 1968）。どれだけ美しい設計を描いても、組織のコミュニケーション構造に反していれば実装されない。3つのチームが1つのシステムを担当すれば、そのシステムは3つのモジュールに分裂する。意図したかどうかに関係なく。

</div>

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

この法則は避けるものではなく、利用するもの。<strong>逆コンウェイ戦略</strong>とは、望ましいアーキテクチャに合わせて組織を設計し、アーキテクチャを自然にそこへ収束させること。Bounded Contextの境界とチームの境界を一致させる。この整合があって初めて、高速なフローが成立する。

</div>

</div>
</div>

</div>

---

## 境界とチームが一致しないとき何が起きるか

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

境界をきれいに引いても、チーム構造がそれと合っていなければ、変更のたびにチーム間の調整が入る。ある機能を変えるのに3チームの合意が必要——これが<strong>組織的な結合</strong>。コードの結合度は低いのにリリースが遅い。原因はコードではなく、人と人の間にある。

</div>

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

技術だけ直しても、組織が変わらなければ効果は一時的。組織だけ変えても、技術が追いつかなければ絵に描いた餅。<strong>両方を同時に、整合性を持って動かす</strong>。境界設計とチーム設計を別々に進めた瞬間に、整合性は崩れ始める。

</div>

<div style="padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">コードの結合度が低くても、組織の結合度が高ければ、速度は出ない。</span>
</div>

</div>

---

## 認知負荷がチーム設計の制約

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

「このシステムをいくつに分割するか」の答えは「チームがいくつあるか」に依存する。<strong>認知負荷</strong>——チームが頭の中に保持しなければならない情報量——こそが唯一の制約。担当範囲が頭に入りきらなくなったとき、それは分割のサインであり、人を増やすサインではない。

</div>

<div style="display: flex; gap: 12px; align-items: stretch;">
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>課題内在性負荷</strong>

ドメイン自体の複雑さ。避けられない。<strong>この負荷と向き合うことこそがチームの仕事</strong>。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>課題外在性負荷</strong>

ツール、インフラ、プロセスの複雑さ。<strong>チーム設計で減らせる負荷</strong>。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>学習関連負荷</strong>

新しいことを学ぶための負荷。<strong>他の負荷が高いと余裕がなくなる</strong>。

</div>
</div>

</div>

---

## 課題外在性負荷を減らすのがチーム設計の仕事

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

プラットフォームチームがデプロイやモニタリングの複雑さを引き受ければ、他のチームはビジネスロジックに集中できる。「デプロイの仕方がわからない」「モニタリングの設定で半日潰れた」——この種の負荷は課題外在性負荷であり、チーム設計で取り除ける。

</div>

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

課題外在性負荷が高い組織では、チームの時間とエネルギーが課題内在性負荷（ドメインの本質）ではなく、ツールとの格闘に消費される。さらに学習関連負荷（新しいことを学ぶ余裕）もなくなり、チームは現状維持で精一杯になる。負荷が適切な範囲にあるとき、速度を上げることが品質向上に直結する。

</div>

<div style="padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">「速度と品質のトレードオフ」に見えるものは、実は認知負荷の設計の問題。</span>
</div>

</div>

---

## 4つのチームタイプと3つのインタラクションモード

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 35%;">
<img src="../../assets/images/2026/figure-11-10-team-interactions.png" alt="Team Interactions" style="width: 100%; height: fit-content;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Figure 11.10 Team interaction modes より引用</div>
</div>

<div style="flex: 1;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 10px;">

4つのチームタイプ——<strong>ストリームアラインド、プラットフォーム、イネイブリング、コンプリケイテッド・サブシステム</strong>——と3つのインタラクションモード——<strong>コラボレーション、X-as-a-Service、ファシリテーション</strong>——は、認知負荷を管理するための処方箋。

</div>

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

新しい領域を立ち上げるときは2チームが密にコラボレーションし、境界が明確になったらX-as-a-Serviceに移行する。インタラクションの型を明示することで、<strong>チーム間の調整コストを設計可能にする</strong>。

</div>

</div>
</div>

</div>

---

## チーム構造は固定ではなく進化する

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

チームトポロジーの処方箋を得ても、それを一度適用して終わりではない。<strong>チーム構造は静的ではなく、システムの進化に合わせて動的にリチーミングする</strong>。イネイブリングチームは他チームの能力を底上げし、役割を果たしたら離れる。プラットフォームチームの責任範囲も、事業の成熟とともに広がったり狭まったりする。

</div>

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

事業の進化段階が変われば方法論が変わるように、チーム構造もまた進化段階に応じて変わるべきもの。組織再編は「数年に一度の大事件」ではなく、<strong>小さく継続的に見直す営み</strong>として設計する。境界とチームの対応関係が常に最新であることが、高速なフローを支える。

</div>

<div style="padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">再編が「大事件」になる組織は、本当に再編が必要なときに動けない。</span>
</div>

</div>

---

## 仕組みの変更では組織は変わらない

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

組織図を変えれば組織が変わると信じるのは「構造の誤謬」。新しいプロセスを導入すれば行動が変わると信じるのは「プロセスの誤謬」。どちらも、<strong>変化を「仕組み」に外注しようとする</strong>点で同じ間違いを犯している。なぜこの誤謬に嵌まるのか。構造やプロセスの変更は<strong>可視的で、意思決定者に「何かをやった」という実感を与える</strong>から。見えやすいものを変えて、見えにくいものを放置する——これが構造の誤謬の構造。

</div>

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

では何が組織を変えるのか。モダナイゼーションの前提条件は<strong>組織文化</strong>にある。組織文化には段階がある——権力で動く組織、規則で動く組織、そして成果で動く組織。失敗を報告した人が評価されるのか、罰せられるのか。新しい技術を試す提案が歓迎されるのか、「余計なことをするな」と言われるのか。<strong>技術的モダナイゼーションは、この成果志向の文化の上でしか持続しない。</strong>

</div>

<div style="padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">仕組みの変更は可視的だが、文化の変化は不可視。見えないものを変えない限り、見えるものは元に戻る。</span>
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

## 3つの軸が交差する場所

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 35%;">
<img src="../../assets/images/2026/figure-1-10-modernization-overview.png" alt="Modernization Overview" style="width: 100%; height: fit-content;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Figure 1.10 Architecture modernization overview より引用</div>
</div>

<div style="flex: 1;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 10px;">

ここまで技術・事業・組織を個別に掘り下げてきた。登壇や発表ではすべてを同時に語ることはできないから、便宜上3つに切り出して語っている。しかし実際の現場では、この3つは常に同時に起きている。<strong>Bounded Contextの境界を引く行為は、同時にチームの責任範囲を決め（組織）、どの領域に投資するかを決める（事業）行為でもある。</strong>

</div>

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

逆に言えば、境界設計の失敗は3つの軸すべてに波及する。言語の境界を無視すれば共有データベースの罠に嵌まり（技術）、Core Domainに集中できず（事業）、チーム間の調整コストが膨らむ（組織）。<strong>3つの軸は交差している。切り離して考えた瞬間に、局所最適が始まる。</strong>

</div>

</div>
</div>

</div>

---

## 1つだけ動かすと何が起きるか

<div style="font-size: 0.7em;">

<div style="display: flex; gap: 12px; align-items: stretch;">
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>技術だけ動かすと</strong>

マイクロサービスに分割しても、チーム構造がモノリスのまま。コードの結合度は下がったのにリリースが遅い。コンウェイの法則がアーキテクチャを元の形に引き戻す。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>事業だけ動かすと</strong>

Core Domainを特定しても、技術的な境界がそれを反映していなければ共有データベースで密結合のまま。進化段階に応じた方法論の切り替えも、コードが分離されていなければ実行できない。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>組織だけ動かすと</strong>

チームを再編しても、どの領域が差別化の源泉かが定まっていなければ、チームは何に集中すべきかわからない。認知負荷を最適化する対象が見えない。

</div>
</div>

<div style="margin-top: 12px; padding: 12px; background-color: #e0e0e0; border-radius: 8px; text-align: center;">
<span style="color: #e65100; font-weight: bold; font-size: 1.1em;">1つだけ動かせば、残り2つが設計図を上書きする。</span>
</div>

</div>

---

## 組織的な慣性と銀の弾丸

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

では、なぜこれほど多くの組織が1つの軸だけを動かしてしまうのか。既存の設計には、過去の決定を守りたい人たちの力、チームの慣れ、属人化した知識が蓄積されている。ある技術を選定した人がまだ社内にいれば、その技術を否定することは人を否定することになる。変更の技術的コストよりも、この<strong>組織的な慣性</strong>の方がはるかに大きい。

</div>

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

「これを入れれば解決する」という言葉が心地よいのは、この慣性に向き合う苦痛を回避できるから。マイクロサービス、クラウドネイティブ、AI——手段の名前は時代とともに変わるが、<strong>「銀の弾丸を求める」構造は同じ</strong>。現状と向き合わなくて済み、導入した瞬間に「何かをやった」実感が得られる。しかし問題の構造は変わっていない。

</div>

<div style="padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">慣性に向き合う苦痛を省略した瞬間に、雰囲気が戦略の代わりを務め始める。</span>
</div>

</div>

---

## 診断から始めるモダナイゼーション

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 20px; align-items: center;">
<div style="width: 35%;">
<img src="../../assets/images/2026/figure-16-8-modernization-core-domain-chart.png" alt="Modernization Strategy" style="width: 100%; height: fit-content;">
<div style="font-size: 0.55em; color: #999; text-align: center; margin-top: 5px;">Figure 16.8 Modernization core domain chart より引用</div>
</div>

<div style="flex: 1;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 10px;">

モダナイゼーションには順序がある。まず現状を診断し、次にドメインと進化段階を可視化し、それからチーム設計と技術選定に進む。「Kubernetesを導入しよう」「マイクロサービスに分割しよう」——これらは診断の結果として出てくるべき結論であって、出発点ではない。

</div>

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 10px;">

<strong>診断とは具体的に何か。</strong>「技術的負債がある」では診断にならない。「受注管理システムのこの部分が、新規プラン追加のリードタイムを3ヶ月にしており、事業の成長を阻害している」——ここまで掘り下げて初めて、どの境界から手をつけるか、どのチームを再設計するかが見えてくる。

</div>

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

小さく変え、各ステップで学び、次の判断を修正する。技術用語ではなくビジネス成果の言葉で語り、3〜6ヶ月で価値を証明する。<strong>継続的に学び適応する組織だけが、モダナイゼーションを持続できる。</strong>

</div>

</div>
</div>

</div>

---

## モダナイゼーションの成果物は何か

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

モダナイゼーションの目的地は「完成したアーキテクチャ」ではない。<strong>変化し続ける能力そのもの</strong>。境界設計を学べばチーム設計が必要になり、チーム設計を学べば進化段階の理解が必要になり、進化段階を学べばビジネス戦略の知識が必要になる。すべてが繋がっている。だからこそ、同時に動かす必要がある。

</div>

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

目指すべきは障害から元に戻る「レジリエンス」ではなく、障害を経るたびに強くなる組織。小さく壊れ、そこから学び、前より強い設計を手に入れる。学習する組織であることが、モダナイゼーションの前提条件であり、同時にその成果でもある。

</div>

<div style="padding: 15px; background-color: #e0e0e0; border-radius: 8px; text-align: center;">
<span style="color: #e65100; font-weight: bold; font-size: 1.1em;">モダナイゼーションの成果物はアーキテクチャではない。診断する習慣そのもの。</span>
</div>

</div>

---

## 参考資料

<div style="font-size: 0.65em;">

- [アーキテクチャモダナイゼーション](https://www.shoeisha.co.jp/book/detail/9784798194073) - Nick Tune, Jean-Georges Perrin 著 / 株式会社スリーシェイク 訳（翔泳社, 2026）
- [Architecture for Flow](https://www.informit.com/store/architecture-for-flow-9780137899937) - Susanne Kaiser（Addison-Wesley, 2025）
- [チームトポロジー](https://pub.jmam.co.jp/book/b593881.html) - Matthew Skelton, Manuel Pais 著 / 原田騎郎, 永瀬美穂, 吉羽龍太郎 訳（日本能率協会マネジメントセンター, 2021）
- [Building Microservices, 2nd Edition](https://www.oreilly.com/library/view/building-microservices-2nd/9781492034018/) - Sam Newman（O'Reilly, 2021）
- [Domain-Driven Design](https://www.domainlanguage.com/ddd/) - Eric Evans（Addison-Wesley, 2003）
- [Wardley Maps](https://learnwardleymapping.com/) - Simon Wardley
- [ソフトウェア設計の結合バランス](https://book.impress.co.jp/books/1124101149) - Vlad Khononov 著 / 島田浩二 訳（インプレス, 2025）
- [良い戦略、悪い戦略](https://www.nikkeibp.co.jp/atclpubmkt/book/12/P50070/) - Richard Rumelt 著 / 村井章子 訳（日本経済新聞出版, 2012）

</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: title dark
-->

![bg](../../assets/images/3shake-background-full.png)

<img src="../../assets/images/3shake-logo.png" alt="3-SHAKE logo" style="position: absolute !important; top: 100px !important; left: 100px !important; width: 240px !important; height: auto !important; z-index: 9999 !important;">

<div class="title" style="text-align: left; margin-top: 150px; margin-left: 80px; padding-left: 0; max-width: 70%;">

# ありがとうございました

### @nwiizo

</div>

<div class="author-info" style="text-align: left; padding-left: 0; text-indent: 0;">
設計ナイト 2026 / 2026-04-10</br>
アーキテクチャモダナイゼーションとは何か
</div>
