---
description: COUNT_BIG (Transact-SQL)
title: COUNT_BIG (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- COUNT_BIG_TSQL
- COUNT_BIG
dev_langs:
- TSQL
helpviewer_keywords:
- totals [SQL Server], COUNT_BIG function
- counting items in group
- groups [SQL Server], number of items in
- number of group items
- COUNT_BIG function
ms.assetid: f2e3601f-487e-4917-bb01-47b1047908cd
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 123e1147809a7fcde2ac5b189ad32b0417242f70
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096096"
---
# <a name="count_big-transact-sql"></a>COUNT_BIG (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Эта функция возвращает количество элементов, найденных в группе. Функция `COUNT_BIG` работает подобно функции [COUNT](../../t-sql/functions/count-transact-sql.md). Эти функции различаются только типами данных в возвращаемых значениях. Функция `COUNT_BIG` всегда возвращает значение типа данных **bigint**. Функция `COUNT` всегда возвращает значение типа данных **int**.
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql

-- Aggregation Function Syntax  
COUNT_BIG ( { [ [ ALL | DISTINCT ] expression ] | * } )  
  
-- Analytic Function Syntax  
COUNT_BIG ( [ ALL ] { expression | * } ) OVER ( [ <partition_by_clause> ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
ALL  
Применяет агрегатную функцию ко всем значениям. Аргумент ALL используется по умолчанию.
  
DISTINCT  
Указывает, что функция `COUNT_BIG` возвращает количество уникальных значений, не равных NULL.
  
*expression*  
[Выражение](../../t-sql/language-elements/expressions-transact-sql.md) любого типа данных. Функция `COUNT_BIG` не поддерживает агрегатные функции и вложенные запросы в выражении.
  
*\**  
Указывает, что функция `COUNT_BIG` должна учитывать все строки, чтобы определить общее количество строк таблицы для возврата. Функция `COUNT_BIG(*)` не принимает параметры и не поддерживает использование аргумента DISTINCT. Для функции `COUNT_BIG(*)` не требуется параметр *expression*, так как по определению она не использует сведения о конкретном столбце. Функция `COUNT_BIG(*)` возвращает количество строк в указанной таблице с учетом повторяющихся строк. Каждая строка учитывается отдельно, включая строки со значением NULL.
  
OVER **(** [ *partition_by_clause* ] [ *order_by_clause* ] **)**  
*partition_by_clause* делит результирующий набор, полученный с помощью предложения `FROM`, на секции, к которым применяется функция `COUNT_BIG`. Если этот параметр не указан, функция обрабатывает все строки результирующего набора запроса как отдельные группы. *order_by_clause* определяет логический порядок выполнения операции. Дополнительные сведения см. в статье [SELECT — предложение OVER (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md).
  
## <a name="return-types"></a>Типы возвращаемых данных
**bigint**
  
## <a name="remarks"></a>Remarks  
Функция COUNT_BIG(\*) возвращает количество элементов в группе. Сюда входят значения NULL и повторяющиеся значения.
  
Функция COUNT_BIG (ALL *expression*) вычисляет *expression* для каждой строки в группе и возвращает количество значений, не равных NULL.
  
Функция COUNT_BIG (DISTINCT *expression*) вычисляет *expression* для каждой строки в группе и возвращает количество уникальных значений, не равных NULL.
  
COUNT_BIG — это детерминированная функция, если она используется **_без_** предложений OVER и ORDER BY. Она не является детерминированой при использовании **_с_** предложениями OVER и ORDER BY. Дополнительные сведения см. в статье [Детерминированные и недетерминированные функции](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).
  
## <a name="examples"></a>Примеры  
Примеры см. в статье [Функция COUNT (Transact-SQL)](../../t-sql/functions/count-transact-sql.md).
  
## <a name="see-also"></a>См. также
[Агрегатные функции (Transact-SQL)](../../t-sql/functions/aggregate-functions-transact-sql.md)  
[COUNT (Transact-SQL)](../../t-sql/functions/count-transact-sql.md)  
[int, bigint, smallint и tinyint (Transact-SQL)](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)  
[Предложение OVER (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md)
  
  
