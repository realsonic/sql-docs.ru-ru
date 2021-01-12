---
description: Операторы работы с наборами — UNION (Transact-SQL)
title: UNION (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 08/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- UNION
- UNION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- UNION queries
- combining query results
- UNION operator [SQL Server]
ms.assetid: 607c296f-8a6a-49bc-975a-b8d0c0914df7
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 980dfe20df5c3909e8b35a6e5b460db2568b0fe3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097106"
---
# <a name="set-operators---union-transact-sql"></a>Операторы работы с наборами — UNION (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Сцепляет результаты двух запросов в один результирующий набор. Вы указываете, будет ли результирующий набор включать повторяющиеся строки:

- **UNION ALL** — повторяющиеся строки включаются.
- **UNION** — повторяющиеся строки исключаются.

Операция **UNION** отличается от операции **[JOIN](../queries/from-transact-sql.md)** :

- В результате операции **UNION** сцепляются результирующие наборы двух запросов. При этом операция **UNION** не создает отдельные строки для столбцов, полученных из двух таблиц.
- Операция **JOIN** сравнивает столбцы из двух таблиц и создает результирующие строки, которые состоят из столбцов из двух таблиц.
  
Ниже приведены основные правила объединения результирующих наборов двух запросов с помощью операции **UNION**:  
  
-   количество и порядок столбцов должны быть одинаковыми во всех запросах;  
  
-   типы данных должны быть совместимыми.  
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
{ <query_specification> | ( <query_expression> ) }   
{ UNION [ ALL ]   
  { <query_specification> | ( <query_expression> ) } 
  [ ...n ] }
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
\<query_specification> | ( \<query_expression> ) Это спецификация запроса или выражение запроса, возвращающее данные для объединения с данными из другой спецификации запроса или выражения запроса. Определения столбцов, которые являются частью операции UNION, не должны совпадать, однако должны быть совместимыми посредством неявного преобразования. Если типы данных различаются, то получившийся тип данных определяется на основе правил [очередности типов данных](../../t-sql/data-types/data-type-precedence-transact-sql.md). Если типы одинаковы, но различаются по точности, масштабу или длине, результат определяется на основе тех же самых правил, которые действуют при объединении выражений. Дополнительные сведения см. в разделе [Точность, масштаб и длина (Transact-SQL)](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).  
  
Столбцы типа данных **xml** должны быть эквивалентны друг другу. Все столбцы должны либо иметь тип, определенный в XML-схеме, либо быть нетипизированными. Типизированные столбцы должны относиться к одной и той же коллекции XML-схем.  
  
UNION  
Указывает на то, что несколько результирующих наборов следует объединить и возвратить в виде единого результирующего набора.  
  
ALL  
Объединяет в результирующий набор все строки, в том числе повторяющиеся. Если обратное не указано, дубликаты строк удаляются.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-a-simple-union"></a>A. Использование простого UNION  
При выполнении следующего примера в результирующий набор включается содержимое столбцов `ProductModelID` и `Name` таблиц `ProductModel` и `Gloves`.  
 
```sql
-- Uses AdventureWorks  
  
IF OBJECT_ID ('dbo.Gloves', 'U') IS NOT NULL  
DROP TABLE dbo.Gloves;  
GO  
-- Create Gloves table.  
SELECT ProductModelID, Name  
INTO dbo.Gloves  
FROM Production.ProductModel  
WHERE ProductModelID IN (3, 4);  
GO  
  
-- Here is the simple union.  
-- Uses AdventureWorks  
  
SELECT ProductModelID, Name  
FROM Production.ProductModel  
WHERE ProductModelID NOT IN (3, 4)  
UNION  
SELECT ProductModelID, Name  
FROM dbo.Gloves  
ORDER BY Name;  
GO  
```  
  
### <a name="b-using-select-into-with-union"></a>Б. Использование SELECT INTO с UNION  
При выполнении следующего примера предложение `INTO` во второй инструкции `SELECT` указывает, что в таблице с именем `ProductResults` содержится итоговый результирующий набор объединения выбранных столбцов таблиц `ProductModel` и `Gloves`. Таблица `Gloves` была создана в результате выполнения первой инструкции `SELECT`.  
  
```sql
-- Uses AdventureWorks  
  
IF OBJECT_ID ('dbo.ProductResults', 'U') IS NOT NULL  
DROP TABLE dbo.ProductResults;  
GO  
IF OBJECT_ID ('dbo.Gloves', 'U') IS NOT NULL  
DROP TABLE dbo.Gloves;  
GO  
-- Create Gloves table.  
SELECT ProductModelID, Name  
INTO dbo.Gloves  
FROM Production.ProductModel  
WHERE ProductModelID IN (3, 4);  
GO  
  
-- Uses AdventureWorks  
  
SELECT ProductModelID, Name  
INTO dbo.ProductResults  
FROM Production.ProductModel  
WHERE ProductModelID NOT IN (3, 4)  
UNION  
SELECT ProductModelID, Name  
FROM dbo.Gloves;  
GO  
  
SELECT ProductModelID, Name   
FROM dbo.ProductResults;  
```  
  
### <a name="c-using-union-of-two-select-statements-with-order-by"></a>В. Использование UNION двух инструкций SELECT с ORDER BY  
При использовании предложения UNION необходимо соблюдать порядок следования определенных параметров. В следующем примере показаны случаи правильного и неверного использования `UNION` в двух инструкциях `SELECT`, в которых необходимо переименовать столбцы на выходе.  
  
```sql
-- Uses AdventureWorks  
  
IF OBJECT_ID ('dbo.Gloves', 'U') IS NOT NULL  
DROP TABLE dbo.Gloves;  
GO  
-- Create Gloves table.  
SELECT ProductModelID, Name  
INTO dbo.Gloves  
FROM Production.ProductModel  
WHERE ProductModelID IN (3, 4);  
GO  
  
/* INCORRECT */  
-- Uses AdventureWorks  
  
SELECT ProductModelID, Name  
FROM Production.ProductModel  
WHERE ProductModelID NOT IN (3, 4)  
ORDER BY Name  
UNION  
SELECT ProductModelID, Name  
FROM dbo.Gloves;  
GO  
  
/* CORRECT */  
-- Uses AdventureWorks  
  
SELECT ProductModelID, Name  
FROM Production.ProductModel  
WHERE ProductModelID NOT IN (3, 4)  
UNION  
SELECT ProductModelID, Name  
FROM dbo.Gloves  
ORDER BY Name;  
GO  
```  
  
### <a name="d-using-union-of-three-select-statements-to-show-the-effects-of-all-and-parentheses"></a>Г. Использование UNION трех инструкций SELECT для демонстрации эффекта от использования скобок и ALL  
В следующих примерах предложение `UNION` используется для комбинирования результатов из трех таблиц, содержащих по 5 одинаковых строк данных. В первом примере используется предложение `UNION ALL`, в результате чего выдаются все 15 строк. Во втором примере предложение `UNION` используется без ключевого слова `ALL`, что позволяет удалить повторяющиеся строки из комбинированного результата выполнения трех инструкций `SELECT` и вывести только 5 строк.  
  
В третьем примере с первым предложением `ALL` используется ключевое слово `UNION`, а во втором предложении `UNION` вместо ключевого слова `ALL` используются скобки. Сначала выполняется второе предложение `UNION`, которое заключено в скобки. В результате возвращаются 5 строк, так как параметр `ALL` не используется и все повторяющиеся строки удаляются. Полученные 5 строк совмещаются с результатами выполнения первой инструкции `SELECT` с помощью ключевого слова `UNION ALL`. В данном случае повторяющиеся строки двух множеств, состоящих из пяти строк, не удаляются. Окончательный результат состоит из 10 строк.  
  
```sql
-- Uses AdventureWorks  
  
IF OBJECT_ID ('dbo.EmployeeOne', 'U') IS NOT NULL  
DROP TABLE dbo.EmployeeOne;  
GO  
IF OBJECT_ID ('dbo.EmployeeTwo', 'U') IS NOT NULL  
DROP TABLE dbo.EmployeeTwo;  
GO  
IF OBJECT_ID ('dbo.EmployeeThree', 'U') IS NOT NULL  
DROP TABLE dbo.EmployeeThree;  
GO  
  
SELECT pp.LastName, pp.FirstName, e.JobTitle   
INTO dbo.EmployeeOne  
FROM Person.Person AS pp JOIN HumanResources.Employee AS e  
ON e.BusinessEntityID = pp.BusinessEntityID  
WHERE LastName = 'Johnson';  
GO  
SELECT pp.LastName, pp.FirstName, e.JobTitle   
INTO dbo.EmployeeTwo  
FROM Person.Person AS pp JOIN HumanResources.Employee AS e  
ON e.BusinessEntityID = pp.BusinessEntityID  
WHERE LastName = 'Johnson';  
GO  
SELECT pp.LastName, pp.FirstName, e.JobTitle   
INTO dbo.EmployeeThree  
FROM Person.Person AS pp JOIN HumanResources.Employee AS e  
ON e.BusinessEntityID = pp.BusinessEntityID  
WHERE LastName = 'Johnson';  
GO  
-- Union ALL  
SELECT LastName, FirstName, JobTitle  
FROM dbo.EmployeeOne  
UNION ALL  
SELECT LastName, FirstName ,JobTitle  
FROM dbo.EmployeeTwo  
UNION ALL  
SELECT LastName, FirstName,JobTitle   
FROM dbo.EmployeeThree;  
GO  
  
SELECT LastName, FirstName,JobTitle  
FROM dbo.EmployeeOne  
UNION   
SELECT LastName, FirstName, JobTitle   
FROM dbo.EmployeeTwo  
UNION   
SELECT LastName, FirstName, JobTitle   
FROM dbo.EmployeeThree;  
GO  
  
SELECT LastName, FirstName,JobTitle   
FROM dbo.EmployeeOne  
UNION ALL  
(  
SELECT LastName, FirstName, JobTitle   
FROM dbo.EmployeeTwo  
UNION  
SELECT LastName, FirstName, JobTitle   
FROM dbo.EmployeeThree  
);  
GO  
  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-a-simple-union"></a>Д. Использование простого UNION  
При выполнении приведенного ниже примера в результирующий набор включается содержимое столбцов `CustomerKey` таблиц `FactInternetSales` и `DimCustomer`. Так как ключевое слово ALL не используется, повторяющиеся значения не включаются в результаты.  
  
```sql
-- Uses AdventureWorks  
  
SELECT CustomerKey   
FROM FactInternetSales    
UNION   
SELECT CustomerKey   
FROM DimCustomer   
ORDER BY CustomerKey;  
```  
  
### <a name="f-using-union-of-two-select-statements-with-order-by"></a>Е. Использование UNION двух инструкций SELECT с ORDER BY  
 Если какая-либо инструкция SELECT в инструкции UNION содержит предложение ORDER BY, это предложение должно находиться после всех инструкций SELECT. В приведенном ниже примере показаны случаи правильного и неправильного использования `UNION` в двух инструкциях `SELECT`, в которых столбец сортируется с помощью предложения ORDER BY.  
  
```sql
-- Uses AdventureWorks  
  
-- INCORRECT  
SELECT CustomerKey   
FROM FactInternetSales    
ORDER BY CustomerKey  
UNION   
SELECT CustomerKey   
FROM DimCustomer  
ORDER BY CustomerKey;  
  
-- CORRECT   
USE AdventureWorksPDW2012;  
  
SELECT CustomerKey   
FROM FactInternetSales    
UNION   
SELECT CustomerKey   
FROM DimCustomer   
ORDER BY CustomerKey;  
```  
  
### <a name="g-using-union-of-two-select-statements-with-where-and-order-by"></a>Ж. Использование UNION двух инструкций SELECT с WHERE и ORDER BY  
В приведенном ниже примере показаны случаи правильного и неправильного использования `UNION` в двух инструкциях `SELECT`, в которых требуются предложения WHERE и ORDER BY.  
  
```sql
-- Uses AdventureWorks  
  
-- INCORRECT   
SELECT CustomerKey   
FROM FactInternetSales   
WHERE CustomerKey >= 11000  
ORDER BY CustomerKey   
UNION   
SELECT CustomerKey   
FROM DimCustomer   
ORDER BY CustomerKey;  
  
-- CORRECT  
USE AdventureWorksPDW2012;  
  
SELECT CustomerKey   
FROM FactInternetSales   
WHERE CustomerKey >= 11000  
UNION   
SELECT CustomerKey   
FROM DimCustomer   
ORDER BY CustomerKey;  
```  
  
### <a name="h-using-union-of-three-select-statements-to-show-effects-of-all-and-parentheses"></a>З. Использование UNION трех инструкций SELECT для демонстрации эффекта от использования скобок и ALL  
В приведенных ниже примерах `UNION` используется для объединения результатов из **той же самой таблицы** с целью продемонстрировать эффект от применения ALL и скобок с `UNION`.  
  
В первом примере `UNION ALL` используется для вывода повторяющихся записей. Каждая строка из исходной таблицы возвращается три раза. Во втором примере `UNION` используется без ключевого слова `ALL`, что позволяет удалить повторяющиеся строки из объединенных результатов выполнения трех инструкций `SELECT` и вывести только неповторяющиеся строки из исходной таблицы.  
  
В третьем примере с первым оператором `UNION` используется ключевое слово `ALL`, а во втором предложении `UNION` вместо ключевого слова `ALL` используются скобки. Второе предложение `UNION` обрабатывается первым, так как оно заключено в скобки. Оно возвращает только неповторяющиеся строки из таблицы, так как параметр `ALL` не используется и повторяющиеся строки исключаются. Полученные строки объединяются с результатами выполнения первой инструкции `SELECT` с помощью ключевого слова `UNION ALL`. В данном случае повторяющиеся строки двух множеств не удаляются.  
  
```sql
-- Uses AdventureWorks  
  
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION ALL   
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION ALL   
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer;  
  
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION   
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION   
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer;  
  
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION ALL  
(  
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION   
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
);  
```  
  
## <a name="see-also"></a>См. также:  
[SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
[Примеры использования инструкции SELECT (Transact-SQL)](../../t-sql/queries/select-examples-transact-sql.md)  
