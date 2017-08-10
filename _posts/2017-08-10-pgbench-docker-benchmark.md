---
layout: post
title: "簡易實測 PostgreSQL with Docker"
date: 2017-08-10
---

使用 Docker 布署升級很方便，但直覺上應該會有一些效能上的損失，所以做了簡易的測試，可以在選擇布署方式時有些參考。

測試方式是在同一台主機（VM）, 一是直接安裝 PostgreSQL，另一為以官方 Docker Hub 方式安裝的 PostgreSQL。系統參數採預設值，不更動。

以 pgbench 的方式測試，scale = 100。

因為測試難免有些波動，所以本測試每次執行 5 分鐘，共執行 5 次供參考。

### 測試版本
1. Ubuntu 17.04 + PostgreSQL 9.6.3
2. Ubuntu 17.04 + Docker 17.06 + PostgreSQL 9.6.3

### 參考指令：
* 建立測試資料庫
```
$ createdb pgbench
```

* 初始化
```
$ pgbench -s 100
```

* 測試
```
$ pgbench -r -v -c -100 -j 100 -T 300 pgbench
```

### 結果：(including connections establishing)

| | 1 | 2 | 3 | 4 | 5 |
|-|-|-|-|-|-|
| 1 | 951.99 | 979.08 | 992.40 |
| 2 | 888.62 | 944.81 | 901.56 |
