---
description: sys.dm_db_xtp_nonclustered_index_stats (Transact-SQL)
title: sys.dm_db_xtp_nonclustered_index_stats (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_db_xtp_nonclustered_index_stats_TSQL
- dm_db_xtp_nonclustered_index_stats
- sys.dm_db_xtp_nonclustered_index_stats_TSQL
- sys.dm_db_xtp_nonclustered_index_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_nonclustered_index_stats
ms.assetid: d55ba31c-296c-419b-9c4b-c126e0a3d156
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1c093479ffad2ea852d9af0806f0e78ac907773a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099887"
---
# <a name="sysdm_db_xtp_nonclustered_index_stats-transact-sql"></a>sys.dm_db_xtp_nonclustered_index_stats (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Динамическое административное представление sys.dm_db_xtp_nonclustered_index_stats содержит статистику операций с некластеризованными индексами в оптимизированных для памяти таблицах. Динамическое административное представление sys.dm_db_xtp_nonclustered_index_stats содержит по одной строке для каждого некластеризованного индекса в оптимизированной для памяти таблице в текущей базе данных.  
  
 Статистика, приведенная в sys.dm_db_xtp_nonclustered_index_stats, собирается во время создания структуры индекса в памяти. Структуры индекса в памяти создаются повторно при перезапуске базы данных.  
  
 Для отслеживания работы индексов и выявления закономерностей при выполнении операций DML и в случае, если база данных переводится в режим «в сети», используйте представление sys.dm_db_xtp_nonclustered_index_stats. При перезапуске базы данных с оптимизированной для памяти таблицей индекс строится путем вставки в память по одной строке за раз. Количество разбиений, объединений и консолидаций страниц позволяет понять, какая работа была проделана для построения индекса при переводе базы данных в режим «в сети». Эти значения также можно оценить до и после выполнения серии операций DML.  
  
 Большое количество повторных попыток указывает на проблему с параллелизмом. Обратитесь в службу поддержки [!INCLUDE[msCoName](../../includes/msconame-md.md)].  
  
 Дополнительные сведения о оптимизированных для памяти некластеризованных индексах см. в разделе [Общие сведения о SQL Server In-Memory OLTP "Обзор](https://t.co/T6zToWc6y6)", стр. 17.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|object_id|**int**|Идентификатор объекта.|  
|xtp_object_id|**bigint**|Идентификатор оптимизированной для памяти таблицы.|  
|index_id|**int**|Идентификатор индекса.|  
|delta_pages|**bigint**|Общее число разностных страниц для этого индекса в дереве.|  
|internal_pages|**bigint**|Для внутреннего использования. Общее число внутренних страниц для этого индекса в дереве.|  
|leaf_pages|**bigint**|Общее число конечных страниц для этого индекса в дереве.|  
|outstanding_retired_nodes|**bigint**|Для внутреннего использования. Отображает общее число узлов для этого индекса во внутренних структурах.|  
|page_update_count|**bigint**|Совокупное количество операций, обновляющих страницу в индексе.|  
|page_update_retry_count|**bigint**|Совокупное количество повторных попыток выполнить операцию, обновляющую страницу в индексе.|  
|page_consolidation_count|**bigint**|Совокупное количество консолидаций страниц в индексе.|  
|page_consolidation_retry_count|**bigint**|Совокупное количество повторных попыток выполнить операцию консолидации страниц.|  
|page_split_count|**bigint**|Совокупное количество операций разбиения страниц в индексе.|  
|page_split_retry_count|**bigint**|Совокупное количество повторных попыток выполнить операцию разбиения страниц.|  
|key_split_count|**bigint**|Совокупное количество разбиения ключей в индексе.|  
|key_split_retry_count|**bigint**|Совокупное количество повторных попыток выполнить операцию разбиения ключа.|  
|page_merge_count|**bigint**|Совокупное количество операций объединения страниц в индексе.|  
|page_merge_retry_count|**bigint**|Совокупное количество повторных попыток выполнить операцию объединения страниц.|  
|key_merge_count|**bigint**|Совокупное количество операций объединения ключей в индексе.|  
|key_merge_retry_count|**bigint**|Совокупное количество повторных попыток выполнить операцию объединения ключей.|  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение VIEW DATABASE STATE на текущую базу данных.  
  
## <a name="see-also"></a>См. также:  
 [Оптимизированные для памяти динамические административные представления таблиц &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
