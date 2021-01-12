---
description: '- (Вычитание) (Transact-SQL)'
title: '- (вычитание) (Transact-SQL) | Документы Майкрософт'
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- subtract
- '-'
- -_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- '- (subtract)'
- subtract operator (-)
- minus operator (-)
- subtracting numbers
ms.assetid: db23145f-f17d-4b90-be09-28a881cacd1a
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c3d9d84d433e25d039d42fa58ffc3ccf68751dc9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100313"
---
# <a name="--subtraction-transact-sql"></a>– (вычитание) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Вычитает одно число из другого (оператор арифметического вычитания). Также можно вычесть из даты определенное число дней.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
expression - expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *expression*  
 Любое допустимое выражение [expression](../../t-sql/language-elements/expressions-transact-sql.md) любого числового типа данных, кроме **bit**. Нельзя использовать с типами данных **date**, **time**, **datetime2** или **datetimeoffset**.  
  
## <a name="result-types"></a>Типы результата  
 Возвращает результат типа данных аргумента с более высоким приоритетом. Дополнительные сведения см. в разделе [Приоритет типов данных (Transact-SQL)](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-subtraction-in-a-select-statement"></a>A. Использование вычитания в инструкции SELECT  
 Следующий пример вычисляет разницу налоговых ставок между штатом или провинцией с наибольшей налоговой ставкой и штатом или провинцией с наименьшей налоговой ставкой.  
  
 **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
```sql  
-- Uses AdventureWorks  
  
SELECT MAX(TaxRate) - MIN(TaxRate) AS 'Tax Rate Difference'  
FROM Sales.SalesTaxRate  
WHERE StateProvinceID IS NOT NULL;  
GO  
```  
  
 Можно изменить порядок выполнения, используя скобки. Сначала производятся вычисления выражений в скобках. В случае вложенных скобок вычисления начинаются с самого глубокого уровня.  
  
### <a name="b-using-date-subtraction"></a>Б. Использование вычитания из даты  
 Следующий пример вычитает указанное число дней из даты `datetime`.  
  
 Применимо к: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
```sql  
-- Uses AdventureWorks  
  
DECLARE @altstartdate DATETIME;  
SET @altstartdate = CONVERT(DATETIME, ''January 10, 1900 3:00 AM', 101);  
SELECT @altstartdate - 1.5 AS 'Subtract Date';  
```  
  
 Результирующий набор:  
  
 ```
 Subtract Date  
 -----------------------  
 1900-01-08 15:00:00.000  

 (1 row(s) affected)
 ```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-subtraction-in-a-select-statement"></a>В. Использование вычитания в инструкции SELECT  
 В следующем примере вычисляется разница базовых окладов между сотрудником с наибольшим базовым окладом и сотрудником с наименьшей налоговой ставкой из таблицы `dimEmployee`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT MAX(BaseRate) - MIN(BaseRate) AS BaseRateDifference  
FROM DimEmployee;  
```  
  
## <a name="see-also"></a>См. также:  
 [–= (присваивание вычитания) (Transact-SQL)](../../t-sql/language-elements/subtract-equals-transact-sql.md)   
 [Составные операторы (Transact-SQL)](../../t-sql/language-elements/compound-operators-transact-sql.md)  
 [Арифметические операторы (Transact-SQL)](../../t-sql/language-elements/arithmetic-operators-transact-sql.md)   
 [– (отрицательное значение) (Transact-SQL)](../../t-sql/language-elements/unary-operators-negative.md)   
 [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [Выражения (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Встроенные функции (Transact-SQL)](~/t-sql/functions/functions.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
  
  


