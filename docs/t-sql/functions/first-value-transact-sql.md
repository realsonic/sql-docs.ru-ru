---
description: FIRST_VALUE (Transact-SQL)
title: FIRST_VALUE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 09/22/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FIRST_VALUE_TSQL
- FIRST_VALUE
dev_langs:
- TSQL
helpviewer_keywords:
- FIRST_VALUE function
- analytic functions, FIRST_VALUE
ms.assetid: 1990c3c7-dad2-48db-b2cd-3e8bd2c49d17
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e8c38ffc45933da64b9921a035e956d337f4e1f4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101199"
---
# <a name="first_value-transact-sql"></a>FIRST_VALUE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает первое значение в упорядоченном наборе значений.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
FIRST_VALUE ( [scalar_expression ] )  [ IGNORE NULLS | RESPECT NULLS ]
    OVER ( [ partition_by_clause ] order_by_clause [ rows_range_clause ] )
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *scalar_expression*  
 Возвращаемое значение. *scalar_expression* может быть столбцом, вложенным запросом или другим произвольным выражением, результатом вычисления которого является единичное значение. Другие аналитические функции использовать нельзя.  

 [ IGNORE NULLS | RESPECT NULLS ]     
 **Область применения**: SQL Azure для пограничных вычислений

 IGNORE NULLS — пропуск значений NULL в наборе данных при вычислении первого значения в секции.     
 RESPECT NULLS — учет значений NULL в наборе данных при вычислении первого значения в секции.     
 
  Дополнительные сведения см. в разделе [Ввод отсутствующих значений](/azure/azure-sql-edge/imputing-missing-values/).
  
 OVER **(** [ *partition_by_clause* ] *order_by_clause* [ *rows_range_clause* ] **)**  
 *partition_by_clause* делит результирующий набор, полученный с помощью предложения FROM, на секции, к которым применяется функция. Если этот параметр не указан, функция обрабатывает все строки результирующего набора запроса как отдельные группы. *order_by_clause* определяет логический порядок, в котором выполняется операция. Аргумент *order_by_clause* является обязательным. *rows_range_clause* еще больше ограничивает строки в пределах секции, задавая начальную и конечную точки. Дополнительные сведения см. в статье [Предложение OVER (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 Тот же тип, что и *scalar_expression*.  
  
## <a name="general-remarks"></a>Общие замечания  
 Функция FIRST_VALUE не детерминирована. Дополнительные сведения см. в разделе [Deterministic and Nondeterministic Functions](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-first_value-over-a-query-result-set"></a>A. Использование FIRST_VALUE по результирующему набору запроса  
 В следующем примере FIRST_VALUE использоваться для получения имени самого дешевого продукта в заданной категории продуктов.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Name, ListPrice,   
       FIRST_VALUE(Name) OVER (ORDER BY ListPrice ASC) AS LeastExpensive   
FROM Production.Product  
WHERE ProductSubcategoryID = 37;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
  
Name                    ListPrice             LeastExpensive  
----------------------- --------------------- --------------------  
Patch Kit/8 Patches     2.29                  Patch Kit/8 Patches  
Road Tire Tube          3.99                  Patch Kit/8 Patches  
Touring Tire Tube       4.99                  Patch Kit/8 Patches  
Mountain Tire Tube      4.99                  Patch Kit/8 Patches  
LL Road Tire            21.49                 Patch Kit/8 Patches  
ML Road Tire            24.99                 Patch Kit/8 Patches  
LL Mountain Tire        24.99                 Patch Kit/8 Patches  
Touring Tire            28.99                 Patch Kit/8 Patches  
ML Mountain Tire        29.99                 Patch Kit/8 Patches  
HL Road Tire            32.60                 Patch Kit/8 Patches  
HL Mountain Tire        35.00                 Patch Kit/8 Patches  
  
```  
  
### <a name="b-using-first_value-over-partitions"></a>Б. Использование FIRST_VALUE для секций  
 В следующем примере FIRST_VALUE используется для получения сотрудника с наименьшим количеством часов отпуска по сравнению с другими сотрудниками, занимающими такую же должность. Предложение PARTITION BY разделяет сотрудников по должностям, а функция FIRST_VALUE применяется к каждой из секций по отдельности. Предложение ORDER BY, указанное в предложении OVER, определяет логический порядок, в котором функция FIRST_VALUE применяется к строкам каждой секции. Предложение ROWS UNBOUNDED PRECEDING задает начальную точку окна в качестве первой строки каждой секции.  
  
```sql  
USE AdventureWorks2012;   
GO  
SELECT JobTitle, LastName, VacationHours,   
       FIRST_VALUE(LastName) OVER (PARTITION BY JobTitle   
                                   ORDER BY VacationHours ASC  
                                   ROWS UNBOUNDED PRECEDING  
                                  ) AS FewestVacationHours  
FROM HumanResources.Employee AS e  
INNER JOIN Person.Person AS p   
    ON e.BusinessEntityID = p.BusinessEntityID  
ORDER BY JobTitle;  
```  
  
 Здесь приводится частичный результирующий набор.  
  
```  
  
JobTitle                            LastName                  VacationHours FewestVacationHours  
----------------------------------- ------------------------- ------------- -------------------  
Accountant                          Moreland                  58            Moreland  
Accountant                          Seamans                   59            Moreland  
Accounts Manager                    Liu                       57            Liu  
Accounts Payable Specialist         Tomic                     63            Tomic  
Accounts Payable Specialist         Sheperdigian              64            Tomic  
Accounts Receivable Specialist      Poe                       60            Poe  
Accounts Receivable Specialist      Spoon                     61            Poe  
Accounts Receivable Specialist      Walton                    62            Poe  
```  
  
## <a name="see-also"></a>См. также:  
 [Предложение OVER (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md)  
 [Last_Value &#40;Transact-SQL&#41;](last-value-transact-sql.md)  

