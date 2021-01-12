---
title: DATEADD (Transact-SQL) | Документы Майкрософт
description: Справочник по Transact-SQL для функции DATEADD. Эта функция возвращает дату, которая была изменена указанной частью даты.
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATEADD
- DATEADD_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- add interval to date or time [SQL Server]
- subtract interval from date or time [SQL Server]
- functions [SQL Server], time
- functions [SQL Server], date and time
- time [SQL Server], functions
- dates [SQL Server], intervals
- date and time [SQL Server], DATEADD
- DATEADD function [SQL Server]
ms.assetid: 89c5ae32-89c6-47e1-979e-15d97908b9f1
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4edc9ff57e9f8d1bd875df6d62814b3317c7ac45
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093687"
---
# <a name="dateadd-transact-sql"></a>DATEADD (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Эта функция добавляет указанное значение *number* (целое число со знаком) к заданному аргументу *datepart* входного значения *date*, а затем возвращает это измененное значение.
  
Обзор всех типов данных и функций даты и времени в языке [!INCLUDE[tsql](../../includes/tsql-md.md)] см. в статье [Типы данных и функции даты и времени (Transact-SQL)](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```sql
DATEADD (datepart , number , date )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
*datepart*  
Компонент даты *date*, к которому `DATEADD` добавляет **целое** *число*. В приведенной ниже таблице перечислены все допустимые аргументы *datepart*. 

> [!NOTE]
> `DATEADD` не принимает эквивалентные переменные, определяемые пользователем, для аргументов *datepart*. 
  
|*datepart*|Сокращения|  
|---|---|
|**year**|**yy**, **yyyy**|  
|**quarter**|**qq**, **q**|  
|**month**|**mm**, **m**|  
|**dayofyear**|**dy**, **y**|  
|**day**|**dd**, **d**|  
|**week**|**wk**, **ww**|  
|**weekday**|**dw**, **w**|  
|**hour**|**hh**|  
|**minute**|**mi**, **n**|  
|**second**|**ss**, **s**|  
|**millisecond**|**ms**|  
|**microsecond**|**mcs**|  
|**nanosecond**|**ns**|  
  
*number*  
Выражение, которое разрешается в тип [int](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md), добавляемый `DATEADD` к компоненту *datepart* даты *date*. `DATEADD` принимает определяемые пользователем значения переменных для *number*. `DATEADD` усечет указанное значение *number*, имеющее десятичную дробь. В этой ситуации значение *number* не округляется.
  
*date*  
Выражение, которое может быть разрешено в одно из следующих значений. 

+ **date**
+ **datetime**
+ **datetimeoffset**
+ **datetime2** 
+ **smalldatetime**
+ **time**

Для *date*`DATEADD` будет принимать столбец выражения, выражение, строковый литерал или определяемую пользователем переменную. Значение строкового литерала должно разрешаться в **datetime**. Во избежание неоднозначности используйте четырехзначную запись года. Сведения о двузначном обозначении года см. в статье [Настройка параметра конфигурации сервера two digit year cutoff](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md).
  
## <a name="return-types"></a>Типы возвращаемых данных

Тип данных возвращаемого значения для этого метода является динамическим. Тип возвращаемого значения зависит от типа аргумента, переданного в `date`. Если значение для `date` является строковым литералом даты, `DATEADD` возвращает значение **datetime**. Если для `date` предоставляется другой тип допустимых входных данных, `DATEADD` возвращает тот же тип данных. Если строковый литерал имеет более трех позиций долей секунды (.nnn) или если строковый литерал содержит часть смещения часового пояса, `DATEADD` выдаст ошибку.
  
## <a name="return-value"></a>Возвращаемое значение  
  
## <a name="datepart-argument"></a>Аргумент datepart  
Функции **dayofyear**, **day** и **weekday** возвращают одинаковое значение.
  
Каждое выражение *datepart* и его краткие формы возвращают одно и то же значение.
  
Если верны следующие условия:

+ *datepart* имеет значение **month**;
+ в месяце *date* больше дней, чем в возвращаемом месяце;
+ день *date* не существует в этом месяце;

то `DATEADD` возвращает последний день возвращаемого месяца. Например, в сентябре 30 (тридцать) дней, поэтому эти инструкции возвращают 2006-09-30 00:00:00.000:
  
```sql
SELECT DATEADD(month, 1, '20060830');
SELECT DATEADD(month, 1, '20060831');
```
  
## <a name="number-argument"></a>Аргумент number  
Аргумент *number* не может выходить за диапазон типа данных **int**. В приведенных ниже инструкциях аргумент *number* превышает диапазон типа данных **int** на 1. Обе эти инструкции возвращают сообщение об ошибке: "`Msg 8115, Level 16, State 2, Line 1. Arithmetic overflow error converting expression to data type int."`
  
```sql
SELECT DATEADD(year,2147483648, '20060731');  
SELECT DATEADD(year,-2147483649, '20060731');  
```  
  
## <a name="date-argument"></a>Аргумент date  
`DATEADD` не будет принимать аргумент *date*, увеличенный до значения, выходящего за диапазон соответствующего типа данных. В приведенных ниже инструкциях значение *number*, добавленное к значению *date*, превышает диапазон типа данных *date*. `DATEADD` возвращает следующее сообщение об ошибке: "`Msg 517, Level 16, State 1, Line 1 Adding a value to a 'datetime' column caused overflow`".
  
```sql
SELECT DATEADD(year,2147483647, '20060731');  
SELECT DATEADD(year,-2147483647, '20060731');  
```  
  
## <a name="return-values-for-a-smalldatetime-date-and-a-second-or-fractional-seconds-datepart"></a>Возвращаемые значения дат с типом данных smalldatetime и частью даты в виде секунд или долей секунды.  
Значение секунд даты типа [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md) всегда равно 00. Для значения **date** типа *smalldatetime* действуют указанные ниже условия. 

-   Для части даты *datepart* секунды **second** и значения *number* в диапазоне от –30 до +29 `DATEADD` не вносит никаких изменений.  
-   Для части даты *datepart* секунды **second** и значения *number* меньше –30 или больше +29 `DATEADD` выполняет добавление, начиная с одной минуты.  
-   Для части даты *datepart* миллисекунды **millisecond** и значения *number* в диапазоне от –30 001 до + 29 998 `DATEADD` не вносит никаких изменений.  
-   Для части даты *datepart* миллисекунды **millisecond** и значения *number* меньше –30 001 или больше +29 998 `DATEADD` выполняет добавление, начиная с одной минуты.  
  
## <a name="remarks"></a>Remarks  
Используйте `DATEADD` в следующих предложениях.

+ GROUP BY
+ HAVING
+ ORDER BY
+ SELECT \<list>
+ WHERE
  
## <a name="fractional-seconds-precision"></a>Точность в долях секунды
`DATEADD` не допускает использование при сложении в качестве аргумента *datepart* значений **microsecond** или **nanosecond** для типов данных *date*: **smalldatetime**, **date** и **datetime**.
  
Миллисекунды имеют точность 3 знака (0,123), микросекунды — 6 знаков (0,123456), наносекунды — 9 знаков (0,123456789). Типы данных **time**, **datetime2** и **datetimeoffset** имеют максимальную точность 7 знаков (0,1234567). Если аргументом *datepart* является **nanosecond**, аргумент *number* должен иметь значение 100 перед увеличением даты *date* на доли секунды. *number* от 1 до 49 округляется до 0, а number от 50 до 99 округляется до 100.
  
Эти инструкции добавляют часть даты *datepart*: **millisecond**, **microsecond** или **nanosecond**.
  
```sql
DECLARE @datetime2 datetime2 = '2007-01-01 13:10:10.1111111';  
SELECT '1 millisecond', DATEADD(millisecond,1,@datetime2)  
UNION ALL  
SELECT '2 milliseconds', DATEADD(millisecond,2,@datetime2)  
UNION ALL  
SELECT '1 microsecond', DATEADD(microsecond,1,@datetime2)  
UNION ALL  
SELECT '2 microseconds', DATEADD(microsecond,2,@datetime2)  
UNION ALL  
SELECT '49 nanoseconds', DATEADD(nanosecond,49,@datetime2)  
UNION ALL  
SELECT '50 nanoseconds', DATEADD(nanosecond,50,@datetime2)  
UNION ALL  
SELECT '150 nanoseconds', DATEADD(nanosecond,150,@datetime2);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
1 millisecond     2007-01-01 13:10:10.1121111  
2 milliseconds    2007-01-01 13:10:10.1131111  
1 microsecond     2007-01-01 13:10:10.1111121  
2 microseconds    2007-01-01 13:10:10.1111131  
49 nanoseconds    2007-01-01 13:10:10.1111111  
50 nanoseconds    2007-01-01 13:10:10.1111112  
150 nanoseconds   2007-01-01 13:10:10.1111113  
```  
  
## <a name="time-zone-offset"></a>Смещение часового пояса
`DATEADD` не допускает добавление для смещения часового пояса.
  
## <a name="examples"></a>Примеры  

### <a name="a-incrementing-datepart-by-an-interval-of-1"></a>A. Увеличение части даты на интервал, равный 1  
Каждая из этих инструкций увеличивает часть даты *datepart* на интервал, равный 1.
  
```sql
DECLARE @datetime2 datetime2 = '2007-01-01 13:10:10.1111111';  
SELECT 'year', DATEADD(year,1,@datetime2)  
UNION ALL  
SELECT 'quarter',DATEADD(quarter,1,@datetime2)  
UNION ALL  
SELECT 'month',DATEADD(month,1,@datetime2)  
UNION ALL  
SELECT 'dayofyear',DATEADD(dayofyear,1,@datetime2)  
UNION ALL  
SELECT 'day',DATEADD(day,1,@datetime2)  
UNION ALL  
SELECT 'week',DATEADD(week,1,@datetime2)  
UNION ALL  
SELECT 'weekday',DATEADD(weekday,1,@datetime2)  
UNION ALL  
SELECT 'hour',DATEADD(hour,1,@datetime2)  
UNION ALL  
SELECT 'minute',DATEADD(minute,1,@datetime2)  
UNION ALL  
SELECT 'second',DATEADD(second,1,@datetime2)  
UNION ALL  
SELECT 'millisecond',DATEADD(millisecond,1,@datetime2)  
UNION ALL  
SELECT 'microsecond',DATEADD(microsecond,1,@datetime2)  
UNION ALL  
SELECT 'nanosecond',DATEADD(nanosecond,1,@datetime2);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Year         2008-01-01 13:10:10.1111111  
quarter      2007-04-01 13:10:10.1111111  
month        2007-02-01 13:10:10.1111111  
dayofyear    2007-01-02 13:10:10.1111111  
day          2007-01-02 13:10:10.1111111  
week         2007-01-08 13:10:10.1111111  
weekday      2007-01-02 13:10:10.1111111  
hour         2007-01-01 14:10:10.1111111  
minute       2007-01-01 13:11:10.1111111  
second       2007-01-01 13:10:11.1111111  
millisecond  2007-01-01 13:10:10.1121111  
microsecond  2007-01-01 13:10:10.1111121  
nanosecond   2007-01-01 13:10:10.1111111  
```  
  
### <a name="b-incrementing-more-than-one-level-of-datepart-in-one-statement"></a>Б. Увеличение нескольких уровней части даты в одной инструкции  
Каждая из этих инструкций увеличивает часть даты *datepart* на число *number*, достаточно большое, чтобы также увеличить следующую часть *datepart* даты *date*.
  
```sql
DECLARE @datetime2 datetime2;  
SET @datetime2 = '2007-01-01 01:01:01.1111111';  
--Statement                                 Result     
-------------------------------------------------------------------   
SELECT DATEADD(quarter,4,@datetime2);     --2008-01-01 01:01:01.1111111  
SELECT DATEADD(month,13,@datetime2);      --2008-02-01 01:01:01.1111111  
SELECT DATEADD(dayofyear,365,@datetime2); --2008-01-01 01:01:01.1111111  
SELECT DATEADD(day,365,@datetime2);       --2008-01-01 01:01:01.1111111  
SELECT DATEADD(week,5,@datetime2);        --2007-02-05 01:01:01.1111111  
SELECT DATEADD(weekday,31,@datetime2);    --2007-02-01 01:01:01.1111111  
SELECT DATEADD(hour,23,@datetime2);       --2007-01-02 00:01:01.1111111  
SELECT DATEADD(minute,59,@datetime2);     --2007-01-01 02:00:01.1111111  
SELECT DATEADD(second,59,@datetime2);     --2007-01-01 01:02:00.1111111  
SELECT DATEADD(millisecond,1,@datetime2); --2007-01-01 01:01:01.1121111  
```  
  
### <a name="c-using-expressions-as-arguments-for-the-number-and-date-parameters"></a>В. Использование выражений в качестве аргументов number и date  
В этих примерах выражения различного типа используются в качестве аргументов для параметров *number* и *date*. В примерах используется база данных AdventureWorks.
  
#### <a name="specifying-a-column-as-date"></a>Указание столбца в качестве аргумента date  
В этом примере к каждому значению в столбце `2` добавляется `OrderDate` (два) дня, чтобы получить новый столбец с именем `PromisedShipDate`.
  
```sql
SELECT SalesOrderID  
    ,OrderDate   
    ,DATEADD(day,2,OrderDate) AS PromisedShipDate  
FROM Sales.SalesOrderHeader;  
```  
  
Частичный результирующий набор имеет следующий вид:
  
```
SalesOrderID OrderDate               PromisedShipDate  
------------ ----------------------- -----------------------  
43659        2005-07-01 00:00:00.000 2005-07-03 00:00:00.000  
43660        2005-07-01 00:00:00.000 2005-07-03 00:00:00.000  
43661        2005-07-01 00:00:00.000 2005-07-03 00:00:00.000  
...  
43702        2005-07-02 00:00:00.000 2005-07-04 00:00:00.000  
43703        2005-07-02 00:00:00.000 2005-07-04 00:00:00.000  
43704        2005-07-02 00:00:00.000 2005-07-04 00:00:00.000  
43705        2005-07-02 00:00:00.000 2005-07-04 00:00:00.000  
43706        2005-07-03 00:00:00.000 2005-07-05 00:00:00.000  
...  
43711        2005-07-04 00:00:00.000 2005-07-06 00:00:00.000  
43712        2005-07-04 00:00:00.000 2005-07-06 00:00:00.000  
...  
43740        2005-07-11 00:00:00.000 2005-07-13 00:00:00.000  
43741        2005-07-12 00:00:00.000 2005-07-14 00:00:00.000  
  
```  
  
#### <a name="specifying-user-defined-variables-as-number-and-date"></a>Указание пользовательских переменных в качестве аргументов number и date  
В этом примере в качестве аргументов *number* и *date* указываются пользовательские переменные.
  
```sql
DECLARE @days INT = 365,   
        @datetime DATETIME = '2000-01-01 01:01:01.111'; /* 2000 was a leap year */;  
SELECT DATEADD(day, @days, @datetime);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------------------  
2000-12-31 01:01:01.110  
  
(1 row(s) affected)  
```  
  
#### <a name="specifying-scalar-system-function-as-date"></a>Указание в качестве аргумента date скалярной системной функции  
В этом примере для аргумента *date* указано значение `SYSDATETIME`. Точное возвращаемое значение зависит от дня и времени выполнения инструкции.
  
```sql
SELECT DATEADD(month, 1, SYSDATETIME());  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
---------------------------  
2013-02-06 14:29:59.6727944  
  
(1 row(s) affected)  
```  
  
#### <a name="specifying-scalar-subqueries-and-scalar-functions-as-number-and-date"></a>Указание в качестве аргументов number и date скалярных вложенных запросов и скалярных функций  
В этом примере в качестве аргументов для *number* и *date* используются скалярные вложенные запросы `MAX(ModifiedDate)`. `(SELECT TOP 1 BusinessEntityID FROM Person.Person)` является искусственным аргументом для числового параметра, показывающим способ выбора аргумента *number* из списка значений.
  
```sql
SELECT DATEADD(month,(SELECT TOP 1 BusinessEntityID FROM Person.Person),  
    (SELECT MAX(ModifiedDate) FROM Person.Person));  
```  
  
#### <a name="specifying-numeric-expressions-and-scalar-system-functions-as-number-and-date"></a>Указание в качестве аргументов number и date числовых выражений и скалярных системных функций  
В этом примере в качестве аргументов *number* и *date* используется числовое выражение (–`(10/2))`, [унарные операторы](../../mdx/unary-operators.md) (`-`), [арифметический оператор](../../mdx/arithmetic-operators.md) (`/`) и скалярные системные функции (`SYSDATETIME`).
  
```sql
SELECT DATEADD(month,-(10/2), SYSDATETIME());  
```  
  
#### <a name="specifying-ranking-functions-as-number"></a>Указание в качестве аргумента number ранжирующих функций  
В этом примере в качестве аргумента *number* используется ранжирующая функция.
  
```sql
SELECT p.FirstName, p.LastName  
    ,DATEADD(day,ROW_NUMBER() OVER (ORDER BY  
        a.PostalCode),SYSDATETIME()) AS 'Row Number'  
FROM Sales.SalesPerson AS s   
    INNER JOIN Person.Person AS p   
        ON s.BusinessEntityID = p.BusinessEntityID  
    INNER JOIN Person.Address AS a   
        ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL   
    AND SalesYTD <> 0;  
```  
  
#### <a name="specifying-an-aggregate-window-function-as-number"></a>Указание в качестве аргумента number статистической оконной функции  
В этом примере в качестве аргумента *number* используется агрегатная оконная функция.
  
```sql
SELECT SalesOrderID, ProductID, OrderQty  
    ,DATEADD(day,SUM(OrderQty)   
        OVER(PARTITION BY SalesOrderID),SYSDATETIME()) AS 'Total'  
FROM Sales.SalesOrderDetail   
WHERE SalesOrderID IN(43659,43664);  
GO  
```  
  
  
## <a name="see-also"></a>См. также раздел
[Функции CAST и CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  

