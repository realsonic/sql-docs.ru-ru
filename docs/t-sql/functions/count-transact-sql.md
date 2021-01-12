---
description: Функция COUNT (Transact-SQL)
title: COUNT (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- COUNT_TSQL
- COUNT
dev_langs:
- TSQL
helpviewer_keywords:
- totals [SQL Server], COUNT function
- totals [SQL Server]
- counting items in group
- groups [SQL Server], number of items in
- number of group items
- COUNT function [Transact-SQL]
ms.assetid: 28d39da6-bc2e-46c7-858c-b1721c938830
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ac784713b7a74eb08acdfa9478b519c0b80940c1
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101324"
---
# <a name="count-transact-sql"></a>Функция COUNT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Эта функция возвращает количество элементов, найденных в группе. Функция `COUNT` работает подобно функции [COUNT_BIG](../../t-sql/functions/count-big-transact-sql.md). Эти функции различаются только типами данных в возвращаемых значениях. Функция `COUNT` всегда возвращает значение типа данных **int**. Функция `COUNT_BIG` всегда возвращает значение типа данных **bigint**.
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql

-- Aggregation Function Syntax  
COUNT ( { [ [ ALL | DISTINCT ] expression ] | * } )  

-- Analytic Function Syntax  
COUNT ( [ ALL ]  { expression | * } ) OVER ( [ <partition_by_clause> ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
**ALL**  
Применяет агрегатную функцию ко всем значениям. Аргумент ALL используется по умолчанию.
  
DISTINCT  
Указывает, что функция `COUNT` возвращает количество уникальных значений, не равных NULL.
  
*expression*  
[Выражение](../../t-sql/language-elements/expressions-transact-sql.md) любого типа, кроме **image**, **ntext** и **text**. Обратите внимание, что функция `COUNT` не поддерживает агрегатные функции и вложенные запросы в выражении.
  
\*  
Указывает, что функция `COUNT` должна учитывать все строки, чтобы определить общее количество строк таблицы для возврата. Функция `COUNT(*)` не принимает параметры и не поддерживает использование аргумента DISTINCT. Для функции `COUNT(*)` не требуется параметр *expression*, так как по определению она не использует сведения о конкретном столбце. Функция `COUNT(*)` возвращает количество строк в указанной таблице с учетом повторяющихся строк. Она подсчитывает каждую строку отдельно. При этом учитываются и строки, содержащие значения NULL.
  
OVER **(** [ *partition_by_clause* ] [ *order_by_clause* ] [ *ROW_or_RANGE_clause* ] **)**  
*partition_by_clause* делит результирующий набор, полученный с помощью предложения `FROM`, на секции, к которым применяется функция `COUNT`. Если этот параметр не указан, функция обрабатывает все строки результирующего набора запроса как отдельные группы. *order_by_clause* определяет логический порядок выполнения операции. Дополнительные сведения см. в статье [SELECT — предложение OVER (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md). 

## <a name="return-types"></a>Типы возвращаемых данных
 **int**  
  
## <a name="remarks"></a>Remarks  
Функция COUNT(\*) возвращает количество элементов в группе. Сюда входят значения NULL и повторяющиеся значения.
  
Функция COUNT(ALL *expression*) вычисляет *expression* для каждой строки в группе и возвращает количество значений, не равных NULL.
  
Функция COUNT(DISTINCT *expression*) вычисляет *expression* для каждой строки в группе и возвращает количество уникальных значений, не равных NULL.
  
Для возвращаемых значений, которые превышают значение 2^31-1, функция `COUNT` возвращает ошибку. В таких случаях используйте вместо нее функцию `COUNT_BIG`.
  
`COUNT` — это детерминированная функция, если она используется ***без** _ предложений OVER и ORDER BY. Она не детерминирована при использовании _*_с_*_ предложениями OVER и ORDER BY. Дополнительные сведения см. в статье [Детерминированные и недетерминированные функции](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-count-and-distinct"></a>A. Использование функции COUNT и параметра DISTINCT  
В этом примере функция возвращает количество различных должностей, которые может иметь сотрудник [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)].
  
```sql
SELECT COUNT(DISTINCT Title)  
FROM HumanResources.Employee;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
67  
  
(1 row(s) affected)
```
  
### <a name="b-using-count_"></a>Б. Использование функции COUNT(\_)  
В этом примере функция возвращает общее количество сотрудников [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)].
  
```sql
SELECT COUNT(*)  
FROM HumanResources.Employee;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
290  
  
(1 row(s) affected)
```
  
### <a name="c-using-count-with-other-aggregates"></a>В. Использование функции COUNT(\*) совместно с другими статистическими функциями  
В этом примере показано, что функция `COUNT(*)` работает с другими статистическими функциями в списке `SELECT`. В этом примере используется база данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].
  
```sql
SELECT COUNT(*), AVG(Bonus)  
FROM Sales.SalesPerson  
WHERE SalesQuota > 25000;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
----------- ---------------------
14            3472.1428
  
(1 row(s) affected)
```
  
### <a name="d-using-the-over-clause"></a>Г. Использование предложения OVER  
В этом примере функции `MIN`, `MAX`, `AVG` и `COUNT` используются с предложением `OVER`, чтобы получить статистические значения для каждого из отделов в таблице `HumanResources.Department` базы данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].
  
```sql
SELECT DISTINCT Name  
       , MIN(Rate) OVER (PARTITION BY edh.DepartmentID) AS MinSalary  
       , MAX(Rate) OVER (PARTITION BY edh.DepartmentID) AS MaxSalary  
       , AVG(Rate) OVER (PARTITION BY edh.DepartmentID) AS AvgSalary  
       ,COUNT(edh.BusinessEntityID) OVER (PARTITION BY edh.DepartmentID) AS EmployeesPerDept  
FROM HumanResources.EmployeePayHistory AS eph  
JOIN HumanResources.EmployeeDepartmentHistory AS edh  
     ON eph.BusinessEntityID = edh.BusinessEntityID  
JOIN HumanResources.Department AS d  
ON d.DepartmentID = edh.DepartmentID
WHERE edh.EndDate IS NULL  
ORDER BY Name;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Name                          MinSalary             MaxSalary             AvgSalary             EmployeesPerDept  
----------------------------- --------------------- --------------------- --------------------- ----------------  
Document Control              10.25                 17.7885               14.3884               5  
Engineering                   32.6923               63.4615               40.1442               6  
Executive                     39.06                 125.50                68.3034               4  
Facilities and Maintenance    9.25                  24.0385               13.0316               7  
Finance                       13.4615               43.2692               23.935                10  
Human Resources               13.9423               27.1394               18.0248               6  
Information Services          27.4038               50.4808               34.1586               10  
Marketing                     13.4615               37.50                 18.4318               11  
Production                    6.50                  84.1346               13.5537               195  
Production Control            8.62                  24.5192               16.7746               8  
Purchasing                    9.86                  30.00                 18.0202               14  
Quality Assurance             10.5769               28.8462               15.4647               6  
Research and Development      40.8654               50.4808               43.6731               4  
Sales                         23.0769               72.1154               29.9719               18  
Shipping and Receiving        9.00                  19.2308               10.8718               6  
Tool Design                   8.62                  29.8462               23.5054               6  
  
(16 row(s) affected)
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-count-and-distinct"></a>Д. Использование функции COUNT и параметра DISTINCT  
В этом примере функция возвращает количество различных должностей, которые может иметь конкретный сотрудник компании.
  
```sql
USE ssawPDW;  
  
SELECT COUNT(DISTINCT Title)  
FROM dbo.DimEmployee;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
67
```  
  
### <a name="f-using-count"></a>Е. Использование функции COUNT(\*)  
В этом примере функция возвращает общее количество строк в таблице `dbo.DimEmployee`.
  
```sql
USE ssawPDW;  
  
SELECT COUNT(*)  
FROM dbo.DimEmployee;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-------------
296
```  
  
### <a name="g-using-count-with-other-aggregates"></a>Ж. Использование функции COUNT(\*) совместно с другими статистическими функциями  
В этом примере функция `COUNT(*)` работает с другими статистическими функциями в списке `SELECT`. Запрос возвращает количество торговых представителей с годовой квотой продаж более 500 000 долл. США и их среднюю квоту продаж.
  
```sql
USE ssawPDW;  
  
SELECT COUNT(EmployeeKey) AS TotalCount, AVG(SalesAmountQuota) AS [Average Sales Quota]  
FROM dbo.FactSalesQuota  
WHERE SalesAmountQuota > 500000 AND CalendarYear = 2001;  
  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
TotalCount  Average Sales Quota
----------  -------------------
10          683800.0000
```
  
### <a name="h-using-count-with-having"></a>З. Использование функции COUNT с предложением HAVING  
В этом примере функция `COUNT` используется с предложением `HAVING`, чтобы получить список подразделений компании, в каждом из которых работает более 15 сотрудников.
  
```sql
USE ssawPDW;  
  
SELECT DepartmentName,   
       COUNT(EmployeeKey)AS EmployeesInDept  
FROM dbo.DimEmployee  
GROUP BY DepartmentName  
HAVING COUNT(EmployeeKey) > 15;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
DepartmentName  EmployeesInDept
--------------  ---------------
Sales           18
Production      179
```
  
### <a name="i-using-count-with-over"></a>И. Использование функции COUNT с предложением OVER  
В этом примере функция `COUNT` используется с предложением `OVER`, чтобы получить количество продуктов, содержащихся в каждом из указанных заказов на продажу.
  
```sql
USE ssawPDW;  
  
SELECT DISTINCT COUNT(ProductKey) OVER(PARTITION BY SalesOrderNumber) AS ProductCount  
    ,SalesOrderNumber  
FROM dbo.FactInternetSales  
WHERE SalesOrderNumber IN (N'SO53115',N'SO55981');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
ProductCount   SalesOrderID`
------------   -----------------
3              SO53115
1              SO55981
```
  
## <a name="see-also"></a>См. также
[Агрегатные функции (Transact-SQL)](../../t-sql/functions/aggregate-functions-transact-sql.md)  
[COUNT_BIG (Transact-SQL)](../../t-sql/functions/count-big-transact-sql.md)  
[Предложение OVER (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md)
  
  


