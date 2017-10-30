---
layout: post
title: "使用 PostgreSQL 字型吧！"
date: 2017-10-30
---

你知道 PostgreSQL 的字型是什麼嗎？

根據[官方 wiki](https://wiki.postgresql.org/wiki/Identity_Guidelines#Font)，PostgreSQL 官方指定的字型是「Strait」。<br/>
一般只有使用在各類圖示上，因為大部份的電腦裡並未內建這個字型。

不過本頁的英文字型就是使用 Strait 唷！<br/>
因為剛好 Google Fonts 就提供 Strait 的網頁字型。
- [Strait - Google Fonts](https://fonts.google.com/specimen/Strait)


只要在 CSS 檔中 import 就可以使用這個字型了。
```
@import url('https://fonts.googleapis.com/css?family=Strait');
```

字型名稱就是 Strait。
```
font-family: 'Strait', sans-serif;
```

順帶一提的是，[本頁中文字型](https://github.com/pgsql-tw/island/blob/master/assets/css/style.scss)使用了思源圓體和黑體。
