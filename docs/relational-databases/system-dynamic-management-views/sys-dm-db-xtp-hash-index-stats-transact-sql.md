---
description: sys.dm_db_xtp_hash_index_stats (Transact-SQL)
title: sys.dm_db_xtp_hash_index_stats (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_db_xtp_hash_index_stats
- sys.dm_db_xtp_hash_index_stats_TSQL
- dm_db_xtp_hash_index_stats
- dm_db_xtp_hash_index_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_hash_index_stats (dynamic management view)
ms.assetid: 45969884-cd61-48e8-aee5-c725c78e3e4c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 239ca2e5a60f8ffeeafd807316e5a903dc840754
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092906"
---
# <a name="sysdm_db_xtp_hash_index_stats-transact-sql"></a>sys.dm_db_xtp_hash_index_stats (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Эти статистические данные могут быть полезными для понимания и настройки числа контейнеров. Их также можно использовать, чтобы определить наличие повторяющихся ключей индекса.  
  
 Большая средняя длина цепочки означает, что множество строк хэшируется в один контейнер. Это могло произойти по следующим причинам.  
  
-   Если количество пустых контейнеров небольшое или средняя и максимальная длина цепочки близки по значению, вероятно, что общее число контейнеров слишком мало. Это приводит к тому, что разные ключи индекса хэшируются в один контейнер.  
  
-   Если пустых контейнеров много или максимальная длина цепочки намного больше средней длины, вероятно, что существует множество строк с повторяющимися значениями ключа индекса или в значениях ключей возникло искажение. Все строки с одним значением ключа индекса хэшируются в один контейнер, следовательно, длина цепочки в этом контейнере высока.  
  
Длинные цепочки могут негативно повлиять на производительность всех операций DML в отдельных строках, в том числе SELECT и INSERT. Короткие цепочки и большое число пустых контейнеров указывают на то, что значение bucket_count слишком велико. Это приводит к снижению производительности просмотра индекса.  
  
> [!WARNING]
> **sys.dm_db_xtp_hash_index_stats** сканирует всю таблицу. Таким образом, если в базе данных есть большие таблицы, **sys.dm_db_xtp_hash_index_stats** может занять длительное время.  
  
Дополнительные сведения см. в статье [хэш-индексы для Memory-Optimized таблиц](../../relational-databases/sql-server-index-design-guide.md#hash_index).  
  
|Имя столбца|Тип|Описание|  
|-----------------|----------|-----------------|  
|object_id|**int**|Идентификатор объекта родительской таблицы.|  
|xtp_object_id|**bigint**|Идентификатор оптимизированной для памяти таблицы.|  
|index_id|**int**|Идентификатор индекса.|  
|total_bucket_count|**bigint**|Общее число хэш-контейнеров в индексе.|  
|empty_bucket_count|**bigint**|Общее число пустых хэш-контейнеров в индексе.|  
|avg_chain_length|**bigint**|Средняя длина цепочек строк во всех хэш-контейнерах в индексе.|  
|max_chain_length|**bigint**|Максимальная длина цепочек строки в хэш-контейнерах.|  
|xtp_object_id|**bigint**|Идентификатор объекта выполняющейся в памяти OLTP, соответствующий таблице, оптимизированной для памяти.|  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение VIEW DATABASE STATE на сервере.  

## <a name="examples"></a>Примеры  
  
### <a name="a-troubleshooting-hash-index-bucket-count"></a>A. Устранение неполадок, связанных с числом контейнеров хэш-индекса

Следующий запрос можно использовать для устранения неполадок с числом контейнеров хэш-индекса существующей таблицы. Запрос возвращает статистику о проценте пустых контейнеров и длине цепочки для всех хэш-индексов в пользовательских таблицах.

```sql
  SELECT  
    QUOTENAME(SCHEMA_NAME(t.schema_id)) + N'.' + QUOTENAME(OBJECT_NAME(h.object_id)) as [table],   
    i.name                   as [index],   
    h.total_bucket_count,  
    h.empty_bucket_count,  
    FLOOR((  
      CAST(h.empty_bucket_count as float) /  
        h.total_bucket_count) * 100)  
                             as [empty_bucket_percent],  
    h.avg_chain_length,   
    h.max_chain_length  
  FROM sys.dm_db_xtp_hash_index_stats as h   
  INNER JOIN sys.indexes as i  
            ON h.object_id = i.object_id  
           AND h.index_id  = i.index_id  
    INNER JOIN sys.memory_optimized_tables_internal_attributes ia ON h.xtp_object_id=ia.xtp_object_id
    INNER JOIN sys.tables t on h.object_id=t.object_id
  WHERE ia.type=1
  ORDER BY [table], [index];  
``` 

Дополнительные сведения о том, как интерпретировать результаты этого запроса, см. в разделе [Устранение неполадок хэш-индексов для Memory-Optimized таблиц](../../relational-databases/in-memory-oltp/hash-indexes-for-memory-optimized-tables.md) .  

### <a name="b-hash-index-statistics-for-internal-tables"></a>Б. Статистика хэш-индекса для внутренних таблиц

Некоторые функции используют внутренние таблицы, использующие хэш-индексы, например индексы columnstore в таблицах, оптимизированных для памяти. Следующий запрос возвращает статистику для хэш-индексов во внутренних таблицах, связанных с пользовательскими таблицами.

```sql
  SELECT  
    QUOTENAME(SCHEMA_NAME(t.schema_id)) + N'.' + QUOTENAME(OBJECT_NAME(h.object_id)) as [user_table],
    ia.type_desc as [internal_table_type],
    i.name                   as [index],   
    h.total_bucket_count,  
    h.empty_bucket_count,  
    h.avg_chain_length,   
    h.max_chain_length  
  FROM sys.dm_db_xtp_hash_index_stats as h   
  INNER JOIN sys.indexes as i  
            ON h.object_id = i.object_id  
           AND h.index_id  = i.index_id  
    INNER JOIN sys.memory_optimized_tables_internal_attributes ia ON h.xtp_object_id=ia.xtp_object_id
    INNER JOIN sys.tables t on h.object_id=t.object_id
  WHERE ia.type!=1
  ORDER BY [user_table], [internal_table_type], [index]; 
```

Обратите внимание, что BUCKET_COUNT индекса во внутренних таблицах нельзя изменить, поэтому выходные данные этого запроса должны рассматриваться только как информативные. Никаких действий не требуется.  

Этот запрос не должен возвращать строки, если не используется функция, использующая хэш-индексы во внутренних таблицах. Следующая оптимизированная для памяти таблица содержит индекс columnstore. После создания этой таблицы вы увидите хэш-индексы во внутренних таблицах.

```sql
  CREATE TABLE dbo.table_columnstore
  (
    c1 INT NOT NULL PRIMARY KEY NONCLUSTERED,
    INDEX ix_columnstore CLUSTERED COLUMNSTORE
  ) WITH (MEMORY_OPTIMIZED=ON)
```

## <a name="see-also"></a>См. также:  
 [Оптимизированные для памяти динамические административные представления таблиц &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
