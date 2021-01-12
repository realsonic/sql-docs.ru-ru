---
title: SELECT (Transact-SQL)
description: SELECT (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SELECT_TSQL
- SELECT
dev_langs:
- TSQL
helpviewer_keywords:
- retrieving rows
- SELECT statement [SQL Server]
- SELECT statement [SQL Server], about SELECT statement
- row retrieval [SQL Server], SELECT statement
- DML [SQL Server], SELECT statement
- data manipulation language [SQL Server], SELECT statement
- row retrieval [SQL Server]
- queries [SQL Server], results
author: VanMSFT
ms.author: vanto
ms.custom: ''
ms.date: 10/24/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: dff1dc7f969435e4a4f112e995fe092e8b4da8be
ms.sourcegitcommit: d681796e8c012eca2d9629d3b816749e9f50f868
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/08/2021
ms.locfileid: "98005381"
---
# <a name="select-transact-sql"></a>SELECT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает строки из базы данных и позволяет делать выборку одной или нескольких строк или столбцов из одной или нескольких таблиц в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Полный синтаксис инструкции SELECT сложен, однако основные предложения можно вкратце описать следующим образом:  
  
[ WITH { [[ XMLNAMESPACES ,]](../../t-sql/xml/with-xmlnamespaces.md) [[ \<common_table_expression> ]](../../t-sql/queries/with-common-table-expression-transact-sql.md) } ]
  
 [SELECT *выбранный_список*](../../t-sql/queries/select-clause-transact-sql.md) [ [INTO *новая_таблица*](../../t-sql/queries/select-into-clause-transact-sql.md) ]  
  
 [ [FROM *источник_таблицы*](../../t-sql/queries/from-transact-sql.md) ] [ [WHERE *условие_поиска*](../../t-sql/queries/where-transact-sql.md) ]  
  
 [ [GROUP BY *выражение_группирования*](../../t-sql/queries/select-group-by-transact-sql.md) ]  
  
 [ [HAVING *условие_поиска*](../../t-sql/queries/select-having-transact-sql.md) ]  
  
 [ [ORDER BY *выражение_упорядочения* [ ASC | DESC ]](../../t-sql/queries/select-order-by-clause-transact-sql.md)]  
  
 Операторы [UNION](../../t-sql/language-elements/set-operators-union-transact-sql.md), [EXCEPT и INTERSECT](../../t-sql/language-elements/set-operators-except-and-intersect-transact-sql.md) можно использовать между запросами, чтобы сравнить их результаты или объединить в один результирующий набор.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
<SELECT statement> ::=    
    [ WITH { [ XMLNAMESPACES ,] [ <common_table_expression> [,...n] ] } ]  
    <query_expression>   
    [ ORDER BY <order_by_expression> ] 
    [ <FOR Clause>]   
    [ OPTION ( <query_hint> [ ,...n ] ) ]   
<query_expression> ::=   
    { <query_specification> | ( <query_expression> ) }   
    [  { UNION [ ALL ] | EXCEPT | INTERSECT }  
        <query_specification> | ( <query_expression> ) [...n ] ]   
<query_specification> ::=   
SELECT [ ALL | DISTINCT ]   
    [TOP ( expression ) [PERCENT] [ WITH TIES ] ]   
    < select_list >   
    [ INTO new_table ]   
    [ FROM { <table_source> } [ ,...n ] ]   
    [ WHERE <search_condition> ]   
    [ <GROUP BY> ]   
    [ HAVING < search_condition > ]   
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
[ WITH <common_table_expression> [ ,...n ] ]  
SELECT <select_criteria>  
[;]  
  
<select_criteria> ::=  
    [ TOP ( top_expression ) ]   
    [ ALL | DISTINCT ]   
    { * | column_name | expression } [ ,...n ]   
    [ FROM { table_source } [ ,...n ] ]  
    [ WHERE <search_condition> ]   
    [ GROUP BY <group_by_clause> ]   
    [ HAVING <search_condition> ]   
    [ ORDER BY <order_by_expression> ]  
    [ OPTION ( <query_option> [ ,...n ] ) ]  
  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Remarks  
 Учитывая сложность инструкции SELECT, элементы ее синтаксиса и аргументы подробно представлены в предложении:  

:::row:::
    :::column:::
        [WITH XMLNAMESPACES](../../t-sql/xml/with-xmlnamespaces.md)
    :::column-end:::
    :::column:::
        [HAVING](../../t-sql/queries/select-having-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [WITH <обобщенное_табличное_выражение>](../../t-sql/queries/with-common-table-expression-transact-sql.md)
    :::column-end:::
    :::column:::
        [UNION](../../t-sql/language-elements/set-operators-union-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [Предложение SELECT](../../t-sql/queries/select-clause-transact-sql.md)
    :::column-end:::
    :::column:::
        [EXCEPT и INTERSECT](../../t-sql/language-elements/set-operators-except-and-intersect-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [Предложение INTO](../../t-sql/queries/select-into-clause-transact-sql.md)
    :::column-end:::
    :::column:::
        [ORDER BY](../../t-sql/queries/select-order-by-clause-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [FROM](../../t-sql/queries/from-transact-sql.md)
    :::column-end:::
    :::column:::
        [Предложение FOR](../../t-sql/queries/select-for-clause-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [WHERE](../../t-sql/queries/where-transact-sql.md)
    :::column-end:::
    :::column:::
        [Предложение OPTION](../../t-sql/queries/option-clause-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [GROUP BY](../../t-sql/queries/select-group-by-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

 Порядок предложений в инструкции SELECT имеет значение. Любое из необязательных предложений может быть опущено; но если необязательные предложения используются, они должны следовать в определенном порядке.  
  
 Инструкции SELECT разрешено использовать в определяемых пользователем функциях только в том случае, если списки выбора этих инструкций содержат выражения, которые присваивают значения переменным, локальным для функций.  
  
 Четырехкомпонентное имя, использующее функцию OPENDATASOURCE в качестве части имени сервера, может служить в качестве исходной таблицы в любом месте инструкции SELECT, где может появляться имя таблицы. Четырехкомпонентное имя не может указываться для [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Для инструкций SELECT, которые задействуют удаленные таблицы, существуют некоторые ограничения на синтаксис.  
  
## <a name="logical-processing-order-of-the-select-statement"></a>Логический порядок обработки инструкции SELECT  
 Следующие действия демонстрируют логический порядок обработки или порядок привязки инструкции SELECT. Этот порядок определяет, когда объекты, определенные в одном шаге, становятся доступными для предложений в последующих шагах. Например, если обработчик запросов можно привязать (для доступа) к таблицам или представлениям, определенным в предложении FROM, эти объекты и их столбцы становятся доступными для всех последующих шагов. И наоборот, поскольку предложение SELECT является шагом 8, любые псевдонимы столбцов или производных столбцов, определенные в этом предложении, не могут быть объектом для ссылки предыдущих предложений. Вместе с тем к ним могут обращаться последующие предложения, например предложение ORDER BY. Фактическое физическое выполнение инструкции определяется обработчиком запросов и порядок из этого списка может значительно отличаться.  
  
1.  FROM  
2.  ON  
3.  JOIN  
4.  WHERE  
5.  GROUP BY  
6.  WITH CUBE или WITH ROLLUP  
7.  HAVING  
8.  SELECT  
9. DISTINCT  
10. ORDER BY  
11. В начало  

> [!WARNING]
> Как правило, применяется предыдущая последовательность. Однако в редких случаях может быть указана другая последовательность.
>
> Например, предположим, что в представлении есть кластеризованный индекс и представление исключает некоторые строки таблицы, а для списка столбцов SELECT представления используется инструкция CONVERT, которая изменяет тип данных с *varchar* на *integer*. В этом случае CONVERT может выполняться до выполнения предложения WHERE. Это нестандартное поведение. Если это имеет значение в вашем случае, можно изменить представление, чтобы исключить использование другой последовательности. 

## <a name="permissions"></a>Разрешения  
 Для выборки данных требуется разрешение **SELECT** на таблицу или представление, которое может быть унаследовано из области более высокого уровня, например разрешение **SELECT** на схему или разрешение **CONTROL** на таблицу. Или необходимо быть членом предопределенных ролей базы данных **db_datareader** или **db_owner** либо предопределенной роли сервера **sysadmin**. Для создания новой таблицы с помощью **SELECT INTO** необходимо также разрешение **CREATE TABLE** и разрешение **ALTER SCHEMA** для схемы, которой принадлежит новая таблица.  
  
## <a name="examples"></a>Примеры:   
В следующих примерах используется база данных [!INCLUDE[ssawPDW](../../includes/ssawpdw-md.md)].
  
### <a name="a-using-select-to-retrieve-rows-and-columns"></a>A. Использование SELECT для получения строк и столбцов  
 В этом разделе приведены три примера кода. В ходе выполнения первого примера кода возвращаются все строки (предложение WHERE не указано), а также все столбцы (используется `*`) таблицы `DimEmployee`.  
  
```sql  
SELECT *  
FROM DimEmployee  
ORDER BY LastName;  
```  
  
 В этом примере для достижения такого же результата используется присвоение псевдонима таблице.  
  
```sql  
SELECT e.*  
FROM DimEmployee AS e  
ORDER BY LastName;  
```  
  
 В ходе выполнения данного примера кода возвращаются все строки (предложение WHERE не задано) и подмножества столбцов (`FirstName`, `LastName`, `StartDate`) таблицы `DimEmployee` базы данных `AdventureWorksPDW2012`. Заголовок третьего столбца переименовывается в `FirstDay`.  
  
```sql  
SELECT FirstName, LastName, StartDate AS FirstDay  
FROM DimEmployee   
ORDER BY LastName;  
```  
  
 Этот пример возвращает только строки для `DimEmployee`, имеющие `EndDate`, не равное NULL, и `MaritalStatus`, равное "M" (состоит в браке).  
  
```sql  
SELECT FirstName, LastName, StartDate AS FirstDay  
FROM DimEmployee   
WHERE EndDate IS NOT NULL   
AND MaritalStatus = 'M'  
ORDER BY LastName;  
```  
  
### <a name="b-using-select-with-column-headings-and-calculations"></a>Б. Использование SELECT с заголовками столбцов и вычислениями  
 В следующем примере возвращаются все строки из таблицы `DimEmployee` и вычисляется заработная плата до вычетов для каждого сотрудника на основе их `BaseRate` и с учетом 40-часовой рабочей недели.  
  
```sql  
SELECT FirstName, LastName, BaseRate, BaseRate * 40 AS GrossPay  
FROM DimEmployee  
ORDER BY LastName;  
```  
  
### <a name="c-using-distinct-with-select"></a>В. Совместное использование DISTINCT и SELECT  
 В следующем примере используется `DISTINCT` для создания списка всех уникальных должностей в таблице `DimEmployee`.  
  
```sql  
SELECT DISTINCT Title  
FROM DimEmployee  
ORDER BY Title;  
```  
  
### <a name="d-using-group-by"></a>Г. Использование GROUP BY  
 В следующем примере вычисляется общий объем всех продаж за каждый день.  
  
```sql  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales  
FROM FactInternetSales  
GROUP BY OrderDateKey  
ORDER BY OrderDateKey;  
```  
  
 Так как в запросе используется предложение `GROUP BY`, то выводится только одна строка, содержащая общий объем продаж по каждому дню.  
  
### <a name="e-using-group-by-with-multiple-groups"></a>Д. Использование GROUP BY с несколькими группами  
 В следующем примере вычисляются значения средней цены и суммы продаж через Интернет за каждый день, сгруппированные по дате заказа и ключу продвижения.  
  
```sql  

SELECT OrderDateKey, PromotionKey, AVG(SalesAmount) AS AvgSales, SUM(SalesAmount) AS TotalSales  
FROM FactInternetSales  
GROUP BY OrderDateKey, PromotionKey  
ORDER BY OrderDateKey;   
```  
  
### <a name="f-using-group-by-and-where"></a>Е. Использование GROUP BY и WHERE  
 В следующем примере после извлечения строк, содержащих даты заказов позднее 1 августа 2002 г., происходит их разделение на группы.  
  
```sql  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales  
FROM FactInternetSales  
WHERE OrderDateKey > '20020801'  
GROUP BY OrderDateKey  
ORDER BY OrderDateKey;  
```  
  
### <a name="g-using-group-by-with-an-expression"></a>Ж. Использование GROUP BY с выражением  
 В следующем примере производится группировка с помощью выражения. Группировку можно производить только с помощью выражения, не содержащего агрегатных функций.  
  
```sql  
SELECT SUM(SalesAmount) AS TotalSales  
FROM FactInternetSales  
GROUP BY (OrderDateKey * 10);  
```  
  
### <a name="h-using-group-by-with-order-by"></a>З. Использование GROUP BY с ORDER BY  
 В следующем примере вычисляется сумма продаж за день и выполняется поиск заказов по определенному дню.  
  
```sql  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales  
FROM FactInternetSales  
GROUP BY OrderDateKey  
ORDER BY OrderDateKey;  
```  
  
### <a name="i-using-the-having-clause"></a>И. Использование предложения HAVING  
 Для ограничения результатов поиска в этом запросе используется предложение `HAVING`.  
  
```sql  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales  
FROM FactInternetSales  
GROUP BY OrderDateKey  
HAVING OrderDateKey > 20010000  
ORDER BY OrderDateKey;  
```  
  
## <a name="see-also"></a>См. также:  
 [Примеры использования инструкции SELECT (Transact-SQL)](../../t-sql/queries/select-examples-transact-sql.md)  
 [Указания (Transact-SQL)](../../t-sql/queries/hints-transact-sql.md)
  

