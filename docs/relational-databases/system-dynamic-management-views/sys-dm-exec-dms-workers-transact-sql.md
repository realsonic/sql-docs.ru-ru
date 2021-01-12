---
description: sys.dm_exec_dms_workers (Transact-SQL)
title: sys.dm_exec_dms_workers (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- SYS.DM_EXEC_DMS_WORKERS_TSQL
- DM_EXEC_DMS_WORKERS_TSQL
- DM_EXEC_DMS_WORKERS
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase,views
- PolyBase
- dm_exec_dms_workers management view
- sys.dm_exec_dms_workers management view
ms.assetid: f468da29-78c3-4f10-8a3c-17905bbf46f2
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: dfcf81fb2a49278c4a65ddc3e1e532d666542a20
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092852"
---
# <a name="sysdm_exec_dms_workers-transact-sql"></a>sys.dm_exec_dms_workers (Transact-SQL)
[!INCLUDE [sqlserver2016-asa-pdw](../../includes/applies-to-version/sqlserver2016-asa-pdw.md)]

  Содержит сведения обо всех рабочих ролях, завершающих шаги DMS.  
  
 В этом представлении отображаются данные за последние 1000 запросов и активных запросов. Активные запросы всегда имеют данные, представленные в этом представлении.  
  
|Имя столбца|Тип данных|Description|Диапазон|  
|-----------------|---------------|-----------------|-----------|  
|execution_id|`nvarchar(32)`|Запрос, частью которого является эта Рабочая роль DMS. <br /><br /> execution_id, step_index и dms_step_index образуют ключ для этого представления.||  
|step_index|`int`|Шаг запроса, частью которого является Рабочая роль DMS.|См. раздел индекс шага в [sys.dm_exec_distributed_request_steps &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-distributed-request-steps-transact-sql.md).|  
|dms_step_index|`int`|Шаг в плане DMS, в котором выполняется этот рабочий процесс.|См. [sys.dm_exec_dms_workers (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-dms-workers-transact-sql.md)|  
|compute_node_id|`int`|Узел, на котором запущена Рабочая роль.|См. раздел [sys.dm_exec_compute_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md).|  
|distribution_id|`int`|||  
|тип|`nvarchar(32)`|Тип рабочего потока DMS, представляемый этой записью.|"DIRECT_CONVERTER", "DIRECT_READER", "FILE_READER", "HASH_CONVERTER", "HASH_READER", "ROUNDROBIN_CONVERTER", "EXPORT_READER", "EXTERNAL_READER", "EXTERNAL_WRITER", "PARALLEL_COPY_READER", "REJECT_WRITER", "WRITER"|  
|status|`nvarchar(32)`|Состояние этого шага|"Pending", "работает", "Complete", "Failed", "Ундофаилед", "Пендингканцел", "recommit", "Undone", "Abortd"|  
|bytes_per_sec|`bigint`|||  
|bytes_processed|`bigint`|||  
|rows_processed|`bigint`|||  
|start_time|`datetime`|Время начала выполнения этапа|Меньше или равно текущему времени и больше или равно end_compile_time запроса, к которому относится этот шаг.|  
|end_time|`datetime`|Время, когда выполнение этого шага было завершено, было отменено или завершилось сбоем.|Меньшее или равное текущему времени и больше или равно start_time, установите значение NULL для шагов, выполняемых в данный момент или в очереди.|  
|total_elapsed_time|`int`|Общее количество времени выполнения шага запроса (в миллисекундах)|Между 0 и разностью между end_time и start_time. 0 для шагов в очереди.|  
|cpu_time|`bigint`|||  
|query_time|`int`|||  
|buffers_available|`int`|||  
|dms_cpid|`int`|||  
|sql_spid|`int`|||  
|error_id|`nvarchar(36)`|||  
|source_info|`nvarchar(4000)`|||  
|destination_info|`nvarchar(4000)`|||  
|.|`nvarchar(4000)`|||
|compute_pool_id|`int`|Уникальный идентификатор пула.|

## <a name="see-also"></a>См. также:  
 [Устранение неполадок в Polybase с помощью динамических административных представлений](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))   
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Динамические административные представления, связанные с базами данных &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
