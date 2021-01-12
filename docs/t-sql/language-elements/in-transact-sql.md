---
description: IN (Transact-SQL)
title: IN (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- IN_TSQL
- IN
dev_langs:
- TSQL
helpviewer_keywords:
- values [SQL Server], matching
- NOT IN keyword
- 8623 (Database Engine error)
- matching values in subquery or list [SQL Server]
- IN keyword
- 8632 (Database Engine error)
ms.assetid: 4419de73-96b1-4dfe-8500-f4507915db04
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1a2be93be39ac3af00895a9260958b512652fc3d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98085333"
---
# <a name="in-transact-sql"></a>IN (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Определяет, совпадает ли указанное значение с одним из значений, содержащихся во вложенном запросе или списке.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
test_expression [ NOT ] IN   
    ( subquery | expression [ ,...n ]  
    )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *test_expression*  
 Любое допустимое выражение [expression](../../t-sql/language-elements/expressions-transact-sql.md).  
  
 *subquery*  
 Вложенный запрос, содержащий результирующий набор, состоящий из одного столбца. Этот столбец должен иметь тот же тип данных, что и аргумент *test_expression*.  
  
 *expression*[ **,**... *n* ]  
 Список выражений для поиска совпадения. Все выражения должны иметь тот же тип, что и аргумент *test_expression*.  
  
## <a name="result-types"></a>Типы результата  
 **Boolean**  
  
## <a name="result-value"></a>Значение результата  
 Если значение аргумента *test_expression* равно одному из значений, возвращенных вложенным запросом *subquery* или одному из значений, содержащихся в списке *expression* (где значения разделяются запятыми), результирующее значение равно TRUE. В противном случае оно равно FALSE.  
  
 Предложение NOT IN инвертирует значение *subquery* или *expression*.  
  
> [!CAUTION]  
>  Для любых значений NULL, возвращаемых в *subquery* или *expression*, которые сравниваются со значением *test_expression* с помощью предложения IN или NOT IN, возвращается результат UNKNOWN. Использование значений NULL с предложениями IN или NOT IN может привести к непредвиденным результатам.  
  
## <a name="remarks"></a>Примечания  
 Явное включение очень большого количества значений (много тысяч значений, разделенных запятыми) в круглые скобки в предложение IN может привести к интенсивному расходованию ресурсов и возврату ошибки 8623 или 8632. Чтобы избежать этой проблемы, храните элементы списка IN в таблице и используйте вложенный запрос SELECT в предложении IN.  
  
 Ошибка 8623.  
  
 `The query processor ran out of internal resources and could not produce a query plan. This is a rare event and only expected for extremely complex queries or queries that reference a very large number of tables or partitions. Please simplify the query. If you believe you have received this message in error, contact Customer Support Services for more information.`  
  
 Ошибка 8632.  
  
 `Internal error: An expression services limit has been reached. Please look for potentially complex expressions in your query, and try to simplify them.`  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-comparing-or-and-in"></a>A. Сравнение OR и IN  
 В следующем примере осуществляется выборка списка имен сотрудников на должностях инженеров-разработчиков, разработчиков средств и сотрудников отдела сбыта.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT p.FirstName, p.LastName, e.JobTitle  
FROM Person.Person AS p  
JOIN HumanResources.Employee AS e  
    ON p.BusinessEntityID = e.BusinessEntityID  
WHERE e.JobTitle = 'Design Engineer'   
   OR e.JobTitle = 'Tool Designer'   
   OR e.JobTitle = 'Marketing Assistant';  
GO  
```  
  
 Этот же результат можно получить при помощи оператора IN.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT p.FirstName, p.LastName, e.JobTitle  
FROM Person.Person AS p  
JOIN HumanResources.Employee AS e  
    ON p.BusinessEntityID = e.BusinessEntityID  
WHERE e.JobTitle IN ('Design Engineer', 'Tool Designer', 'Marketing Assistant');  
GO  
```  
  
 Ниже представлен результирующий набор, возвращаемый каждым из запросов.  
  
```  
FirstName   LastName      Title  
---------   ---------   ---------------------  
Sharon      Salavaria   Design Engineer                                     
Gail        Erickson    Design Engineer                                     
Jossef      Goldberg    Design Engineer                                     
Janice      Galvin      Tool Designer                                       
Thierry     D'Hers      Tool Designer                                       
Wanida      Benshoof    Marketing Assistant                                 
Kevin       Brown       Marketing Assistant                                 
Mary        Dempsey     Marketing Assistant                                 
  
(8 row(s) affected)  
```  
  
### <a name="b-using-in-with-a-subquery"></a>Б. Применение IN с вложенным запросом  
 В следующем примере осуществляется поиск идентификаторов менеджеров по продажам в таблице `SalesPerson`, имеющих объем продаж более 250 000 долларов в год, а затем выборка из таблицы `Employee` имен и фамилий всех сотрудников, идентификаторы `EmployeeID` которых совпадают с результатами вложенного запроса `SELECT`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT p.FirstName, p.LastName  
FROM Person.Person AS p  
    JOIN Sales.SalesPerson AS sp  
    ON p.BusinessEntityID = sp.BusinessEntityID  
WHERE p.BusinessEntityID IN  
   (SELECT BusinessEntityID  
   FROM Sales.SalesPerson  
   WHERE SalesQuota > 250000);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
FirstName   LastName                                             
---------   --------   
Tsvi         Reiter                                              
Michael      Blythe                                              
Tete         Mensa-Annan                                         
  
(3 row(s) affected)  
```  
  
### <a name="c-using-not-in-with-a-subquery"></a>В. Применение NOT IN с вложенным запросом  
 В следующем примере производится поиск торговцев, квота которых не выше 250 000 долларов США. С помощью `NOT IN` можно найти торговцев, которые не соответствуют списку значений.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT p.FirstName, p.LastName  
FROM Person.Person AS p  
    JOIN Sales.SalesPerson AS sp  
    ON p.BusinessEntityID = sp.BusinessEntityID  
WHERE p.BusinessEntityID NOT IN  
   (SELECT BusinessEntityID  
   FROM Sales.SalesPerson  
   WHERE SalesQuota > 250000);  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-using-in-and-not-in"></a>Г. Использование IN и NOT IN  
 В следующем примере показан поиск всех записей в таблице `FactInternetSales`, которые соответствуют значениям `SalesReasonKey` в таблице `DimSalesReason`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT * FROM FactInternetSalesReason   
WHERE SalesReasonKey   
IN (SELECT SalesReasonKey FROM DimSalesReason);   
```  
  
 В следующем примере показан поиск всех записей в таблице `FactInternetSalesReason`, которые не соответствуют значениям `SalesReasonKey` в таблице `DimSalesReason`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT * FROM FactInternetSalesReason   
WHERE SalesReasonKey   
NOT IN (SELECT SalesReasonKey FROM DimSalesReason);  
```  
  
### <a name="e-using-in-with-an-expression-list"></a>Д. Использование IN в списке выражений  
 В следующем примере осуществляется поиск всех идентификаторов продавцов в таблице `DimEmployee` сотрудников с именами `Mike` или `Michael`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT FirstName, LastName  
FROM DimEmployee  
WHERE FirstName IN ('Mike', 'Michael');  
```  
  
## <a name="see-also"></a>См. также:  
 [CASE (Transact-SQL)](../../t-sql/language-elements/case-transact-sql.md)   
 [Выражения (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Встроенные функции (Transact-SQL)](~/t-sql/functions/functions.md)   
 [Операторы (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)   
 [ALL (Transact-SQL)](../../t-sql/language-elements/all-transact-sql.md)   
 [SOME | ANY (Transact-SQL)](../../t-sql/language-elements/some-any-transact-sql.md)  
  
  



