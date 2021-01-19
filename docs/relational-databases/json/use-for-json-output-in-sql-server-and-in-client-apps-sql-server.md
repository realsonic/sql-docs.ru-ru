---
description: Использование выходных данных FOR JSON в SQL Server и клиентских приложениях (SQL Server)
title: Использование выходных данных FOR JSON в SQL Server и клиентских приложениях
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- FOR JSON, using in client apps
- FOR JSON, using in SQL Server
ms.assetid: 302e5397-b499-4ea3-9a7f-c24ccad698eb
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4bd5911552144084550abdadb3d432ab08f3bf9a
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172506"
---
# <a name="use-for-json-output-in-sql-server-and-in-client-apps-sql-server"></a>Использование выходных данных FOR JSON в SQL Server и клиентских приложениях (SQL Server)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sqlserver2016-asdb.md)]

В следующих примерах показаны некоторые способы использования предложения **FOR JSON** или выходных данных JSON в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или в клиентских приложениях.  
  
## <a name="use-for-json-output-in-sql-server-variables"></a>Использование выходных данных FOR JSON в переменных SQL Server  
Выходные данные предложения FOR JSON имеют тип NVARCHAR(MAX), поэтому их можно назначить любой переменной, как показано в следующем примере.  
  
```sql  
DECLARE @x NVARCHAR(MAX) =
  (SELECT TOP 10 *
     FROM Sales.SalesOrderHeader
     FOR JSON AUTO)  
```  
  
## <a name="use-for-json-output-in-sql-server-user-defined-functions"></a>Использование выходных данных FOR JSON в определяемых пользователем функциях SQL Server  
 Вы можете создать определяемые пользователем функции, которые форматируют результирующие наборы как JSON и возвращают эти выходные данные JSON. В следующем примере создается определяемая пользователем функция, которая производит выборку некоторых строк детализации заказа на продажу и форматирует их как массив JSON.  
  
```sql  
CREATE FUNCTION GetSalesOrderDetails(@salesOrderId int)  
 RETURNS NVARCHAR(MAX)  
AS  
BEGIN  
   RETURN (SELECT UnitPrice, OrderQty  
           FROM Sales.SalesOrderDetail  
           WHERE SalesOrderID = @salesOrderId  
           FOR JSON AUTO)  
END
```  
  
 Можно использовать эту функцию в пакете или запросе, как показано в следующем примере.  
  
```sql  
DECLARE @x NVARCHAR(MAX) = dbo.GetSalesOrderDetails(43659)

PRINT dbo.GetSalesOrderDetails(43659)

SELECT TOP 10
  H.*, dbo.GetSalesOrderDetails(H.SalesOrderId) AS Details
FROM Sales.SalesOrderHeader H
```  
  
## <a name="merge-parent-and-child-data-into-a-single-table"></a>Слияние родительских и дочерних данных в одну таблицу  
В следующем примере каждый набор дочерних строк форматируется как массив JSON. Массив JSON становится значением столбца сведений в родительской таблице.  
  
```sql  
SELECT TOP 10 SalesOrderId, OrderDate,  
      (SELECT TOP 3 UnitPrice, OrderQty  
         FROM Sales.SalesOrderDetail D  
         WHERE H.SalesOrderId = D.SalesOrderID  
         FOR JSON AUTO) AS Details  
INTO SalesOrder  
FROM Sales.SalesOrderHeader H  
```  
  
## <a name="update-the-data-in-json-columns"></a>Обновление данных в столбцах JSON  
 В следующем примере показано, что можно обновлять значения столбцов, содержащих текст JSON.  
  
```sql  
UPDATE SalesOrder  
SET Details =  
     (SELECT TOP 1 UnitPrice, OrderQty  
       FROM Sales.SalesOrderDetail D  
       WHERE D.SalesOrderId = SalesOrder.SalesOrderId  
      FOR JSON AUTO) 
```  
  
## <a name="use-for-json-output-in-a-c-client-app"></a>Использование выходных данных FOR JSON в клиентском приложении C#  
 В приведенном ниже примере показано, как получить выходные данные JSON запроса в объект StringBuilder в клиентском приложении C#. Предположим, что переменная `queryWithForJson` содержит текст инструкции SELECT с предложением FOR JSON.  
  
```csharp  
var queryWithForJson = "SELECT ... FOR JSON";
using(var conn = new SqlConnection("<connection string>"))
{
    using(var cmd = new SqlCommand(queryWithForJson, conn))
    {
        conn.Open();
        var jsonResult = new StringBuilder();
        var reader = cmd.ExecuteReader();
        if (!reader.HasRows)
        {
            jsonResult.Append("[]");
        }
        else
        {
            while (reader.Read())
            {
                jsonResult.Append(reader.GetValue(0).ToString());
            }
        }
    }
}
```  

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>Дополнительные сведения о JSON в SQL Server и базе данных SQL Azure  
  
### <a name="microsoft-videos"></a>Видео Майкрософт

Наглядные инструкции по встроенной поддержке JSON в SQL Server и базе данных SQL Azure см. в следующих видео.

-   [SQL Server 2016 and JSON Support](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support) (SQL Server 2016 и поддержка JSON)

-   [Using JSON in SQL Server 2016 and Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database) (Использование JSON в SQL Server 2016 и базе данных SQL Azure)

-   [JSON as a bridge between NoSQL and relational worlds](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) (JSON как мост между NoSQL и реляционными решениями)
 
## <a name="see-also"></a>См. также:  
 [Форматирование результатов запроса как JSON с помощью предложения FOR JSON (SQL Server)](../../relational-databases/json/format-query-results-as-json-with-for-json-sql-server.md)  
  
  
