css-fixme
=========
`-webkit-` prefix のついたプロパティを `-webkit-` prefix がついていない状態へと**よしなに**変換します。
なお、このツールは[hallvors/css-fixme](https://github.com/hallvors/css-fixme)をコマンドライン上で叩けるように魔改造したものです。

使用する際には、[注意事項](#注意事項)をよく読んでください。
手動で変換する必要のあるものがいくつか残っています。

余裕があれば、どなたか自動でよしなにするように修正していただけるとありがたいです。


インストール
------------

```shell
npm i -g git://github.com/Kuniwak/css-fixme.git
```


使い方
------

```shell
css-fixme path/to/legacy.css > path/to/converted.css
```

Whoa, Sir!が出る場合は、その箇所の変換に自信がないことを示しています。
なので、該当する箇所を変換後のCSSから探し、修正してください。


注意事項
--------

### `missing '{'` が出る場合

css-fixmeはCSSしか解析ができません。
Less等の構文`//`によるコメントや、mixinなどが返還前のLessに含まれているとこのようなメッセージが出ます。
css-fixmeを使う場合は、Lessの構文が含まれないように気をつけてください。


### Multiple background について

複数指定の`background`がきたとき、うまく処理できません。
手動で書き換えてください。

```css
.hoge {
background:-webkit-gradient(linear, left top, left bottom, from(#990), to(#066)), url(hoge.png);
}
```


### `-webkit-gradient` の `180deg` について

`180deg`指定は、S Browserを搭載するAndroid端末のみで問題になる変換結果になってしまいます（[参考情報](https://developer.mozilla.org/ja/docs/Web/CSS/linear-gradient)）。

```css
.hoge {
background:-webkit-gradient(linear, 0 0, 0 100%, from(#fff), to(#edeae0));
}
```

は下のように変換されるので、

```css
.hoge {
background: -webkit-gradient(linear, 0 0, 0 100%, from(#fff), to(#edeae0));
background: linear-gradient(180deg, #fff 0%, #edeae0 100%);
}
```

`180deg`を`to bottom` に書き換えてください。

```css
/* 手動で変更 */
.hoge {
background: linear-gradient(to bottom, #fff 0%, #edeae0 100%);
}
```


### `-webkit-device-pixel-ratio` について

`-webkit-device-pixel-ratio` は変換されません。
手動で`min-resolution`へと変換してください（[参考情報](http://www.w3.org/TR/css3-mediaqueries/#resolution)）。


```css
@media only (-webkit-device-pixel-ratio: 1.5){}
```

```css
/* 手動で変更 */
@media only (min-resolution: 1.5){}
```


謝辞
----

- [Hallvord R. M. Steen](https://github.com/hallvors)
