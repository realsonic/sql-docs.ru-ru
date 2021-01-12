---
description: ~ (побитовое НЕ) (Transact-SQL)
title: ~ (побитовое НЕ) (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 01/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ~_TSQL
- bitwise
- NOT
- "~"
- Bitwise NOT
dev_langs:
- TSQL
helpviewer_keywords:
- NOT keyword
- bitwise NOT (~)
- ~ (bitwise NOT)
ms.assetid: 02da8016-f6c0-41ae-8d59-33eaa02bfc95
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fe1193cb7251b690c4d610ee0a2fd691fc298d0f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092294"
---
# <a name="-bitwise-not-transact-sql"></a>~ (побитовое НЕ) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Выполняет побитовую логическую операцию НЕ с целочисленным значением.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
~ expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *expression*  
 Любое допустимое [выражение](../../t-sql/language-elements/expressions-transact-sql.md) любого типа данных из категории целочисленных типов или **bit** либо типов данных **binary** или **varbinary**. *expression* трактуется как двоичное числовое значение для выполнения побитовой операции.  
  
> [!NOTE]  
>  В побитовой операции только одно выражение *expression* может иметь тип **binary** или **varbinary**.  
  
## <a name="result-types"></a>Типы результата  
 **int**, если входные значения имеют тип **int**.  
  
 **smallint**, если входные значения имеют тип **smallint**.  
  
 **tinyint**, если входные значения имеют тип **tinyint**.  
  
 **bit**, если входные значения имеют тип **bit**.  
  
## <a name="remarks"></a>Комментарии  
 Побитовый оператор **~** выполняет побитовую логическую операцию НЕ для аргумента *expression*, обрабатывая последовательно каждый бит. Если аргумент *expression* имеет значение 0, то биты в результирующем наборе принимают значение 1. В противном случае бит в результате принимает значение 0. Другими словами, единицы меняются на нули, нули меняются на единицы.  
  
> [!IMPORTANT]  
>  При выполнении любого вида побитовой операции важна длина хранимого выражения, используемого в операции. Рекомендуется использование такого же количества битов при сохранении значений. Например, при сохранении десятичного значения 5 как **tinyint**, **smallint** или **int** получаются значения, занимающие различное количество байтов при хранении: значения типа **tinyint** занимают 1, **smallint** — 2, а **int** — 4 байта. Таким образом, при выполнении побитовой операции для десятичного представления числа типа **int** могут быть получены результаты, отличные от результатов, полученных для двоичного или шестнадцатеричного представления этого числа, особенно при использовании оператора **~** (побитового НЕ). Побитовую операцию «НЕ» можно провести для более короткой переменной. В случае, когда переменная более короткого типа данных преобразуется в переменную более длинного типа данных, старшие 8 бит могут принять неожиданное значение. Рекомендуется явно преобразовать переменные более короткого типа данных в переменные более длинного типа, а затем выполнять операцию «НЕ».  
  
## <a name="examples"></a>Примеры  
 В приведенном ниже примере создается таблица с использованием типа данных **int** для сохранения значений, и два значения вставляются в одну строку.  
  
```sql  
CREATE TABLE bitwise (  
  a_int_value INT NOT NULL,  
  b_int_value INT NOT NULL); 
GO  
INSERT bitwise VALUES (170, 75);  
GO  
```  
  
 В следующем запросе выполняется побитовая операция «НЕ» для столбцов `a_int_value` и `b_int_value`.  
  
```sql  
SELECT ~ a_int_value, ~ b_int_value  
FROM bitwise;  
```  
  
 Результирующий набор:  
  
```  
--- ---   
-171  -76   
  
(1 row(s) affected)  
```  
  
 Двоичное представление числа 170 (`a_int_value` или `A`) — `0000 0000 1010 1010`. Выполнение побитовой операции НЕ для этого значения дает двоичный результат `1111 1111 0101 0101`, который является десятичным числом –171. Двоичное представление числа 75: `0000 0000 0100 1011`. Выполнение побитовой операции «НЕ» дает результат `1111 1111 1011 0100`, который является десятичным числом -76.  
  
```  
 (~A)     
         0000 0000 1010 1010  
         -------------------  
         1111 1111 0101 0101  
(~B)     
         0000 0000 0100 1011  
         -------------------  
         1111 1111 1011 0100  
```  
  
 
## <a name="see-also"></a>См. также:  
 [Выражения (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Операторы (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [Побитовые операторы (Transact-SQL)](../../t-sql/language-elements/bitwise-operators-transact-sql.md)  
  
  


