---
layout: page
title: CSS / JavaScript
---

このページでは、独自のテーマを作成する際に CSS や JavaScript で気をつけるべきことをご紹介します。

## CMS で更新されることを意識する

まず Concrete CMS に限らず全ての CMS を利用したサイトに共通して気をつけるべき点をいくつかご紹介します。

### 文字量は変化するものと心得る

これはひょっとするとコーディング時点で気をつけていては手遅れで、デザイン時点で気をつけるべき点なのかもしれないですが、文字量は変化するものと想定してコーディングしてください。
「ダミーテキストダミーテキスト」だとなんら問題なく表示できていても、CMS を使ってコンテンツを更新する人が必ずこの文字数に合わせてくれるわけではありません。
少ない文字数の場合でも、逆に多い文字数で複数行に渡る場合でも、崩れないようコーディングしておく必要があります。

文字数の変化を想定していなかった場合に、テキストを途中で切って後ろに「...」を付けるのはありがちな回避策ですが、10文字程度でそれを行なってしまっては、ユーザーの利便性を著しく損なうでしょう。

### トルツメに対応する

CMS を使って更新する際、常に魅力的なサムネイル画像や簡潔な説明文が入力されるわけではありません。
また、Concrete CMS の「ページリスト」ブロックでは、ブロックのオプションとしてサムネイル画像や日付などを表示させるかどうかを設定できます。
それらの要素がHTMLに出力されなくても、崩れないように CSS をコーディングしましょう。

### style 属性や class 属性による微調整に依存しない

上記の文字量の変化やトルツメに対応する際、HTML の構造に変化が生じないことが望ましい対応です。

避けた方が良い対応：

```html
<div class="post">
  <div class="title shoft-title">ダミータイトル</div>
  <div class="description">ダミー説明ダミー説明ダミー説明ダミー説明</div>
</div>
<div class="post">
  <div class="title long-title">ダミータイトルダミータイトルダミータイトルダミータイトルダミータイトル</div>
  <div class="description" style="padding: 0;"></div>
</div>
```

上記の例では、文字量が多い場合や文字が入力されていない場合に、専用の class 属性値を追加したり、style 属性を追加することで表示崩れを回避しようとしています。テンプレート内で PHP を使った条件分岐を記述すればできそうですが、できるかどうかの見極めに神経を使うよりは、はじめからこのような対応を行わないほうが良いでしょう。

なお、CMS で更新する際に WYSIWYG エディタで編集させる領域で class 属性や style 属性による微調整に依存すると、それはそのまま使いにくさに繋がります。詳しくは[エディタ](./editor.html)のページも参照してください。

## Concrete CMS の編集モードとウェブページの共存

Concrete CMS では、ウェブページを見たまま編集することができます。Concrete CMS のインターフェースと、ウェブページそのものが、同じ HTML の中に両方出力されているという状態です。普段 CSS や JavaScript を書いている方であれば、そりゃマズイ、とピンとくるかもしれません。
何の対策もしなければ、お互いがお互いに影響を与えてしまい、Concrete CMS のインターフェースが使えなくなったり、ウェブページが崩れてしまったりしてしまいます。

干渉を防ぐためには、Concrete CMS が行なっている干渉を防ぐための工夫を知り、あなたが作成するテーマで使用する CSS や JavaScript のコーディングで特定の記述を避ける必要があります。

### ウェブページは全て div.ccm-page の中へ

「同じ HTML の中に Concrete CMS のインターフェースとウェブページそのものが同時に出力される」問題に対して、Concrete CMS が採用した解決策はシンプルです。

* ウェブページは全て `<div class="ccm-page"></div>` の中に入れること。
* Concrete CMS のインターフェースは全て `<div class="ccm-ui"></div>` の中に入れること。

このシンプルなルールを守れば、お互いのCSSが影響し合うことを避けられます。  
Concrete CMS サイトのために HTML コーディングを行う際には、必ずこのルールを守ってください。

悪い例：このまま Concrete CMS のテーマにしてしまうと Concrete CMS のインターフェースと干渉します。

```html
<body>
  <h1>タイトル</h1>
</body>
```

```css
body { background: green; }
h1 { font-size: 18px; }
```

良い例：Concrete CMS のインターフェースと分けられているので良い例です。

```html
<body>
  <div class="ccm-page">
    <h1>タイトル</h1>
  </div>
</body>
```

```css
.ccm-page { background: green; }
.ccm-page h1 { font-size: 18px; }
```

### Concrete CMS 関連の JavaScript は命名規則で分離

Concrete CMS の直感的なインターフェースの挙動は、JavaScript によって実現されています。Concrete CMS が定義する関数は `Concrete` や `ccm_` が先頭に付き、jQuery プラグインは `concrete` が先頭に付きます。

例： `ConcreteToolbar`, `ccm_addError`, `$.concreteMenu()`

Concrete CMS サイトのために JavaScript を記述する際は、`concrete` や `ccm` という単語を使わないように気をつけてください。

## Concrete CMS の編集モードを破壊する避けるべき記述

