---
description: Создание определяемых пользователем функций (компонент Database Engine)
title: Создание пользовательских функций (ядро СУБД) | Документация Майкрософт
ms.custom: ''
ms.date: 11/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- SCHEMABINDING clause
- schema-bound functions [SQL Server]
- user-defined functions [SQL Server], creating
- CREATE FUNCTION statement
- valid statements [SQL Server]
- UDF
- TVF
ms.assetid: f0d5dd10-73fd-4e05-9177-07f56552bdf7
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3303df6ac71b7a6c92d4a1c65a8b7b4b3229cd6e
ms.sourcegitcommit: 4419e99d77ee2c73f9da1559c7944f7702f2de30
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/23/2020
ms.locfileid: "97743681"
---
# <a name="create-user-defined-functions-database-engine"></a>Создание определяемых пользователем функций (компонент Database Engine)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  В этом разделе описывается создание определяемой пользователем функции в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с помощью [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Ограничения  
  
-   Определяемые пользователем функции не могут выполнять действия, изменяющие состояние базы данных.  
  
-   Определяемые пользователем функции не могут содержать предложение `OUTPUT INTO`, целью которого является таблица.  
  
-   Определяемые пользователем функции не могут возвращать несколько результирующих наборов. Используйте хранимую процедуру, если нужно возвращать несколько результирующих наборов.  
  
-   Обработка ошибок в функциях, определяемых пользователем, ограниченна. UDF не поддерживает тип `TRY...CATCH`, `@ERROR` и `RAISERROR`.  
  
-   Определяемые пользователем функции не могут вызывать хранимую процедуру, но могут вызывать расширенную хранимую процедуру.  
  
-   Определяемые пользователем функции не могут использовать динамический SQL и временные таблицы. Табличные переменные разрешены к использованию.  
  
-   Инструкцию `SET` нельзя использовать в определяемых пользователем функциях.  
  
-   Пустое предложение `FOR XML` запрещено.  
  
-   Определяемые пользователем функции могут быть вложенными, то есть из одной функции может быть вызвана другая. Уровень вложенности увеличивается на единицу каждый раз, когда начинается выполнение вызванной функции и уменьшается на единицу, когда ее выполнение завершается. Вложенность определяемых пользователем функций не может превышать 32 уровней. Превышение максимального уровня вложенности приводит к ошибке выполнения для всей цепочки вызываемых функций. Каждый вызов управляемого кода из определяемой пользователем функции Transact-SQL считается одним уровнем вложенности из 32 возможных. Методы, вызываемые из управляемого кода, под это ограничение не подпадают.  
  
-   Следующие инструкции компонента Service Broker **не могут быть включены** в определение пользовательской функции [!INCLUDE[tsql](../../includes/tsql-md.md)]:  
  
    -   `BEGIN DIALOG CONVERSATION`  
  
    -   `END CONVERSATION`  
  
    -   `GET CONVERSATION GROUP`  
  
    -   `MOVE CONVERSATION`  
  
    -   `RECEIVE`  
  
    -   `SEND`  
  
###  <a name="permissions"></a><a name="Security"></a> Permissions 

Требуется разрешение `CREATE FUNCTION` на базу данных и разрешение `ALTER` для схемы, в которой создается функция. Если в функции указан определяемый пользователем тип, требуется разрешение `EXECUTE` на этот тип.  
  
##  <a name="scalar-functions"></a><a name="Scalar"></a> Скалярные функции  
 В следующем примере создается **скалярная функция (скалярная UDF)** из нескольких инструкций в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Функция имеет один входной параметр `ProductID`и возвращает одно значение — количество указанного товара на складе.  
  
```sql  
IF OBJECT_ID (N'dbo.ufnGetInventoryStock', N'FN') IS NOT NULL  
    DROP FUNCTION ufnGetInventoryStock;  
GO  
CREATE FUNCTION dbo.ufnGetInventoryStock(@ProductID int)  
RETURNS int   
AS   
-- Returns the stock level for the product.  
BEGIN  
    DECLARE @ret int;  
    SELECT @ret = SUM(p.Quantity)   
    FROM Production.ProductInventory p   
    WHERE p.ProductID = @ProductID   
        AND p.LocationID = '6';  
     IF (@ret IS NULL)   
        SET @ret = 0;  
    RETURN @ret;  
END; 
```  
  
 В следующем примере функция `ufnGetInventoryStock` используется для получения сведений о количестве товаров с идентификаторами `ProductModelID` от 75 до 80.  
  
