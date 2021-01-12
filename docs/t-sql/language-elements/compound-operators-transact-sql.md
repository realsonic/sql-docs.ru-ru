---
description: Составные операторы (Transact-SQL)
title: Составные операторы (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- compound operators
- compound operators, described
ms.assetid: 5072fe91-02d3-42a7-831f-756eff714a17
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2666168b9123d5656c2e9822cb8c5bfc328e9a0f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092194"
---
# <a name="compound-operators-transact-sql"></a>Составные операторы (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Составные операторы выполняют определенные операции и задают исходной величине значение результата операции. Например, если переменная @x равна 35, то @x += 2 принимает исходное значение @x, прибавляет 2 и присваивает переменной @x новое значение (37).  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] поддерживает следующие составные операторы.  
  
|Оператор|Ссылка на дополнительные сведения|Действие|  
|--------------|------------------------------|------------|  
|+=|[+= (присваивание сложения) (Transact-SQL)](../../t-sql/language-elements/add-equals-transact-sql.md)|Добавляет некоторое значение к исходному значению и задает исходной величине значение результата.|  
|-=|[–= (присваивание вычитания) (Transact-SQL)](../../t-sql/language-elements/subtract-equals-transact-sql.md)|Вычитает определенное значение из исходного значения и задает исходной величине значение результата.|  
|*=|[&#42;= (присваивание умножения) (Transact-SQL)](../../t-sql/language-elements/multiply-equals-transact-sql.md)|Умножает исходное значение на определенное значение и задает исходной величине значение результата.|  
|/=|[(присваивание деления) (Transact-SQL)](../../t-sql/language-elements/divide-equals-transact-sql.md)|Делит исходное значение на определенное значение и задает исходной величине значение результата.|  
|%=|[Присваивание модуля (Transact-SQL)](../../t-sql/language-elements/modulo-equals-transact-sql.md)|Делит исходное значение на определенное значение и задает исходной величине значение остатка от деления.|  
|&=|[&= (присваивание побитового И) (Transact-SQL)](../../t-sql/language-elements/bitwise-and-equals-transact-sql.md)|Выполняет операцию побитового И и задает исходной величине значение результата.|  
|^=|[^= (присваивание побитового исключающего ИЛИ) (Transact-SQL)](../../t-sql/language-elements/bitwise-exclusive-or-equals-transact-sql.md)|Выполняет операцию побитового исключающего ИЛИ и задает исходной величине значение результата.|  
|&#124;=|[&#124;= (присваивание побитового ИЛИ) (Transact-SQL)](../../t-sql/language-elements/bitwise-or-equals-transact-sql.md)|Выполняет операцию побитового исключающего И и задает исходной величине значение результата.|  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
expression operator expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *expression*  
 Любое допустимое [выражение](../../t-sql/language-elements/expressions-transact-sql.md) любого типа данных числовой категории.  
  
## <a name="result-types"></a>Типы результата  
 Возвращает результат типа данных аргумента с более высоким приоритетом. Дополнительные сведения см. в разделе [Приоритет типов данных (Transact-SQL)](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
  
## <a name="remarks"></a>Комментарии  
 Дополнительные сведения см. в разделах, посвященных соответствующим операторам.  
  
## <a name="examples"></a>Примеры  
 В следующих примерах демонстрируется использование составных операторов.  
  
```sql  
DECLARE @x1 INT = 27;  
SET @x1 += 2 ;  
SELECT @x1 AS Added_2;  
  
DECLARE @x2 INT = 27;  
SET @x2 -= 2 ;  
SELECT @x2 AS Subtracted_2;  
  
DECLARE @x3 INT = 27;  
SET @x3 *= 2 ;  
SELECT @x3 AS Multiplied_by_2;  
  
DECLARE @x4 INT = 27;  
SET @x4 /= 2 ;  
SELECT @x4 AS Divided_by_2;  
  
DECLARE @x5 INT = 27;  
SET @x5 %= 2 ;  
SELECT @x5 AS Modulo_of_27_divided_by_2;  
  
DECLARE @x6 INT = 9;  
SET @x6 &= 13 ;  
SELECT @x6 AS Bitwise_AND;  
  
DECLARE @x7 INT = 27;  
SET @x7 ^= 2 ;  
SELECT @x7 AS Bitwise_Exclusive_OR;  
  
DECLARE @x8 INT = 27;  
SET @x8 |= 2 ;  
SELECT @x8 AS Bitwise_OR;  
```  
  
## <a name="see-also"></a>См. также:  
 [Операторы (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [Побитовые операторы (Transact-SQL)](../../t-sql/language-elements/bitwise-operators-transact-sql.md)  
  
  
