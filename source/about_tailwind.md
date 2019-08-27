---
title: Tailwindcssは素晴らしいという話
date: 2019-08-27
---

## Tailwindcssとは

https://tailwindcss.com/

CSSライブラリの一種。

BootstrapやBulmaのような他のCSSライブラリと異なり、
`.btn`や`.card`といったコンポーネント系のクラスは定義されておらず、
<b>1つのスタイルのみを変更するユーティリティ系のクラスがたくさん詰まっている</b>のが特徴。

Tailwindcssで定義されているクラスの例: 
```css
.font-bold { font-weight: 700; }
.rounded { border-radius: .25rem; }
.hidden { display: none; }
```

この特性がWeb開発で一体どんな効果をもたらすのか？
Web開発ではおなじみのBootstrapと比較しながら学んでいこう。

## Bootstrap vs Tailwindcss : ボタンの例

Bootstrapを使っているなら、ボタンを実装するのは以下のようにすればいいだけだ。

```html
<button class="btn btn-primary">
  Button
</button>
```

似たようなボタンをTailwindcssで作るなら、以下のようになるだろう。

```html
<button class="bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded select-none">
  Button
</button>
```

Bootstrapはたかが2個のクラスを指定するだけで、いい感じのボタンができた。
一方で、Tailwindcssは色々とユーティリティクラスを指定してようやくボタンが出来上がっている。

一見すると、Bootstrapの方が簡単だし見通しもよい。
Tailwindcssのほうはいちいちたくさんクラスを書かなきゃダメだから面倒くさそうだな…と思われるかもしれない。

しかし、ここからがTailwindcssの本領だ。

## カスタマイズ性

Bootstrapを使っているサイトは多いとはいえ、全くカスタマイズしないで使っているサイトはそれほど多くないだろう。多かれ少なかれ、ユーザー定義によるCSSにより元のスタイルが書き換えられている場合が多いはずである。

そこで、先ほどのボタンをカスタマイズしてみよう。
上下の余白を大きくして、文字を大きく太くして、横幅いっぱいに広がるようにしてみる。

