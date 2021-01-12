---
description: '&amp; (побитовое И) (Transact-SQL)'
title: '&amp; (побитовое И) (Transact-SQL) | Документы Майкрософт'
ms.custom: ''
ms.date: 01/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- bitwise
- '&'
- '&_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- AND, bitwise AND
- '& (bitwise AND)'
- bitwise AND (&)
ms.assetid: 20275755-4fa7-47b1-a9be-ac85606d63b0
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a6d75027d1ccd171a8b860bfa536da0b51fb457b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095997"
---
# <a name="amp-bitwise-and-transact-sql"></a>&amp; (побитовое И) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Выполняет побитовую логическую операцию «И» между двумя целочисленными значениями.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
expression & expression  
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
  
 **tinyint**, если входные значения имеют тип **tinyint** или **bit**.  
  
## <a name="remarks"></a>Remarks  
 Побитовый оператор **&** выполняет побитовую логическую операцию И с соответствующими битами двух выражений. Значение результирующих битов устанавливается равным 1 только в том случае, если оба бита (для текущего разрешаемого бита) в переданных выражениях имеют значение 1; в остальных случаях значение результирующего бита устанавливается равным 0.  
  
 Если левое и правое выражения принадлежат к различным целочисленным типам данных (например, левое выражение *expression* — к типу **smallint**, а правое выражение *expression* — к типу **int**), аргумент более короткого типа данных преобразовывается в более длинный тип данных. В этом случае **smallint** _expression_ преобразуется в тип **int**.  
  
## <a name="examples"></a>Примеры  
 В приведенном ниже примере создается таблица с использованием типа данных **int** для хранения значений, и два значения вставляются в одну строку.  
  
```sql
CREATE TABLE bitwise (   
  a_int_value INT NOT NULL,  
  b_int_value INT NOT NULL);  
GO  
INSERT bitwise VALUES (170, 75);  
GO  
```  
  
 В этом запросе побитовое И выполняется на столбцах `a_int_value` и `b_int_value`.  
  
```sql  
SELECT a_int_value & b_int_value  
FROM bitwise;  
GO  
```  
  
 Результирующий набор:  
  
```  
-----------   
10            
  
(1 row(s) affected)  
```  
  
 Двоичное представление числа 170 (`a_int_value` или `A`) — `0000 0000 1010 1010`. Двоичное представление числа 75 (`b_int_value` или `B`) — `0000 0000 0100 1011`. Выполнение побитовой операции И в отношении этих двух значений дает двоичный результат `0000 0000 0000 1010`, который имеет десятичное значение 10.  
  
```  
(A & B)  
0000 0000 1010 1010  
0000 0000 0100 1011  
-------------------  
0000 0000 0000 1010  
```  
  
  
## <a name="see-also"></a>См. также:  
 [Выражения (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Операторы (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [Побитовые операторы (Transact-SQL)](../../t-sql/language-elements/bitwise-operators-transact-sql.md)   
 [&= (присваивание побитового И) (Transact-SQL)](../../t-sql/language-elements/bitwise-and-equals-transact-sql.md)   
 [Составные операторы (Transact-SQL)](../../t-sql/language-elements/compound-operators-transact-sql.md)  
  
  


