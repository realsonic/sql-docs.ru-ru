---
description: Оператор BETWEEN (Transact-SQL)
title: BETWEEN (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 08/28/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- BETWEEN
- BETWEEN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- inclusive ranges
- testing range
- exclusive ranges
- NOT BETWEEN operator
- BETWEEN operator
- range to test [SQL Server]
ms.assetid: a5d5b050-203e-4355-ac85-e08ef5ca7823
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3d41c356b20dd0bb0577f10ee0abc47681ed38e7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096021"
---
# <a name="between-transact-sql"></a>Оператор BETWEEN (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Определяет диапазон для проверки.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
test_expression [ NOT ] BETWEEN begin_expression AND end_expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *test_expression*  
 [Выражение](../../t-sql/language-elements/expressions-transact-sql.md) для проверки на принадлежность диапазону в пределах от *begin_expression* до *end_expression*. Выражение *test_expression* должно иметь тот же тип данных, что и *begin_expression* и *end_expression*.  
  
 NOT  
 Указывает, что результат предиката должен быть инвертирован.  
  
 *begin_expression*  
 Любое допустимое выражение. Выражение *begin_expression* должно иметь тот же тип данных, что и *test_expression* и *end_expression*.  
  
 *end_expression*  
 Любое допустимое выражение. Выражение *end_expression* должно иметь тот же тип данных, что и *test_expression* и *begin_expression*.  
  
 AND  
 Служит заполнителем, который указывает на то, что значение *test_expression* должно находиться в диапазоне от *begin_expression* до *end_expression*.  
  
## <a name="result-types"></a>Типы результата  
 **Boolean**  
  
## <a name="result-value"></a>Значение результата  
 Оператор BETWEEN возвращает значение **TRUE**, если значение аргумента *test_expression* больше значения аргумента *begin_expression* или равно ему и меньше значения аргумента *end_expression* или равно ему.  
  
 Оператор NOT BETWEEN возвращает значение **TRUE**, если значение аргумента *test_expression* меньше значения аргумента *begin_expression* или больше значения аргумента *end_expression*.  
  
## <a name="remarks"></a>Комментарии  
 Для задания исключающего диапазона используйте операторы "больше" (>) и "меньше" (<). Если любой параметр предиката BETWEEN или NOT BETWEEN имеет значение NULL, результат не определен (UNKNOWN).  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-between"></a>A. Использование оператора BETWEEN  
 В приведенном ниже примере возвращаются сведения о ролях базы данных. Первый запрос возвращает все роли. Во втором примере с помощью предложения `BETWEEN` роли ограничиваются указанными значениями `database_id`.  
  
```sql  
SELECT principal_id, name 
FROM sys.database_principals
WHERE type = 'R';

SELECT principal_id, name 
FROM sys.database_principals
WHERE type = 'R'
AND principal_id BETWEEN 16385 AND 16390;
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]   
```  
principal_id    name
------------  ---- 
0               public
16384           db_owner
16385           db_accessadmin
16386           db_securityadmin
16387           db_ddladmin
16389           db_backupoperator
16390           db_datareader
16391           db_datawriter
16392           db_denydatareader
16393           db_denydatawriter
```  
```  
principal_id    name
------------  ---- 
16385           db_accessadmin
16386           db_securityadmin
16387           db_ddladmin
16389           db_backupoperator
16390           db_datareader
```  
  
### <a name="b-using--and--instead-of-between"></a>Б. Использование операторов > и < вместо BETWEEN  
 В следующем примере используются операторы «больше» (`>`) и «меньше» (`<`); так как они позволяют задавать исключающий диапазон, здесь выводятся только девять строк вместо десяти из предыдущего примера.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT e.FirstName, e.LastName, ep.Rate  
FROM HumanResources.vEmployee e   
JOIN HumanResources.EmployeePayHistory ep   
    ON e.BusinessEntityID = ep.BusinessEntityID  
WHERE ep.Rate > 27 AND ep.Rate < 30  
ORDER BY ep.Rate;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```  
 FirstName   LastName             Rate  
 ---------   -------------------  ---------  
 Paula       Barreto de Mattos    27.1394  
 Janaina     Bueno                27.4038  
 Dan         Bacon                27.4038  
 Ramesh      Meyyappan            27.4038  
 Karen       Berg                 27.4038  
 David       Bradley              28.7500  
 Hazem       Abolrous             28.8462  
 Ovidiu      Cracium              28.8462  
 Rob         Walters              29.8462  
 ```    
  
### <a name="c-using-not-between"></a>В. Использование оператора NOT BETWEEN  
 В следующем примере выводятся все строки вне указанного диапазона от `27` до `30`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT e.FirstName, e.LastName, ep.Rate  
FROM HumanResources.vEmployee e   
JOIN HumanResources.EmployeePayHistory ep   
    ON e.BusinessEntityID = ep.BusinessEntityID  
WHERE ep.Rate NOT BETWEEN 27 AND 30  
ORDER BY ep.Rate;  
GO  
```  
  
### <a name="d-using-between-with-datetime-values"></a>Г. Использование оператора BETWEEN со значениями типа datetime  
 В приведенном ниже примере возвращаются строки, в которых значения типа **datetime** находятся между `'20011212'` и `'20020105'` включительно.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT BusinessEntityID, RateChangeDate  
FROM HumanResources.EmployeePayHistory  
WHERE RateChangeDate BETWEEN '20011212' AND '20020105';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```  
 BusinessEntityID RateChangeDate  
 ----------- -----------------------  
 3           2001-12-12 00:00:00.000  
 4           2002-01-05 00:00:00.000  
 ```  
 
 Запрос извлекает ожидаемые строки, так как значения даты в запросе и значения типа **datetime**, хранящиеся в столбце `RateChangeDate`, были заданы без указания времени. Если время не указано, по умолчанию оно принимается равным 0:00. Обратите внимание, что строка, время в которой позднее 12:00. 05.01.2002, не будет возвращена данным запросом, так как она находится за пределами диапазона.  
  
  
## <a name="see-also"></a>См. также  
 [> (больше) (Transact-SQL)](../../t-sql/language-elements/greater-than-transact-sql.md)   
 [&#60; (меньше) (Transact-SQL)](../../t-sql/language-elements/less-than-transact-sql.md)   
 [Выражения (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Встроенные функции (Transact-SQL)](~/t-sql/functions/functions.md)   
 [Операторы (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)  
  
  


