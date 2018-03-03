---
layout: post
title: "[譯]資料庫中不使用外部鍵的 9 個理由"
date: 2018-03-03
---

# [譯]9 Reasons Why There Are No Foreign Keys in Your Database (Referential Integrity Checks)[^1]

最近我和幾位DBA和架構師爭論，他們對一些資料庫沒有外鍵感到震驚，並聲稱這是一個設計上的缺陷，不應該發生。如果是這樣，應該馬上予以修正。我想與此進行討論。我的經驗告訴我，很多資料庫（大多數我曾經使用的）不包含外部鍵，並不一定是壞事。在這篇文章中，我想把重點放在為什麼它是這樣的原因。

## 為什麼這會是一個問題？ {#toc_0}

### 1. 潛在的資料完整性問題，當然

缺少外鍵的明顯問題是資料庫不能強制處理引用的完整性，如果在上層應用沒有正確處理，則可能會導致資料不一致（子資料列沒有相對應的父資料列）。

### 2. 資料表關連性不清楚

資料庫中缺少外部鍵另一個不太明顯的負面影響是，不了解該資料模式的人很難找到正確的資料表並找出資料表的關連。這可能會導致嚴重的資料庫查詢和報表問題。[^2]

## 為什麼資料庫不需要有外部鍵？ {#toc_1}

讓我們來看看資料庫不用外部鍵的原因。但首先，一個簡短的免責聲明（因為文章引發了一些關於 LinkedIn 群組的爭議）：

> 因為是免責聲明，所以保留原文，僅就大意翻譯。

Reasons presented below are **in no way encouragement not to use foreign key constraints **in the databases. It is merely a collection of reasons I was able to find in various sources \(internet forums mostly\) on why many developers, architects or vendors do not use them. I personally \(and many other experienced database professionals\) advise to use them wherever you can \(where they are not causing more problems than they solve\). I leave you decision which of those reasons actually make a good case. More about problems lack of FKs cause in another article.

下面的理由絕對不是鼓勵不要在資料庫中使用外部鍵限制條件。這僅僅是我在各種管道（主要是網際網路論壇）能找到的許多開發人員、系統架構師或服務供應商為什麼不使用它們的原因。我個人（和許多其他經驗豐富的資料庫專家）建議在任何可能的地方使用它們（不會導致更多衍生的問題）。我讓你做決定，其中那些原因實際上是一個很好的例子。 在另一篇文章中更多關於缺乏外部鍵的問題。

### 1. 效能

在資料表上擁有有意義的外部鍵可以提高資料品質，但會影響插入資料、更新資料和刪除資料操作的效能。在這些任務之前，資料庫需要檢查它是否違反資料完整性。這也就是為什麼一些系統架構師和 DBA 完全放棄外部鍵的原因。資料倉儲和分析資料庫尤其如此，這些資料倉儲和分析資料庫不以交易方式（一次一個資料列）處理資料，而是批量處理資料。效能是資料倉儲和智慧商業的一切。

### 2. 老舊資料

許多資料庫在設計時需要儲存來自舊資料庫和來源的殘留資料，這些資料可能對資料品質和完整性沒有那麼嚴格。為了能夠容納又舊又髒資料的架構師，可以選擇，a：清理和轉換殘留資料（昂貴的練習），或者，b：放棄在資料庫級別上強制維護參照完整性。 一些整合好的 ERP 和 CRM 應用服務也會使用這種方法。

### 3. 資料表重建

一些資料庫，如資料倉儲，分段或中介資料庫，需要經常從外部資料來源重新載入資料。這會導致重新載入時資料的不一致（在父資料表為空的情況下，子資料表可能已載入）。這可以通過在重新載入時禁用外部鍵來繞過。然而，這會引入了額外的邏輯和複雜性，以及另一個可能的錯誤點。如上所述，對效能也會有負面影響。通常，成本大於效益，但開發人員不需要擔心細節。

### 4. 有更高層級的開發管理

有些應用程式具有程式設計層的框架，在實體資料庫上建立了另一個邏輯層。開發人員不使用 INSERT 或 UPDATE 語句來修改資料，他們使用 API，或者採用在後台執行操作的框架。像是 ORM（Object-Relational Mapping）框架或 Ruby on Rails 框架就是這種情況。這些工具負責維護資料參照的完整性，並與 RDBMS 一起建立更高等級的資料庫引擎。這些框架可以自己建立資料庫資料表，而通常不建立外部鍵。使用這些工具的開發人員很少干預自動產生的資料庫結構，並且也不使用主鍵。

### 5. 跨資料庫關連

這並不是資料庫不使用外部鍵的正確理由，但也確實是某種因素。有些資料庫跨越更多實體資料庫甚至引擎，並且建立跨越資料庫的主鍵在技術上可能不太可行。 SQL Server就是一個很好的例子 - 它不能在同一台服務器上的兩個資料庫上建立主鍵。這種架構在大型系統中很常見。

### 6. 資料庫可移植考量

與前一個類似，一些應用程式設計上假設資料庫平台（DBMS）是不可知的，並能夠在 Oracle、SQL Server、DB2 或 Sybase 等各種資料庫上運作。這是我讀過的關於PeopleSoft（目前由Oracle擁有）的內容。設計人員不想被綁定在任何特定的平台，並將所有邏輯推送到應用程式層，盡可能簡單地維持資料庫層就好。

### 7. 改變的可能性

與 Oracle 保持緊密聯繫，我聽說過有關其應用程式的另一個故事，這次是它自己的孩子 - Oracle 電子商務套件 - 就是它被設計為盡可能客製化。Oracle 提供了堅實的基礎，使實施團隊具有彈性，盡可能彈性地確定設計的各個方面。至少這是他們自己說的。也許原因與前一點相同，或者是下一個：

### 8. 簡易的軟體工程管理

在建立資料庫時，如果要儲存資料，則需要建立一些資料表和資料列。這是最低的限度。但是，你不必建立保持資料一致性的結構，如主鍵、唯一鍵、外部鍵或限制條件。這需要一些努力，但沒有直接的好處。一些架構師和資料庫管理員只是忽略了這一部分。

### 9. 隱藏資料關連模型

也許這是一個遙不可及的過程，但也許有時候是因為人們不希望別人太容易就知道太多。一般來說，人們希望被需要和不可替代。完美且不言自明的設計可能會使它們過時。但這只是我的理論。

## 結論 {#toc_2}

我希望我能夠提出明智的理由，為什麼外部鍵的使用並不像許多人想像中的那麼廣泛。如果您知道其他原因或不同意我的論點，請發表評論。

## 延伸資訊 {#toc_3}

1. [Why Is Metadata Important for Databases](https://dataedo.com/blog/why-is-metadata-important-for-databases)
2. [6 Typical Metadata Fields Stored by Applications](https://dataedo.com/blog/typical-metadata-fields-stroed-by-applications)
3. [8 Good Occasions to Start Documenting Your Databases](https://dataedo.com/blog/good-occasions-to-start-documenting-your-databases)

---

[^1]:  [9 Reasons Why There Are No Foreign Keys in Your Database \(Referential Integrity Checks\)](https://dataedo.com/blog/why-there-are-no-foreign-keys-in-your-database-referential-integrity-checks)
[^2]:  [Common SQL Join Traps \(with Test Queries\)](https://dataedo.com/blog/2-common-sql-join-traps-with-test-queries)
