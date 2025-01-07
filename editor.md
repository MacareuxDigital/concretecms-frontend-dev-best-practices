---
layout: page
title: エディタ
---

## HTML を見なくても編集できる、そのためのエディタ

記事ブロックなどで使われる WYSIWYG エディタは、HTML を直接書く必要なく、タイプした文章に装飾を加えたり、画像や表組みを挿入できる便利な機能です。
「太字」ボタンをクリックすれば `<strong>` タグが自動で挿入されますし、書式で「見出し１」を選べば段落が `<h1>` タグに変わります。
これらのタグを理解している必要はありませんし、実際に使用するのはタグを理解していない人だと想定しましょう。

しかし、スタイルシートの設計が、しばしばエディタでの編集を困難なものにすることがあります。例えば、次のような例です。

```html
<h1 class="content-header margin-bottom-20 margin-top-40">見出し１</h1>
<p class="content-text margin-bottom-20">本文</p>
```

`content-header` や `margin-bottom-20` といったクラス属性値が設定されていないと、余白もなし、文字サイズの変化もなし、というスタイルシートの設計がされる場合があります。全ての要素のスタイルをいったん「リセット」し、クラス属性によって変化をつけていくこのような設計は、スタイルシートの干渉を防ぐ安全な設計としてよく使われていますが、エディタとの相性は最悪です。コンテンツの編集者は、いちいちエディタの「ソース」ボタンをクリックし、これらのクラス属性値を書き込む手間を強いられるでしょう。

CMS で更新されることを考慮すれば、スタイルシートの設計は複雑になるかもしれませんが、最適なマークアップは次の通りです。

```html
<h1>見出し１</h1>
<p>本文</p>
```

## CKEditor エディタが挿入する HTML タグ

各種ボタンで挿入されるタグの一覧（インストール直後の記事ブロックエディター設定では無効になっているボタンもあります）

| ボタン | 挿入されるタグ |
| ---- | ---- |
| 太字 | `strong` |
| 斜体 | `em` |
| 下線 | `u` |
| 打ち消し線 | `s` |
| 下付き | `sub` |
| 上付き | `sup` |
| 番号付きリスト | `ol` |
| 番号なしリスト | `ul` |
| インデント | `style="margin-left: 40px;"` |
| 中央 | `style="text-align: center;"` |
| 右揃え | `style="text-align: right;"` |
| 両端揃え | `style="text-align: justify;"` |
| ブロック引用文 | `blockquote` |
| 水平線 | `hr` |
| 顔文字 | `<img alt="smiley" height="23" src="regular_smile.png" title="smiley" width="23" />` など |
| 改ページ | `<div style="page-break-after: always"><span style="display: none;">&nbsp;</span></div>` |
| フォント | `<span style="font-family:Arial,Helvetica,sans-serif;">` など |
| サイズ | `<span style="font-size:20px;">` など |
| 文字色 | `<span style="color:#1abc9c;">` など |
| 背景色 | `<span style="background-color:#1abc9c;">` など |

段落の書式の選択肢の一覧

| 書式 | 挿入されるタグ |
| ---- | ---- |
| 標準 | `p` |
| 見出し１ | `h1` |
| 見出し２ | `h2` |
| 見出し３ | `h3` |
| 見出し４ | `h4` |
| 見出し５ | `h5` |
| 見出し６ | `h6` |
| 書式つき | `pre` |
| アドレス | `address` |
| 標準（DIV） | `div` |

表ボタンで挿入されるタグのサンプル

```html
<table border="1" cellpadding="1" cellspacing="1" style="width:500px;" summary="概要">
	<caption>キャプション</caption>
	<thead>
		<tr>
			<th scope="col">ヘッダ</th>
			<th scope="col">ヘッダ</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>セル</td>
			<td>セル</td>
		</tr>
		<tr>
			<td>セル</td>
			<td>セル</td>
		</tr>
	</tbody>
</table>
```

## Concrete CMS のテーマの影響

