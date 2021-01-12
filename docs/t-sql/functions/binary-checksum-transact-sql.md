---
description: BINARY_CHECKSUM (Transact-SQL)
title: BINARY_CHECKSUM (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- BINARY_CHECKSUM
- BINARY_CHECKSUM_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- BINARY_CHECKSUM function
- binary [SQL Server], checksum values
ms.assetid: 07fece4d-58e3-446e-a3b5-92fe24d2d1fb
author: cawrites
ms.author: chadam
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 33dbfaeaeef8e6505ee5c90e1a0b4712b0895669
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98088690"
---
# <a name="binary_checksum--transact-sql"></a>BINARY_CHECKSUM (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Возвращает двоичное значение контрольной суммы, вычисленное для строки таблицы или для списка выражений.
  
![Значок ссылки на статью](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на статью") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
BINARY_CHECKSUM ( * | expression [ ,...n ] )   
```  
  
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
*\**  
Указывает, что вычисление выполняется для всех столбцов таблицы. При выполнении функции BINARY_CHECKSUM столбцы с несопоставимыми типами данных игнорируются. Перечень несопоставимых типов данных  
* **курсор**  
* **image**  
* **ntext**  
* **text**  
* **xml**  

А также несопоставимые определяемые пользователем типы среды CLR.
  
*expression*  
[Выражение](../../t-sql/language-elements/expressions-transact-sql.md) любого типа данных. При выполнении функции BINARY_CHECKSUM выражения с несопоставимыми типами данных игнорируются.

## <a name="return-types"></a>Типы возвращаемых данных  
 **int**
  
## <a name="remarks"></a>Remarks  
Результат, возвращаемый функцией `BINARY_CHECKSUM(*)` для строки таблицы, изменяется только при изменении указанной строки. `BINARY_CHECKSUM` удовлетворяет свойствам хэш-функции. Функция, примененная к двум любым спискам выражений, возвращает одинаковое значение, если при сравнении оператором равенства (=) соответствующие элементы двух списков имеют одинаковый тип и равны. Для данного определения значения NULL указанного типа рассматриваются при сравнении как равные. Если хотя бы одно из значений в списке выражений меняется, то обычно меняется и контрольная сумма выражений. Но такие изменения не гарантируются. Поэтому, чтобы определить факт изменения значений, рекомендуется использовать функцию `BINARY_CHECKSUM` только в том случае, если в вашем приложении допускается случайный пропуск изменений. В противном случае рекомендуется вместо этого использовать функцию `HASHBYTES`. Когда указан хэш-алгоритм MD5, вероятность возвращения функцией `HASHBYTES` одинакового результата для двух различных входных параметров намного ниже, чем при использовании `BINARY_CHECKSUM`.
  
Функция `BINARY_CHECKSUM` может применяться к списку выражений, причем ее результаты для данного списка совпадают. Функция `BINARY_CHECKSUM` вернет одинаковый результат для двух любых списков выражений в том случае, если тип и байтовое представление соответствующих друг другу элементов списков совпадают. В данном случае значения NULL одинакового типа рассматриваются как значения с одинаковым байтовым представлением.
  
Функции `BINARY_CHECKSUM` и `CHECKSUM` похожи. Они могут быть использованы для вычисления контрольной суммы списка выражений, при этом порядок следования выражений влияет на значение результата. Порядок столбцов, используемый с `BINARY_CHECKSUM(*)`, является порядком столбцов, указанным в таблице или определении представления. Такой порядок включает вычисляемые столбцы.
  
Функции `BINARY_CHECKSUM` и `CHECKSUM` возвращают различные значения для строковых типов данных, в которых из-за языкового стандарта строки с различными представлениями могут оказаться эквивалентными. Строковые типы данных  

* **char**  
* **nchar**  
* **nvarchar**  
* **varchar**  

или  

* **sql_variant** (если базовым типом для **sql_variant** является строковый тип данных).  
  
Например, для строк McCavity и Mccavity функция `BINARY_CHECKSUM` возвращает разные значения. Однако при использовании функции `CHECKSUM` на сервере, нечувствительном к регистру, значения контрольных сумм для данных строк совпадают. Следует избегать сравнения значения `CHECKSUM` со значением `BINARY_CHECKSUM`.
 
Функция `BINARY_CHECKSUM` поддерживает любую длину типа **varbinary(max)** и до 255 символов типа **nvarchar(max)**.
  
## <a name="examples"></a>Примеры  
В этом примере для выявления изменений в строке таблицы используется функция `BINARY_CHECKSUM`.
  
```sql
USE AdventureWorks2012;  
GO  
CREATE TABLE myTable (column1 INT, column2 VARCHAR(256));  
GO  
INSERT INTO myTable VALUES (1, 'test');  
GO  
SELECT BINARY_CHECKSUM(*) from myTable;  
GO  
UPDATE myTable set column2 = 'TEST';  
GO  
SELECT BINARY_CHECKSUM(*) from myTable;  
GO  
```  
  
## <a name="see-also"></a>См. также
[Агрегатные функции (Transact-SQL)](../../t-sql/functions/aggregate-functions-transact-sql.md)  
[CHECKSUM_AGG (Transact-SQL)](../../t-sql/functions/checksum-agg-transact-sql.md)  
[CHECKSUM (Transact-SQL)](../../t-sql/functions/checksum-transact-sql.md)  
[HASHBYTES (Transact-SQL)](../../t-sql/functions/hashbytes-transact-sql.md)  
  
  
