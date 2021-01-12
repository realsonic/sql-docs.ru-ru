---
description: ALL (Transact-SQL)
title: ALL (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALL_TSQL
- ALL
dev_langs:
- TSQL
helpviewer_keywords:
- single-column set of values [SQL Server]
- ALL (Transact-SQL)
ms.assetid: 4b0c002e-1ffd-4425-a980-11fdc1f24af7
author: cawrites
ms.author: chadam
ms.openlocfilehash: b531df59f8350058d88afd0d974aa873d88b7520
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101059"
---
# <a name="all-transact-sql"></a>ALL (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Сравнивает скалярное значение с набором значений, состоящим из одного столбца.  
  
 ![Значок ссылки на статью](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на статью") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
scalar_expression { = | <> | != | > | >= | !> | < | <= | !< } ALL ( subquery )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *scalar_expression*  
 Любое допустимое выражение [expression](../../t-sql/language-elements/expressions-transact-sql.md).  
  
 { = \| <> \| != \| > \| >= \| !> \| < \| <= \| !< }  
 Оператор сравнения.  
  
 *subquery*  
 Вложенный запрос, возвращающий результирующий набор, состоящий из одного столбца. Тип данных возвращаемого столбца должен совпадать с типом данных аргумента *scalar_expression*.  
  
 Ограниченная инструкция SELECT, в которой запрещено предложение ORDER BY, а также ключевое слово INTO.  
  
## <a name="result-types"></a>Типы результата  
 **Boolean**  
  
## <a name="result-value"></a>Значение результата  
 Возвращает TRUE, если заданное сравнение возвращает TRUE для всех пар (_scalar_expression_ **,** _x)_ , где *x* является значением из набора строк, состоящего из одного столбца. В противном случае возвращает значение FALSE.  
  
## <a name="remarks"></a>Remarks  
 Если аргумент *scalar_expression* установлен в значение ALL, будет выполнено сравнение каждого значения, возвращаемого вложенным запросом. Например, если вложенный запрос возвращает значения 2 и 3, то при *scalar_expression* <= ALL будет возвращаться TRUE для значения *scalar_expression*, равного 2. Если вложенный запрос возвращает значения 2 и 3, то при *scalar_expression* = ALL (subquery) будет возвращаться FALSE, так как некоторые значения вложенного запроса (значение 3) могут не отвечать критериям этого выражения.  
  
 Инструкции, которым необходим аргумент *scalar_expression* для сравнения каждого значения, возвращенного вложенным запросом, перечислены в разделе [SOME &#124; ANY (Transact-SQL)](../../t-sql/language-elements/some-any-transact-sql.md).  
  
 В этой статье приводятся ссылки на выражение ALL при его использовании совместно с вложенными запросами. ALL может также использоваться с инструкциями [UNION](../../t-sql/language-elements/set-operators-union-transact-sql.md) и [SELECT](../../t-sql/queries/select-transact-sql.md).  
  
## <a name="examples"></a>Примеры  
 В приведенном ниже примере показано создание хранимой процедуры, определяющей, могут ли в течение заданного количества дней быть выполнены все части заказа с указанным идентификатором `SalesOrderID` из базы данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. В этом примере вложенный запрос используется для создания списка числовых значений `DaysToManufacture` для всех компонентов с указанным идентификатором `SalesOrderID`, после чего происходит проверка того, что время производства всех заказов (`DaysToManufacture`) находится в пределах указанного числа дней.  
  
```sql  
-- Uses AdventureWorks  
  
CREATE PROCEDURE DaysToBuild @OrderID INT, @NumberOfDays INT  
AS  
IF   
@NumberOfDays >= ALL  
   (  
    SELECT DaysToManufacture  
    FROM Sales.SalesOrderDetail  
    JOIN Production.Product   
    ON Sales.SalesOrderDetail.ProductID = Production.Product.ProductID   
    WHERE SalesOrderID = @OrderID  
   )  
PRINT 'All items for this order can be manufactured in specified number of days or less.'  
ELSE   
PRINT 'Some items for this order can''t be manufactured in specified number of days or less.' ;  
```  
  
 Для проверки этой процедуры выполните ее, используя заказ `SalesOrderID 49080`. В него входит один компонент, производство которого занимает `2` дня, и два компонента, производство которых занимает 0 дней. Первая из нижеследующих инструкций отвечает этим критериям. Второй запрос — нет.  
  
```sql  
EXECUTE DaysToBuild 49080, 2 ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `All items for this order can be manufactured in specified number of days or less.`  
  
```sql  
EXECUTE DaysToBuild 49080, 1 ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `Some items for this order can't be manufactured in specified number of days or less.`  
  
## <a name="see-also"></a>См. также:  
 [CASE (Transact-SQL)](../../t-sql/language-elements/case-transact-sql.md)   
 [Выражения (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Встроенные функции (Transact-SQL)](~/t-sql/functions/functions.md)   
 [LIKE (Transact-SQL)](../../t-sql/language-elements/like-transact-sql.md)   
 [Операторы (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)   
 [IN (Transact-SQL)](../../t-sql/language-elements/in-transact-sql.md)  
  