ウェブページの HTML を全て `div.ccm-page` の中に収めることでほとんどの問題は回避できますが、いくつかそのほかにも気をつけるべき点があります。

### CSS で避けるべき記述

#### z-index, position

Concrete CMS の編集モードで、クリックするとメニューが出現する領域や、ブロックをドラッグする際につかむための領域は、`position: absolute;` で配置されています。

![メニュークリックプロキシ](https://raw.githubusercontent.com/MacareuxDigital/concretecms-frontend-dev-best-practices/master/images/click-proxy.png)

そのため、ウェブページ内の要素でも同様に CSS で `position` を利用して配置を行なっていると、Concrete CMS のこれらのインターフェースより上の階層にかぶさってしまい、ブロックの編集や移動ができなくなる場合があります。それを避けるために、ウェブページ内の要素では、CSS の `z-index` の値で 500 以上の値を使ってはいけません。また、`z-index` の値が 500 未満であっても、スタック文脈を変更することで、やはり Concrete CMS のインターフェースより上にかぶさってしまう現象が起こります。

* [ブロックをドラッグするハンドルが掴めなくなる現象のデモ](https://codepen.io/hissy-github/pen/XEBYJB)
* [スタック文脈 - MDN ウェブデベロッパーガイド](https://developer.mozilla.org/ja/docs/Web/Guide/CSS/Understanding_z_index/The_stacking_context)

もっとも確実な回避方法は、Concrete CMS で編集したい領域では、いっさい `position` や `z-index` を使わないことです。しかし、デザインによってはこの制限は不可能な場合もありますので、その際は[編集モードかどうかを判定し](https://concrete5-japan.org/help/5-7/developer/working-with-pages/getting-data-about-a-page/)、編集モード中は `position` や `z-index` を打ち消す CSS を追加で読み込ませることになるでしょう。
ただし、CSS の打ち消しは必ずしも常にうまくいくわけではなく、場合によっては CSS の大幅な修正が発生することになります。

### JavaScript で避けるべき記述

JavaScript では、幸いなことに CSS ほどの制限はありませんが、外部ライブラリには気をつける必要があります。特に気をつけるべきは **jQuery** です。

#### Concrete CMS に同梱されている jQuery 以外の jQuery に依存する

[jQuery](http://jquery.com/) は非常に普及している JavaScript ライブラリで、多くのサイトやウェブアプリケーションで使われていますが、Concrete CMS も例外ではありません。実際、Concrete CMS の編集モードは jQuery に多くを依存しているため、jQuery の読み込みをやめると言うことはできません。そのため、ウェブページで使用するための JavaScript ファイルの方が譲歩する必要があります。まず、jQuery のバージョンは、[Concrete CMS に同梱されているもの](https://github.com/concretecms/concretecms/blob/develop/concrete/js/jquery.js)と同じにする必要があります。jQuery は現在バージョン3.xがリリースされていますが、Concrete CMS に同梱されているのはバージョン1.xです。

また、jQuery のバージョンを合わせているのに Concrete CMS が動かなくなると言うトラブルも起こりがちですが、この場合は Concrete CMS に同梱されている jQuery とは別に、重複して jQuery を読み込ませていることが原因です。Concrete CMS はその動作のために独自の jQuery プラグインを実装していますが、重複して jQuery を読み込ませることで、それらのプラグインが全てリセットされてしまうのです。

#### Concrete CMS に同梱されている Bootstrap 以外の Bootstrap に依存する

[Bootstrap](https://getbootstrap.com/) もレスポンシブデザインのウェブサイトではよく使われるライブラリです。Concrete CMS は[バージョン3.xの Bootstrap のライブラリのいくつかを使用しています](https://github.com/concretecms/concretecms/tree/develop/concrete/js/build/vendor/bootstrap)。
Bootstrap は現在バージョン4.xがリリースされていますが、jQuery と同様に Concrete CMS に同梱されているバージョンに合わせる必要があります。

#### その他のライブラリ

そのほかにも Concrete CMS では様々な JavaScript ライブラリを利用していますが、編集モードでは使われないものや、利用される頻度の低いものが多いため、あまり問題にはなりません。
気になる方はドキュメントの[アセットリスト](https://concrete5-japan.org/help/5-7/developer/appendix/asset-list/)をご確認ください。

## Concrete CMS が出力するコードを知って活用しよう

Concrete CMS が出力するコードには、CSS や JavaScript の実装の際に便利なものがあります。また、独自に実装しなくても、そもそも Concrete CMS に用意されている機能を使えば済むものもあります。

### ページラッパークラスを活用しよう

ページラッパークラスとは、ウェブページ全体を囲む div 要素に付けられる class 属性のことです。すでにでてきた `.ccm-page` もそのひとつです。テーマの中の `<?php echo $c->getPageWrapperClass()?>` という記述で出力されます。

`.ccm-page` クラス以外にも、現在のページタイプごとに出力される `.page-type-example` クラス（`example` 部分はページタイプのハンドル）、`.page-template-example` クラス（`example` 部分はページテンプレートのハンドル）が出力されますので、ページタイプやページテンプレートごとにスタイルシートを調整したい用途に使うことができます。

### デザイン機能でできること、起こること

Concrete CMS には、ブロックやエリアに対して、編集モードから背景色や余白などを調整できるデザイン機能があります。
この機能でできる範囲であれば、あらかじめ CSS のクラスを用意しておくことなく、Concrete CMS の機能を利用して微調整が可能です。

![デザイン機能](https://raw.githubusercontent.com/MacareuxDigital/concretecms-frontend-dev-best-practices/master/images/design-panel.png)

| デザインパネル名称 | 機能 |
| ---- | ---- |
| テキストサイズと色 | 文字色、リンク色、文字サイズ、位置合わせ（`text-align`）を設定できます。 |
| 背景色と画像 | 背景色と背景画像（`background-image`, `background-repeat`, `background-size`, `background-position`）を設定できます。 |
| 枠 | 枠（`border-width`, `border-style`, `border-color`, `border-radius`）を設定できます。 |
| マージンとパディング | margin と padding を上下左右それぞれ px で設定できます。 |
| シャドウと回転 | シャドウ（`box-shadow`）と回転（`transform: rotate`）、デバイスごとの表示・非表示を設定できます。デバイスごとの表示・非表示は、テーマが採用している CSS フレームワークがサポートしている場合に限ります（Bootstrap における `hidden-xs` クラスなど）。 |
| カスタムCSSクラス、ブロック名、カスタムテンプレートやスタイルのリセット | [カスタムテンプレート](https://concrete5-japan.org/help/5-7/developer/working-with-blocks/working-with-existing-block-types/creating-additional-custom-view-templates/)を選択できます。そのほか、クラス属性値、ID属性値、任意の属性値（data-* 属性など）の設定、グリッドコンテナの無効化・有効化が行えます。グリッドコンテナの無効化・有効化は、テーマが採用している CSS フレームワークが対応している場合に限ります（Bootstrap における `div.container` が該当）。 |

特にカスタムクラス機能は、ブロックのデザインにバリエーションを持たせたい場合に、class 属性値を簡単に設定することができ、かつカスタムテンプレートを開発する必要がないため、重宝します。
これはデザイナーおよびフロントエンドエンジニアの都合ではありますが、コンテンツ編集者の利便性も損なわないよう、[テーマからカスタムクラスの選択肢をあらかじめ設定できる](https://concrete5-japan.org/help/5-7/developer/designing-for-Concrete CMS/advanced-css-and-javascript-usage/adding-custom-css-classes-to-blocks-areas-and-the-editor/)ようになっています（編集者に英語で `column` などの class 属性値をタイプすることに納得してもらうのは骨の折れる作業ですが、選択肢の中から選ぶだけでいいとなればいくらか緩和します）。

これらのデザイン機能をあてにして CSS を設計する場合に、実際にどのようにデザインの変更が適用されるのかを知っておく必要があります。

カスタムデザイン適用前（記事ブロックの場合）：

```html
<p>記事ブロックで入力したコンテンツ</p>
```

カスタムデザイン適用後：

* 背景色を指定
* カスタムクラスとして `example-custom-class` を指定
* カスタムIDとして `example-custom-id` を指定
* カスタムエレメント属性として `example-element="example"` を指定

```html
<div class="ccm-custom-style-container ccm-custom-style-main-188 example-custom-class"
        id="example-custom-id"
            example-element="example">
    <p>記事ブロックで入力したコンテンツ</p>
</div>
```

このように、ブロックの外側に `<div>` 要素が**追加され**、追加された要素に対してカスタムクラスやカスタムIDなどが適用されます。
背景色などの設定は、`<head>` 要素内に `<style>` 要素が追加され、その中に出力されます。

```html
<style type="text/css" data-area-style-area-handle="Main" data-block-style-block-id="188" data-style-set="34">
.ccm-custom-style-container.ccm-custom-style-main-188{
    background-color:rgb(255, 0, 0)
}
</style>
```

この仕様は、カスタムテンプレートと共存させる場合に重要な意味があります。例えば、カスタムテンプレートで、ブロックを `div.content-wrapper` で囲んだとします。

```html
<div class="content-wrapper">
    <p>記事ブロックで入力したコンテンツ</p>
</div> 
```

さらに色のバリエーションを持たせるために、Concrete CMS のカスタムクラス機能に期待するとしましょう。しかし、このような html を想定してスタイルシートを用意してはいけません。

```html
<div class="content-wrapper color-red">
    <p>記事ブロックで入力したコンテンツ</p>
</div> 
```

カスタムクラス機能を使って実際に出力されるのはこうです。

```html
<div class="color-red">
    <div class="content-wrapper">
        <p>記事ブロックで入力したコンテンツ</p>
    </div>
</div>
```

また、**デザイナーやフロントエンドエンジニアがこのデザイン機能を使う予定がなかったとしても、コンテンツ編集者は使う可能性があります**。  
**`<div>` 要素がブロックの外側に追加されても破綻しないよう、スタイルシートを設計してください**。