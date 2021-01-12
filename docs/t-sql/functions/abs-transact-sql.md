---
description: ABS (Transact-SQL)
title: ABS (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ABS_TSQL
- ABS
dev_langs:
- TSQL
helpviewer_keywords:
- values [SQL Server], positive
- values [SQL Server], absolute
- ABS function
- absolute positive value
ms.assetid: e2ea7a6d-3e2f-472c-afbc-437d3b835c03
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 42ea812d8d54a307951bbc1c88ab018cc31dc52a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98090936"
---
# <a name="abs-transact-sql"></a>ABS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Математическая функция, возвращающая абсолютное (положительное) значение указанного числового выражения. (`ABS` изменяет отрицательные значения на положительные значения. `ABS` не влияет на ноль или положительные значения.)
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
ABS ( numeric_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
*numeric_expression*  
Выражение категории точного числового или приблизительного числового типа данных.
  
## <a name="return-types"></a>Типы возвращаемых данных  
Возвращает тот же тип, что и аргумент *numeric_expression*.
  
## <a name="examples"></a>Примеры  
В этом примере показаны результаты применения функции `ABS` к трем различным числам.
  
```sql
SELECT ABS(-1.0), ABS(0.0), ABS(1.0);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
---- ---- ----  
1.0  .0   1.0  
```  
  
Функция `ABS` может вызвать ошибку переполнения, если абсолютное значение числа превышает наибольшее число, которое может быть представлено указанным типом данных. Например, тип данных `int` поддерживает диапазон значений от `-2,147,483,648` до `2,147,483,647`. Расчет абсолютного значения для целого числа со знаком `-2,147,483,648` приводит к ошибке переполнения, так как его абсолютное значение превышает предел положительного диапазона для типа данных `int`.
  
```sql
DECLARE @i INT;  
SET @i = -2147483648;  
SELECT ABS(@i);  
GO  
```  
  
Возвращает следующее сообщение об ошибке:
  
«Сообщение 8115, уровень 16, состояние 2, строка 3».
  
«Арифметическое переполнение при преобразовании выражения к типу данных int».

  
## <a name="see-also"></a>См. также раздел
[Функции CAST и CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)  
[Математические функции (Transact-SQL)](../../t-sql/functions/mathematical-functions-transact-sql.md)  
[Встроенные функции (Transact-SQL)](../../t-sql/functions/functions.md)
  
  

