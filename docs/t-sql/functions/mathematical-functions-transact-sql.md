---
description: Математические функции (Transact-SQL)
title: Математические функции (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- calculations [SQL Server]
- mathematical functions [SQL Server]
- functions [SQL Server], mathematical
ms.assetid: 46495a2e-81d0-4677-9d72-9db083cd1023
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2fa744343341f8076c59d626218ef29cdcbbb3bc
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102580"
---
# <a name="mathematical-functions-transact-sql"></a>Математические функции (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Следующие скалярные функции выполняют вычисление, обычно на основании входных значений, заданных в качестве аргументов, и возвращают числовые значения:  
  
- [ABS](../../t-sql/functions/abs-transact-sql.md)
- [ACOS](../../t-sql/functions/acos-transact-sql.md)
- [ASIN](../../t-sql/functions/asin-transact-sql.md)
- [ATAN](../../t-sql/functions/atan-transact-sql.md)
- [ATN2](../../t-sql/functions/atn2-transact-sql.md)
- [CEILING](../../t-sql/functions/ceiling-transact-sql.md)
- [COS](../../t-sql/functions/cos-transact-sql.md)
- [COT](../../t-sql/functions/cot-transact-sql.md)
- [DEGREES](../../t-sql/functions/degrees-transact-sql.md)
- [EXP](../../t-sql/functions/exp-transact-sql.md)
- [FLOOR](../../t-sql/functions/floor-transact-sql.md)
- [LOG](../../t-sql/functions/log-transact-sql.md)
- [LOG10](../../t-sql/functions/log10-transact-sql.md)
- [PI](../../t-sql/functions/pi-transact-sql.md)
- [POWER](../../t-sql/functions/power-transact-sql.md)
- [RADIANS](../../t-sql/functions/radians-transact-sql.md)  
- [RAND](../../t-sql/functions/rand-transact-sql.md)  
- [ROUND](../../t-sql/functions/round-transact-sql.md)  
- [SIGN](../../t-sql/functions/sign-transact-sql.md)  
- [SIN](../../t-sql/functions/sin-transact-sql.md)  
- [SQRT](../../t-sql/functions/sqrt-transact-sql.md)  
- [SQUARE](../../t-sql/functions/square-transact-sql.md)  
- [TAN](../../t-sql/functions/tan-transact-sql.md)
  
> [!NOTE]  
> Арифметические функции, такие как ABS, CEILING, DEGREES, FLOOR, POWER, RADIANS и SIGN, возвращают значение того же типа, что и входное значение. Тригонометрические и другие функции, включая EXP, LOG, LOG10, SQUARE и SQRT, преобразуют входные значения в тип **float** и возвращают значение типа **float**.  
  
Все математические функции, кроме RAND, являются детерминированными. Это значит, что они возвращают одни и те же результаты каждый раз, когда вызываются с одними и теми же входными значениями. Функция RAND является детерминированной только в том случае, если задан параметр начального значения. Дополнительные сведения о детерминированности функций см. в статье [Детерминированные и недетерминированные функции](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="see-also"></a>См. также:

- [Арифметические операторы (Transact-SQL)](../../t-sql/language-elements/arithmetic-operators-transact-sql.md)
- [Встроенные функции (Transact-SQL)](~/t-sql/functions/functions.md)  
