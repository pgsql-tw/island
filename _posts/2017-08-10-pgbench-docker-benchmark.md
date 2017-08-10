---
layout: post
title: "簡易實測 PostgreSQL with Docker"
date: 2017-08-10
---

使用 Docker 布署升級很方便，但直覺上應該會有一些效能上的損失，<br/>
所以做了簡易的測試，可以在選擇布署方式時有些參考。<br/>
測試方式是在同一台主機（VM）, 一是直接安裝 PostgreSQL，另一為以官方 Docker Hub 方式安裝的 PostgreSQL。<br/>
系統參數採預設值，不更動。<br/>
以 pgbench（也安裝於同一主機）的方式測試，scaling factor = 100。<br/>
因為測試難免有些波動，所以本測試每次執行 5 分鐘，共執行 5 次供參考。<br/>

以此測試結果來說，效能約減損 8%。<br/>
但永遠記得，效能不是一切，使用時請多方衡量。<br/>

### 測試環境
* A. Ubuntu 17.04 + PostgreSQL 9.6.3 (比值為 100)
* B. Ubuntu 17.04 + Docker 17.06 + PostgreSQL 9.6.3

### 結果：TPS (including connections establishing)

| | 1 | 2 | 3 | 4 | 5 | 平均 | 標準差 | 誤差 | 比值 |
|-|-|-|-|-|-|-|-|-|-|
| A | 951.99 | 979.08 | 992.40 | 1067.75 | 1014.78 | 1001.20 | 43.59 | 4.35% | 100 |
| B | 888.62 | 944.81 | 901.56 | 944.71 | 920.34 | 920.01 | 25.25 | 2.74% | 91.89 |

### 參考指令：
* 建立測試資料庫
```
$ createdb pgbench
```

* 初始化
```
$ pgbench -s 100 pgbench
```

* 測試
```
$ pgbench -r -v -c -100 -j 100 -T 300 pgbench
```
