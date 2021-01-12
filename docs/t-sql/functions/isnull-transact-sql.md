---
description: Функция ISNULL (Transact-SQL)
title: Функция ISNULL (Transact-SQL)
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ISNULL
- ISNULL_TSQL
- IFNULL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- replacing null values
- null values [SQL Server], ISNULL
- null values [SQL Server], replacement values
- ISNULL function
ms.assetid: 6f3e5802-864b-4e77-9862-657bb5430b68
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1a3a5b4a1f22a84435c0658c3e0cfc8a14c6f086
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100474"
---
# <a name="isnull-transact-sql"></a>Функция ISNULL (Transact-SQL) 

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Заменяет значение NULL указанным замещающим значением.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
ISNULL ( check_expression , replacement_value )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *check_expression*  
 [Выражение](../../t-sql/language-elements/expressions-transact-sql.md), которое необходимо проверить на равенство значению NULL. Аргумент *check_expression* может быть любого типа.  
  
 *replacement_value*  
 Выражение, возвращаемое, если *check_expression* имеет значение NULL. Аргумент *replacement_value* должен иметь тип, который может быть неявно преобразован в тип *check_expression*.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 Возвращает тип, совпадающий с типом выражения *check_expression*. Если в аргументе *check_expression* предоставлено литеральное значение NULL, возвращает тип данных *replacement_value*. Если в аргументе *check_expression* предоставлено литеральное значение NULL, а аргумент *replacement_value* не задан, возвращает **int**.  
  
## <a name="remarks"></a>Remarks  
 Возвращается значение *check_expression*, если это выражение не равно NULL. В противном случае возвращается значение *replacement_value*. Если типы являются разными, то тип replacement_value неявно преобразуется в тип *check_expression*. Значение *replacement_value* может усекаться, если значение *replacement_value* длиннее, чем *check_expression*.  
  
> [!NOTE]  
>  Для возврата первого значения, отличного от NULL, используйте функцию [COALESCE (Transact-SQL)](../../t-sql/language-elements/coalesce-transact-sql.md).  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-isnull-with-avg"></a>A. Использование функции ISNULL с функцией AVG  
 Следующий пример демонстрирует расчет среднего значения веса всех продуктов. Все записи со значением NULL в столбце `50` таблицы `Weight` заменяются значением `Product`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT AVG(ISNULL(Weight, 50))  
FROM Production.Product;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 -------------------------- 
59.79  
  
 (1 row(s) affected)
 ```  
  
### <a name="b-using-isnull"></a>Б. Использование функции ISNULL  
 Следующий пример производит выборку описания, процента скидки, минимального и максимального количества для всех специальных предложений из базы `AdventureWorks2012`. Если максимальное количество для отдельного специального предложения равно NULL, отображаемое значение `MaxQty` в результирующем наборе заменяется на `0.00`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Description, DiscountPct, MinQty, ISNULL(MaxQty, 0.00) AS 'Max Quantity'  
FROM Sales.SpecialOffer;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|  Description       |  DiscountPct    |   MinQty    |   Максимальное количество       |
|  ---------------   |  -------------  |   --------  |   ---------------    |
|  Без скидки       |  0,00           |   0         |   0                  |
|  Оптовая скидка   |  0,02           |   11        |   14                 |
|  Оптовая скидка   |  0,05           |   15        |   4                  |
|  Оптовая скидка   |  0,10           |   25        |   0                  |
|  Оптовая скидка   |  0,15           |   41        |   0                  |
|  Оптовая скидка   |  0,20           |   61        |   0                  |
|  Mountain-100 Cl   |  0,35           |   0         |   0                  |
|  Sport Helmet Di   |  0,10           |   0         |   0                  |
|  Road-650 Overst   |  0,30           |   0         |   0                  |
|  Mountain Tire S   |  0,50           |   0         |   0                  |
|  Sport Helmet Di   |  0,15           |   0         |   0                  |
|  LL Road Frame S   |  0,35           |   0         |   0                  |
|  Touring-3000 Pr   |  0,15           |   0         |   0                  |
|  Touring-1000 Pr   |  0,20           |   0         |   0                  |
|  Half-Price Peda   |  0,50           |   0         |   0                  |
|  Mountain-500 Si   |  0,40           |   0         |   0                  |

 `(16 row(s) affected)`  
  
### <a name="c-testing-for-null-in-a-where-clause"></a>В. Проверка значений NULL в предложении WHERE  
 Не используйте для поиска значений NULL выражение ISNULL, вместо него следует использовать выражение IS NULL. В следующем примере выполняется поиск всех продуктов, имеющих значение `NULL` в столбце веса. Заметьте, что между словами `IS` и `NULL` стоит пробел.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Name, Weight  
FROM Production.Product  
WHERE Weight IS NULL;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-using-isnull-with-avg"></a>Г. Использование функции ISNULL с функцией AVG  
 В приведенном ниже примере рассчитывается среднее значение веса всех продуктов в образце таблицы. Все записи со значением NULL в столбце `50` таблицы `Weight` заменяются значением `Product`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT AVG(ISNULL(Weight, 50))  
FROM dbo.DimProduct;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
--------------------------   
52.88   
```  
  
### <a name="e-using-isnull"></a>Д. Использование функции ISNULL  
 В приведенном ниже примере функция ISNULL используется для поиска значений NULL в столбце `MinPaymentAmount` и отображения значения `0.00` для соответствующих строк.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ResellerName,   
       ISNULL(MinPaymentAmount,0) AS MinimumPayment  
FROM dbo.DimReseller  
ORDER BY ResellerName;  
  
```  
  
 Здесь приводится частичный результирующий набор.  
  
|  ResellerName                |  MinimumPayment    |
|  -------------------------   |  --------------    |
|  A Bicycle Association       |     0,0000         |
|  A Bike Store                |     0,0000         |
|  A Cycle Shop                |     0,0000         |
|  A Great Bicycle Company     |     0,0000         |
|  A Typical Bike Shop         |   200,0000         |
|  Acceptable Sales & Service  |     0,0000         |
  
### <a name="f-using-is-null-to-test-for-null-in-a-where-clause"></a>Е. Использование функции IS NULL для проверки на значение NULL в предложении WHERE  
 В приведенном ниже примере выполняется поиск всех продуктов, имеющих значение `NULL` в столбце `Weight`. Заметьте, что между словами `IS` и `NULL` стоит пробел.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT EnglishProductName, Weight  
FROM dbo.DimProduct  
WHERE Weight IS NULL;  
```  
  
## <a name="see-also"></a>См. также:  
 [Выражения (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [IS NULL (Transact-SQL)](../../t-sql/queries/is-null-transact-sql.md)   
 [Системные функции (Transact-SQL)](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)   
 [COALESCE (Transact-SQL)](../../t-sql/language-elements/coalesce-transact-sql.md)  
  
  

