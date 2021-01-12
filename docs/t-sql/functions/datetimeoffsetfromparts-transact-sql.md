---
description: DATETIMEOFFSETFROMPARTS (Transact-SQL)
title: DATETIMEOFFSETFROMPARTS (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATETIMEOFFSETFROMPARTS_TSQL
- DATETIMEOFFSETFROMPARTS
dev_langs:
- TSQL
helpviewer_keywords:
- DATETIMEOFFSETFROMPARTS function
ms.assetid: 463da1f4-b4b6-45a3-9a95-ea1f99575542
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ab37520d60c482afccb40bde7392de2515a79753
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101203"
---
# <a name="datetimeoffsetfromparts-transact-sql"></a>DATETIMEOFFSETFROMPARTS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Возвращает значение **datetimeoffset** для указанных аргументов даты и времени. Точность возвращаемого значения определяется аргументом precision, а смещение — аргументами offset.  
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
DATETIMEOFFSETFROMPARTS ( year, month, day, hour, minute, seconds, fractions, hour_offset, minute_offset, precision )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы

*year*  
Целочисленное выражение, задающее год.  
  
*month*  
Целочисленное выражение, задающее месяц.  
  
*day*  
Целочисленное выражение, задающее день.  
  
*hour*  
Целочисленное выражение, задающее часы.  
  
*minute*  
Целочисленное выражение, задающее минуты.  
  
*секунд*  
Целочисленное выражение, задающее секунды.  
  
*fractions*  
Целочисленное выражение, задающее дробное значение секунд.  
  
*hour_offset*  
Целочисленное выражение, задающее часть смещения часового пояса в часах.  
  
*minute_offset*  
Целочисленное выражение, задающее часть смещения часового пояса в минутах.  
  
*precision*  
Целочисленное литеральное значение, которое определяет точность значения **datetimeoffset**, возвращаемого функцией `DATETIMEOFFSETFROMPARTS`.  
  
## <a name="return-types"></a>Типы возвращаемых данных
**datetimeoffset(** *precision* **)**  
  
## <a name="remarks"></a>Комментарии  

Функция `DATETIMEOFFSETFROMPARTS` возвращает полностью инициализированный тип данных **datetimeoffset**. Аргументы смещения представляют смещение часового пояса. Если аргументы смещения пропущены, в `DATETIMEOFFSETFROMPARTS` предполагается, что смещение часового пояса равно `00:00`, то есть отсутствует. Если аргументы смещения указаны, `DATETIMEOFFSETFROMPARTS` требует наличия значений обоих аргументов, причем оба должны быть или положительными, или отрицательными. Если *minute_offset* указывается без значения *hour_offset*, `DATETIMEOFFSETFROMPARTS` вызывает ошибку. Если другие аргументы имеют недопустимые значения, `DATETIMEOFFSETFROMPARTS` вызывает ошибку. `DATETIMEOFFSETFROMPARTS` возвращает `NULL`, если хотя бы один обязательный аргумент имеет значение `NULL`. Но если *precision* имеет значение `NULL`, `DATETIMEOFFSETFROMPARTS` вызывает ошибку.  
  
Аргумент *fractions* зависит от аргумента precision. Например, если значение precision равно 7, каждая дробная часть представляет 100 наносекунд, а если значение precision равно 3, каждая дробная часть представляет миллисекунду. Если значение precision равно нулю, значение fractions также должно быть равно нулю. В противном случае `DATETIMEOFFSETFROMPARTS` вызывает ошибку.  
  
Эта функция поддерживает удаленное взаимодействие с серверами [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] и более поздних версий. Она не поддерживает удаленное взаимодействие с серверами версий ниже [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].
  
## <a name="examples"></a>Примеры  
  
### <a name="a-an-example-without-fractions-of-a-second"></a>A. Пример без долей секунды  
  
```sql
SELECT DATETIMEOFFSETFROMPARTS ( 2010, 12, 31, 14, 23, 23, 0, 12, 0, 7 ) AS Result;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Result  
----------------------------------
2010-12-31 14:23:23.0000000 +12:00  
  
(1 row(s) affected)  
```  
  
### <a name="b-example-with-fractions-of-a-second"></a>Б. Пример с долями секунд  

В этом примере показано использование параметров *fractions* и *precision*:  

1. Если параметр *fractions* имеет значение 5, а параметр *precision* — значение 1, то значение параметра *fractions* представляет 5/10 секунды.  

2. Если параметр *fractions* имеет значение 50, а параметр *precision* — значение 2, то значение параметра *fractions* представляет 50/100 секунды.  

3. Если параметр *fractions* имеет значение 500, а параметр *precision* — значение 3, то значение параметра *fractions* представляет 500/1000 секунды.  
  
```sql
SELECT DATETIMEOFFSETFROMPARTS ( 2011, 8, 15, 14, 30, 00, 5, 12, 30, 1 );  
SELECT DATETIMEOFFSETFROMPARTS ( 2011, 8, 15, 14, 30, 00, 50, 12, 30, 2 );  
SELECT DATETIMEOFFSETFROMPARTS ( 2011, 8, 15, 14, 30, 00, 500, 12, 30, 3 );  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
----------------------------------  
2011-08-15 14:30:00.5 +12:30  
  
(1 row(s) affected)  
  
----------------------------------  
2011-08-15 14:30:00.50 +12:30  
  
(1 row(s) affected)  
  
----------------------------------  
2011-08-15 14:30:00.500 +12:30  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>См. также
[datetimeoffset (Transact-SQL)](../../t-sql/data-types/datetimeoffset-transact-sql.md)  
[AT TIME ZONE (Transact-SQL)](../../t-sql/queries/at-time-zone-transact-sql.md)
  
  


