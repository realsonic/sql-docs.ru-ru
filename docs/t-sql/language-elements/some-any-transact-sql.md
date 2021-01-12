---
description: SOME | ANY (Transact-SQL)
title: SOME | ANY (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SOME
- SOME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- scalar values
- comparing scalar with single-column set
- ANY operator
- SOME | ANY keyword
- single-column set of values [SQL Server]
ms.assetid: 1f717ad6-f67b-4980-9397-577ecb0e5789
author: cawrites
ms.author: chadam
ms.openlocfilehash: 42df6fc94ca0ca62b98156db73f3394d1850219f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095945"
---
# <a name="some--any-transact-sql"></a>SOME | ANY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Сравнивает скалярное значение с набором значений, состоящим из одного столбца. Ключевые слова SOME и ANY эквивалентны.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
scalar_expression { = | < > | ! = | > | > = | ! > | < | < = | ! < }   
     { SOME | ANY } ( subquery )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *scalar_expression*  
 Любое допустимое выражение [expression](../../t-sql/language-elements/expressions-transact-sql.md).  
  
 { = \| <> \| != \| > \| >= \| !> \| < \| <= \| !< }  
 Любой допустимый оператор сравнения.  
  
 SOME | ANY  
 Указывает на необходимость сравнения.  
  
 *subquery*  
 Вложенный запрос, содержащий результирующий набор, состоящий из одного столбца. Тип данных возвращаемого столбца должен совпадать с типом данных аргумента *scalar_expression*.  
  
## <a name="result-types"></a>Типы результата  
 **Boolean**  
  
## <a name="result-value"></a>Значение результата  
 При использовании ключевого слова SOME или ANY значение **TRUE** возвращается, если указанное сравнение имеет значение TRUE для любой пары (_scalar_expression_ **,** _x_), где *x* является одним из значений набора из одного столбца; иначе возвращается значение **FALSE**.  
  
## <a name="remarks"></a>Remarks  
 Для ключевого слова SOME необходим аргумент *scalar_expression*, чтобы провести непосредственное сравнение по крайней мере одного значения, возвращенного вложенным запросом. Инструкции, которым необходим аргумент *scalar_expression* для сравнения каждого значения, возвращенного вложенным запросом, перечислены в разделе [ALL (Transact-SQL)](../../t-sql/language-elements/all-transact-sql.md). Например, если вложенный запрос возвращает значения 2 и 3, то при значении *scalar_expression* = SOME (subquery) для выражения *scalar_express*, равного 2, будет возвращаться TRUE. Если вложенный запрос возвращает значения 2 и 3, то при *scalar_expression* = ALL (subquery) будет возвращаться FALSE, так как некоторые значения вложенного запроса (значение 3) могут не отвечать критериям этого выражения.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-running-a-simple-example"></a>A. Выполнение простого примера  
 Следующие инструкции создают простую таблицу и добавляют значения `1`, `2`, `3` и `4` в столбец `ID`.  
  
```sql  
CREATE TABLE T1  
(ID INT) ;  
GO  
INSERT T1 VALUES (1) ;  
INSERT T1 VALUES (2) ;  
INSERT T1 VALUES (3) ;  
INSERT T1 VALUES (4) ;  
```  
  
 Следующий запрос возвращает значение `TRUE`, так как `3` меньше некоторых из значений в таблице.  
  
```sql  
IF 3 < SOME (SELECT ID FROM T1)  
PRINT 'TRUE'   
ELSE  
PRINT 'FALSE' ;  
```  
  
 Следующий запрос возвращает значение `FALSE`, так как `3` не меньше каждого из значений в таблице.  
  
```sql  
IF 3 < ALL (SELECT ID FROM T1)  
PRINT 'TRUE'   
ELSE  
PRINT 'FALSE' ;  
```  
  
### <a name="b-running-a-practical-example"></a>Б. Выполнение практического примера  
 В приведенном ниже примере показано создание хранимой процедуры, определяющей, могут ли в течение заданного количества дней быть выполнены все части заказа с указанным идентификатором `SalesOrderID` из базы данных `AdventureWorks2012`. В этом примере для создания списка количества значений `DaysToManufacture` для всех компонентов `SalesOrderID` используется вложенный запрос, а затем проводится проверка того, превышают ли все значения, возвращаемые вложенным запросом, указанное количество дней. Если каждое возвращаемое значение `DaysToManufacture` меньше заданного значения, то условие равно TRUE и печатается первое сообщение.  
  
```sql  
-- Uses AdventureWorks  
  
CREATE PROCEDURE ManyDaysToComplete @OrderID INT, @NumberOfDays INT  
AS  
IF   
@NumberOfDays < SOME  
   (  
    SELECT DaysToManufacture  
    FROM Sales.SalesOrderDetail  
    JOIN Production.Product   
    ON Sales.SalesOrderDetail.ProductID = Production.Product.ProductID   
    WHERE SalesOrderID = @OrderID  
   )  
PRINT 'At least one item for this order can't be manufactured in specified number of days.'  
ELSE   
PRINT 'All items for this order can be manufactured in the specified number of days or less.' ;  
  
```  
  
 Для проверки этой процедуры выполните ее, используя `SalesOrderID``49080`, имеющий один компонент, требующий на выполнение `2` дня, и два компонента, требующих немедленного выполнения. Первая инструкция отвечает этим критериям. Второй запрос — нет.  
  
```sql  
EXECUTE ManyDaysToComplete 49080, 2 ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `All items for this order can be manufactured in the specified number of days or less.`  
  
```  
EXECUTE ManyDaysToComplete 49080, 1 ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `At least one item for this order can't be manufactured in specified number of days.`  
  
## <a name="see-also"></a>См. также:  
 [ALL (Transact-SQL)](../../t-sql/language-elements/all-transact-sql.md)   
 [CASE (Transact-SQL)](../../t-sql/language-elements/case-transact-sql.md)   
 [Встроенные функции (Transact-SQL)](~/t-sql/functions/functions.md)   
 [Операторы (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)   
 [IN (Transact-SQL)](../../t-sql/language-elements/in-transact-sql.md)  
  
