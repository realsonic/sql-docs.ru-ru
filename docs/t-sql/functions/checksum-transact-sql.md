---
description: CHECKSUM (Transact-SQL)
title: CHECKSUM (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CHECKSUM_TSQL
- CHECKSUM
dev_langs:
- TSQL
helpviewer_keywords:
- hash indexes
- CHECKSUM function
- checksum values
ms.assetid: e26d3339-845c-49c2-9d89-243376874c13
author: cawrites
ms.author: chadam
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3534f5011275035b11adc1984604f8795f4208f2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092394"
---
# <a name="checksum-transact-sql"></a>CHECKSUM (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Функция `CHECKSUM` возвращает значение контрольной суммы, вычисленное для строки таблицы или для списка выражений. Используйте функцию `CHECKSUM` для построения хэш-индексов.
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
CHECKSUM ( * | expression [ ,...n ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Аргументы
\*  
Этот аргумент указывает, что вычисление контрольной суммы охватывает все столбцы таблицы. Функция `CHECKSUM` возвращает ошибку, если какой-либо столбец имеет несопоставимый тип данных. Перечень несопоставимых типов данных:

- **курсор**
- **image**
- **ntext**
- **text**
- **XML**

Несопоставимым типом данных также является **sql_variant** с любым из вышеперечисленных типов в качестве базового типа.
  
*expression*  
[Выражение](../../t-sql/language-elements/expressions-transact-sql.md) любого типа, за исключением несопоставимого типа данных.
  
## <a name="return-types"></a>Типы возвращаемых данных
 **int**  
  
## <a name="remarks"></a>Remarks  
Функция `CHECKSUM` вычисляет хэш-значение, которое называется контрольной суммой, для списка своих аргументов. Используйте это хэш-значение для построения хэш-индексов. Если аргументы функции `CHECKSUM` являются столбцами, а индекс построен на рассчитанном значении `CHECKSUM`, то в результате получится хэш-индекс. Он может использоваться для поиска равенств в столбцах.
  
Функция `CHECKSUM` удовлетворяет свойствам хэш-функции. Функция `CHECKSUM`, примененная к любым двум спискам выражений, возвращает одинаковое значение, если при сравнении оператором равенства (=) соответствующие элементы двух списков имеют одинаковый тип и равны. Для данного определения значения NULL указанного типа рассматриваются при сравнении с помощью функции `CHECKSUM` как равные. Если хотя бы одно из значений в списке выражений меняется, то обычно меняется и контрольная сумма выражений. Однако это не гарантируется. Таким образом, чтобы определить факт изменения значений, рекомендуется использовать функцию `CHECKSUM` только в том случае, если в вашем приложении допускается случайный пропуск изменений. В противном случае рекомендуется вместо этого использовать функцию `HASHBYTES`. Когда указан хэш-алгоритм MD5, вероятность возвращения функцией `HASHBYTES` одинакового результата для двух различных входных параметров намного ниже по сравнению с функцией `CHECKSUM`.
  
Порядок выражений влияет на вычисляемое значение `CHECKSUM`. Порядок столбцов, используемый с `CHECKSUM(*)`, является порядком столбцов, указанным в таблице или определении представления. Он включает в себя вычисляемые столбцы.
  
Значение `CHECKSUM` зависит от параметров сортировки. Такое же значение, сохраненное с другими параметрами сортировки, возвратит другое значение `CHECKSUM`.
  
`CHECKSUM ()` не гарантирует уникальные результаты.

## <a name="examples"></a>Примеры  
В этих примерах показывается использование функции `CHECKSUM` для построения хэш-индексов.
  
Чтобы построить хэш-индекс, в первом примере в таблицу, которую требуется индексировать, добавляется столбец вычисленной контрольной суммы. После этого создается индекс на основе столбца контрольной суммы. 
  
```sql
-- Create a checksum index.  

SET ARITHABORT ON;  
USE AdventureWorks2012;   
GO  
ALTER TABLE Production.Product  
ADD cs_Pname AS CHECKSUM(Name);  
GO  
CREATE INDEX Pname_index ON Production.Product (cs_Pname);  
GO  
```  
  
В этом примере показано использование индекса контрольной суммы в качестве хэш-индекса. Это позволяет повысить скорость индексирования в том случае, если столбец индекса содержит длинные символьные значения. Индекс контрольной суммы может быть использован для поиска равенств.
  
```sql
/*Use the index in a SELECT query. Add a second search   
condition to catch stray cases where checksums match,   
but the values are not the same.*/  

SELECT *   
FROM Production.Product  
WHERE CHECKSUM(N'Bearing Ball') = cs_Pname  
AND Name = N'Bearing Ball';  
GO  
```  
  
Создание индекса по вычисляемому столбцу материализует столбец контрольной суммы, и любые изменения для значения `ProductName` будут распространены на столбец контрольной суммы. Кроме того, индекс можно построить непосредственно на основе столбца, который требуется индексировать. Тем не менее для ключей с длинными значениями обычный индекс действует не так эффективно, как индекс контрольной суммы.
  
## <a name="see-also"></a>См. также раздел
[CHECKSUM_AGG (Transact-SQL)](../../t-sql/functions/checksum-agg-transact-sql.md)  
[HASHBYTES (Transact-SQL)](../../t-sql/functions/hashbytes-transact-sql.md)  
[BINARY_CHECKSUM (Transact-SQL)](../../t-sql/functions/binary-checksum-transact-sql.md)
  
  