```sql  
SELECT ProductModelID, Name, dbo.ufnGetInventoryStock(ProductID)AS CurrentSupply  
FROM Production.Product  
WHERE ProductModelID BETWEEN 75 and 80;  
```  

> [!NOTE]  
> Дополнительные сведения см. в разделе [CREATE FUNCTION (Transact-SQL)](../../t-sql/statements/create-function-transact-sql.md). 

##  <a name="table-valued-functions"></a><a name="TVF"></a> Функции с табличными значениями  
Результатом следующего примера является **встроенная функция, возвращающая табличное значение (TVF)**, в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Функция имеет один входной параметр — идентификатор клиента (магазина) — и возвращает столбцы `ProductID`, `Name`и столбец `YTD Total` со сведениями о продажах продукта за текущий год.  
  
```sql  
IF OBJECT_ID (N'Sales.ufn_SalesByStore', N'IF') IS NOT NULL  
    DROP FUNCTION Sales.ufn_SalesByStore;  
GO  
CREATE FUNCTION Sales.ufn_SalesByStore (@storeid int)  
RETURNS TABLE  
AS  
RETURN   
(  
    SELECT P.ProductID, P.Name, SUM(SD.LineTotal) AS 'Total'  
    FROM Production.Product AS P   
    JOIN Sales.SalesOrderDetail AS SD ON SD.ProductID = P.ProductID  
    JOIN Sales.SalesOrderHeader AS SH ON SH.SalesOrderID = SD.SalesOrderID  
    JOIN Sales.Customer AS C ON SH.CustomerID = C.CustomerID  
    WHERE C.StoreID = @storeid  
    GROUP BY P.ProductID, P.Name  
);  
```  
  
В следующем примере функция вызывается с идентификатором 602.  
  
```sql  
SELECT * FROM Sales.ufn_SalesByStore (602);  
```  
  
Результатом следующего примера является **многооператорная встроенная функция, возвращающая табличное значение (MSTVF)**, в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Функция имеет один входной параметр `EmployeeID` и возвращает список всех сотрудников, которые напрямую или косвенно отчитываются перед заданным сотрудником. Затем функция вызывается с указанием идентификатора сотрудника 109.  
  
```sql  
IF OBJECT_ID (N'dbo.ufn_FindReports', N'TF') IS NOT NULL  
    DROP FUNCTION dbo.ufn_FindReports;  
GO  
CREATE FUNCTION dbo.ufn_FindReports (@InEmpID INTEGER)  
RETURNS @retFindReports TABLE   
(  
    EmployeeID int primary key NOT NULL,  
    FirstName nvarchar(255) NOT NULL,  
    LastName nvarchar(255) NOT NULL,  
    JobTitle nvarchar(50) NOT NULL,  
    RecursionLevel int NOT NULL  
)  
--Returns a result set that lists all the employees who report to the   
--specific employee directly or indirectly.*/  
AS  
BEGIN  
WITH EMP_cte(EmployeeID, OrganizationNode, FirstName, LastName, JobTitle, RecursionLevel) -- CTE name and columns  
    AS (  
        SELECT e.BusinessEntityID, e.OrganizationNode, p.FirstName, p.LastName, e.JobTitle, 0 -- Get the initial list of Employees for Manager n  
        FROM HumanResources.Employee e   
INNER JOIN Person.Person p   
ON p.BusinessEntityID = e.BusinessEntityID  
        WHERE e.BusinessEntityID = @InEmpID  
        UNION ALL  
        SELECT e.BusinessEntityID, e.OrganizationNode, p.FirstName, p.LastName, e.JobTitle, RecursionLevel + 1 -- Join recursive member to anchor  
        FROM HumanResources.Employee e   
            INNER JOIN EMP_cte  
            ON e.OrganizationNode.GetAncestor(1) = EMP_cte.OrganizationNode  
INNER JOIN Person.Person p   
ON p.BusinessEntityID = e.BusinessEntityID  
        )  
-- copy the required columns to the result of the function   
   INSERT @retFindReports  
   SELECT EmployeeID, FirstName, LastName, JobTitle, RecursionLevel  
   FROM EMP_cte   
   RETURN  
END;  
GO  
```  
  
