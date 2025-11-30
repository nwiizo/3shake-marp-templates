---
marp: true
theme: ../themes/3shake-theme.css
paginate: true
math: mathjax
mermaid: true
style: |
  :root {
    --logo-url: url("../assets/images/3shake-cover.png");
    --mini-font-size: 20px;
    --header-footer-height: 50px;
    --black: #333;
  }
  /* 通常ページの左下にロゴを表示 */
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
  /* Mermaidのスタイル設定 */
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
  /* 非表示用のクラス */
  .hidden {
    display: none !important;
  }
---

<!--
_backgroundColor: #0a1929
_color: white
_class: title dark
-->

![bg](../assets/images/3shake-background-full.png)

<img src="../assets/images/3shake-logo.png" alt="3-SHAKE logo" style="position: absolute !important; top: 100px !important; left: 100px !important; width: 240px !important; height: auto !important; z-index: 9999 !important;">

<div class="title" style="text-align: left; margin-top: 100px; margin-left: 20px; padding-left: 0; max-width: 70%;">

# <span style="font-size: 1.2em;">プレゼンテーション</span></br><span class="highlight-yellow">タイトル</span>

### サブタイトル

</div>

<div class="author-info" style="text-align: left; padding-left: 0; text-indent: 0;">
2025/XX/XX イベント名</br>
@your_name 発表時間
</div>

---

<!-- _backgroundColor: white -->

![bg left:30% fit](../assets/images/nwiizo_icon.jpg)
## 自己紹介

<div class="info-box">
株式会社スリーシェイクで
ソフトウェアエンジニアをやっています
趣味や専門分野を記載してください
</div>

<p style="margin-top: 30px !important;">人生を通して大切にしていることを記載</p>

---

## about 3-shake

<div style="text-align: center; margin-top: 30px;">
  <img src="../assets/images/3shake-about.png" alt="3-shake about" style="width: 80%; margin-top: 10px;">
</div>

---

## We are Hiring!!

<div style="text-align: center; margin-top: 30px;">

3-shakeは一緒にSRE界隈を盛り上げてくれる<strong>仲間を大募集中</strong>です！
Mobility、FinTech、通信など大規模SREを存分に経験できます
是非、カジュアル面談しましょう！

  <img src="../assets/images/3shake-hiring.png" alt="3-shake hiring" style="width: 80%; margin-top: 10px;">
</div>

---

<!-- _backgroundColor: white -->

## アジェンダ

* <span class="highlight-blue">項目 1</span>
* <span class="highlight-green">項目 2</span>
* <span class="highlight-yellow">項目 3</span>
* 項目 4
* 項目 5

---

<!-- _backgroundColor: white -->

## <span class="highlight-blue">セクション 1</span>

<div class="info-box">
このセクションについての重要情報をここに記載します。
</div>

* 要点 1
* 要点 2
* 要点 3

---

<!-- _backgroundColor: white -->

## <span class="highlight-green">セクション 2</span>

### 2カラムレイアウト

<div style="display: flex; gap: 40px;">
<div style="flex: 1;">

**左カラム**
- 項目 1
- 項目 2
- 項目 3

</div>
<div style="flex: 1;">

**右カラム**
- 項目 A
- 項目 B
- 項目 C

</div>
</div>

---

<!-- _backgroundColor: white -->

## <span class="highlight-yellow">セクション 3</span>

| 項目 | 説明 |
|------|------|
| 項目 1 | 説明文 |
| 項目 2 | 説明文 |
| 項目 3 | 説明文 |

---

<!-- _backgroundColor: white -->

## 画像 + テキストレイアウト

<div style="display: flex; gap: 40px;">
<div style="width: 35%;">
<img src="../assets/images/placeholder.png" alt="description" style="width: 100%; height: fit-content;">
<div style="font-size: 0.7em; text-align: left; margin-top: 5px;">
画像の説明やソース
</div>
</div>

<div style="flex: 1;">
画像に関する説明文をここに記載します。</br></br>

1. **ポイント 1**
2. **ポイント 2**
3. **ポイント 3**
</div>
</div>

---

<!-- _backgroundColor: white -->

## 右側に画像配置

![bg right:30% 80%](../assets/images/placeholder.png)

画像を右側に配置した場合のレイアウト例です。

* 要点 1
* 要点 2
* 要点 3

<div class="reference-right">
参考：ソース名やURL
</div>

---

<!-- _backgroundColor: white -->

## まとめ

<div class="info-box">
本発表の要点をまとめます。
</div>

* セクション 1のまとめ
* セクション 2のまとめ
* セクション 3のまとめ
* 今後の展望と課題

---

## 参考資料

* [リンク 1](https://example.com)
* [リンク 2](https://example.com)
* [リンク 3](https://example.com)

---

<!--
_backgroundColor: #0a1929
_color: white
_class: title dark
-->

![bg](../assets/images/3shake-background-full.png)

<!-- タイトルページ左上に大きなロゴを表示 -->
<div style="position: absolute !important; top: 5px !important; left: 5px !important; z-index: 9999 !important; margin: 0 !important; padding: 0 !important;">
  <img src="../assets/images/3shake-logo.png" style="width: 240px !important; height: auto !important; display: block !important;">
</div>

<div style="text-align: center; margin-top: 200px;">

# ありがとう<span class="highlight-yellow">ございました</span>

### ご質問・ご相談はお気軽にお問い合わせください

@your_name | https://3-shake.com
</div>
