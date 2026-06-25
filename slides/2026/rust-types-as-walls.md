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

<div class="title" style="text-align: left; margin-top: 100px; margin-left: 80px; padding-left: 0; max-width: 80%;">

# <span style="font-size: 1.0em;">型は壁、Rustでもバグを<br>直すな、表現できなくせよ</span>

### <code>is_paid = true</code> なのに <code>payment_id</code> が null、という話

</div>

<div class="author-info" style="text-align: left; padding-left: 0; text-indent: 0;">
関数型まつり2026 公募セッション<br>
@nwiizo 50min
</div>

---

<!-- _backgroundColor: white -->

![bg left:30% fit](../../assets/images/nwiizo_icon.jpg)

## nwiizo

<div style="font-size: 0.75em;">

株式会社スリーシェイクでプロのソフトウェアエンジニアをやっているものです。SREやクラウドネイティブ技術を専門にしていますが、<strong>型システムと設計の話が好きです</strong>。

趣味は読書、格闘技、グラビア。仕事では型で壁を作り、格闘技では壁を壊そうとしています。

インターネット上では <strong>nwiizo</strong> を名乗り、ブログ「<strong>じゃあ、おうちで学べる</strong>」を運営しています。X / GitHub もこのIDでやっています。

</div>

---

## about 3-shake

<div style="display: flex; justify-content: center; align-items: center; margin-top: 20px;">
<img src="../../assets/images/3shake-about.png" alt="3-SHAKE会社概要" style="width: 85%; height: fit-content;" />
</div>

---

## 今日お話しすること

<div style="font-size: 0.78em;">

1. <strong>なぜ型を「壁」にするのか</strong> — バグは直すのではなく存在させない
2. <strong>関数型の道具をRustに持ち込む</strong> — ADT・Option・Resultの対応
3. <strong>4つの基本パターン</strong> — 状態型・newtype・Smart Constructor・Make Illegal States Unrepresentable
4. <strong>Rust固有の型プログラミング</strong> — Type State・PhantomData・既製の制約型
5. <strong>非純粋な現実と向き合う</strong> — 所有権・境界・DB・既存コード
6. <strong>まとめ</strong>

</div>

<div style="margin-top: 15px; padding: 12px; background-color: #f5f5f5; border-radius: 8px; font-size: 0.72em;">
関数型の基本的な道具（直和型・<code>Option</code> / <code>Result</code>・Smart Constructor など）はご存じの前提で進めます。本セッションの焦点は、それらを<strong>Rustに持ち込むと何が起きるか</strong>です。<br>
<strong>Rust が初見の方へ</strong>：構文や記号は本編で登場するたびに補足します。分からない記号があっても、コード例は雰囲気で追えるようにしています。Rust以外の言語での対応は、まとめの後に示します。
</div>

---

## この発表で解決できること

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 20px; margin-top: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>こんな状態に身に覚えはありませんか？</strong>

- <code>is_paid = true</code> なのに <code>payment_id</code> が null
- <code>status = "verified"</code> なのに <code>verified_at</code> が欠落
- <code>CustomerId</code> と <code>OrderId</code> が同じ <code>u64</code> で取り違え

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>この発表で持ち帰れるもの</strong>

- 型を「壁」として使う設計思想
- 関数型の道具をRustに持ち込む具体パターン
- 所有権・Type State・PhantomDataでさらに強化する方法
- AI時代に通用する「破れない制約」の作り方

</div>
</div>

<div style="margin-top: 15px; padding: 12px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">バグは、対処するものから、書けないものへ</span>
</div>

</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: transition
-->

<div style="display: flex; justify-content: center; align-items: center; height: 100%; flex-direction: column; color: white;">

## <span style="color: white;">なぜ型を「壁」にするのか</span>

<span style="color: white; font-weight: bold;">バグを直すのではなく、存在させない</span>

</div>

---

## 3年運用したDBには、こんなレコードが眠っている

<div style="font-size: 0.75em;">

<p>新規のシステムなら、こんなデータは絶対に生まれない——と思うかもしれません。でも、3年運用したデータベースの中には、たいてい<strong>こういう矛盾したレコードが眠っています</strong>。</p>

```json
{
  "order_id": 12345,
  "is_paid": true,
  "payment_id": null,
  "status": "verified",
  "verified_at": null
}
```

<p>このレコードは、単体の値だけを見るとそれらしく見えます。<code>is_paid</code> も <code>payment_id</code> も、<code>status</code> も <code>verified_at</code> も、それぞれの型としては正しい。</p>

</div>

---

## 矛盾はあとから効いてくる

<div style="font-size: 0.75em;">

<p>問題は、値を<strong>組み合わせた瞬間</strong>に起きます。</p>

<div style="display: flex; gap: 18px; align-items: center; margin-top: 12px;">
<div style="flex: 1; background-color: #f5f5f5; padding: 14px; border-radius: 8px;">
<p><strong>is_paid = true なのに payment_id が null</strong></p>
<p>どの決済で完了したかの記録がなく、返金も、会計との突き合わせもできない。</p>
</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 14px; border-radius: 8px;">
<p><strong>status = "verified" なのに verified_at が null</strong></p>
<p>再認証ポリシーは比較の基準日を持てず、落ちるか素通りするかの二択になる。</p>
</div>
</div>

<p style="margin-top: 12px;">このレコードを作った瞬間にアラートが鳴ったりはしません。静かにDBに残り、数ヶ月後に返金業務や再認証処理で突然例外を投げる。</p>

</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">単独では正しい値が、組み合わせると矛盾する</span>
</div>

---

## なぜ、型はこれを止められなかったのか

<div style="font-size: 0.78em;">

<p>先ほどのレコードを受け取る型は、たぶんこう書かれていました。</p>

```rust
pub struct Order {
    pub is_paid: bool,
    pub payment_id: Option<PaymentId>,
    pub status: OrderStatus,
    pub verified_at: Option<DateTime<Utc>>,
}
```

<p>この型は、<code>bool</code> と <code>Option&lt;PaymentId&gt;</code> の<strong>任意の組み合わせ</strong>を許します。「正しい組み合わせ」と「あり得ない組み合わせ」を区別する情報は、型のどこにも書かれていません。</p>

<p>つまり、<strong>型が「正しい形」を規定していない</strong>。正しさはコメントやバリデーション関数の中に散らばり、どこかで抜け落ちる。抜け落ちた瞬間、矛盾レコードが静かに誕生します。</p>

</div>

---

## バグを「直す」発想から離れる

<div style="font-size: 0.8em;">

<p>不正な状態を見つけたとき、私たちはつい <code>if</code> 文で弾き、テストで落とし、レビューで指摘して直します。どれも実行時のチェックや人間の注意力に依存した<strong>後追い</strong>です。</p>

<p>値そのものは型が許す限り流れ続けます。<strong>不正が生まれる根本原因</strong>は、手付かずのまま。</p>

<p style="margin-top: 10px;">起きたバグを追いかけるのは、<strong>火が出るたびに消して回る作業</strong>。起きえない形を先に作るのは、<strong>燃えない素材で建てる設計</strong>。どちらも要る。ただし、<strong>燃えない素材を知らなければ建てられない</strong>。</p>

</div>

<div style="margin-top: 20px; padding: 15px; background-color: #e0e0e0; border-radius: 8px; text-align: center; font-size: 1.1em;">
<span style="color: #e65100; font-weight: bold;">バグを直すな。表現できなくせよ。</span>
</div>

---

## AI時代、コメントは破れるが型は破れない

