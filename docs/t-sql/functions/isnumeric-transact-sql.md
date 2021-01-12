---
description: ISNUMERIC (Transact-SQL)
title: ISNUMERIC (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ISNUMERIC
- ISNUMERIC_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- expressions [SQL Server], valid numeric type
- numeric data
- ISNUMERIC function
- verifying valid numeric type
- valid numeric type [SQL Server]
- checking valid numeric type
ms.assetid: 7aa816de-529a-4f6c-a99f-4d5a9ef599eb
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cc5cee521a7a22429986bc7e6b71d96d729c03ef
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100446"
---
# <a name="isnumeric-transact-sql"></a>ISNUMERIC (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Определяет, имеет ли переданное выражение допустимый числовой тип.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql 
ISNUMERIC ( expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *expression*  
 Выражение [expression](../../t-sql/language-elements/expressions-transact-sql.md), которое необходимо вычислить.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 **int**  
  
## <a name="remarks"></a>Remarks  
 Функция ISNUMERIC возвращает 1, если при оценке входного выражения получается допустимый числовой тип данных. В противном случае возвращается 0. Ниже приведены допустимые [числовые типы данных](../../t-sql/data-types/numeric-types.md).  

| Область | Числовые типы данных |
|-|-|
| [Точные числа](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md) | **bigint**, **int**, **smallint**, **tinyint**, **bit** |
| [Фиксированная точность](../../t-sql/data-types/decimal-and-numeric-transact-sql.md) | **decimal**, **numeric** |
| [Приблизительные значения](../../t-sql/data-types/float-and-real-transact-sql.md) | **float**, **real** |
| [Денежные значения](../../t-sql/data-types/money-and-smallmoney-transact-sql.md) | **money**, **smallmoney** |

> [!NOTE]  
> ISNUMERIC возвращает «1» для некоторых символов, которые не являются числами (например, плюс (+), минус (-) и такие символы валют, как знак доллара ($)). Полный список символов валют см. в статье [Типы money и smallmoney (Transact-SQL)](../../t-sql/data-types/money-and-smallmoney-transact-sql.md).  
  
## <a name="examples"></a>Примеры  
 Следующий пример использует функцию `ISNUMERIC` для возврата всех почтовых индексов, не являющихся числовыми значениями.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT City, PostalCode  
FROM Person.Address   
WHERE ISNUMERIC(PostalCode) <> 1;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 Следующий пример использует функцию `ISNUMERIC` для возврата всех почтовых индексов, не являющихся числовыми значениями.  
  
```sql
USE master;  
GO  
SELECT name, ISNUMERIC(name) AS IsNameANumber, database_id, ISNUMERIC(database_id) AS IsIdANumber   
FROM sys.databases;  
GO  
```  
  
## <a name="see-also"></a>См. также раздел

- [Выражения (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)
- [Системные функции (Transact-SQL)](../../relational-databases/system-functions/system-functions-category-transact-sql.md)
- [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)
