---
title: Выражение CASE в скомпилированной в собственном коде хранимой процедуре
description: Скомпилированные в собственном коде модули T-SQL поддерживают выражения CASE в некоторых версиях SQL Server. В этом примере выражение CASE реализуется в запросе.
ms.custom: seo-dt-2019
ms.date: 11/21/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 2f82db01-da7e-4a7d-8bc0-48b245e6f768
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 92e8976ec52409d877bdd2bfe2b90a8784731d12
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171966"
---
# <a name="implementing-a-case-expression-in-a-natively-compiled-stored-procedure"></a>Реализация выражения CASE в скомпилированной в собственном коде хранимой процедуре
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

**Область применения:** [!INCLUDE[ssSDSFull_md](../../includes/sssdsfull-md.md)] и SQL Server, начиная с версии [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)]

Выражения CASE поддерживаются в скомпилированных в машинный код модулях T-SQL. В следующем примере показан вариант использования выражения CASE в запросе. 

``` 
-- Query using a CASE expression in a natively compiled stored procedure.
CREATE PROCEDURE dbo.usp_SOHOnlineOrderResult  
   WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER  
   AS BEGIN ATOMIC WITH  (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE=N'us_english')  
   SELECT   
      SalesOrderID,   
      CASE (OnlineOrderFlag)   
      WHEN 1 THEN N'Order placed online by customer'  
      ELSE N'Order placed by sales person'  
      END  
   FROM Sales.SalesOrderHeader_inmem
END  
GO  
  
EXEC dbo.usp_SOHOnlineOrderResult  
GO  
``` 

**Область применения:** [!INCLUDE[ssSQL14-md](../../includes/ssSQL14-md.md)] и SQL Server, начиная с версии [!INCLUDE[ssSQL15-md](../../includes/sssql16-md.md)]

  Выражения CASE *не* поддерживаются в скомпилированных в машинный код модулях T-SQL. В следующем примере показан способ реализации функциональности выражения CASE в скомпилированной в собственном коде хранимой процедуре.  
  
 В примерах кода используется табличная переменная для построения одного результирующего набора. Это применимо только при обработке ограниченного количества строк, так как требуется создание дополнительной копии строк данных.  
  
 Обязательно проверьте производительность этого обходного пути.  
  
```  
-- original query  
SELECT   
   SalesOrderID,   
   CASE (OnlineOrderFlag)   
   WHEN 1 THEN N'Order placed online by customer'  
   ELSE N'Order placed by sales person'  
   END  
FROM Sales.SalesOrderHeader_inmem  
  
--  workaround for CASE in natively compiled stored procedures  
--  use a table for the single resultset  
CREATE TYPE dbo.SOHOnlineOrderResult AS TABLE  
(  
   SalesOrderID uniqueidentifier not null index ix_SalesOrderID,  
     OrderFlag nvarchar(100) not null  
) with (memory_optimized=on)  
go  
  
-- natively compiled stored procedure that includes the query  
CREATE PROCEDURE dbo.usp_SOHOnlineOrderResult  
   WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER  
   AS BEGIN ATOMIC WITH  
      (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE=N'us_english')  
  
   -- table variable for creating the single resultset  
   DECLARE @result dbo.SOHOnlineOrderResult  
  
   -- CASE OnlineOrderFlag=1  
   INSERT @result   
   SELECT SalesOrderID, N'Order placed online by customer'  
      FROM Sales.SalesOrderHeader_inmem  
      WHERE OnlineOrderFlag=1  
  
   -- ELSE  
   INSERT @result   
   SELECT SalesOrderID, N'Order placed by sales person'  
      FROM Sales.SalesOrderHeader_inmem  
      WHERE OnlineOrderFlag!=1  
  
   -- return single resultset  
   SELECT SalesOrderID, OrderFlag FROM @result  
END  
GO  
  
EXEC dbo.usp_SOHOnlineOrderResult  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Проблемы миграции, связанные с хранимыми процедурами, скомпилированными в собственном коде](./a-guide-to-query-processing-for-memory-optimized-tables.md)   
 [Конструкции языка Transact-SQL, не поддерживаемые в выполняющейся в памяти OLTP](../../relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp.md)  
  