<div style="font-size: 0.75em;">

<p>「表現できなくする」が<strong>なぜ今さら重要なのか</strong>。自然言語の制約が、以前にも増して破れやすくなっているからです。人間向けの制約は、こう書かれます。</p>

```rust
// payment_id は is_paid=true のときだけ Some、それ以外は None にすること
pub struct Order {
    pub is_paid: bool,
    pub payment_id: Option<PaymentId>,
}
```

<p style="margin-top: 12px;">コメントは<strong>条件付きのお願い</strong>——「できればこうしてください」。人間も読み飛ばすし、AIコーディングエージェントも読み飛ばします。<br>
型は<strong>条件のない掟</strong>——「こうでなければコードは動きません」。型が間違っていれば <code>cargo build</code> が赤くなります。</p>

<p style="margin-top: 12px;"><strong>コメントに従わないAIはいても、コンパイラに従わないAIはいません</strong>。だから AI 時代こそ、「お願い」ではなく「壁」で守るべきです。</p>

</div>

<div style="margin-top: 10px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">型は「お願い」ではなく「壁」である</span>
</div>

---

## ただし、型の壁にも抜け穴はある

<div style="font-size: 0.78em;">

<p>「コンパイラに従わないAIはいない」と言いましたが、誠実さのために付け加えます。<strong>壁には3つの抜け穴</strong>があります。</p>

<div style="background-color: #f5f5f5; padding: 14px; border-radius: 8px; margin-top: 12px;">

<ul>
<li><code>unsafe</code> ブロック — 型チェックを部分的に無効化する明示的な脱出口</li>
<li><code>.unwrap()</code> / <code>panic!</code> — 型を通した後で実行時にクラッシュさせる</li>
<li><strong>型定義そのものを緩める PR</strong> — <code>NonZero</code> を <code>u32</code> に戻す、enum に <code>Option</code> を足す</li>
</ul>

</div>

<p style="margin-top: 12px;">ただし、コメントは黙って破れますが、型は破るとき必ず痕が残ります。<code>unsafe</code> はブロックとして、型定義の変更は diff として可視化される。</p>

<p style="margin-top: 12px;">型は壁ですが、<strong>壁の設計図を守るのは人間のレビュー</strong>です。AI が型定義を緩める PR を出したとき、気づけるのは人間だけ。<strong>型はコードレビューを不要にはしません</strong>。</p>

</div>

<div style="margin-top: 8px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">型は壁。でも、壁の設計図を守るのは人間</span>
</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: transition
-->

<div style="display: flex; justify-content: center; align-items: center; height: 100%; flex-direction: column; color: white;">

## <span style="color: white;">なぜ壁を築くかはわかった</span>

<span style="color: white; font-weight: bold;">では、何で築くのか？</span>

<div style="margin-top: 24px; font-size: 0.8em; color: #aaa;">
関数型の道具をRustに持ち込む
</div>

</div>

---

## 今日使う言葉は、型・AND・OR の3つだけ

<div style="font-size: 0.75em;">

<p>皆さんには釈迦に説法ですが、用語を Rust 語に揃えます。</p>

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-top: 12px; text-align: center; font-size: 1.1em;">
<strong>型 = 値の集合に付けた名前</strong>（<code>bool</code> = {true, false}、<code>u64</code> = 0〜約1800京）
</div>

<div style="display: flex; gap: 18px; margin-top: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 14px; border-radius: 8px;">

<strong>AND型（struct）</strong>

<p>フィールドが<strong>全部同時に存在</strong>する。</p>

```rust
struct User {
    name: String,
    age: u32,
}
```

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 14px; border-radius: 8px;">

<strong>OR型（enum）</strong>

<p>いくつかの候補から<strong>ちょうど1つ</strong>。</p>

```rust
enum Shape {
    Circle(f64),
    Square(f64),
}
```

</div>
</div>

<p style="margin-top: 12px;"><code>Option</code>（あるかないか）と <code>Result</code>（成功か失敗か）も OR型の仲間です。型を<strong>値の集合</strong>と捉え直すと、「集合を小さくする＝不正な値を集合から除外する」という発想が出てきます。もうひとつ、集合と見ると状態の数が<strong>数えられます</strong>。AND型は掛け算（直積）、OR型は足し算（直和）。この算数が、後半で効いてきます。</p>

</div>

<div style="margin-top: 8px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">形は AND と OR で描ける。値の制約は、まだ描けない</span>
</div>

---

## 関数型の道具は、Rustにある

<div style="font-size: 0.78em;">

<p>釈迦に説法かもしれませんが、関数型の道具と Rust の型の<strong>主要な対応</strong>だけ整理します。</p>

| 関数型（F# / OCaml / Scala など）        | Rust                                                           |
| ---------------------------------------- | -------------------------------------------------------------- |
| Record / Sum type（Choice, DU）          | <code>struct</code> / <code>enum</code>                        |
| <code>Maybe</code> / <code>Either</code> | <code>Option&lt;T&gt;</code> / <code>Result&lt;T, E&gt;</code> |
| Pattern matching（網羅性チェック）       | <code>match</code>（網羅をコンパイラが強制）                   |
| Single case DU + Smart constructor       | タプル構造体 + <code>pub fn new() -&gt; Result</code>          |

<p>ADTもパターンマッチも揃っているので、関数型で書いていたドメインモデリングは<strong>ほぼそのまま Rust に持ち込めます</strong>。問題は、Rust だけにある道具と、Rust にない制約です。</p>

<div style="margin-top: 12px; padding: 12px; background-color: #f5f5f5; border-radius: 8px; font-size: 0.9em;">
<strong>Rust豆知識（Programming Rust 3rd Ch.9）</strong><br>
Rust の <code>enum</code> は、ML 系の言語で直和型・代数的データ型と呼ばれてきた系譜にあります。面白いのは、そこに参照・可変性・メモリ安全性まで同居させている点で、書籍はその組み合わせを借用チェッカーが可能にしていると説明しています。
</div>

</div>

---

## Rust記号ミニ辞典

<div style="font-size: 0.75em;">

<p>本編で頻繁に登場する記号だけ先に整理します。これでコード例は雰囲気で読めます。</p>

<div style="display: flex; gap: 18px; margin-top: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 14px; border-radius: 8px;">

<strong>参照と所有権</strong>

- <code>&T</code> — 読み取り専用の参照（「借りる」）
- <code>&mut T</code> — 書き換え可能な借用
- 参照なし（<code>T</code>）— <strong>所有権ごと渡す</strong>（呼び出し元では使えなくなる）

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 14px; border-radius: 8px;">

<strong>エラー処理</strong>

- <code>Result&lt;T, E&gt;</code> — 成功 <code>Ok(T)</code> か失敗 <code>Err(E)</code>
- <code>?</code> 演算子 — 失敗なら早期リターン、成功なら中身を取り出す
- <code>Option&lt;T&gt;</code> — 値があるか（<code>Some</code>）ないか（<code>None</code>）

</div>
</div>

<p style="margin-top: 12px;">細かい記号は登場時に補足します。<strong>構文に悩んだら「型が何を受けて何を返すか」だけ見てください</strong>。</p>

</div>

---

## Option / Result / ? の連携を、1つの例で

<div style="font-size: 0.72em;">

<p>3つの記号はバラバラではなく、<strong>連携して使います</strong>。小さな例で流れを見ます。</p>

