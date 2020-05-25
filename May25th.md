# 5月25日の学び

## TIPS
- `margin: 0 auto;`は`inline-block`化したオブジェクトには効かない。
- 要素を中央に配置する方法として、親要素に対して`text-aligin: center;`を指定する方法がある。
- プロパティのインデントを揃えると綺麗。

## display: inline-block; について
要素の横並びを実現させたい時に使用する`display: inline-block;`は  
横並びにすることができるが、要素の下部に揃って横並びになってしまう。  

つまり縦のラインが揃わなくなってしまう。  
これを解決するには`vertical-align`を使用する。  

`vertical-align`は  
`top`を指定すれば上部揃え  
`middle`を指定すれば中央揃え  
にすることができるので便利。  

## position: absoluteについて
position: absoluteは絶対値の考え方で要素の位置を指定する。  
絶対値の初期値となる位置は`position: relative`で指定する。  
この初期値からどれだけの位置に要素を配置するかを指定するのに使用する。  

## クラスの命名規則に関して
クラスの命名規則は、HTMLやCSSのファイルを見た人がそのページを頭でイメージを浮かべられるくらいのわかりやすさで記述する。  

一番親の要素が`main`というクラス名だと、何のmainなのかがわからない。  
`少し長いが意味がわかりやすい > 短く簡潔だが意味がわかりづらい`  
これを意識してコードを書くクセをつける。  
書籍のリーダブルコードは早めに読んで、コード書くさいの考え方を学んだ方が良い。  
初任給もいただいたので、早速買おうと思います。  

## 様々な記述方法　適切な書き方はどれか
同じような要素の記事が並ぶ場合、同じCSSを何度も記述することがある。  
その場合にはCSSを何度も記述しなくて済むようにする方法がいくつかある。  
```
<section class=“article-A section-container”>
</section>
<section class=“article-B section-container”>
</section>
```
### mixinとinclude
クラスで共通して使用するプロパティをひとまとめにすることで、自由に呼び出して使用することができる。  
しかしこの方法でSCSSをコンパイルして生成したCSSはそれぞれのクラスに同じプロパティが記述されたものとなる。  
つまり、記述数が少なく、見た目もすっきりと書くことができるが、
コンピュータが読み込む際にはコードが長く、同じ記述を何度も読み込むことになり  
これが積もるとviewを表示するのに時間のかかる結果となってしまう。  
```
// 共通クラスのmixin
mixin section-container {
  width:       400px;
  text-aligin: center;
}

.article-A {
color: red;
include: section-container;
}

.article-B {
color: blue;
include: section-container;
}

```

### サブクラス
クラスとは別にサブクラスを与えて、そのサブクラスに対してCSSを当てることで  
共通のCSSの記述を一つにまとめることができる。
この方法ならコンパイルしても重複した記述はなく、読み込みも速い  
```
.article-A {
color: red;
}
.article-B {
color: blue;
}

// 共通のサブクラス
.section-container {
  width:       400px;
  text-aligin: center;
}
```

### extend
CSSでも継承を使用することができる。  
継承元のクラス名をextendで指定することで同じプロパティを継承することができる。  
コンパイル時には継承元のクラスも、通常のCSSと同じように記述される。  
クラス名の先頭に%をつけることでコンパイル時に表示されないようにできる。  
コンパイル時にはincludeと同じく同じプロパティがそれぞれのクラスに書き込まれることになるので、積もると読み込みの遅さにつながる。  

```
// extendsされることが前提のclass

.section-container {
  width:       400px;
  text-aligin: center;
}

// %をつけるとコンパイル時に記述されない
%section-container {
  width:       400px;
  text-aligin: center;
}
.article-A {
color: red;
  extend section-container;
}
.article-B {
color: blue;
  extend section-container;
}
```

以上のような挙動になる。  
それぞれプロダクトの記述ルールがあると思うが、サブクラスが読み込みなどの影響も含めて最適な記述に感じた。  

## mainタグとarticleタグ
<main>タグは文書内の主要なコンテンツを表す。  
<article>タグはその名の通り記事などを表すタグ。  

