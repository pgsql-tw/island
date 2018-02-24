---
layout: post
title: "PostgreSQL的Table可以有多大？"
date: 2018-02-24
---

根據官方網站的資訊：Table 最大是 32TB。

不過這似乎只是宣告的問題而已。<br/>
因為 PostgreSQL 所編譯的 block size 是 8,192 Bytes，<br/>
但實際上是可以[修改到 32,768 Bytes](https://www.postgresql.org/docs/10/static/install-procedure.html)。<br/>
如此一來，Table 的極限就不是 32TB，而是 128TB，<br/>
只不過，你也需要重建一次你的資料庫就是了。（重編譯+回存資料）<br/>
一般人大概不會這麼做，所以要說最大是 32TB，也是沒錯的。

那如果更進一步加入 [Partition Table](https://docs.postgresql.tw/tw.10/ii-the-sql-language/data-definition/510-table-partitioning.html) 的支援呢？<br/>
一個資料表可以集合許多資料表而成，雖然使用上有一些限制，但這裡只考慮大小。<br/>
在 PostgreSQL 10 支援到 65,535 個子資料表，<br/>
而 PostgreSQL 11 則支援到 2^32 個子資料表。

總結來看，
1. 如果是 PostgreSQL 9.6 或更早的版本的話：32TB (Terabytes)。
2. 如果是 PostgreSQL 10 的話：2EB (2 Exabytes) = 2,048PB (Petabytes) = 2,097,152 TB
3. 如果是 PostgreSQL 11 的話：131YB(131 [Yottabytes](https://en.wikipedia.org/wiki/Yottabyte)) ...... 自己算吧XD

雖然還不知道存下來是不是真的有後續處理的能力，<br/>
但至少不會在資料儲存的限制上飽受批評了。

---
參考資料：
1. [PostgreSQL: About](https://www.postgresql.org/about/)
2. [PostgreSQL Maximum Table Size](https://blog.2ndquadrant.com/postgresql-maximum-table-size/)
3. [PostgreSQL evolves to fill tomorrow's data oceans - Tech Wire Asia](http://techwireasia.com/2018/02/postgresql-evolves-to-fill-tomorrows-data-oceans/)
