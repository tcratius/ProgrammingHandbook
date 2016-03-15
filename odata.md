# OData
###採用架構
---
* C#
* ASP.NET 4.5.2
* Web API 2.2
* MVC 5
* OData v4.0
* Web.Odata
* Entity Framework 6
  * Entity Framework (又稱ADO.NET Entity Framework) 是微軟以 ADO.NET 為基礎所發展出來的物件關聯對應 (O/R Mapping) 解決方案，早期被稱為 ObjectSpace，現已經包含在 Visual Studio 2008 Service Pack 1 以及 .NET Framework 3.5 Service Pack 1 以上發表。
  * ADO.NET Entity Framework 以 Entity Data Model (EDM) 為主，將資料邏輯層切分為三塊，分別為 Conceptual Schema, Mapping Schema 與 Storage Schema 三層，其上還有 Entity Client，Object Context 以及 LINQ 可以使用。
  * LINQ (Language Integrated Query)，是一項微軟技術，新增一種自然查詢的 SQL 語法到.NET Framework的程式語言中。
* OData.Edm 6.14.0 (至少 6.5.0 版本以上)

###常用指令
---
| 指令 | 說明 | 範例 |
| -- | -- | -- |
| top | 挑出最前面的幾筆 | 	?$top=3 |
| skip |略過幾筆。可用於分頁顯示 | ?$skip=10 |
| orderby | 排序 | ?$orderby=SupplierID,ProductID |
| filter | 篩選 |  |
|  | gt : > , 大於 | $filter=ProductID gt 10 |
|  | lt : < , 小於 | $filter=ProductID lt 10 |
|  | ge : >=, 大於等於 | $filter=ProductID ge 10 |
|  | le : <=, 小於等於 | $filter=ProductID le 10 |
|  | eq : =, 等於 | $filter=ProductID eq 10 |
|  | ne : <>, 不等於 | $filter=ProductID ne 10 |

###開始一個 VS 專案
* 檔案 > 新增 > 專案 > 範本 > Visual C# > Web > ASP.NET Web 應用程式
* 可以選擇是否採用 MVC 架構 : 本次採用 Web API 範本
  * Web API (已加入 MVC 及 Web API 核心參考) > 變更驗證 > 無驗證
  * Empty (至少需勾選 Web API 核心參考)
![](webapi_mvc.png)

###安裝 OData Package