```rust
fn parse_user_id(s: &str) -> Result<UserId, ParseError> {
    // (1) 文字列を数字にパース。失敗すれば Err で早期リターン
    let n: u64 = s.parse()?;

    // (2) UserId を作る。0 は不正なので失敗しうる
    let id = UserId::new(n)?;

    // (3) 成功したら Ok で包んで返す
    Ok(id)
}
```

<p><strong><code>?</code> 演算子</strong>は、失敗なら呼び出し元に委ね、成功なら中身だけを取り出します。</p>

<p><code>Option&lt;T&gt;</code>（値があるか、ないか）と <code>Result&lt;T, E&gt;</code>（成功か、失敗か）は、どちらも <code>?</code> で連鎖できる型です。連鎖させると、失敗のたびに <code>if</code> 文で分岐する必要がなくなります。</p>

</div>

---

## 所有権の壁が状態遷移を強制する

<div style="font-size: 0.75em;">

<p>Rust の<strong>「所有権」</strong>は、<strong>「値は常に一人の主人にしか持てない」</strong>というルールです。家の鍵を誰かに渡したら自分の手元には残らない——それと同じ感覚。関数に値を渡すと、呼び出し元からはその値が<strong>なくなります</strong>。</p>

<p style="margin-top: 12px;">この仕組みを使うと、「検証前の注文は、検証した後に残ってはいけない」を型で表現できます。「検証前の注文をもう1回使う」コードが、<strong>そもそも書けません</strong>。前の状態が残らないことを、所有権ルールが型レベルで保証してくれます。</p>

<p style="margin-top: 12px;"><strong>このルールが、後半で「状態遷移の壁」になります</strong>。コードはパターン1でお見せします。</p>

</div>

---

## イミュータビリティの壁は書き換えを拒む

<div style="font-size: 0.75em;">

<p>Rust では変数も参照も、<strong>デフォルトで書き換え不可</strong>です。書き換えたいときは <code>mut</code> を明示する必要があります。</p>

```rust
let order = ValidatedOrder { items: vec![...] };
order.items.push(new_item);   // ← コンパイルエラー。order は不変

let mut order = ValidatedOrder { items: vec![...] };
order.items.push(new_item);   // ← これは通る
```

<p>この仕組みは、メソッドの設計にも影響します。関数型と同じく「入力を受けて<strong>新しい値を作る</strong>」が自然になります。</p>

```rust
// 関数型的な書き方: 新しい値を返す
fn apply_discount(o: PricedOrder, rate: f64) -> PricedOrder { /* ... */ }

// OOP 的な書き方: 既存の値を書き換える
impl PricedOrder {
    fn apply_discount(&mut self, rate: f64) { /* ... */ }
}
```

<p>前者のほうが「元の値は変わらない」ことが型で明示され、並行処理でも扱いやすくなります。</p>

</div>

---

## 所有権の正体は、アフィン型システム

<div style="font-size: 0.72em;">

<p>所有権の正体は、<strong>部分構造型システム（substructural type system）</strong>の一種です。源流は1987年に Jean-Yves Girard が提唱した線形論理。値の「使い回し」をどこまで許すかで、2つの兄弟があります。</p>

<div style="display: flex; gap: 18px; margin-top: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 14px; border-radius: 8px;">

<strong>線形型 (Linear types)</strong>

<p>値は<strong>ちょうど1回</strong>使う。捨てることも複製することもできない。リソースリーク防止に強い。</p>

<p style="font-size: 0.9em; color: #666; margin-top: 8px;">代表例: Clean の uniqueness types、Idris 2 の linear types</p>

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 14px; border-radius: 8px;">

<strong>アフィン型 (Affine types)</strong>

<p>値は<strong>1回以下</strong>使う。複製はできないが、<strong>捨てることはできる</strong>（drop）。</p>

<p style="font-size: 0.9em; color: #666; margin-top: 8px;">代表例: Rust の所有権、ATS、Vale</p>

</div>
</div>

<p style="margin-top: 12px;">Rust がアフィン型を選んだのは <strong>drop（捨てる）を許すため</strong>。線形型だと使い忘れの解放まで型エラーになり、日常のコードが書きづらい。<code>#[must_use]</code> は特定の値だけを線形寄りに引き上げる手段です。</p>

</div>

