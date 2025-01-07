---
layout: page
title: ブロック一覧
---

Concrete CMS に標準インストールされているブロックの一覧です。
ブロックの概要を記載していますので、サイトの設計時に参考にしてください。

また、ブロックが出力する HTML のサンプルを記載しています。
HTML / CSS コーディングの際に標準の出力結果に合わせることで、カスタムテンプレートの開発コストを下げることができます。

## 記事 / HTML

記事ブロックは WYSIWYG エディタ、HTML ブロックはコードエディタで入力した内容を、それぞれ `<div>` 要素で囲んだりせず、そのまま出力します。

### 注意事項

上述の通り、記事ブロックの中のコンテンツは、`<div>` 要素で囲まれずそのまま出力されます。マークアップの際も記事ブロックを何かの要素で囲むようにするべきではありません。どうしても何かの要素で囲まないと難しいという場合、テンプレートをカスタマイズすれば `<div class="content-block"></div>` など class 属性をつけて囲むとも可能ですが、記事ブロックの編集中はテンプレートが適用されないということにご注意ください。編集中にスタイルが外れてしまうことを防ぐには、CSS 側で追加の対応が必要です。

```css
.content-block p,
.ccm-block-edit p,
.cke_editable p {
    margin-bottom: 1em;
}
```

エディタに関する詳細については、[エディタ](./editor.html)のページも参考にしてください。

## 画像

ファイルマネージャーにアップロードされた任意の画像をページに出力します。画像の alt / title 属性値と、画像からのリンクの設定ができます。

### 出力

テーマがレスポンシブ画像をサポートしている場合：

```html
<a href="http://www.example.com/" target="_blank">
    <picture>
        <!--[if IE 9]><video style='display: none;'><![endif]-->
        <source srcset="/application/files/thumbnails/large/6115/2282/3080/sunset.jpg" media="(min-width: 900px)">
        <source srcset="/application/files/thumbnails/medium/6115/2282/3080/sunset.jpg" media="(min-width: 768px)">
        <source srcset="/application/files/thumbnails/small/6115/2282/3080/sunset.jpg">
        <!--[if IE 9]></video><![endif]-->
        <img src="/application/files/thumbnails/small/6115/2282/3080/sunset.jpg" alt="Altテキスト（任意）" class="ccm-image-block img-responsive bID-193" title="タイトル（任意）">
    </picture>
</a>
```

テーマがレスポンシブ画像をサポートしていない場合：

```html
<a href="http://www.example.com/" target="_blank">
    <img src="/application/files/6115/2282/3080/sunset.jpg" alt="Altテキスト（任意）" width="1600" height="953" class="ccm-image-block img-responsive bID-193" title="タイトル（任意）">
</a>
```

## ファイル

ファイルマネージャーにアップロードされた任意のファイルのダウンロードリンクを出力します。

### 出力

```html
<div class="ccm-block-file">
    <a href="https://concretecms.text/index.php/download_file/18/218">リンク文字（任意）</a>
</div>
```

## 水平線

`<hr />` タグを出力するだけのシンプルなブロックです。

### 出力

```html
<hr/>
```

## 特色

アイコンとタイトルと説明文という、ウェブサイトで頻出のパターンを出力できるブロックです。アイコンは Font Awesome 3 のアイコンを選択することができます。

### 出力

```html
<div class="ccm-block-feature-item">
    <h4><i class="fa fa-500px"></i> <a href="http://www.example.com/">タイトル（任意）</a></h4>
    <p>説明文（任意）</p>
</div>
```

## オートナビ

サイトマップの特定の階層や任意のページを起点に、階層の深さを指定して表示します。サイトマップと連動したナビゲーションの設置ができます。

### 出力

`nav` `nav-path-selected` `nav-selected` などの class 属性値はカスタムテンプレートで変更できます。

```html
<ul class="nav">
    <li class=""><a href="https://concretecms.text/index.php/services" target="_self" class="">Services</a></li>
    <li class=""><a href="https://concretecms.text/index.php/portfolio" target="_self" class="">Portfolio</a></li>
    <li class="nav-path-selected"><a href="https://concretecms.text/index.php/team" target="_self" class="nav-path-selected">Team</a>
        <ul>
            <li class="nav-selected nav-path-selected"><a href="https://concretecms.text/index.php/team/about" target="_self" class="nav-selected nav-path-selected">About</a></li>
            <li class=""><a href="https://concretecms.text/index.php/team/faq" target="_self" class="">Frequently Asked Questions</a></li>
            <li class=""><a href="https://concretecms.text/index.php/team/careers" target="_self" class="">Careers</a>
                <ul>
                    <li class=""><a href="https://concretecms.text/index.php/team/careers/web-developer" target="_self" class="">Web Developer</a></li>
                    <li class=""><a href="https://concretecms.text/index.php/team/careers/sales-associate" target="_self" class="">Sales Associate</a></li>
                </ul>
            </li>
        </ul>
    </li>
    <li class=""><a href="https://concretecms.text/index.php/blog" target="_self" class="">Blog</a></li>
    <li class=""><a href="https://concretecms.text/index.php/contact" target="_self" class="">Contact</a></li>
</ul>
```

## ページタイトル

現在のページのタイトルを出力できるブロックです。`h1` から `h6` まで出力されるタグを選択できます。

### 出力

