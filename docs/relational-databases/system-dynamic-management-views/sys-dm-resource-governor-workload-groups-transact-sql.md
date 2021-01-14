---
description: sys.dm_resource_governor_workload_groups (Transact-SQL)
title: sys.dm_resource_governor_workload_groups (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/15/2020
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_resource_governor_workload_groups
- sys.dm_resource_governor_workload_groups_TSQL
- dm_resource_governor_workload_groups
- dm_resource_governor_workload_groups_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_resource_governor_workload_groups dynamic management view
ms.assetid: f63c4914-1272-43ef-b135-fe1aabd953e0
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2db4a28a9199cc090f18cc1f79d8e755c621c7fd
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172146"
---
# <a name="sysdm_resource_governor_workload_groups-transact-sql"></a>sys.dm_resource_governor_workload_groups (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает статистику группы рабочей нагрузки и текущую конфигурацию группы рабочей нагрузки в памяти. Это представление можно объединить с представлением sys.dm_resource_governor_resource_pools для получения имени пула ресурсов.  
  
> [!NOTE]  
>  Чтобы вызвать эту функцию из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте имя **sys.dm_pdw_nodes_resource_governor_workload_groups**.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|group_id|**int**|Идентификатор группы рабочей нагрузки. Не допускает значение NULL.|  
|name|**sysname**|Имя группы рабочей нагрузки. Не допускает значение NULL.|  
|pool_id|**int**|Идентификатор пула ресурсов. Не допускает значение NULL.|  
|external_pool_id|**int**|**Применимо к**: начиная с [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] .<br /><br /> ИДЕНТИФИКАТОР внешнего пула ресурсов. Не допускает значение NULL.|  
|statistics_start_time|**datetime**|Время, когда был выполнен сброс коллекции статистики для группы рабочей нагрузки. Не допускает значение NULL.|  
|total_request_count|**bigint**|Совокупное количество выполненных запросов в группе рабочей нагрузки. Не допускает значение NULL.|  
|total_queued_request_count|**bigint**|Совокупное количество запросов, помещенных в очередь по достижении предельного значения GROUP_MAX_REQUESTS. Не допускает значение NULL.|  
|active_request_count|**int**|Текущее количество запросов. Не допускает значение NULL.|  
|queued_request_count|**int**|Текущее количество запросов, помещенных в очередь. Не допускает значение NULL.|  
|total_cpu_limit_violation_count|**bigint**|Совокупное количество запросов, превышающих предельное значение, заданное для ЦП. Не допускает значение NULL.|  
|total_cpu_usage_ms|**bigint**|Совокупное использование ЦП, в миллисекундах, для группы рабочей нагрузки. Не допускает значение NULL.|  
|max_request_cpu_time_ms|**bigint**|Максимальное использование ЦП, в миллисекундах, для отдельного запроса. Не допускает значение NULL.<br /><br /> **Примечание.** Это измеряемое значение, в отличие от request_max_cpu_time_sec, которое является настраиваемым параметром. Дополнительные сведения см. в разделе [Класс событий CPU Threshold Exceeded](../../relational-databases/event-classes/cpu-threshold-exceeded-event-class.md).|  
|blocked_task_count|**int**|Текущее количество заблокированных задач. Не допускает значение NULL.|  
|total_lock_wait_count|**bigint**|Совокупное количество возникших ожиданий блокировок. Не допускает значение NULL.|  
|total_lock_wait_time_ms|**bigint**|Совокупная продолжительность блокировки в миллисекундах. Не допускает значение NULL.|  
|total_query_optimization_count|**bigint**|Совокупное количество операций по оптимизации запросов в данной группе рабочей нагрузки. Не допускает значение NULL.|  
|total_suboptimal_plan_generation_count|**bigint**|Совокупное количество неоптимальных планов, созданных в данной группе рабочей нагрузки по причине нехватки памяти. Не допускает значение NULL.|  
|total_reduced_memgrant_count|**bigint**|Совокупное количество операций предоставления памяти, достигших максимально допустимого размера запроса. Не допускает значение NULL.|  
|max_request_grant_memory_kb|**bigint**|Максимальный объем предоставленной памяти, в килобайтах, для отдельного запроса после сброса статистики. Не допускает значение NULL.|  
|active_parallel_thread_count|**bigint**|Текущее количество используемых параллельных потоков. Не допускает значение NULL.|  
|importance|**sysname**|Текущее значение конфигурации для относительной важности запроса в данной группе рабочей нагрузки. Параметр важность имеет одно из следующих значений, где Medium — это значение по умолчанию: низкий, средний или высокий.<br /><br /> Не допускает значение NULL.|  
|request_max_memory_grant_percent|**int**|Текущее значение параметра максимального объема предоставляемой памяти, в процентах, для отдельного запроса. Не допускает значение NULL.|  
|request_max_cpu_time_sec|**int**|Текущее значение параметра максимально допустимого использования ЦП, в секундах, для отдельного запроса. Не допускает значение NULL.|  
|request_memory_grant_timeout_sec|**int**|Текущее значение параметра времени ожидания предоставления, в секундах, для отдельного запроса. Не допускает значение NULL.|  
|group_max_requests|**int**|Текущее значение параметра максимального числа параллельных запросов. Не допускает значение NULL.|  
|max_dop|**int**|Настроена максимальная степень параллелизма для группы рабочей нагрузки. Для значения по умолчанию 0 используются глобальные параметры. Не допускает значение NULL.| 
|effective_max_dop|**int**|**Применимо к**: начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] .<br /><br />Эффективная максимальная степень параллелизма для группы рабочей нагрузки. Не допускает значение NULL.| 
|total_cpu_usage_preemptive_ms|**bigint**|**Применимо к**: начиная с [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] .<br /><br />Общее время ЦП, используемое в планировщике в режиме с вытеснением для группы рабочей нагрузки, измеряется в мс. Не допускает значение NULL.<br /><br />Чтобы выполнить код, внешний по отношению к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (например, расширенную хранимую процедуру или распределенный запрос), поток должен выйти из-под управления планировщика, работающего в режиме без вытеснения. Для этого исполнитель переходит в режим с вытеснением.| 
|request_max_memory_grant_percent_numeric|**float**|**Применимо к**: начиная с [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] .<br /><br />Текущее значение параметра максимального объема предоставляемой памяти, в процентах, для отдельного запроса. Не допускает значение NULL.| 
|pdw_node_id|**int**|**Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение.|  
  