<div style="position: absolute; bottom: 20px; right: 40px; font-size: 0.5em; color: #999;">
Girard (1987) "Linear Logic" / Bernardy, Boespflug, Newton, Peyton Jones, Spiwack (2018) "Retrofitting Linear Types" (POPL'18)
</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: transition
-->

<div style="display: flex; justify-content: center; align-items: center; height: 100%; flex-direction: column; color: white;">

## <span style="color: white;">道具は揃った</span>

<span style="color: white; font-weight: bold;">では、どう使うのか？</span>

<div style="margin-top: 24px; font-size: 0.8em; color: #aaa;">
4つの基本パターン（状態型・newtype・Smart Constructor・Make Illegal States Unrepresentable）
</div>

</div>

---

## パターン1 状態ごとに型を分ける

<div style="font-size: 0.72em;">

<p>「検証前の注文」と「検証済みの注文」を<strong>同じ型</strong>で扱うと、検証をスキップしたコードが通ります。</p>

```rust
struct Order {
    validated: bool,
    items: Vec<Item>,
}

fn calculate_total(order: &Order) -> Money {
    // validated == false でも呼べてしまう
    order.items.iter().map(|i| i.price).sum()
}
```

<p><code>validated</code> フラグは実行時の情報であり、呼び出し側が確認を忘れても<strong>コンパイラは何も言いません</strong>。</p>

</div>

---

## 状態が違うなら、型を分ける

<div style="font-size: 0.72em;">

<p>検証前と検証後を<strong>別の型</strong>にします。</p>

```rust
struct UnvalidatedOrder { items: Vec<Item> }
struct ValidatedOrder   { items: Vec<Item> }

fn validate(o: UnvalidatedOrder) -> Result<ValidatedOrder, OrderError> {
    // チェックを通った場合だけ ValidatedOrder が生まれる
}

fn calculate_total(o: &ValidatedOrder) -> Money {
    o.items.iter().map(|i| i.price).sum()
}
```

<p><code>calculate_total(&unvalidated)</code> はコンパイルエラーになります。「検証前の注文を価格計算に渡す」バグは、そもそも書けません。</p>

</div>

---

## ワークフロー全体を型で貫く

<div style="font-size: 0.72em;">

<p>状態ごとに型を分ける発想を<strong>注文処理の一連の流れ</strong>に適用すると、こうなります。</p>

```rust
struct UnvalidatedOrder { items: Vec<RawItem> }
struct ValidatedOrder   { items: Vec<Item> }
struct PricedOrder      { items: Vec<Item>, subtotal: Money }
struct PaidOrder        { items: Vec<Item>, subtotal: Money, payment: PaymentId }

fn validate(o: UnvalidatedOrder) -> Result<ValidatedOrder, OrderError>;
fn price(o: ValidatedOrder)      -> PricedOrder;
fn charge(o: PricedOrder, card: &Card) -> Result<PaidOrder, PaymentError>;
```

<p>各ステップの入出力が型で固定されているので、<strong>順序を間違えるコードは書けません</strong>。検証前の注文に価格をつけたり、支払い前の注文を確定したり——どれもコンパイルが通らない。</p>

</div>

<div style="margin-top: 8px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">関数シグネチャが、ワークフローの仕様書になる</span>
</div>

---

## ワークフローは左から右へ流せる

<div style="font-size: 0.75em;">

<p>状態型で固めた各ステップは、<strong>関数合成でそのまま繋がります</strong>。<code>tap</code> クレートの <code>.pipe()</code> を挟むと、ネストせず左から右に読める形になります。</p>

```rust
let paid = raw
    .pipe(validate)              // Unvalidated → Result<Validated>
    .map(price)                  // Validated   → Priced
    .map_err(WorkflowError::from)
    .and_then(|o| charge(o, &card).map_err(WorkflowError::from))?;
```

<p>パイプの途中で型が合わなければ、その行が赤くなります。<strong>合成できる順序はコンパイラが保証</strong>する。関数型の「データを変換で流す」発想が、状態遷移にそのまま乗ります。</p>

</div>

---

## パターン2 newtypeで取り違えを防ぐ

<div style="font-size: 0.72em;">

<p>状態型（パターン1）とその応用はここまで。残り3つのパターンに進みます。まずは<strong>取り違えの防止</strong>。</p>

<p>顧客IDと注文ID、どちらも <code>u64</code> で扱うと、引数の順序ミスがすり抜けます。</p>

```rust
fn charge(customer_id: u64, order_id: u64) { /* ... */ }

let customer = 1001u64;
let order = 5678u64;

charge(order, customer); // ← 引数逆でもコンパイルが通る
```

<p>これは<strong>実行時に初めて気づく</strong>バグです。ユニットテストを全網羅しない限り、本番で発覚します。</p>

</div>

---

## 単一フィールドの構造体で型を別にする

<div style="font-size: 0.72em;">

<p>Rust本でも紹介されている<strong>ニュータイプ</strong>パターンです。タプル構造体で1フィールドだけ包みます。</p>

```rust
struct CustomerId(u64);
struct OrderId(u64);

fn charge(customer: CustomerId, order: OrderId) { /* ... */ }

let customer = CustomerId(1001);
let order = OrderId(5678);

charge(order, customer); // ← 型エラー、コンパイルが通らない
```

<p>実行時コストはゼロ（メモリ上は <code>u64</code> そのまま）なのに、取り違えはコンパイラが検出してくれます。</p>

<div style="margin-top: 10px; padding: 10px; background-color: #f5f5f5; border-radius: 8px; font-size: 0.9em;">
<strong>Rust豆知識（Programming Rust 3rd Ch.8）</strong><br>
書籍はニュータイプを、単なる <code>Vec&lt;u8&gt;</code> や <code>u32</code> をコメントで区別するより、Rust に厳密な型検査をさせる方法として紹介しています。意味の違う値を同じプリミティブに戻さないのがポイントです。
</div>

</div>

<div style="margin-top: 10px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">同じ u64 を二人で持つと、必ずどこかで混ざる</span>
</div>

---

## パターン3 Smart Constructorで制約を型に埋める

<div style="font-size: 0.72em;">

<p>「メールアドレスには @ が必要」をコメントで書いても守られません。<strong>作り方を限定する</strong>ことで、不正値の存在を禁止します。</p>

```rust
pub struct Email(String);

impl Email {
    pub fn new(s: &str) -> Result<Self, EmailError> {
        if !s.contains('@') { return Err(EmailError::Invalid); }
        Ok(Email(s.to_owned()))
    }

    pub fn as_str(&self) -> &str { &self.0 }
}
```

<p>中身のフィールドは<strong>非公開</strong>なので、<code>Email::new</code> を通らない限り <code>Email</code> 値は作れません。一度作れた <code>Email</code> は、以降のコードで必ず <code>@</code> を含んでいます。</p>

</div>

---

## 複数の制約を組み合わせる

<div style="font-size: 0.72em;">

<p>実務では<strong>複数の制約を束ねる</strong>ことがよくあります。</p>

```rust
pub struct CustomerName { first: String, last: String }

impl CustomerName {
    pub fn new(first: &str, last: &str) -> Result<Self, NameError> {
        let first = first.trim();
        let last  = last.trim();
        if first.is_empty() || last.is_empty() {
            return Err(NameError::Empty);
        }
        if first.chars().count() > 50 || last.chars().count() > 50 {
            return Err(NameError::TooLong);
        }
        Ok(CustomerName { first: first.into(), last: last.into() })
    }
}
```

<p>このコンストラクタを通った <code>CustomerName</code> は、<strong>空でも長すぎもしない</strong>ことが型で保証されます。</p>

</div>

---

## Parse, don't validate

<div style="font-size: 0.75em;">

<p>Smart Constructor の背後にある思想は、Alexis King の一言に集約されます（2019年）。</p>

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-top: 10px; font-style: italic; text-align: center;">
"Parse, don't validate."<br>
<span style="font-size: 0.85em;">検証するな。解釈せよ。</span>
</div>

<p style="margin-top: 14px;"><strong>validate</strong> は <code>bool</code> を返すだけ。通った値も通らなかった値も、同じ <code>String</code> のまま旅を続けます。<strong>parse</strong> は別の型に変換し、<strong>「検証済みである」情報が型に刻まれます</strong>。下流のコードは、もう検証を気にしません。</p>

```rust
fn validate(s: &str) -> bool;             // 情報は型に残らない
fn parse(s: &str) -> Result<Email, _>;    // 情報が型に刻まれる
```

<div style="position: absolute; bottom: 20px; right: 40px; font-size: 0.5em; color: #999;">
Alexis King (2019) "Parse, don't validate" — lexi-lambda.github.io/blog/2019/11/05/parse-don-t-validate/
</div>

</div>

<div style="margin-top: 8px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">検証済みの証明を、値そのものに運ばせる</span>
</div>

---

## パターン4 フラグとOptionの組み合わせ爆発

<div style="font-size: 0.72em;">

<p>値の制約は Smart Constructor で守れました。残るは<strong>値の組み合わせ</strong>の整合性です。「認証済みユーザーには <code>verified_at</code> がある」を素直にstructで書くと、こうなりがちです。</p>

```rust
struct User {
    email: String,
    is_verified: bool,
    verified_at: Option<DateTime<Utc>>,
}
```

<p>この型が表現できる状態は、<strong>4通り</strong>です。</p>

| <code>is_verified</code> | <code>verified_at</code> | 意味                                            |
| ------------------------ | ------------------------ | ----------------------------------------------- |
| false                    | None                     | 未認証（正しい）                                |
| true                     | Some(t)                  | 認証済み（正しい）                              |
| true                     | None                     | <strong>不正</strong>：認証済みなのに日時がない |
| false                    | Some(t)                  | <strong>不正</strong>：未認証なのに日時がある   |

<p>4通り中2通りが不正。つまり<strong>型の半分がバグの置き場所</strong>です。これは偶然ではなく算数です。AND型は状態数を<strong>掛け算</strong>で増やします。正しい状態はビジネス上2つと決まっているのに、フラグを1つ足すたび状態空間は倍になり、<strong>増えた分はすべて不正の置き場所</strong>になる。</p>

</div>

---

## 不正な状態を表現不可能にする

<div style="font-size: 0.72em;">

<p>「同時に存在する」のではなく、「どちらか一方」を <code>enum</code> で表現します。</p>

```rust
enum User {
    Unverified { email: String },
    Verified   { email: String, verified_at: DateTime<Utc> },
}

fn send_receipt(user: &User) {
    match user {
        User::Verified { email, verified_at } => { /* 送る */ }
        User::Unverified { .. } => { /* 送らない */ }
    }
}
```

<p><code>Verified</code> なら <code>verified_at</code> は<strong>必ず存在</strong>し、<code>Unverified</code> にはそもそもフィールドがありません。不正な組み合わせは<strong>書けない</strong>。<code>match</code> はバリアントの網羅をコンパイラが強制するので、「認証済みのケースを書き忘れる」バグも防げます。</p>

<p>状態の算数で言えば、掛け算の 2×2=4通り（うち2つが不正）が、<strong>足し算の 1+1=2通り（すべて正しい）</strong>に置き換わりました。</p>

</div>

<div style="margin-top: 10px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">Make Illegal States Unrepresentable</span>
</div>

---

## 型は、コンパイル時のユニットテストになる

<div style="font-size: 0.78em;">

<p>enum で「ありえない状態」を表現不可能にすると、<strong>副産物として良いこと</strong>があります。</p>

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-top: 14px;">
<p><strong>「認証済みなのに verified_at が null のケース」のテストは、書く必要がなくなります</strong>。</p>
<p>なぜなら、そのケースは<strong>コンパイラが通さない</strong>からです。</p>
</div>

<p style="margin-top: 14px;">この仕組みを、Scott Wlaschin は<strong>「コンパイル時のユニットテスト」</strong>と呼びます。型が、実行前にテストの役目を果たしてくれるのです。テストコードが不要になるのではなく、<strong>型そのものがテスト</strong>になります。</p>

</div>

---

## 4つの基本パターン、一覧で確認

<div style="font-size: 0.78em;">

<p>ここまで駆け足で見てきた4パターンを、一枚にまとめます。</p>

<div style="display: flex; gap: 14px; margin-top: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>1. 状態ごとに型を分ける</strong>

<p style="font-size: 0.9em; margin-top: 6px;"><code>UnvalidatedOrder</code> と <code>ValidatedOrder</code> は別の型。検証前の注文を価格計算に渡すコードは、書けない。</p>

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>2. newtype で取り違えを防ぐ</strong>

<p style="font-size: 0.9em; margin-top: 6px;"><code>CustomerId</code> と <code>OrderId</code> を別の型に。引数の順序を間違えればコンパイルが通らない。</p>

</div>
</div>

<div style="display: flex; gap: 14px; margin-top: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>3. Smart Constructor で制約を埋め込む</strong>

<p style="font-size: 0.9em; margin-top: 6px;">コンストラクタを限定すれば、不正な値の <code>Email</code> は存在できない。parse の成功がそのまま型に乗る。</p>

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>4. 不正な状態を表現不可能にする</strong>

<p style="font-size: 0.9em; margin-top: 6px;">フラグ + Option の組み合わせ爆発を enum で潰す。型の半分がバグの置き場所ではなくなる。</p>

</div>
</div>

<p style="margin-top: 12px;">この4つだけでも、『is_paid と payment_id が矛盾する』系のバグは<strong>書きようがなくなります</strong>。</p>

</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: transition
-->

<div style="display: flex; justify-content: center; align-items: center; height: 100%; flex-direction: column; color: white;">

## <span style="color: white;">ここまでは関数型の延長</span>

<span style="color: white; font-weight: bold;">ここからが、Rust固有の型プログラミング</span>

<div style="margin-top: 24px; font-size: 0.8em; color: #aaa;">
Type State / PhantomData / 既製の制約型
</div>

</div>

---

## Type State パターンで状態を型パラメータに乗せる

<div style="font-size: 0.78em;">

<p>パターン1では <code>UnvalidatedOrder</code> と <code>ValidatedOrder</code> を<strong>別の構造体</strong>として書きました。中身がほぼ同じなら、<strong>型パラメータ</strong>で状態を表現するほうがきれいになります。</p>

<div style="background-color: #f5f5f5; padding: 14px; border-radius: 8px; margin-top: 12px;">

<strong>発想の核</strong>

<p>「注文」という1つの構造体を用意し、<strong>状態だけを型パラメータで切り替える</strong>。<code>Order&lt;Unvalidated&gt;</code> と <code>Order&lt;Validated&gt;</code> は、<strong>中身は同じでも別の型</strong>として扱われる。ジェネリクスの <code>&lt;State&gt;</code> は、関数型の <code>Maybe a</code> の <code>a</code> と同じ発想です。</p>

</div>

<div style="background-color: #f5f5f5; padding: 14px; border-radius: 8px; margin-top: 10px;">

<strong>なぜ <code>PhantomData</code> が必要？</strong>

<p>Rust は型パラメータを実際に使わない構造体を嫌います。<code>Order</code> の本体（<code>items</code>）には <code>State</code> が登場しないため、「<strong>型だけ使ったふり</strong>」をするために <code>PhantomData&lt;State&gt;</code> を置きます。このフィールドは<strong>実行時には0バイト</strong>。型の上にだけ、コストゼロで状態を存在させられます。</p>

</div>

</div>

---

## Type State パターンのコード例

<div style="font-size: 0.72em;">

```rust
use std::marker::PhantomData;

struct Unvalidated;  struct Validated;
struct Order<State> { items: Vec<Item>, _state: PhantomData<State> }

impl Order<Unvalidated> {
    fn validate(self) -> Result<Order<Validated>, OrderError> {
        Ok(Order { items: self.items, _state: PhantomData })  // 検証は省略
    }
}
impl Order<Validated> {
    fn total(&self) -> Money {                 // Validated にだけ実装
        self.items.iter().map(|i| i.price).sum()
    }
}
```

<p><code>total()</code> は <code>Order&lt;Validated&gt;</code> にしか実装されていないので、<strong><code>Order&lt;Unvalidated&gt;</code> に対しては呼べません</strong>。状態遷移は <code>self</code> 消費で表現され、<code>Unvalidated</code> の注文は検証後には残りません。</p>

</div>

---

## 既製の制約型を積極的に使う

<div style="font-size: 0.75em;">

<p>ここまでの壁は全部自作でした。でも、自作する前に<strong>すでに用意されている制約型</strong>で済まないかを確認するのが先です。標準ライブラリとエコシステムに、制約が型に埋め込まれた型はいくつも揃っています。</p>

<div style="display: flex; gap: 18px; margin-top: 10px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 14px; border-radius: 8px;">

<strong>標準ライブラリ</strong>

- <code>NonZero&lt;T&gt;</code> — ゼロではない整数型（<code>NonZero&lt;u32&gt;</code>、<code>NonZeroU32</code>は別名）
- <code>Option&lt;&T&gt;</code> — null になり得る参照を型で明示し、使う前のチェックを強制
- <code>String</code> / <code>&str</code> — UTF-8 保証

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 14px; border-radius: 8px;">

<strong>エコシステム</strong>

- <code>nonempty</code> — <code>NonEmpty&lt;T&gt;</code> で空でないVecを保証
- <code>nutype</code> — マクロで newtype + バリデーションを一括生成
- <code>validator</code> — フィールド単位のバリデーション属性

</div>
</div>

<p style="margin-top: 12px;"><code>n: u32</code> ではなく <code>n: NonZero&lt;u32&gt;</code>、<code>items: Vec&lt;Item&gt;</code> ではなく <code>items: NonEmpty&lt;Item&gt;</code> と書くだけで、「ゼロ・空を渡してはいけない」がコンパイル時に表現されます。</p>

<div style="margin-top: 10px; padding: 10px; background-color: #f5f5f5; border-radius: 8px; font-size: 0.88em;">
<strong>Rust豆知識（Programming Rust 3rd Ch.9）</strong><br>
<code>Option&lt;Box&lt;T&gt;&gt;</code> はポインタ側がゼロを許さないため、<strong>1マシンワードに圧縮</strong>されます。null と同じ大きさで、使う前の <code>Some</code> チェックは型が強制する仕組み。
</div>

</div>

---

## 公開APIの拡張点を、型で閉じる

<div style="font-size: 0.75em;">

<p>型は壁ですが、<strong>公開APIの拡張点</strong>には穴が残ります。<code>enum</code> はもともと閉じていますが、<code>trait</code> は放っておくと<strong>外部のコードから実装を増やせます</strong>。</p>

<p>そこで <code>trait</code> を非公開の supertrait で封じます（sealed trait）。</p>

```rust
mod sealed { pub trait Sealed {} }

pub trait PaymentState: sealed::Sealed { /* ... */ }

pub struct Authorized;
impl sealed::Sealed for Authorized {}   // 実装できるのは自分のクレートだけ
impl PaymentState for Authorized { /* ... */ }
```

<p><code>sealed::Sealed</code> が非公開なので、<strong>外部クレートは <code>PaymentState</code> を実装できません</strong>。「どこまで拡張を許すか」を、お願いではなく型で決められます。</p>

</div>

---

## フィールドを足すとOptionの海に戻る

<div style="font-size: 0.75em;">

<p>要件が増えたとき、つい既存の型に<strong>フィールドを足したくなります</strong>。でもそれは、組み合わせ爆発とOptionの海に戻る道です。</p>

```rust
// Before: ValidatedOrder に配送料の情報を足したい
struct ValidatedOrder {
    items: Vec<Item>,
    shipping_cost: Option<Money>,      // ← 計算前は None
    shipping_address: Option<Address>, // ← 計算前は None
}
```

<p>配送料が計算済みかどうか、配送先が確定しているかどうかが、また <code>Option</code> の組み合わせに散らばります。</p>

</div>

---

## 設計を進化させるときも、型を作る

<div style="font-size: 0.75em;">

<p><strong>新しい状態は、新しい型にする</strong>。配送情報が必要になったら、それを必須フィールドに持つ型を作ります。</p>

```rust
struct PricedOrder { items: Vec<Item>, subtotal: Money }
struct PricedOrderWithShipping {           // 配送情報は必須・Optionにしない
    items: Vec<Item>, subtotal: Money, shipping: ShippingInfo,
}
```

<p>型が増えると、コンパイラが依存箇所を<strong>全部追跡</strong>します。「直し忘れ」はビルドエラーになる。<code>Option</code> で様子を見るより安全です。</p>

<p style="margin-top: 12px;">ここまでは型で何でも解決できそうに見えます。でも、<strong>Rust固有の摩擦</strong>もあるのが現実です。</p>

</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: transition
-->

<div style="display: flex; justify-content: center; align-items: center; height: 100%; flex-direction: column; color: white;">

## <span style="color: white;">非純粋な現実と向き合う</span>

<span style="color: white; font-weight: bold;">所有権・境界・DB・既存コード</span>

</div>

---

## 摩擦1 所有権は状態遷移と同居しにくい

<div style="font-size: 0.78em;">

<p><strong>ここまで型で何でも守れるかのように話してきました</strong>。でも、実際にコードを書くと、最初につまずくのがここです。</p>

<p>状態遷移を <code>fn validate(o: UnvalidatedOrder) -&gt; Result&lt;ValidatedOrder, _&gt;</code> の形で書くと、入力の注文は<strong>所有権ごと消費</strong>されます。</p>

<p>これが問題になるのは、<strong>同じ注文を複数の処理で並行して見ている場面</strong>です。</p>

```rust
let order: UnvalidatedOrder = receive();

log_for_audit(&order);           // 参照で見たい
send_to_metrics(&order);         // 参照で見たい
let valid = validate(order)?;    // ここで消費される
// let again = validate(order)?; // ← もう使えない
```

<p>複数箇所で参照を保持しつつ状態遷移を起こしたい場合、<code>Clone</code> を挟む、<code>Arc</code> で共有する、あるいは借用ベースの設計に切り替える、といった判断が必要になります。<strong>「消費して新しい値を作る」純粋な状態遷移と、実務の参照共有には距離があります</strong>。</p>

</div>

---

## 古い値を残したまま、更新する

<div style="font-size: 0.75em;">

<p>「消費して作り直す」と「参照で共有する」の間に、もうひとつ関数型らしい選択肢があります。<strong>永続データ構造</strong>です。<code>im-rc</code> の <code>Vector</code> / <code>HashMap</code> は、更新しても<strong>古い値をそのまま残し、新しい値を返します</strong>。</p>

```rust
use im_rc::Vector;

fn add_item(items: &Vector<Item>, new: Item) -> Vector<Item> {
    let mut next = items.clone();   // clone は構造共有なので安価
    next.push_back(new);            // next だけが伸びる
    next                            // 元の items は不変のまま残る
}
```

<p>ここでの <code>clone()</code> は全体の複製ではなく<strong>構造共有</strong>で、変更した差分だけが新しく確保されます。所有権を消費せずに「変換前」と「変換後」の両方を安価に持てる。イミュータブルな更新を、現実的なコストで実務に持ち込めます。</p>

</div>

---

## 摩擦2 バリアント間でフィールドが重複する

<div style="font-size: 0.78em;">

<p>パターン4の enum では、共通の <code>email</code> が両方のバリアントで繰り返されていました。Rust には「enum 全体で共通のフィールド」を直接書く機能はありません。<strong>対処は2つ</strong>あります。</p>

<div style="display: flex; gap: 18px; margin-top: 10px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>素直に繰り返す</strong>

<p>パターン4のコードのまま。可読性は高いが DRY ではない。</p>

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>状態だけを enum にする</strong>

<pre><code>struct User {
    email: String,
    state: UserState,
}
enum UserState {
    Unverified,
    Verified(DateTime&lt;Utc&gt;),
}</code></pre>

</div>
</div>

<p style="margin-top: 10px;">後者は共通フィールドが1箇所に集まる代わりに、<strong>「検証済みのときだけ <code>verified_at</code> が取れる」保証</strong>がやや弱くなります。トレードオフです。</p>

</div>

---

## 摩擦3 型状態は認知コストを上げる

<div style="font-size: 0.75em;">

<p><code>Order&lt;Unvalidated&gt;</code> と <code>Order&lt;Validated&gt;</code> で状態を分けると壁は強固になりますが、<strong>型シグネチャが重くなります</strong>。</p>

```rust
// 2状態なら、まだ読める
fn process(o: Order<Validated>) -> Result<Order<Priced>, Error>;

// 3状態以上で、複数のジェネリクスが絡むと急に重くなる
fn handle<S>(o: Order<S>) -> Result<Output, Error>
where
    S: Into<FinalState>,
    Order<S>: Processable;
```

<p>型が表す制約は強くなりますが、読み手には「この <code>S</code> は何か」「どの状態へ進むのか」を追う負荷が増えます。</p>

</div>

---

## 型状態を使う境界を決める

<div style="font-size: 0.78em;">

<p>関数型まつりの皆さんには馴染みがあっても、<strong>チームの全員がこのシグネチャを読めるとは限りません</strong>。壁の強さは、読み手の認知コストとのトレードオフです。</p>

<div style="background-color: #f5f5f5; padding: 14px; border-radius: 8px; margin-top: 14px;">

<ul>
<li><strong>状態数が2〜3</strong>で、中身が大きいなら Type State</li>
<li><strong>状態数が多い</strong>、またはジェネリクスが絡むなら別 struct</li>
<li>閾値は PR レビュー時間・オンボーディング時間・Rust 習熟度で前後する</li>
</ul>

</div>

</div>

<div style="margin-top: 8px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">チームが読めない壁は、守りの価値が逆転する</span>
</div>

---

## 境界では、データは必ず生

<div style="font-size: 0.75em;">

<p>型で守れるのは<strong>ドメインの内側</strong>だけです。外側にはまだ「型が貼られていない生のデータ」が流れています。</p>

```rust
#[derive(Deserialize)]               // HTTPから来た型のない生データ
struct CreateOrderRequest {
    customer_id: u64,      // ← まだ CustomerId ではない
    items: Vec<ItemInput>, // ← まだ ValidatedItem ではない
    email: String,         // ← まだ Email ではない
}
```

<p>HTTP、JSON、DB、メッセージキューから来る値は、最初はただのプリミティブや文字列です。ここにドメイン型を貼る処理が必要になります。</p>

</div>

---

## 境界でparseして内側へ渡す

<div style="font-size: 0.75em;">

```rust
fn create_order(req: CreateOrderRequest) -> Result<ValidatedOrder, ApiError> {
    let customer = CustomerId::new(req.customer_id)?;
    let email    = Email::new(&req.email)?;
    let items    = req.items.into_iter().map(Item::try_from)
                            .collect::<Result<Vec<_>, _>>()?;
    ValidatedOrder::new(customer, email, items)
}
```

<p><strong>境界のただ1か所</strong>で parse を通し、以降は型付きの安全な値だけが流れる。内側の壁と外側の検問所は役割が違います。</p>

</div>

<div style="margin-top: 8px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">境界で型を貼り、内側は型で守る</span>
</div>

---

## DBスキーマはフラットに戻る

<div style="font-size: 0.75em;">

<p>DB は歴史的に<strong>フラット</strong>です。<code>is_verified BOOLEAN, verified_at TIMESTAMP NULL</code> のようなフラグ＋Option形式になりがち。</p>

<p>Rust 側では <code>enum User</code> で受けられますが、<strong>書き込み時には再びフラグとnullに分解</strong>する必要があります。</p>

```rust
// ADT → フラット（書き込み）
fn to_row(user: &User) -> UserRow {
    match user {
        User::Unverified { email } => UserRow {
            email: email.clone(),
            is_verified: false,
            verified_at: None,
        },
        User::Verified { email, verified_at } => UserRow {
            email: email.clone(),
            is_verified: true,
            verified_at: Some(*verified_at),
        },
    }
}
```

</div>

---

## DBからenumへ戻すときはErrにする

<div style="font-size: 0.75em;">

<p>逆方向（DBから読み込んで enum に戻す）では、フラットな行をもう一度ドメイン型へ parse します。</p>

```rust
fn from_row(row: UserRow) -> Result<User, UserRowError> {
    match (row.is_verified, row.verified_at) {
        (false, None)    => Ok(User::Unverified { email: row.email }),
        (true, Some(t))  => Ok(User::Verified { email: row.email, verified_at: t }),
        (true, None)     => Err(UserRowError::MissingVerifiedAt),
        (false, Some(_)) => Err(UserRowError::UnexpectedVerifiedAt),
    }
}
```

<p>冒頭の「3年眠っていたレコード」と再会するのは、まさにこの <code>(true, None)</code> の行です。型は新しい矛盾を防ぎ、<strong>すでにある矛盾はここで名前付きのエラーとして表面化する</strong>。</p>

<p>不正な組み合わせは <code>Err</code> にする必要があります。<strong>DBを先に設計すると enum の恩恵が削がれる</strong>ので、ドメイン型を先に決めて永続化を後から合わせるほうが効きます。</p>

</div>

---

## 既存コードへの段階導入

<div style="font-size: 0.78em;">

<p><code>u64</code> が配り回っているコードベースに newtype を一気に入れるのは大工事です。影響範囲が全ファイルに及ぶこともあります。</p>

<p>現実解は、<strong>境界から小さく導入</strong>することです。</p>

<div style="background-color: #f5f5f5; padding: 14px; border-radius: 8px; margin-top: 10px;">

<ol>
<li><strong>API入口</strong>：HTTPハンドラや deserialize 直後で newtype に変換</li>
<li><strong>リポジトリ層</strong>：DB から取り出した直後、返す型を newtype に</li>
<li><strong>内側に染み込ませる</strong>：呼び出されている関数のシグネチャを順次 newtype に置き換え</li>
</ol>

</div>

<p style="margin-top: 12px;">すべてを一度に直す必要はありません。<strong>境界に置いた newtype が、内側の型付けを徐々に強制していく</strong>のが実務的なパターンです。</p>

</div>

---

## rust-analyzerで壁を常時表示する

<div style="font-size: 0.78em;">

<p>所有権・境界・DB・既存コード。摩擦との付き合い方はここまでです。摩擦を越えて段階導入を続けられるかは、<strong>壁のフィードバックの速さ</strong>にかかっています。</p>

<p>型で illegal state を表現不能にする戦略は、<strong>コンパイラに任せた瞬間に効果が最大化</strong>します。保存ごとに赤線が出て、CI で <code>cargo check</code> が止まる。常時走る <strong>rust-analyzer</strong> のおかげです。</p>

<div style="background-color: #f5f5f5; padding: 14px; border-radius: 8px; margin-top: 16px;">

<strong>rust-analyzer が壁に効く理由</strong>

- 保存ごとに型エラーを即時表示
- newtype / enum の <code>match</code> 漏れも実時間で指摘
- IDE の上で「書けないコード」が分かる

</div>

<p style="margin-top: 14px;">型の壁は、CI で初めて効くより、エディタで書いた瞬間に効くほうが強い。フィードバックが早いほど、壁は設計の一部になります。</p>

</div>

---

## rowanで独自の壁を足す

<div style="font-size: 0.78em;">

<p><strong>rowan</strong> は、コメント・空白まで保持する<strong>ロスレスな構文木</strong>です。rust-analyzer の補完・リネーム・diagnostics の基礎であり、ドメイン規約の自家製 lint にも使えます。</p>

<div style="background-color: #f5f5f5; padding: 14px; border-radius: 8px; margin-top: 16px;">

<strong>rowan で書ける独自の壁</strong>

- <code>pub \*\_id: String</code> → newtype に矯正
- <code>is_paid: bool</code> + <code>payment_id: Option</code> → enum 化を促す
- <code>pub enum \*Error</code> に <code>#[non_exhaustive]</code> 必須

</div>

<p style="margin-top: 14px;"><code>cargo check</code> に乗らない<strong>ドメインの規約</strong>も、rowan の自家製 lint で固定化できる。<strong>決定論的な壁を重ねるほど、最後に人間が検証すべき核心が絞られる</strong>。</p>

</div>

<div style="margin-top: 8px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">CI で弾くな。書いた瞬間に弾け</span>
</div>

---

## 型で防げること、防げないこと

<div style="font-size: 0.78em;">

<p>ここまでの話を踏まえて、<strong>型の守備範囲</strong>を整理します。</p>

<div style="display: flex; gap: 18px; margin-top: 14px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 14px; border-radius: 8px;">

<strong>型で防げること</strong>

- 「検証前の値」を「検証済みの値」として扱うこと
- 同じ表現の別の概念の取り違え
- ありえない組み合わせの同時存在
- コメントだけに頼った不変条件の流出

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 14px; border-radius: 8px;">

<strong>型で防げないこと</strong>

- ビジネスロジックの間違い（計算式の誤り等）
- 外部システムの値の変化
- パフォーマンスや可用性の問題
- 仕様の理解違い

</div>
</div>

<p style="margin-top: 14px;">型が防ぐのは<strong>「不正な状態が作れてしまうこと」</strong>であり、<strong>「何が正解か」を決めるのは人間</strong>です。この区別を忘れると、型原理主義に陥って苦しくなります。</p>

</div>

---

## 通っても、正しいとは限らない

<div style="font-size: 0.75em;">

<p>AI に「定員に空きがあれば参加登録する」と頼むと、こんな素直なコードが出ます。<strong>型もテストも通ります</strong>。</p>

```rust
let count = participant_count(event_id)?;        // 取得
if count < capacity {                            // 確認
    set_participant_count(event_id, count + 1)?; // 更新
}
```

<p>でも二人がほぼ同時に申し込むと、<strong>確認と更新の間に割り込み</strong>、定員を超えて登録される（ロストアップデート）。並行性やトランザクション整合性は<strong>型が表現できる範囲の外</strong>で、システム全体の文脈に宿ります。</p>

<p>文脈を<strong>名前や型に込めれば、AI もそこから推論できる</strong>ようになります。それでも、何を正しい振る舞いとするかを規定し、最後に検証する責任は、<strong>外側を知る人間</strong>に残ります。</p>

</div>

<div style="margin-top: 8px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">型は形を守る。正しさは、文脈に宿る</span>
</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: transition
-->

<div style="display: flex; justify-content: center; align-items: center; height: 100%; flex-direction: column; color: white;">

## <span style="color: white;">まとめ</span>

<span style="color: white; font-weight: bold;">型は壁であり、お願いではない</span>

</div>

---

## 今日の要点

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 20px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>設計思想</strong>

- 型は<strong>コンパイラのための注釈</strong>ではなく、<strong>不正な状態を物理的に禁止する壁</strong>
- バグは直すのではなく存在させない
- AIが書く時代も破れない壁は型。正しさが宿る<strong>文脈の検証は人間に残る</strong>

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>Rustで壁を築く道具立て</strong>

- 基本4パターン：状態型・newtype・Smart Constructor・Make Illegal States Unrepresentable
- Rust固有：Type State / PhantomData / 既製の制約型
- 進化のとき：フィールドを足さず、<strong>新しい型を作る</strong>
- 境界：検問所で parse、内側は型で守る

</div>
</div>

<div style="margin-top: 15px; padding: 15px; background-color: #e0e0e0; border-radius: 8px; text-align: center; font-size: 1.15em;">
<span style="color: #e65100; font-weight: bold;">バグを直すな。表現できなくせよ。</span>
</div>

</div>

---

## この考え方は、Rustに限らない

<div style="font-size: 0.78em;">

<p>本セッションでは Rust を題材に使いました。ただし、<strong>「型で不正を表現不能にする」という発想は、代数的データ型と Smart Constructor を書ける言語すべてに通じます</strong>。</p>

<div style="display: flex; gap: 18px; margin-top: 14px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 14px; border-radius: 8px;">

<strong>関数型言語（ネイティブ）</strong>

- F# / OCaml: Discriminated Union
- Scala 3: <code>enum</code>（ADT 構文）
- Elm: Custom Types
- ReScript: Variants

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 14px; border-radius: 8px;">

<strong>マルチパラダイム言語</strong>

- TypeScript: discriminated union + literal types
- Kotlin: <code>sealed class</code> / <code>sealed interface</code>
- Swift: <code>enum</code> with associated values
- Java: <code>sealed interface</code>（Java 17+）

</div>
</div>

<p style="margin-top: 14px;">Rust を知らない方も、普段使いの言語で同じパターンが書けるはずです。<strong>持ち帰っていただきたいのは Rust の構文ではなく、型で守る設計思想</strong>です。</p>

</div>

---

## 参考資料① 書籍と記事

<div style="font-size: 0.72em;">

- <strong>Programming Rust, 3rd Edition</strong>（Jim Blandy, Jason Orendorff, Leonora F. S. Tindall 著, O'Reilly, Rust 2024 edition対応）
  - 第8章 構造体（タプル構造体とニュータイプ）
  - 第9章 列挙型とパターン（代数的データ型と <code>match</code>）
- <strong>Domain Modeling Made Functional</strong>（Scott Wlaschin 著, Pragmatic Bookshelf, 2018）
  - 第4章 Understanding Types（choice types / discriminated unions）
  - 第5章 Domain Modeling with Types（Constrained Values, Modeling with Choice Types）
  - 第6章 Integrity and Consistency in the Domain（smart constructors / Making Illegal States Unrepresentable / "compile-time unit tests"）
- <strong>"Designing with Types" シリーズ</strong>（Scott Wlaschin, F# for Fun and Profit, 全13回）
  - 代表記事: "Making Illegal States Unrepresentable"
  - シリーズ目次: fsharpforfunandprofit.com/series/designing-with-types/
- <strong>"Parse, don't validate"</strong>（Alexis King, 2019）
  - lexi-lambda.github.io/blog/2019/11/05/parse-don-t-validate/
- <strong>"Rustでも学べる関数型ドメイン駆動設計"</strong>（nwiizo, 2026）
  - syu-m-5151.hatenablog.com

</div>

---

## 参考資料② 理論的背景とツール

<div style="font-size: 0.72em;">

- <strong>線形型・アフィン型の理論的背景</strong>
  - Girard (1987) "Linear Logic" — 線形論理の原典（線形型の理論的源流）
  - Bernardy, Boespflug, Newton, Peyton Jones, Spiwack (2018) "Retrofitting Linear Types"（POPL'18）— 既存の言語に線形型を後付けする設計論文
  - Walker (2005) "Substructural Type Systems"（部分構造型システム概観）
- <strong>型を IDE と Linter で常時強制する基盤</strong>
  - <strong>rust-analyzer</strong>: github.com/rust-lang/rust-analyzer — IDE 向け Rust 解析サーバ。型エラー / <code>match</code> 漏れをリアルタイムで弾く
  - <strong>rowan</strong>: github.com/rust-analyzer/rowan — rust-analyzer の心臓部にあるロスレス構文木ライブラリ。ドメイン規約の自家製 lint にも使える

<p style="margin-top: 12px; font-size: 0.85em; color: #666;">補足: "Make illegal states unrepresentable" という言い回しは Yaron Minsky（Jane Street）に由来し、Scott Wlaschin が F# およびドメインモデリング文脈で広く普及させたものです。</p>

</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: transition
-->

<div style="display: flex; justify-content: center; align-items: center; height: 100%; flex-direction: column; color: white;">

## <span style="color: white;">どの型から書き始めますか？</span>

<div style="font-size: 0.85em; margin-top: 30px; color: #aaa;">
変えるのは、型の形ではなく、発想の形
</div>

<div style="margin-top: 16px; font-size: 0.7em; color: #888;">
<code>is_paid = true</code> なのに <code>payment_id</code> が null。あのレコードを、もう作れなくする型から。
</div>

</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: title dark
-->

![bg](../../assets/images/3shake-background-full.png)

<div style="position: absolute !important; top: 5px !important; left: 5px !important; z-index: 9999 !important; margin: 0 !important; padding: 0 !important;">
  <img src="../../assets/images/3shake-logo.png" style="width: 240px !important; height: auto !important; display: block !important;">
</div>

<div style="text-align: center; margin-top: 200px;">

# ありがとうございました

### バグを直すな、書けないコードにせよ

### <span style="font-size: 0.6em; color: #ccc;">どうしても書けたら、型が足りない</span>

@nwiizo | https://3-shake.com

</div>
