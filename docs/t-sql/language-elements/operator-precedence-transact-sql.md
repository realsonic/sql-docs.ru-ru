---
description: Приоритет операторов (Transact-SQL)
title: Приоритет операторов (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- operators [Transact-SQL], precedence
- operator precedence [Transact-SQL]
- order of operator execution [Transact-SQL]
- precedence [SQL Server], operators
ms.assetid: f04d2439-6fff-4e4c-801f-cc62faef510a
author: cawrites
ms.author: chadam
ms.openlocfilehash: 66cb4af95ca6c0167570a1edd1fea4dea0a8e838
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100983"
---
# <a name="operator-precedence-transact-sql"></a>Приоритет операторов (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Если сложное выражение содержит множество операторов, порядок выполнения операций определяется приоритетом операторов. Порядок исполнения может существенно повлиять на результирующее значение.  
  
 Уровни приоритета операторов показаны в следующей таблице. Оператор с более высоким уровнем выполняется прежде, чем оператор с более низким уровнем. В следующей таблице самым высоким уровнем является 1, а самым низким — 8.
  
|Level|Операторы|  
|-----------|---------------|  
|1|~ (побитовое НЕ)|  
|2|* (умножение), / (деление), % (остаток деления)|  
|3|+ (положительное), – (отрицательное), + (сложение), +( объединение), – (вычитание), & (побитовое И), ^ (побитовое исключающее ИЛИ), &#124; (побитовое ИЛИ)|  
|4|=, >, \<, >=, <=, <>, !=, !>, !< (операторы сравнения)|  
|5|NOT|  
|6|AND|  
|7|ALL, ANY, BETWEEN, IN, LIKE, OR, SOME|  
|8|= (присваивание)|  
  
 Если два оператора в выражении имеют один и тот же уровень приоритета, они вычисляются в порядке слева направо по мере их появления в выражении. Например, если выражение использует инструкцию `SET`, то оператор вычитания будет выполнен до оператора сложения.  
  
```sql  
DECLARE @MyNumber INT;  
SET @MyNumber = 4 - 2 + 27;  
-- Evaluates to 2 + 27 which yields an expression result of 29.  
SELECT @MyNumber;  
```  
  
 Чтобы изменить приоритет операторов в выражении, следует использовать скобки. Все находящееся внутри скобок вычисляется для получения одного значения. Это значение может быть использовано любым оператором за пределами скобок.  
  
 Например, если выражение использует инструкцию `SET`, то у оператора умножения более высокий приоритет, чем у оператора сложения. Оператор умножения вычисляется первым, и результатом выражения будет `13`.  
  
```sql  
DECLARE @MyNumber INT;  
SET @MyNumber = 2 * 4 + 5;  
-- Evaluates to 8 + 5 which yields an expression result of 13.  
SELECT @MyNumber;  
```  
  
 В выражении следующей инструкции `SET` скобки показывают, что сначала нужно вычислить операцию сложения. Результатом выражения будет `18`.  
  
```sql  
DECLARE @MyNumber INT;  
SET @MyNumber = 2 * (4 + 5);  
-- Evaluates to 2 * 9 which yields an expression result of 18.  
SELECT @MyNumber;  
```  
  
 Если в выражении содержатся вложенные скобки, то сначала вычисляется результат наиболее глубоко вложенных скобок. В следующем примере показано использование вложенных скобок, в наиболее глубоко вложенных скобках записано выражение `5 - 3`. Результатом этого выражения будет `2`. После этого оператор сложения (`+`) добавляет этот результат к `4`, чтобы получить значение `6`. Наконец, `6` умножается на `2`, чтобы получить результат выражения `12`.  
  
```sql  
DECLARE @MyNumber INT;  
SET @MyNumber = 2 * (4 + (5 - 3) );  
-- Evaluates to 2 * (4 + 2) which then evaluates to 2 * 6, and   
-- yields an expression result of 12.  
SELECT @MyNumber;  
```  
  
## <a name="see-also"></a>См. также:  
 [Логические операторы (Transact-SQL)](../../t-sql/language-elements/logical-operators-transact-sql.md)   
 [Операторы (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [Встроенные функции (Transact-SQL)](~/t-sql/functions/functions.md)  
  