エディタの機能は Concrete CMS の機能として用意されているものですが、デザインの都合に合わせて、テーマからいくつかのカスタマイズが可能です。

### レスポンシブ画像

ひとつの HTML でデスクトップの大きな画面からスマートフォンの小さな画面まで対応するレスポンシブデザインでは、レイアウトはスタイルシートで調整できますが、大きなデスクトップ用の画像を回線の遅いスマートフォンにも配信してしまう問題があります。これを解決するのが、HTML5 で追加された [`<picture>` 要素](https://developer.mozilla.org/ja/docs/Web/HTML/Element/picture)です。このレスポンシブ画像を使うと、画面サイズに応じて画像ファイルを出し分けることができます。レスポンシブ画像は、テーマ内の `page_theme.php` ファイルで、[レスポンシブ画像用の設定を行う](https://concrete5-japan.org/help/5-7/developer/designing-for-Concrete CMS/supporting-responsive-images-in-your-Concrete CMS-theme/)ことで有効にできます。エディタで挿入した画像は、このテーマの設定によって出力が変化します。

テーマがレスポンシブ画像をサポートしている場合：

```html
<picture>
    <!--[if IE 9]><video style='display: none;'><![endif]-->
    <source srcset="/application/files/thumbnails/large/6115/2282/3080/sunset.jpg" media="(min-width: 900px)">
    <source srcset="/application/files/thumbnails/medium/6115/2282/3080/sunset.jpg" media="(min-width: 768px)">
    <source srcset="/application/files/thumbnails/small/6115/2282/3080/sunset.jpg">
    <!--[if IE 9]></video><![endif]-->
    <img src="/application/files/thumbnails/small/6115/2282/3080/sunset.jpg" alt="sunset.jpg">
</picture>
```

テーマがレスポンシブ画像をサポートしていない場合：

```html
<img src="/application/files/6115/2282/3080/sunset.jpg" alt="sunset.jpg" width="1600" height="953">
```

#### 画像に関するその他の注意点

`width` 属性と `height` 属性は、エディタの画像ボタンで挿入すると自動的に挿入されます。「画像のプロパティ」ウィンドウで数値を削除しない限り必ず入ります。両属性が設定されていても崩れないようにスタイルシートをコーディングしてください。

「画像のプロパティ」ウィンドウで行揃えを設定した場合は、`style` 属性が挿入されます。

```html
<!-- 左寄せ -->
<picture>
    <!--[if IE 9]><video style='display: none;'><![endif]-->
    <source srcset="/application/files/thumbnails/large/6115/2282/3080/sunset.jpg" media="(min-width: 900px)">
    <source srcset="/application/files/thumbnails/medium/6115/2282/3080/sunset.jpg" media="(min-width: 768px)">
    <source srcset="/application/files/thumbnails/small/6115/2282/3080/sunset.jpg">
    <!--[if IE 9]></video><![endif]-->
    <img src="/application/files/thumbnails/small/6115/2282/3080/sunset.jpg" alt="sunset.jpg" style="float: left;" width="200" height="119">
</picture>

<!-- 右寄せ -->
<picture>
    <!--[if IE 9]><video style='display: none;'><![endif]-->
    <source srcset="/application/files/thumbnails/large/6115/2282/3080/sunset.jpg" media="(min-width: 900px)">
    <source srcset="/application/files/thumbnails/medium/6115/2282/3080/sunset.jpg" media="(min-width: 768px)">
    <source srcset="/application/files/thumbnails/small/6115/2282/3080/sunset.jpg">
    <!--[if IE 9]></video><![endif]-->
    <img src="/application/files/thumbnails/small/6115/2282/3080/sunset.jpg" alt="sunset.jpg" style="float: right;" width="200" height="119">
</picture>

<!-- 中央揃えは設定できません -->
```

#### 拡張画像

管理画面の「記事ブロックエディター設定」ページで「拡張画像」ボタンを有効にすると、画像にキャプションをつけることができます。

```html
<figure class="content-editor-image-captioned">
    <picture>
        <!--[if IE 9]><video style='display: none;'><![endif]-->
        <source srcset="/application/files/thumbnails/large/6115/2282/3080/sunset.jpg" media="(min-width: 900px)">
        <source srcset="/application/files/thumbnails/medium/6115/2282/3080/sunset.jpg" media="(min-width: 768px)">
        <source srcset="/application/files/thumbnails/small/6115/2282/3080/sunset.jpg">
        <!--[if IE 9]></video><![endif]-->
        <img src="/application/files/thumbnails/small/6115/2282/3080/sunset.jpg" alt="sunset.jpg" height="953" width="1600">
    </picture>
    <figcaption>キャプション</figcaption>
</figure>
```

また、行揃えを設定した際に出力されるコードも、`style` 属性での直接出力ではなく、class 属性値が変化する方式に変わります。

```html
<!-- 左寄せ、キャプションなし -->
<p>
    <picture>
        <!--[if IE 9]><video style='display: none;'><![endif]-->
        <source srcset="/application/files/thumbnails/large/6115/2282/3080/sunset.jpg" media="(min-width: 900px)">
        <source srcset="/application/files/thumbnails/medium/6115/2282/3080/sunset.jpg" media="(min-width: 768px)">
        <source srcset="/application/files/thumbnails/small/6115/2282/3080/sunset.jpg">
        <!--[if IE 9]></video><![endif]-->
        <img src="/application/files/thumbnails/small/6115/2282/3080/sunset.jpg" alt="sunset.jpg" class="content-editor-image-left" height="953" width="1600">
    </picture>
</p>

<!-- 中央寄せ、キャプションなし -->
<p>
    <picture><!--[if IE 9]><video style='display: none;'><![endif]-->
        <source srcset="/application/files/thumbnails/large/6115/2282/3080/sunset.jpg" media="(min-width: 900px)">
        <source srcset="/application/files/thumbnails/medium/6115/2282/3080/sunset.jpg" media="(min-width: 768px)">
        <source srcset="/application/files/thumbnails/small/6115/2282/3080/sunset.jpg">
        <!--[if IE 9]></video><![endif]-->
        <img src="/application/files/thumbnails/small/6115/2282/3080/sunset.jpg" alt="sunset.jpg" class="content-editor-image-center" height="953" width="1600">
    </picture>
</p>

<!-- 右寄せ、キャプションあり -->
<figure class="content-editor-image-captioned content-editor-image-right">
    <picture>
        <!--[if IE 9]><video style='display: none;'><![endif]-->
        <source srcset="/application/files/thumbnails/large/6115/2282/3080/sunset.jpg" media="(min-width: 900px)">
        <source srcset="/application/files/thumbnails/medium/6115/2282/3080/sunset.jpg" media="(min-width: 768px)">
        <source srcset="/application/files/thumbnails/small/6115/2282/3080/sunset.jpg">
        <!--[if IE 9]></video><![endif]-->
        <img src="/application/files/thumbnails/small/6115/2282/3080/sunset.jpg" alt="sunset.jpg" height="953" width="1600">
    </picture>
    <figcaption>キャプション</figcaption>
</figure>
```

### エディタークラス

エディタで挿入される要素に class 属性を設定するのは避けた方が良いと冒頭で紹介しましたが、とはいえデザイン上のバリエーションを持たせるために、どうしても必要な場合もあります。そのような場合に、エディタの「スタイル」メニューから選択させるという機能を使うことができます。

![スタイルメニューの例](https://raw.githubusercontent.com/MacareuxDigital/concretecms-frontend-dev-best-practices/master/images/editor-class.png)

詳しくは、[エディタークラスのドキュメント](https://concrete5-japan.org/help/5-7/developer/designing-for-concrete5/advanced-css-and-javascript-usage/adding-custom-css-classes-to-blocks-areas-and-the-editor/#editor-class)をご参照ください。