## <a name="remarks"></a>Примечания  
 Данное динамическое административное представление отображает конфигурацию, хранимую в памяти. Чтобы просмотреть метаданные сохраненной конфигурации, используйте представление каталога [&#41;sys.resource_governor_workload_groups &#40;Transact-SQL ](../../relational-databases/system-catalog-views/sys-resource-governor-workload-groups-transact-sql.md) .  
  
 При `ALTER RESOURCE GOVERNOR RESET STATISTICS` успешном выполнении следующие счетчики сбрасываются: `statistics_start_time` , `total_request_count` ,, `total_queued_request_count` , `total_cpu_limit_violation_count` `total_cpu_usage_ms` , `max_request_cpu_time_ms` , `total_lock_wait_count` , `total_lock_wait_time_ms` , `total_query_optimization_count` , `total_suboptimal_plan_generation_count` , `total_reduced_memgrant_count` и `max_request_grant_memory_kb` . Счетчику `statistics_start_time` присваивается значение текущей системной даты и времени, а другим счетчикам присваивается нулевое значение (0).  
  
## <a name="permissions"></a>Разрешения  
 Требуется разрешение `VIEW SERVER STATE`.  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [sys.dm_resource_governor_resource_pools &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md)   
 [sys.resource_governor_workload_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-resource-governor-workload-groups-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR (Transact-SQL)](../../t-sql/statements/alter-resource-governor-transact-sql.md)  
  
