---
layout: post
title: "TABLESAMPLE"
date: 2018-07-24
ogimage: "/assets/posts/tablesample.png"
---

本文對 TABLESAMPLE 做一個簡單的實驗，<br/>
(https://www.postgresql.org/docs/10/static/sql-select.html)<br/>
提供給大家參考，也同時思考**「是不是每一個查詢都需要使用全部的資料？」**<br/>
這並非全新的思維，只是比較少看到實際上的運用。

抽樣查詢有幾個優勢：<br/>
1. 減少運算資源浪費：你只運算到你需要的程度。
2. 增加運算效率：你可以更快得到你需要的結果。
3. 降低系統成本：較少的運算負載，你可以使用輕量級的設備。
4. 增加系統使用率：對資料庫更多短查詢，更容易調配共享資源。

不囉嗦，就看圖說故事吧。（指令如後所示）

![](/assets/posts/tablesample.png)

請注意，不同的資料分佈會産生不同的曲線。<br/>
本例中，執行 50 次，時間誤差約 7%，<br/>
所以在中間段視為誤差範圍內，為持平的結果。

平均值（正確值：49,999,320.92）的最大誤差為 0.56%（50,278,074.81），發生於抽樣 0.001% 時，<br/>
執行時間只需要 0.05%（42.29 ms / 77,297.41 ms）。

例如：如果你需要每 10 秒得到一次答案，可以取 5% 抽樣（9.6 秒完成），誤差率約為 0.04%。<br/>
數字為金額的話，表示平均大概是 5000 萬元左右，價差在 3 萬元以下。

其實最重要的是**「你有多精確掌握你的資料需求？」**<br/>
而非無差別地去使用資料庫。<br/>
（實務上抽樣多少較適當，需要常態性資料實驗與明確的決策需求相輔相成。）

這個功能及概念的還有一些延伸運用，<br/>
就交給有興趣的朋友自行查閱囉。

---

建立資料表：
```
CREATE TABLE items AS
  SELECT
    (random()*100000000)::integer AS n,
    md5(random()::text) AS s
  FROM
    generate_series(1,100000000);
```

查詢平均值：（測試標的）
```
SELECT avg(n) FROM items TABLESAMPLE SYSTEM(10);
```

使用 pgbench 批次測試：（會得到平均執行時間及標準差）
```
echo "SELECT avg(n) FROM items TABLESAMPLE SYSTEM(10);" | pgbench -d postgres -t 50 -P 60 -f -
```
