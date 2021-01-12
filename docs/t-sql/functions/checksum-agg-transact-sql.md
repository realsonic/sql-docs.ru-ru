---
description: CHECKSUM_AGG (Transact-SQL)
title: CHECKSUM_AGG (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CHECKSUM_AGG
- CHECKSUM_AGG_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- checksum values
- CHECKSUM_AGG function
- groups [SQL Server], checksum values
ms.assetid: cdede70c-4eb5-4c92-98ab-b07787ab7222
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2bea0aad7839bc0b487b77b6ac1434396b84b765
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102650"
---
# <a name="checksum_agg-transact-sql"></a>CHECKSUM_AGG (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Эта функция возвращает контрольную сумму значений в группе. Значения NULL функция `CHECKSUM_AGG` не учитывает. [Предложение OVER](../../t-sql/queries/select-over-clause-transact-sql.md) может следовать за функцией `CHECKSUM_AGG`.
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
CHECKSUM_AGG ( [ ALL | DISTINCT ] expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
**ALL**  
Применяет агрегатную функцию ко всем значениям. ALL является аргументом по умолчанию.
  
DISTINCT  
Указывает, что `CHECKSUM_AGG` возвращает контрольную сумму уникальных значений.
  
*expression*  
Целочисленное [выражение](../../t-sql/language-elements/expressions-transact-sql.md). Функция `CHECKSUM_AGG` не позволяет использовать агрегатные функции или вложенные запросы.
  
## <a name="return-types"></a>Типы возвращаемых данных
Возвращает контрольную сумму всех значений *expression* как **int**.
  
## <a name="remarks"></a>Комментарии  
Функция `CHECKSUM_AGG` может обнаруживать изменения в таблице.
  
Результат функции `CHECKSUM_AGG` не зависит от порядка строк в таблице. Кроме того, функции `CHECKSUM_AGG` позволяют использовать ключевое слово `DISTINCT` и предложение `GROUP BY`.
  
Если значение в списке выражений изменяется, скорее всего также изменится значение контрольной суммы списка. Тем не менее существует небольшая вероятность того, что вычисленная контрольная сумма не изменится.
  
`CHECKSUM_AGG` имеет функциональные возможности, аналогичные другим агрегатным функциям. Дополнительные сведения см. в статье [Агрегатные функции (Transact-SQL)](../../t-sql/functions/aggregate-functions-transact-sql.md).
  
## <a name="examples"></a>Примеры  
В этих примерах функция `CHECKSUM_AGG` используется для обнаружения изменений в столбце `Quantity` таблицы `ProductInventory` в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].
  
```sql
--Get the checksum value before the column value is changed.  

SELECT CHECKSUM_AGG(CAST(Quantity AS INT))  
FROM Production.ProductInventory;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
------------------------  
262  
```  
  
```sql
UPDATE Production.ProductInventory   
SET Quantity=125  
WHERE Quantity=100;  
GO  

--Get the checksum of the modified column.  
SELECT CHECKSUM_AGG(CAST(Quantity AS INT))  
FROM Production.ProductInventory;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
------------------------  
287  
```  
  
## <a name="see-also"></a>См. также
[CHECKSUM (Transact-SQL)](../../t-sql/functions/checksum-transact-sql.md)  
[HASHBYTES (Transact-SQL)](../../t-sql/functions/hashbytes-transact-sql.md)  
[BINARY_CHECKSUM (Transact-SQL)](../../t-sql/functions/binary-checksum-transact-sql.md)
[Предложение OVER (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md)
  
