---
layout: post
title: "PostgreSQL 大補帖"
date: 2017-08-30
---

PostgreSQL 是很棒的資料庫系統，<br/>
但更棒的是，PostgreSQL 還有其他的延伸功能，讓使用上更加完備。<br/>

PostgreSQL 官方也提供了 Docker 的使用方式，<br/>
[PostgreSQL Official Docker Repository](https://hub.docker.com/_/postgres/)　<br/>
不過只提供標準安裝，要自行新增套件就不太方便。

只是，也許是因為多數都是自由軟體的關係，<br/>
各家都有自身的安裝方式，時常會造成困擾。<br/>
所以我們開啓了「PostgreSQL 大補帖」的專案，<br/>
剛好搭上 Docker 開始成熟的時期，<br/>
幫大家先測試好 Dockerfile，發佈到 DockerHub，就可以無痛安裝使用。<br/>

雖然 Docker 這類的 Microservice，講究的是小而巧的「容器」。<br/>
不過我們是以試用為目標，所以會建立一個大容器，<br/>
就好像過去台灣曾經大量販售的「大補帖」一樣。<br/>
當然，現在一切合法XD。<br/>

試用愉快～

* GitHub： [https://github.com/pgsql-tw/docker](https://github.com/pgsql-tw/docker)
* DockerHub： [https://hub.docker.com/r/pgsqltw/postgres-big/](https://hub.docker.com/r/pgsqltw/postgres-big/)

已設定好 Autobuild，所以在 DockerHub 上可以 pull 的就是可用的版本了。<br/>

[![Docker Automated buil](https://img.shields.io/docker/automated/pgsqltw/postgres-big.svg)](https://hub.docker.com/r/pgsqltw/postgres-big/)
[![Docker Pulls](https://img.shields.io/docker/pulls/pgsqltw/postgres-big.svg)](https://hub.docker.com/r/pgsqltw/postgres-big/)
[![Docker Build Statu](https://img.shields.io/docker/build/pgsqltw/postgres-big.svg)](https://hub.docker.com/r/pgsqltw/postgres-big/builds/)