В следующем примере функция вызывается с идентификатором сотрудника 1.  
  
```sql  
SELECT EmployeeID, FirstName, LastName, JobTitle, RecursionLevel  
FROM dbo.ufn_FindReports(1);  
```  

> [!NOTE]  
> Дополнительные сведения и примеры встроенных функций с табличными значениями (встроенные TVF) или многооператорных функций с табличными значениями (MSTVF) см. в разделе [CREATE FUNCTION (Transact-SQL)](../../t-sql/statements/create-function-transact-sql.md). 

## <a name="best-practices"></a>Рекомендации  
Если определяемая пользователем функция (UDF) создана без применения предложения `SCHEMABINDING`, то изменения базовых объектов могут повлиять на определение функции и привести к непредвиденным результатам при вызове функции. Рекомендуется реализовать один из следующих методов, чтобы обеспечить, что функция не устареет из-за изменения ее базовых объектов.  
  
-   Укажите при создании функции UDF предложение `WITH SCHEMABINDING`. Это обеспечит невозможность изменения объектов, на которые ссылается определение функции, если при этом не изменяется сама функция.  
  
-   Выполняйте хранимую процедуру [sp_refreshsqlmodule](../../relational-databases/system-stored-procedures/sp-refreshsqlmodule-transact-sql.md) после изменения любого объекта, указанного в определении функции UDF.  

Если вы создаете определяемую пользователем функцию, не имеющую доступа к данным, укажите параметр `SCHEMABINDING`. Это не позволит оптимизатору запросов создавать ненужные операторы очередей для планов запроса, содержащих такие определяемые пользователем функции. Дополнительные сведения об очередях см. в [справочнике по логическим и физическим операторам Showplan](../../relational-databases/showplan-logical-and-physical-operators-reference.md). Дополнительные сведения о создании функций, привязанных к схеме, см. в [соответствующем разделе](../../relational-databases/user-defined-functions/user-defined-functions.md#SchemaBound).

Присоединение к MSTVF в предложении `FROM` возможно, но может привести к снижению производительности. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не может использовать все оптимизированные методы для некоторых инструкций, которые можно включить в функцию MSTVF, и в результате план запроса оказывается неоптимальным. Чтобы получить наилучшую производительность, по возможности задавайте соединения не между функциями, а между базовыми таблицами.  

> [!IMPORTANT]
> Функции MSTVF имеют фиксированное предполагаемое значение кратности 100 начиная с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и 1 в более ранних версиях [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].    
> Начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] для оптимизации плана выполнения, который использует функции MSTVF, можно использовать выполнение с чередованием, что обеспечивает фактическую кратность вместо приведенной выше эвристики.     
> Дополнительные сведения см. в разделе [Выполнение с чередованием для функций с табличным значением с несколькими инструкциями](../../relational-databases/performance/intelligent-query-processing.md#interleaved-execution-for-mstvfs).

> [!NOTE]  
> Параметры ANSI_WARNINGS не годятся для передачи в хранимые процедуры, пользовательские функции и при объявлении и установке переменных в пакетных инструкциях. Например, если объявить переменную как **char(3)** , а затем присвоить ей значение длиннее трех символов, данные будут усечены до размера переменной, а инструкция `INSERT` или `UPDATE` завершится без ошибок.

## <a name="see-also"></a>См. также  
 [Определяемые пользователем функции](../../relational-databases/user-defined-functions/user-defined-functions.md)     
 [CREATE FUNCTION (Transact-SQL)](../../t-sql/statements/create-function-transact-sql.md)    
 [ALTER FUNCTION (Transact-SQL)](../../t-sql/statements/alter-function-transact-sql.md)    
 [DROP FUNCTION (Transact-SQL)](../../t-sql/statements/drop-function-transact-sql.md)     
 [DROP PARTITION FUNCTION (Transact-SQL)](../../t-sql/statements/drop-partition-function-transact-sql.md)    
 Другие примеры ( [сообщество](https://www.bing.com/search?q=user%20defined%20function%20%22sql%20server%202016%22%20examples&qs=n&form=QBRE&pq=user%20defined%20function%20%22sql%20server%202016%22%20examples&sc=0-48&sp=-1&sk=&cvid=C3AD337125A840AD9EEFA3AAC36A3712))   
  
