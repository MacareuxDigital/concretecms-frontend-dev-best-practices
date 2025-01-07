---
layout: page
title: レイアウト
---

## レイアウト機能の概要

Concrete CMS には編集モードでコンテンツ編集者がレイアウトを作成できる「レイアウト機能」があります。
この機能のおかげで、エンジニアにテンプレートの新規開発を依頼することなく、コンテンツ編集者が自分でコンテンツに合わせてレイアウトを調整することができます。

Concrete CMS のテーマを開発する際、デザイン上のレイアウトのバリエーションのうちの一部はレイアウト機能に期待することで、開発するテンプレートの数を減らすことができます。テンプレートの数を減らすことは、開発コストを下げるだけでなく、コンテンツ編集者が選択すべきテンプレートの選択肢も少なくなることで、使いやすさにつながります。

しかし、レイアウト機能はどのようなデザインも実現できる万能の機能ではありません。
レイアウト機能がどのようなものを知っておきましょう。

### レイアウトメニュー

Concrete CMS では、編集モードでブロックをドロップできる領域を「エリア」と呼びます。
このエリアを複数の「子エリア」に分割できるのがレイアウト機能です。

レイアウトを追加するには、エリアのタブをクリックしてメニューを開き、「レイアウトを追加」をクリックします。
クリックすると、下図のインターフェースが表示されます。

![レイアウトを追加](https://raw.githubusercontent.com/MacareuxDigital/concretecms-frontend-dev-best-practices/master/images/layout-menu.png)

上図ではグリッド方式として「Twitter Bootstrap」と「自由形式のレイアウト」、プリセットとして「Left Sidebar」「Right Sidebar」が選べるようになっています。

#### Twitter Bootstrap

Twitter Bootstrap はレスポンシブデザインのフレームワークです。
Twitter Bootstrap を選択すると、このフレームワークのグリッドシステムに準拠した HTML が出力されます。

Twitter Bootstrap では、エリアを12等分したグリッドに沿って分割することができます。
分割数は「カラム」の数字で指定します。
緑の □ アイコンをドラッグすることで分割の比率を変更することもできます。

![レイアウトの比率を変更](https://raw.githubusercontent.com/MacareuxDigital/concretecms-frontend-dev-best-practices/master/images/layout-modified.png)

上図のレイアウトの HTML 出力結果は下記のようになります。

```html
<div class="row">
    <div class="col-sm-5"></div>
    <div class="col-sm-3 col-sm-offset-1"></div>
    <div class="col-sm-3"></div>
</div>
```

Twitter Bootstrap では、デバイスサイズごとのグリッドのクラス（`.col-xs-*`, `.col-md-*`, `.col-lg-*`）も用意されていますが、Concrete CMS のグリッド機能では対応していません。

Twitter Bootstrap には、先頭から順に左から右へと配置されるグリッドの順序を変更することができますが、Concrete CMS のグリッド機能では対応していません。

Concrete CMS のレイアウトで Twitter Bootstrap が選択できるのは、テーマが Bootstrap に対応している場合のみです。
Bootstrap を使って CSS を作成していない場合は、Foundation など他の有名なフレームワークや、自作のグリッドフレームワークにも対応させることができます。
詳しくは[ドキュメント](https://concrete5-japan.org/help/5-7/developer/designing-for-concrete5/adding-grid-support-to-your-theme/)をご参照ください。

#### 自由形式のレイアウト

自由形式のレイアウトは、最大12までの列にエリアを分割できる機能ですが、レスポンシブデザインには対応していません。
下記のような HTML と CSS が出力されます。

```html
<div class="ccm-layout-column-wrapper" id="ccm-layout-column-wrapper-25">
    <div class="ccm-layout-column" id="ccm-layout-column-60">
        <div class="ccm-layout-column-inner"></div>
    </div>
    <div class="ccm-layout-column" id="ccm-layout-column-61">
        <div class="ccm-layout-column-inner"></div>
    </div>
    <div class="ccm-layout-column" id="ccm-layout-column-62">
        <div class="ccm-layout-column-inner"></div>
    </div>
</div>
```

```css
div.ccm-layout-column {
    float: left;
}

/* clearfix */

div.ccm-layout-column-wrapper {*zoom:1;}
div.ccm-layout-column-wrapper:before, div.ccm-layout-column-wrapper:after {display:table;content:"";line-height:0;}
div.ccm-layout-column-wrapper:after {clear:both;}

#ccm-layout-column-wrapper-25 div.ccm-layout-column {
    width: 33.333333333333%;
}
#ccm-layout-column-wrapper-25 div.ccm-layout-column-inner {
    margin-right: 5px;
    margin-left: 5px;
}

#ccm-layout-column-wrapper-25 div.ccm-layout-column:first-child div.ccm-layout-column-inner {
    margin-left: 0px;
}

#ccm-layout-column-wrapper-25 div.ccm-layout-column:last-child div.ccm-layout-column-inner  {
    margin-right: 0px;
}
```

#### プリセット

分割の比率などを毎回設定する「グリッド」機能とは違い、よく使うレイアウトを「プリセット」として登録しておき、選択するだけで適用させることができます。
グリッドを使って作成したレイアウトを、今後同じ設定で再利用したい場合に、下図のレイアウト編集メニューから「レイアウトをプリセットとして保存」することができます。

![レイアウト編集メニュー](https://raw.githubusercontent.com/MacareuxDigital/concretecms-frontend-dev-best-practices/master/images/layout-edit-menu.png)

また、テーマから独自のレイアウトをプリセットとして登録することも可能です。
通常の Bootstrap レイアウトでは使えない `.col-md-*` クラスを使用したり、12等分に合わない比率で分割するグリッドなどを登録できます。
詳しくは[ドキュメント](https://concrete5-japan.org/help/5-7/developer/designing-for-concrete5/adding-complex-custom-layout-presets-in-your-theme/)をご参照ください。

## その他のレイアウト機能の制限

レイアウトは、既存のエリアを複数の「子エリア」に分割する機能です。
そのため、すでにあるエリアの外側に作ることはできません。

「ブロック」は「エリア」の中に並べるもので、レイアウト機能で作成する「子エリア」についても同様です。
ブロックの中にレイアウトを入れることはできません。