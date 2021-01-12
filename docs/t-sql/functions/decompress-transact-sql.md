---
description: DECOMPRESS (Transact-SQL)
title: DECOMPRESS (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 10/11/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DECOMPRESS
- DECOMPRESS_TSQL
helpviewer_keywords:
- DECOMPRESS function
ms.assetid: 738d56be-3870-4774-b112-3dce27becc11
author: cawrites
ms.author: chadam
ms.openlocfilehash: 35d503318f71750691d8dccabfc60284f57f51ee
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101165"
---
# <a name="decompress-transact-sql"></a>DECOMPRESS (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

Эта функция распаковывает значение входного выражения с использованием алгоритма GZIP. `DECOMPRESS` возвращает массив байтов (тип VARBINARY(MAX)).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
DECOMPRESS ( expression )  
```  
  
## <a name="arguments"></a>Аргументы
 *expression*  
Значение **varbinary(** _n_ **)** , **varbinary(max)** или **binary(** _n_ **)** . Дополнительные сведения см. в статье [Выражения (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md).  
  
## <a name="return-types"></a>Типы возвращаемых данных  
Значение типа данных **varbinary(max)**. Для распаковки входного аргумента `DECOMPRESS` использует алгоритм ZIP. При необходимости пользователю следует явно привести результат к требуемому конечному типу.  
  
## <a name="remarks"></a>Remarks  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-decompress-data-at-query-time"></a>A. Распаковка данных во время выполнения запроса  
В этом примере показано возвращение сжатых данных таблицы.  
  
```sql  
SELECT _id, name, surname, datemodified,  
             CAST(DECOMPRESS(info) AS NVARCHAR(MAX)) AS info  
FROM player;  
```  
  
### <a name="b-display-compressed-data-using-computed-column"></a>Б. Отображение сжатых данных с помощью вычисляемого столбца  
В этом примере показано создание таблицы для хранения распакованных данных.  
  
```sql  
CREATE TABLE example_table (  
    _id INT PRIMARY KEY IDENTITY,  
    name NVARCHAR(max),  
    surname NVARCHAR(max),  
    info VARBINARY(max),  
    info_json as CAST(DECOMPRESS(info) as NVARCHAR(max))  
);  
```  
  
## <a name="see-also"></a>См. также  
 [Строковые функции (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)   
 [COMPRESS (Transact-SQL)](../../t-sql/functions/compress-transact-sql.md)  
  
  