[Bootstrapのドキュメント](https://getbootstrap.com/docs/4.3/components/buttons/)を見ると、
`btn-block`を指定すれば横幅いっぱいに広がるようだ。
しかし余白と文字に関しては自分でCSSを書かなくちゃいけないらしい。

ということで、CSSファイルにボタンに関するクラスを追加する。

```css
.btn-custom {
  font-size: 1.5rem;
  font-weight: 700;
  padding-top: 1.0rem;
  padding-bottom: 1.0rem;
}
```

HTMLを以下の様に書き換える。

```html
<button class="btn btn-primary btn-custom btn-block">
  Button
</button>
```

カスタムする前と比べると、HTMLで指定するクラスが2つ増えて、CSSにユーザー定義のクラスが1つ増えた。
しかも、ユーザー定義のクラスはすでにスタイルが定義されているクラスと名前が衝突しないように気をつかって名前をつけなくてはいけない。名前が衝突すると、本来無関係の要素までスタイルが書き換わってしまう恐れがあるからだ。

カスタムする前はとても簡単に思えたHTMLが、指定するクラスは増えて独自定義のCSSの面倒まで見なければいけなくなったのだ。

今度はTailwindcssのほうで同様のカスタマイズをやってみよう。

```html
<button class="bg-blue-500 hover:bg-blue-600 text-white px-4 py-4 rounded select-none w-full font-bold text-2xl">
  Button
</button>
```

こちらはクラスが色々とあるのは相変わらずだが、
新たにCSSにクラスを定義することはない。
HTMLの変更のみで終わった。

ここで重要なのは、CSSの管理から解放されていることだ。
CSSのユーザー定義が出てこないのである。
見た目の変更はクラスを修正した該当のHTML要素のみで行われる。
CSSクラスのスタイルの修正を間違えて、関係のない要素まで巻き込むことはない。
安全に見た目のカスタマイズが可能なのだ。
HTMLとCSSを交互に見てカスタマイズする必要もない。

そうはいっても、これじゃ同じボタン作るたびにそのクラス群をコピペすんのかよ？ と思われるかもしれない。
それなら、その部分のHTMLを切り取って使い回せるようにすればいいのだ。
Ruby on Railsならパーシャルとして分割できる。
ReactやVue.jsなどのコンポーネントベースのフレームワークなら、
ボタンをコンポーネントにしてしまえばいい。

でもやっぱり独自のコンポーネントクラスを作りたい、と思うかもしれない。
実はTailwindcssにはCSSのスタイル定義にユーティリティクラスが使えてしまう。
普通にCSSを書くより楽にCSSのユーザー定義が書けるのだ。

```css
.btn {
  @apply bg-blue-500 text-white px-4 py-2 rounded select-none;
}

.btn:hover {
  @apply bg-blue-600;
}
```

当然ユーザー定義のため名前の衝突は起こりうる。
しかし、ユーティリティクラスの命名規則さえ覚えておけば、
ライブラリの用意したコンポーネントクラスなんて無いから、
少なくともTailwindcssの定義したCSSとの名前衝突は起こらないだろう。

## スタイルの把握しやすさ 

ところで、Bootstrapの`.btn`にはどんなスタイルが適応されているかご存知だろうか？

```css
.btn {
  display: inline-block;
  font-weight: 400;
  color: #212529;
  text-align: center;
  vertical-align: middle;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  background-color: transparent;
  border: 1px solid transparent;
  padding: .375rem .75rem;
  font-size: 1rem;
  line-height: 1.5;
  border-radius: .25rem;
  transition: color .15s ease-in-out,background-color .15s ease-in-out,border-color .15s ease-in-out,box-shadow .15s ease-in-out;
}
```

何か色々とスタイルがあって把握しづらく感じるのではないだろうか。
まあ、そのおかげでいい感じの見た目になっているのだが。

通常のCSSを読み解くには、1行ずつスタイルを見て、
指定されている値は何で、そこで指定されている単位は何か、
ということを把握してようやく適用されているスタイルが把握できる。

このとき、そこで指定されている値と単位の組み合わせで具体的なスタイルがパッとイメージできるだろうか？
そもそも、そのような数値や単位はどういう基準で決められているのだろうか？
フレームワークの作成者の決めたそのマジックナンバーを、独自のカスタマイズ時には無視していいものだろうか？

一見使うのが簡単に思えたフレームワークでも、
いざ手を加えようと思うと、このような混沌に陥ってしまうだろう。
結果的に、見た目をカスタマイズするのが怖くなり、
CSSが嫌いになっていくかもしれない。

Tailwindcssはこの混沌を解決するアプローチを取っている。

今一度TailwindcssのボタンのHTMLを見てみよう。

```html
<button class="bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded select-none">
  Button
</button>
```

さっきのCSSを読むより、
ここで指定されているクラス群を読む方が、
どのようなスタイルが当てられているかが端的にわかると思わないだろうか？
（`transition`のアニメーションが無いことには目をつむっていただきたい）

Tailwindcssのユーティリティクラスは名前を見ればどのようにスタイルが指定されているか一目瞭然である。
大抵の場合1つのスタイルの変更しかしないのだから当然である。

さらに重要な特性として、
スタイルで使える値がTailwindcssによって限定されているということが挙げられる。

例えば、Tailwindcssでパディングを適用する場合は`p-2`、`px-8`といった感じで使うことができる。
`p`の後の文字でパディングを適用する方向(何もなければ全体に適用)と、`-`の後の数字でその大きさが決定される。

ここで重要なのが、大きさの値がすでにTailwindcssで決められているということだ。
Tailwindcssのパディングのスタイルを[ドキュメント](https://tailwindcss.com/docs/padding/)から見てみよう（一部抜粋）。

```css
.p-0 { padding: 0; }
.p-1 { padding: 0.25rem; }
.p-2 { padding: 0.5rem; }
.p-3 { padding: 0.75rem; }
.p-4 { padding: 1rem; }
```

単位は統一され、適当な値で大きさが割り振られている。

このことにより、Tailwindcssでパディングを使いたいと思った時は、
`p-`の後の値でいい感じに見えるものを決めて、それをそのまま使えばいいだけである。
パディングに限らず、マージンや色などについても同じだ。

Tailwindcssが使える値を限定していることで、
ユーザーがスタイルの具体的な値や単位に悩む必要性はなくなる。
自分で値や単位を設定せず、Tailwindcssが用意しているユーティリティクラスをそのまま使えばいいだけの話なのだ。

## CSSの理解

Tailwindcssのメリットを述べてきたが、
そうはいっても所詮ユーティリティクラスの集合である。
ノンデザイナーでも簡単にいい感じの見た目を作れる、
というようなことはTailwindcssにはできない。

Tailwindcss単体でいい感じの見た目を作るには、
CSSの理解、デザイン力、UIの設計力が必要になるだろう。
それに加えて、インタラクティブなUIを作りたいと思ったら、
JavaScript(TypeScript)の知識も必要となる。

では、デザインができないとか、特別CSSに詳しくないならTailwindcssは使うに値しないだろうか？

私は答えはノーと思っている。

ノンデザイナーでも、デザイナーの要望通りに画面を作りたいと思ったら、
やはりCSSの知識は必要になる。
Tailwindcssを使っていれば、
把握しやすいユーティリティクラスがCSS1つ1つのスタイルと1対1で対応しているので、
使っていくうちに自然とCSSの知識が、CSS単体で学ぶより身についてくる。

これはBootstrapでコンポーネントクラスの使い方を勉強しても、CSSの基礎的な知識は身につかないのと対照的だろう。

自分だけではいい感じのUIが思い浮かばない場合でも、
既存のCSSフレームワークの例を真似してみたり、
参考になるサイトを真似してみれば良いだろう。
もしくは、いっそのことデザイナーにいい感じの画面を考えて貰えばいいのだ。

もしデザイナーに頼れず自分だけでUIをまともに見せたいと思うなら、
Tailwindcssの作者が書いた[Refactoring UI](https://refactoringui.com/book)を読むことをオススメする。
UIを適切に見せるTipsが豊富に載っているので、
参考になること間違いない。

## まとめ

TailwindcssはCSSを「まともに」書けるようになるための
ツールだということがおわかりいただけただろうか。
ここでは詳しくは述べないが、Tailwindcssはレスポンシブな画面を作るのも簡単にできるようになっている。

CSSはスタイルの上書きとかタイプミスをしても実行エラーを出さないとか、
不可解な優先順位でスタイルがうまく適用されないとか、
様々なストレスがつきまとうものである。
Tailwindcssを使ってそのストレスから解放されて、自由にWebの画面を作り込む楽しさを味わいたいと思わないだろうか？

Tailwindcssには既製品のコンポーネントは存在しない。
あるのは普通よりまともな小さな部品の集合だけだ。
そこには、1からよりよいものを組み立てる喜びがあると私は信じたい。