```html
<h1 class="page-title">ページ名がここに自動で出力されます。</h1>
```

## FAQ

質問と答えのセットを繰り返し表示でき、目次も出力します。
答えは WYSIWYG エディタで入力できますので、複数行に渡って任意の HTML タグを使えます。

### 出力

```html
<div class="ccm-faq-container">
    <div class="ccm-faq-block-links">
        <a href="#1981">リンクテキスト（任意）</a>
        <a href="#1982">質問２</a>
    </div>
    <div class="ccm-faq-block-entries">
        <div class="faq-entry-content">
            <a name="1981"></a>
            <h3>タイトルテキスト（任意）</h3>
            <p>説明文（任意）</p>
        </div>
        <div class="faq-entry-content">
             <a name="1982"></a>
             <h3>質問２</h3>
             <p>質問２への答え</p>
        </div>
    </div>
</div>
```

## ページリスト

サイト内のページを指定した条件で絞り込んで、指定した並び順で表示します。ページ送りナビゲーションも表示できます。
class 属性値はカスタムテンプレートで変更できます。
[ページ送り部分のカスタマイズは可能](https://concrete5-japan.org/help/5-7/recipes/customize-pagination/)ですが、少し面倒なので、この出力に合わせて CSS をコーディングする方が楽です。

### 出力

```html
<div class="ccm-block-page-list-wrapper">
    <div class="ccm-block-page-list-header">
        <h5>ページリストのタイトル（任意）</h5>
    </div>
    <div class="ccm-block-page-list-pages">
        <div class="ccm-block-page-list-page-entry">
            <div class="ccm-block-page-list-page-entry-text">
                <div class="ccm-block-page-list-title">
                    <a href="https://concretecms.text/index.php/218" target="_self">ページ名</a>
                </div>
                <div class="ccm-block-page-list-date">2018/05/06 13:12</div>
                <div class="ccm-block-page-list-description">説明文</div>
            </div>
        </div>
        <div class="ccm-block-page-list-page-entry-horizontal">
            <div class="ccm-block-page-list-page-entry-thumbnail">
                <img src="/application/files/6015/2282/3080/chinese_house2.jpg" alt="chinese_house2.jpg" width="200" height="200" class="img-responsive">
            </div>
            <div class="ccm-block-page-list-page-entry-text">
                <div class="ccm-block-page-list-title">
                    <a href="https://concretecms.text/index.php/blog/209" target="_self">サムネイル画像あり</a>
                </div>
                <div class="ccm-block-page-list-date">2018/04/11 13:56</div>
                <div class="ccm-block-page-list-description"></div>
            </div>
        </div>
    </div><!-- end .ccm-block-page-list-pages -->
</div><!-- end .ccm-block-page-list-wrapper -->
<div class="ccm-pagination-wrapper">
    <ul class="pagination">
        <li class="prev disabled"><span>&larr; 前へ</span></li>
        <li class="active"><span>1 <span class="sr-only">（このページ）</span></span></li>
        <li><a href="/index.php?cID=218&amp;ccm_paging_p=2&amp;ccm_order_by=cv.cvDatePublic&amp;ccm_order_by_direction=desc">2</a></li>
        <li><a href="/index.php?cID=218&amp;ccm_paging_p=3&amp;ccm_order_by=cv.cvDatePublic&amp;ccm_order_by_direction=desc">3</a></li>
        <li><a href="/index.php?cID=218&amp;ccm_paging_p=4&amp;ccm_order_by=cv.cvDatePublic&amp;ccm_order_by_direction=desc">4</a></li>
        <li><a href="/index.php?cID=218&amp;ccm_paging_p=5&amp;ccm_order_by=cv.cvDatePublic&amp;ccm_order_by_direction=desc">5</a></li>
        <li><a href="/index.php?cID=218&amp;ccm_paging_p=6&amp;ccm_order_by=cv.cvDatePublic&amp;ccm_order_by_direction=desc">6</a></li>
        <li><a href="/index.php?cID=218&amp;ccm_paging_p=7&amp;ccm_order_by=cv.cvDatePublic&amp;ccm_order_by_direction=desc">7</a></li>
        <li><a href="/index.php?cID=218&amp;ccm_paging_p=8&amp;ccm_order_by=cv.cvDatePublic&amp;ccm_order_by_direction=desc">8</a></li>
        <li><a href="/index.php?cID=218&amp;ccm_paging_p=9&amp;ccm_order_by=cv.cvDatePublic&amp;ccm_order_by_direction=desc">9</a></li>
        <li class="next"><a href="/index.php?cID=218&amp;ccm_paging_p=2&amp;ccm_order_by=cv.cvDatePublic&amp;ccm_order_by_direction=desc" rel="next">次へ &rarr;</a></li>
    </ul>
</div>
```

条件に該当するページがなかった場合：

```html
<div class="ccm-block-page-list-wrapper">
    <div class="ccm-block-page-list-pages">
    </div><!-- end .ccm-block-page-list-pages -->
    <div class="ccm-block-page-list-no-pages">表示するページがない場合のメッセージ（任意）</div>
</div><!-- end .ccm-block-page-list-wrapper -->
```

## 「次へ」「前へ」ナビ

前後のページへのリンクを出力するブロックです。前後を判定する基準は、サイトマップ順と時系列順から選べます。

### 出力

```html
<div class="ccm-block-next-previous-wrapper">
    <div class="ccm-block-next-previous-header">
        <h5>前</h5>
    </div>
    <p class="ccm-block-next-previous-previous-link">
        <a href="https://concretecms.text/index.php/contact">Contact</a>
    </p>
    <div class="ccm-block-next-previous-header">
        <h5>次</h5>
    </div>
    <p class="ccm-block-next-previous-next-link">
        <a href="https://concretecms.text/index.php/services">Services</a>
    </p>
    <p class="ccm-block-next-previous-parent-link">
        <a href="https://concretecms.text/index.php">上へ</a>
    </p>
</div>
```

## 日付ナビ

ページリストとセットで使用し、このブロックで表示された日付で絞り込んだ結果をページリストに表示することができます。

### 出力

```html
<div class="ccm-block-date-navigation-wrapper">
    <div class="ccm-block-date-navigation-header">
        <h5>アーカイブ</h5>
    </div>
    <ul class="ccm-block-date-navigation-dates">
        <li><a href="https://concretecms.text/index.php/218">すべて</a></li>
        <li><a href="https://concretecms.text/index.php/218/2018/04">4月 2018</a></li>
        <li><a href="https://concretecms.text/index.php/218/2014/08">8月 2014</a></li>
        <li><a href="https://concretecms.text/index.php/218/2014/07">7月 2014</a></li>
    </ul>
</div>
```

## タグ

ページリストとセットで使用し、このブロックで表示されたタグで絞り込んだ結果をページリストに表示することができます。

```html
<div class="ccm-block-tags-wrapper">
    <div class="ccm-block-tags-header">
        <h5>タイトル（任意）</h5>
    </div>
    <span class="ccm-block-tags-tag label">Apple</span>
    <span class="ccm-block-tags-tag label">Banana</span>
    <span class="ccm-block-tags-tag label">Carrot</span>
</div>
```

## トピックリスト

ページリストとセットで使用し、このブロックで表示されたトピックで絞り込んだ結果をページリストに表示することができます。

### 出力

```html
<div class="ccm-block-topic-list-wrapper">
    <div class="ccm-block-topic-list-header">
        <h5>タイトル（任意）</h5>
    </div>
    <ul class="ccm-block-topic-list-list">
        <li>レビュー
            <ul class="ccm-block-topic-list-list">
                <li><a href="https://concretecms.text/index.php/218/topic/26/gadgets" class="ccm-block-topic-list-topic-selected">ガジェット</a></li>
                <li><a href="https://concretecms.text/index.php/218/topic/27/movies">映画</a></li>
                <li><a href="https://concretecms.text/index.php/218/topic/28/books">本</a></li>
                <li><a href="https://concretecms.text/index.php/218/topic/29/music">ミュージック</a></li>
            </ul>
        </li>
        <li><a href="https://concretecms.text/index.php/218/topic/30/projects">プロジェクト</a></li>
        <li><a href="https://concretecms.text/index.php/218/topic/31/sports">スポーツ</a></li>
        <li><a href="https://concretecms.text/index.php/218/topic/32/humor">ユーモア</a></li>
    </ul>
</div>
```

## RSS 表示

RSS フィードを読み込み、記事のタイトル、日付、サマリーを表示します。表示件数も指定できます（最大件数はフィードに含まれる記事数に依存します）。
RSS フィードの内容はデータベースに取り込まれるわけではなく、都度取得します（ただし、キャッシュはされます）。

### 出力

```html
<div class="ccm-block-rss-displayer-wrapper">
    <div class="ccm-block-rss-displayer">
        <div class="ccm-block-rss-displayer-header">
        	<h5>タイトル（任意）</h5>
        </div>
		<div class="ccm-block-rss-displayer-item">
			<div class="ccm-block-rss-displayer-item-title">
				<a href="https://www.youtube.com/watch?v=mPhvYQZvw-Q" target="_blank">
					concrete5 Weekly #250 with PortlandLabs				</a>
			</div>
			<div class="ccm-block-rss-displayer-item-date">2018/03/14</div>
			<div class="ccm-block-rss-displayer-item-summary">
            </div>
		</div>
		<div class="ccm-block-rss-displayer-item">
			<div class="ccm-block-rss-displayer-item-title">
				<a href="https://www.youtube.com/watch?v=yzCi1Oz2lSs" target="_blank">
					8.2.1 新機能紹介 - 週刊 concrete5 Vol.249				</a>
			</div>
			<div class="ccm-block-rss-displayer-item-date">2017/08/17</div>
			<div class="ccm-block-rss-displayer-item-summary">
            </div>
		</div>
    </div>
</div>
```

## 言語切り替え

多言語サイトを有効にしている際、現在見ているページに対応する他の言語のページへのリンクを表示できます。

### 出力

```html
<div class="ccm-block-switch-language">
    <form method="post" class="form-inline">
        ラベル（任意）
        <select id="language" name="language" data-select="multilingual-switch-language" data-action="https://concretecms.text/index.php/218/switch_language/218/--language--/206" ccm-passed-value="1" class="form-control">
            <option value="1" selected="selected">日本語</option>
            <option value="219">English</option>
        </select>
    </form>
</div>
```

## フォーム

フォームを設置できるブロックです。回答結果はデータベースに保管され、管理画面から確認でき、CSV に出力できます。
様々な入力フィールドの種類がありますので、それぞれの出力結果は実際に設定してみてご確認ください。
[フォームのカスタマイズ方法](https://documentation.concretecms.org/developers/express/express-forms-controllers/form-theming/using-contexts-to-customize-form-and-attribute-markup)は少し複雑ですので、出力されるマークアップに合わせてスタイルシートをコーディングする方が無難です。

```html
<div class="ccm-block-express-form">
    <div class="ccm-form">
        <a name="form207"></a>
        <form enctype="multipart/form-data" class="form-stacked" method="post" action="https://concretecms.text/index.php/218/submit/207#form207">
            <input type="hidden" name="express_form_id" value="93f430a4-50f4-11e8-982a-a8667f39691c">
            <input type="hidden" name="ccm_token" value="1525597795:12f0f46cca74960031a5e4c70a371b58" />
            <div class="ccm-dashboard-express-form">
                <fieldset>
                    <div class="form-group">
                        <label class="control-label" for="akID[77][value]">質問（任意）</label>
                        <span class="text-muted small">必須</span>
                        <input type="text" id="akID[77][value]" name="akID[77][value]" value="" placeholder="プレースホルダ（任意）" class="form-control ccm-input-text" />
                    </div>
                </fieldset>
            </div>
            <div class="form-actions">
                <button type="submit" name="Submit" class="btn btn-primary">送信</button>
            </div>
        </form>
    </div>
</div>
```

## 検索

キーワードでサイト内検索できるブロックです。検索対象にするページの範囲を、サイトマップで選択できます。

### 出力

```html
<form action="https://concretecms.text/index.php/218" method="get" class="ccm-search-block-form">
    <h3>タイトル（任意）</h3>
    <input name="search_paths[]" type="hidden" value="" />
    <input name="query" type="text" value="" class="ccm-search-block-text" />
    <input name="submit" type="submit" value="ボタンテキスト（任意）" class="btn btn-default ccm-search-block-submit" />
</form>
```

## アンケート

複数の選択肢の中から、任意の一つを選んで回答させることができます。
回答はログイン不要にするか、ログインユーザーのみにするかが選べます。
ログイン不要の場合は、回答済みかどうかが Cookie に保存されます。
回答結果はデータベースに保管され、管理画面から確認できます。

### 出力

回答前：

```html
<div class="poll">
    <div id="surveyQuestion" class="form-group">
        質問（任意）
    </div>
    <form method="post" action="https://concretecms.text/index.php/218/form_save_vote/209">
        <input type="hidden" name="rcID" value="218"/>
        <div class="radio">
            <label>
                <input type="radio" name="optionID" value="1"/>
                回答１（任意）
            </label>
        </div>
        <div class="radio">
            <label>
                <input type="radio" name="optionID" value="2"/>
                回答２（任意） 
            </label>
        </div>
        <div class="form-group">
            <button class="btn btn-primary">回答</button>
        </div>
    </form>
</div>
```

回答後：

```html
<div class="poll">
    <h3>すでにこのアンケートに答えています。</h3>

    <div class="row">
        <div class="col-sm-9">
            <div id="surveyQuestion">
                <strong>質問:</strong> <span>質問（任意）</span>
            </div>
            <div id="surveyResults">
                <table class="table">
                    <tr>
                        <td class="col-sm-2" style="white-space: nowrap">
                            <div class="surveySwatch" style="background:#FFCC33"></div>
                            &nbsp;50%
                        </td>
                        <td>
                            <strong>回答１（任意）</strong>
                        </td>
                    </tr>
                    <tr>
                        <td class="col-sm-2" style="white-space: nowrap">
                            <div class="surveySwatch" style="background:#FFFF33"></div>
                            &nbsp;50%
                        </td>
                        <td>
                            <strong>回答２（任意）</strong>
                        </td>
                    </tr>
                </table>
                <div class="help-block">2 票</div>
            </div>
        </div>
        <div class="col-sm-3">
            <img border=""
                    src="//chart.apis.google.com/chart?chf=bg,s,FFFFFF00&cht=p&chd=t:50,50&chs=180x180&chco=FFCC33,FFFF33"
                    alt="フォーム一覧"/>
        </div>
    </div>
    <div class="spacer">&nbsp;</div>
    <div class="spacer">&nbsp;</div>
</div>
```

## コメント欄

このブロックが設置された場所に、コメント欄を設置できます。

## ソーシャルリンク

SNS アカウントへのリンクをアイコンで表示できるブロックです。
選択できる SNS は[設定ファイルでカスタマイズ](https://concrete5-japan.org/help/5-7/recipes/customize-social-links/)できます。

### 出力

```html
<div id="ccm-block-social-links210" class="ccm-block-social-links">
    <ul class="list-inline">
        <li>
            <a target="_blank" href="http://facebook.com/Concrete CMS" aria-label="Facebook">
                <i class="fa fa-facebook" aria-hidden="true" title="Facebook"></i>
            </a>
        </li>
        <li>
            <a target="_blank" href="http://twitter.com/Concrete CMS" aria-label="Twitter">
                <i class="fa fa-twitter" aria-hidden="true" title="Twitter"></i>
            </a>
        </li>
    </ul>
</div>
```

## 紹介

人物紹介（プロフィール）を表示するためのブロックです。

### 出力

```html
<div class="ccm-block-testimonial-wrapper">
    <div class="ccm-block-testimonial">
        <div class="ccm-block-testimonial-image">
            <img src="/application/files/2515/2282/3073/avatar_none.png" alt="名前（任意）" width="200" height="200">
        </div>
        <div class="ccm-block-testimonial-text">
            <div class="ccm-block-testimonial-name">名前（任意）</div>
            <div class="ccm-block-testimonial-position">ポジション（任意）, <a href="URL（任意）">会社（任意）</a></div>
            <div class="ccm-block-testimonial-paragraph">自己紹介（任意）</div>
        </div>
    </div>
</div>
```

## このページをシェア

SNS などへのシェアするリンクをアイコンで表示するブロックです。
選択できるサービスは下記の通りで、カスタマイズはできません。

* Facebook
* Twitter
* LinkedIn
* Reddit
* Pinterest
* Google Plus
* 印刷
* メール

### 出力

```html
<div class="ccm-block-share-this-page">
    <ul class="list-inline">
        <li>
            <a href="https://www.facebook.com/sharer/sharer.php?u=" target="_blank" aria-label="Facebook"><i class="fa fa-facebook" aria-hidden="true" title="Facebook"></i></a>
        </li>
        <li>
            <a href="https://twitter.com/intent/tweet?url=" target="_blank" aria-label="Twitter"><i class="fa fa-twitter" aria-hidden="true" title="Twitter"></i></a>
        </li>
    </ul>
</div>
```

## カレンダー

カレンダーを表示するブロックです。カレンダーには、管理画面から登録したイベントを表示できます。

表示には JavaScript ライブラリの [FullCalendar](https://fullcalendar.io/) を使用しています。
デザインのカスタマイズ方法はライブラリのドキュメントを参照してください。

## イベントリスト

イベントの一覧を表示するブロックです。任意のトピック、または現在のページで選択されているトピックで絞り込んで表示することができます。

### 出力

データベースから取得する件数と、一度に表示する件数を設定できます。
一度に表示する件数を超えたイベントは、次へ・前へ式のボタンをクリックすると JavaScript で送り表示します。
JavaScript を含めテンプレートに記載されているので、カスタムテンプレートで挙動はカスタマイズできます。

```html
<div class="ccm-block-calendar-event-list-wrapper widget-featured-events unbound" data-page="1">
    <h2>タイトル（任意）</h2>
    <div class="ccm-block-calendar-event-list" style="display:none">
        <div class="ccm-block-calendar-event-list-event">
            <div class="ccm-block-calendar-event-list-event-date">
                <span>5月</span>
                <span>07</span>
            </div>
            <div class="ccm-block-calendar-event-list-event-title">全社会議</div>
            <div class="ccm-block-calendar-event-list-event-date-full">2018/05/07 10:00 – 11:00</div>
        </div>
        <div class="ccm-block-calendar-event-list-event">
            <div class="ccm-block-calendar-event-list-event-date">
                <span>5月</span>
                <span>10</span>
            </div>
            <div class="ccm-block-calendar-event-list-event-title">テストイベント</div>
            <div class="ccm-block-calendar-event-list-event-date-full">2018/05/10 3:30 – 4:30</div>
        </div>
    </div>
    <div class="btn-group ccm-block-calendar-event-list-controls">
        <button type="button" class="btn btn-default" data-cycle="previous"><i class="fa fa-angle-left"></i></button>
        <button type="button" class="btn btn-default" data-cycle="next"><i class="fa fa-angle-right"></i></button>
    </div>
</div>
```

## カレンダーイベント

特定のイベントを表示します。

### 出力

```html
<div class="ccm-block-calendar-event-wrapper">
    <div class="ccm-block-calendar-event-header">
        <h3>全社会議</h3>
    </div>
    <div class="ccm-block-calendar-event-date-time">2018/05/07 10:00 – 11:00</div>
    <div class="ccm-block-calendar-event-attributes">会議</div>
</div>
```

## ページ属性

任意のページ属性を表示します。属性の前につけるタイトルは任意に設定できます。
表示に使用するタグは なし, `h1`, `h2`, `p`, `b`, `address`, `pre`, `blockquote`, `div` の中から選べます。

### 出力

```html
<h1 class="ccm-block-page-attribute-display-wrapper">
    <span class="ccm-block-page-attribute-display-title">タイトル（任意） </span>
    属性値
</h1>
```

## 画像スライダー

画像スライドを表示するブロックです。
スライドのエフェクトはディゾルブのみですので、変更したい場合はカスタムテンプレートの作成が必要です。
JavaScript ライブラリの [ResponsiveSlides.js](http://responsiveslides.com/) を利用して表示しています。

### 出力

```html
<div class="ccm-image-slider-container ccm-block-image-slider-pages" >
    <div class="ccm-image-slider">
        <div class="ccm-image-slider-inner">
            <ul class="rslides" id="ccm-image-slider-58">
                <li>
                    <img src="/application/files/8215/2282/3070/slider1.png" alt="Stand Out on the Web" width="1100" height="368">
                    <div class="ccm-image-slider-text">
                        <h2 class="ccm-image-slider-title">Stand Out on the Web</h2>                    
                        <p>Share your business with an impressive, yet minimal presentation. Let your customers understand your web presence through elegance and clarity.</p>
                    </div>
                </li>
                <li>
                    <img src="/application/files/3115/2282/3072/slider2.png" alt="A Simple Image Slider" width="1100" height="368">
                    <div class="ccm-image-slider-text">
                        <h2 class="ccm-image-slider-title">A Simple Image Slider</h2>
                        <p>This image slider can have any content that you want in it.</p>
                    </div>
                </li>
            </ul>
        </div>
    </div>
</div>
```

## ビデオプレイヤー

ファイルマネージャーにアップロードされた動画を `<video>` タグで表示します。

### 出力

```html
<div style="text-align:center; margin-top: 20px; margin-bottom: 20px;">
    <video controls="controls" style="max-width: 100%;">
        <source src="file_example.webm" type="video/webm">
        <source src="file_example.ogg" type="video/ogg">
        <source src="file_example.mp4" type="video/mp4">
        このブラウザは HTML ビデオタグをサポートしていません。
    </video>
</div>
```

## ドキュメントライブラリ

ファイルマネージャーの任意のフォルダの内容を表示・検索できるブロックです。
管理画面のファイルマネージャーにアクセスさせずに、ユーザーにファイルをアップロードさせることもできます。

### 出力

```html
<h2>テーブル名（任意）</h2>
<p>テーブル説明（任意）</p>
<form method="get" action="https://concretecms.text/index.php/218">
    <div class="form-inline">
        <div class="form-group">
            <label for="keywords" class="control-label">キーワード検索</label>
            <input type="text" id="keywords" name="keywords" value="" class="form-control ccm-input-text" />
        </div>
        <button type="submit" class="btn btn-primary" name="search">検索</button>
        <a href="#" data-document-library-advanced-search="219" class="ccm-block-document-library-advanced-search">詳細検索</a>
        <a href="#" data-document-library-add-files="219" class="ccm-block-document-library-add-files">ファイル追加</a>
    </div>
    <div data-document-library-advanced-search-fields="219" class="ccm-block-document-library-advanced-search-fields">
        <input type="hidden" name="advancedSearchDisplayed" value="">
        <h4>作成日時</h4>
        <div>
            <div class="form-inline">
                <input type="checkbox" id="date_from_activate" class="ccm-activate-date-time" ccm-date-time-id="date_from" name="date_from_activate" />
                <span class="ccm-input-date-wrapper" id="date_from_dw">
                    <input type="text" id="date_from_dt_pub" class="form-control ccm-input-date" disabled="disabled" />
                    <input type="hidden" id="date_from_dt" name="date_from_dt" disabled="disabled" />
                </span>
                <span class="ccm-input-time-wrapper form-inline" id="date_from_tw">
                    <select class="form-control" id="date_from_h" name="date_from_h" disabled="disabled">
                        <option value="0">0</option>
                        <option value="1">1</option>
                        <option value="2">2</option>
                        <option value="3">3</option>
                        <option value="4" selected="selected">4</option>
                        <option value="5">5</option>
                        <option value="6">6</option>
                        <option value="7">7</option>
                        <option value="8">8</option>
                        <option value="9">9</option>
                        <option value="10">10</option>
                        <option value="11">11</option>
                        <option value="12">12</option>
                        <option value="13">13</option>
                        <option value="14">14</option>
                        <option value="15">15</option>
                        <option value="16">16</option>
                        <option value="17">17</option>
                        <option value="18">18</option>
                        <option value="19">19</option>
                        <option value="20">20</option>
                        <option value="21">21</option>
                        <option value="22">22</option>
                        <option value="23">23</option>
                    </select>
                    <span class="separator">:</span>
                    <select class="form-control" id="date_from_m" name="date_from_m" disabled="disabled">
                        <option value="00">00</option>
                        <option value="01">01</option>
                        <option value="02">02</option>
                        <option value="03">03</option>
                        <option value="04">04</option>
                        <option value="05">05</option>
                        <option value="06">06</option>
                        <option value="07">07</option>
                        <option value="08">08</option>
                        <option value="09">09</option>
                        <option value="10">10</option>
                        <option value="11">11</option>
                        <option value="12">12</option>
                        <option value="13">13</option>
                        <option value="14">14</option>
                        <option value="15">15</option>
                        <option value="16">16</option>
                        <option value="17" selected="selected">17</option>
                        <option value="18">18</option>
                        <option value="19">19</option>
                        <option value="20">20</option>
                        <option value="21">21</option>
                        <option value="22">22</option>
                        <option value="23">23</option>
                        <option value="24">24</option>
                        <option value="25">25</option>
                        <option value="26">26</option>
                        <option value="27">27</option>
                        <option value="28">28</option>
                        <option value="29">29</option>
                        <option value="30">30</option>
                        <option value="31">31</option>
                        <option value="32">32</option>
                        <option value="33">33</option>
                        <option value="34">34</option>
                        <option value="35">35</option>
                        <option value="36">36</option>
                        <option value="37">37</option>
                        <option value="38">38</option>
                        <option value="39">39</option>
                        <option value="40">40</option>
                        <option value="41">41</option>
                        <option value="42">42</option>
                        <option value="43">43</option>
                        <option value="44">44</option>
                        <option value="45">45</option>
                        <option value="46">46</option>
                        <option value="47">47</option>
                        <option value="48">48</option>
                        <option value="49">49</option>
                        <option value="50">50</option>
                        <option value="51">51</option>
                        <option value="52">52</option>
                        <option value="53">53</option>
                        <option value="54">54</option>
                        <option value="55">55</option>
                        <option value="56">56</option>
                        <option value="57">57</option>
                        <option value="58">58</option>
                        <option value="59">59</option>
                    </select>
                </span>
            </div>
            から
            <div class="form-inline">
                <input type="checkbox" id="date_from_activate" class="ccm-activate-date-time" ccm-date-time-id="date_from" name="date_from_activate" />
                <span class="ccm-input-date-wrapper" id="date_from_dw">
                    <input type="text" id="date_from_dt_pub" class="form-control ccm-input-date" disabled="disabled" />
                    <input type="hidden" id="date_from_dt" name="date_from_dt" disabled="disabled" />
                </span>
                <span class="ccm-input-time-wrapper form-inline" id="date_from_tw">
                    <select class="form-control" id="date_from_h" name="date_from_h" disabled="disabled">
                        <option value="0">0</option>
                        <option value="1">1</option>
                        <option value="2">2</option>
                        <option value="3">3</option>
                        <option value="4" selected="selected">4</option>
                        <option value="5">5</option>
                        <option value="6">6</option>
                        <option value="7">7</option>
                        <option value="8">8</option>
                        <option value="9">9</option>
                        <option value="10">10</option>
                        <option value="11">11</option>
                        <option value="12">12</option>
                        <option value="13">13</option>
                        <option value="14">14</option>
                        <option value="15">15</option>
                        <option value="16">16</option>
                        <option value="17">17</option>
                        <option value="18">18</option>
                        <option value="19">19</option>
                        <option value="20">20</option>
                        <option value="21">21</option>
                        <option value="22">22</option>
                        <option value="23">23</option>
                    </select>
                    <span class="separator">:</span>
                    <select class="form-control" id="date_from_m" name="date_from_m" disabled="disabled">
                        <option value="00">00</option>
                        <option value="01">01</option>
                        <option value="02">02</option>
                        <option value="03">03</option>
                        <option value="04">04</option>
                        <option value="05">05</option>
                        <option value="06">06</option>
                        <option value="07">07</option>
                        <option value="08">08</option>
                        <option value="09">09</option>
                        <option value="10">10</option>
                        <option value="11">11</option>
                        <option value="12">12</option>
                        <option value="13">13</option>
                        <option value="14">14</option>
                        <option value="15">15</option>
                        <option value="16">16</option>
                        <option value="17" selected="selected">17</option>
                        <option value="18">18</option>
                        <option value="19">19</option>
                        <option value="20">20</option>
                        <option value="21">21</option>
                        <option value="22">22</option>
                        <option value="23">23</option>
                        <option value="24">24</option>
                        <option value="25">25</option>
                        <option value="26">26</option>
                        <option value="27">27</option>
                        <option value="28">28</option>
                        <option value="29">29</option>
                        <option value="30">30</option>
                        <option value="31">31</option>
                        <option value="32">32</option>
                        <option value="33">33</option>
                        <option value="34">34</option>
                        <option value="35">35</option>
                        <option value="36">36</option>
                        <option value="37">37</option>
                        <option value="38">38</option>
                        <option value="39">39</option>
                        <option value="40">40</option>
                        <option value="41">41</option>
                        <option value="42">42</option>
                        <option value="43">43</option>
                        <option value="44">44</option>
                        <option value="45">45</option>
                        <option value="46">46</option>
                        <option value="47">47</option>
                        <option value="48">48</option>
                        <option value="49">49</option>
                        <option value="50">50</option>
                        <option value="51">51</option>
                        <option value="52">52</option>
                        <option value="53">53</option>
                        <option value="54">54</option>
                        <option value="55">55</option>
                        <option value="56">56</option>
                        <option value="57">57</option>
                        <option value="58">58</option>
                        <option value="59">59</option>
                    </select>
                </span>
            </div>
        </div>
        <h4>タイプ</h4>
        <div>
            <select id="type" name="type" style="width: 120px" class="form-control">
                <option value="" selected="selected">** ファイル種別</option>
                <option value="5">ドキュメント</option>
                <option value="1">画像</option>
                <option value="2">ビデオ</option>
                <option value="4">オーディオ</option>
                <option value="3">テキスト</option>
                <option value="6">アプリケーション</option>
                <option value="99">ファイル</option>
            </select>
        </div>
        <h4>拡張子</h4>
        <div>
            <select id="extension" name="extension" style="width: 120px" class="form-control">
                <option value="" selected="selected">** ファイル拡張子</option>
                <option value="jpeg">jpeg</option>
                <option value="jpg">jpg</option>
                <option value="png">png</option>
            </select>
        </div>
    </div>
    <br/>
</form>
<div data-document-library-upload-action="https://concretecms.text/index.php/218/upload/219" data-document-library-add-files="219" class="ccm-block-document-library-add-files-uploader">
    <div class="ccm-block-document-library-add-files-pending">ファイルをアップロード</div>
    <div class="ccm-block-document-library-add-files-uploading">アップロード中 <i class="fa fa-spin fa-spinner"></i></div>
    <input type="file" name="file" />
    <input type="hidden" name="ccm_token" value="1525635411:be1cc29b17799efc395be33be6ebfc6b" />
</div>
<div id="ccm-block-document-library-wrapper-219">
    <table
        id="ccm-block-document-library-table-219"
        class="table ccm-block-document-library-table ">
        <thead>
            <tr>
                <th class="ccm-block-document-library-column-thumbnail">
                    <span>サムネイル</span>
                </th>
                <th class="ccm-block-document-library-column-title ccm-block-document-library-column-sortable">
                    <a href="https://concretecms.text/index.php/218?sort=title&dir=asc">タイトル</a>
                </th>
                <th class="ccm-block-document-library-column-filename ccm-block-document-library-column-sortable">
                    <a href="https://concretecms.text/index.php/218?sort=filename&dir=asc">ファイル名</a>
                </th>
                <th class="ccm-block-document-library-column-date ccm-block-document-library-column-sortable ccm-block-document-library-active-sort-desc">
                    <a href="https://concretecms.text/index.php/218?sort=date&dir=asc">作成日時</a>
                </th>
                <th class="ccm-block-document-library-column-size ccm-block-document-library-column-sortable">
                    <a href="https://concretecms.text/index.php/218?sort=size&dir=asc">サイズ</a>
                </th>
            </tr>
        </thead>
        <tbody>
            <tr class="ccm-block-document-library-row-a">
                <td><img src="/concrete/images/icons/filetypes/ogg.svg" width="60" height="60" class="img-responsive ccm-generic-thumbnail" alt="OGG file icon"></td>
                <td><a href="https://concretecms.text/index.php/download_file/21/218">file_example.ogg</a></td>
                <td>file_example.ogg</td>
                <td>2018/05/06</td>
                <td>1,694.26 KB</td>
            </tr>
            <tr class="ccm-block-document-library-row-b">
                <td><img src="/concrete/images/icons/filetypes/mp4.svg" width="60" height="60" class="img-responsive ccm-generic-thumbnail" alt="MP4 file icon"></td>
                <td><a href="https://concretecms.text/index.php/download_file/20/218">file_example.mp4</a></td>
                <td>file_example.mp4</td>
                <td>2018/05/06</td>
                <td>1,533.23 KB</td>
            </tr>
        </tbody>
    </table>
</div>
<div class="ccm-pagination-wrapper">
    <ul class="pagination">
        <li class="prev disabled"><span>&larr; 前へ</span></li>
        <li class="active"><span>1 <span class="sr-only">（このページ）</span></span></li>
        <li><a href="/index.php/218?survey_voted=1&amp;ccm_paging_p=2&amp;ccm_order_by=fv.fvDateAdded&amp;ccm_order_by_direction=desc">2</a></li>
        <li><a href="/index.php/218?survey_voted=1&amp;ccm_paging_p=3&amp;ccm_order_by=fv.fvDateAdded&amp;ccm_order_by_direction=desc">3</a></li>
        <li><a href="/index.php/218?survey_voted=1&amp;ccm_paging_p=4&amp;ccm_order_by=fv.fvDateAdded&amp;ccm_order_by_direction=desc">4</a></li>
        <li><a href="/index.php/218?survey_voted=1&amp;ccm_paging_p=5&amp;ccm_order_by=fv.fvDateAdded&amp;ccm_order_by_direction=desc">5</a></li>
        <li><a href="/index.php/218?survey_voted=1&amp;ccm_paging_p=6&amp;ccm_order_by=fv.fvDateAdded&amp;ccm_order_by_direction=desc">6</a></li>
        <li><a href="/index.php/218?survey_voted=1&amp;ccm_paging_p=7&amp;ccm_order_by=fv.fvDateAdded&amp;ccm_order_by_direction=desc">7</a></li>
        <li class="disabled"><span>&hellip;</span></li>
        <li><a href="/index.php/218?survey_voted=1&amp;ccm_paging_p=10&amp;ccm_order_by=fv.fvDateAdded&amp;ccm_order_by_direction=desc">10</a></li>
        <li class="next"><a href="/index.php/218?survey_voted=1&amp;ccm_paging_p=2&amp;ccm_order_by=fv.fvDateAdded&amp;ccm_order_by_direction=desc" rel="next">次へ &rarr;</a></li>
    </ul>
</div>
```

## YouTubeビデオ

YouTube の動画プレイヤーを埋め込み表示できるブロックです。

### 出力

```html
<div id="youtube220" class="youtubeBlock youtubeBlockResponsive16by9">
    <iframe class="youtube-player" src="//www.youtube.com/embed/eLtPd4OeZYk?color=red&controls=1&hl=ja&iv_load_policy=1&modestbranding=1&rel=0&showinfo=1&start=1" frameborder="0" allowfullscreen></iframe>
</div>
```

## Googleマップ

Google マップを埋め込み表示できるブロックです。

### 出力

```html
<h3>タイトル（任意）</h3>
<div class="googleMapCanvas"
     style="width: 100%; height: 400px"
     data-zoom="14"
     data-latitude="35.6915755"
     data-longitude="139.7636529"
     data-scrollwheel="true"
     data-draggable="true"
>
</div>
```

## エクスプレス

エクスプレスデータベース機能は複雑なため、場所を改めて解説します。

### エクスプレス一覧

エクスプレスデータベースから任意のエンティティのエントリーの一覧を表示するブロックです。

### エクスプレス詳細

エクスプレスデータベースから任意のエントリーを表示するブロックです。
