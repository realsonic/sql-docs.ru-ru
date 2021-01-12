---
description: DIFFERENCE (Transact-SQL)
title: DIFFERENCE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DIFFERENCE
- DIFFERENCE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DIFFERENCE function
- comparing SOUNDEX values
- SOUNDEX values
ms.assetid: c58ca25d-d6ea-48fa-93bb-c9374b0b2a2b
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 15d9c2b7dfad9e6ac5610b35fbedc583ac6f6cb2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100495"
---
# <a name="difference-transact-sql"></a>DIFFERENCE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Эта функция возвращает целочисленную разницу между значениями [SOUNDEX()](./soundex-transact-sql.md) двух разных символьных выражений.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
DIFFERENCE ( character_expression , character_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
*character_expression*  
Буквенно-цифровое [выражение](../../t-sql/language-elements/expressions-transact-sql.md) символьных данных. *character_expression* может быть константой, переменной или столбцом.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
**int**  
 
## <a name="remarks"></a>Remarks  
Функция `DIFFERENCE` сравнивает два разных значения `SOUNDEX` и возвращает целочисленный результат. Это значение указывает на степень сходства значений `SOUNDEX` по шкале от 0 до 4. Значение 0 указывает на слабое сходство значений SOUNDEX или его отсутствие; значение 4 указывает на сильное сходство или одинаковые значения SOUNDEX.  
  
В функциях `DIFFERENCE` и `SOUNDEX` учитываются параметры сортировки.  
  
## <a name="examples"></a>Примеры  
В первой части приведенного ниже примера сравниваются значения `SOUNDEX` двух очень похожих строк. Для параметров сортировки Latin1_General функция `DIFFERENCE` возвращает значение `4`. Во второй части примера сравниваются значения `SOUNDEX` двух сильно различающихся строк. Для параметров сортировки Latin1_General функция `DIFFERENCE` возвращает значение `0`.  
  
```sql  
-- Returns a DIFFERENCE value of 4, the least possible difference.  
SELECT SOUNDEX('Green'), SOUNDEX('Greene'), DIFFERENCE('Green','Greene');  
GO  
-- Returns a DIFFERENCE value of 0, the highest possible difference.  
SELECT SOUNDEX('Blotchet-Halls'), SOUNDEX('Greene'), DIFFERENCE('Blotchet-Halls', 'Greene');  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
----- ----- -----------   
G650  G650  4             
  
(1 row(s) affected)  
  
----- ----- -----------   
B432  G650  0             
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>См. также  
 [SOUNDEX (Transact-SQL)](../../t-sql/functions/soundex-transact-sql.md)   
 [Строковые функции (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)  
  
  

