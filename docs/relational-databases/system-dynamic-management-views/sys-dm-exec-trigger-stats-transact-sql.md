---
description: sys.dm_exec_trigger_stats (Transact-SQL)
title: sys.dm_exec_trigger_stats (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/03/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_exec_trigger_stats
- dm_exec_trigger_stats_TSQL
- sys.dm_exec_trigger_stats_TSQL
- sys.dm_exec_trigger_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_trigger_stats dynamic management function
ms.assetid: 863498b4-849c-434d-b748-837411458738
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 04a931ffe95e58f0db20de619df48f16464f1e01
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097695"
---
# <a name="sysdm_exec_trigger_stats-transact-sql"></a>sys.dm_exec_trigger_stats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает суммарную статистику производительности для кэшированных триггеров. Представление содержит одну строку для каждого триггера, а время существования строки равно времени пребывания триггера в кэше. Когда триггер удаляется из кэша, соответствующая строка исключается из данного представления. В этот момент возникает событие трассировки SQL статистики производительности аналогично **sys.dm_exec_query_stats**.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|Идентификатор базы данных, в которой располагается триггер.|  
|**object_id**|**int**|Идентификатор триггера.|  
|**type**|**char(2)**|Тип объекта:<br /><br /> TA = триггер сборки (среда CLR)<br /><br /> TR = триггер SQL|  
|**Type_desc**|**nvarchar(60)**|Описание типа объекта:<br /><br /> CLR_TRIGGER<br /><br /> SQL_TRIGGER|  
|**sql_handle**|**varbinary (64)**|Это можно использовать для сопоставления с запросами в **sys.dm_exec_query_stats** , которые были выполнены в этом триггере.|  
|**plan_handle**|**varbinary (64)**|Идентификатор плана в оперативной памяти. Этот идентификатор является временным и константным, только пока план сохраняется в кэше. Это значение можно использовать с динамическим административным представлением **sys.dm_exec_cached_plans** .|  
|**cached_time**|**datetime**|Время, когда триггер был добавлен в кэш.|  
|**last_execution_time**|**datetime**|Время последнего выполнения триггера.|  
|**execution_count**|**bigint**|Количество выполнений триггера со времени последней компиляции.|  
|**total_worker_time**|**bigint**|Общее количество времени ЦП, затраченное на выполнение триггера с момента его компиляции, в микросекундах.|  
|**last_worker_time**|**bigint**|Время ЦП, затраченное на последнее выполнение триггера, в микросекундах.|  
|**min_worker_time**|**bigint**|Максимальное время ЦП в микросекундах, которое этот триггер когда-либо использовал во время одного выполнения.|  
|**max_worker_time**|**bigint**|Максимальное время ЦП в микросекундах, которое этот триггер когда-либо использовал во время одного выполнения.|  
|**total_physical_reads**|**bigint**|Общее число физических операций чтения, выполненных выполнением триггера с момента его компиляции.|  
|**last_physical_reads**|**bigint**|Число операций физического считывания, выполненных при последнем выполнении триггера.|  
|**min_physical_reads**|**bigint**|Минимальное число физических операций чтения, которые этот триггер когда-либо выполнял во время одного выполнения.|  
|**max_physical_reads**|**bigint**|Максимальное число физических операций чтения, которые этот триггер когда-либо выполнял во время одного выполнения.|  
|**total_logical_writes**|**bigint**|Общее число логических операций записи, выполненных выполнением триггера с момента его компиляции.|  
|**last_logical_writes**|**bigint**|Число операций логической записи, выполненных при последнем выполнении триггера.|  
|**min_logical_writes**|**bigint**|Минимальное число логических операций записи, которые этот триггер когда-либо выполнял во время одного выполнения.|  
|**max_logical_writes**|**bigint**|Максимальное число логических операций записи, которые этот триггер когда-либо выполнял во время одного выполнения.|  
|**total_logical_reads**|**bigint**|Общее число логических операций чтения, выполненных выполнением триггера с момента его компиляции.|  
|**last_logical_reads**|**bigint**|Число операций логического считывания за время последнего выполнения триггера.|  
|**min_logical_reads**|**bigint**|Минимальное число операций логического считывания, которые этот триггер когда-либо выполнял во время одного выполнения.|  
|**max_logical_reads**|**bigint**|Максимальное число операций логического считывания, которые этот триггер когда-либо выполнял во время одного выполнения.|  
|**total_elapsed_time**|**bigint**|Общее время, затраченное на выполнение триггера, в микросекундах.|  
|**last_elapsed_time**|**bigint**|Время, затраченное на последнее выполнение триггера, в микросекундах.|  
|**min_elapsed_time**|**bigint**|Минимальное время, затраченное на выполнение триггера, в микросекундах.|  
|**max_elapsed_time**|**bigint**|Максимальное время, затраченное на выполнение триггера, в микросекундах.| 
|**total_spills**|**bigint**|Общее число страниц, сброшенных при выполнении этого триггера с момента его компиляции.<br /><br /> Область **применения**: начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**last_spills**|**bigint**|Число страниц, сброшенных при последнем выполнении триггера.<br /><br /> Область **применения**: начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**min_spills**|**bigint**|Минимальное число страниц, которые этот триггер когда-либо заблокировал во время одного выполнения.<br /><br /> Область **применения**: начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**max_spills**|**bigint**|Максимальное число страниц, которые этот триггер когда-либо заблокировал во время одного выполнения.<br /><br /> Область **применения**: начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**total_page_server_reads**|**bigint**|Общее число операций чтения сервера страниц, выполненных с момента компиляции триггера.<br /><br /> Область **применения**: масштабирование базы данных SQL Azure|  
|**last_page_server_reads**|**bigint**|Число операций чтения сервера, выполненных при последнем выполнении триггера.<br /><br /> Область **применения**: масштабирование базы данных SQL Azure|  
|**min_page_server_reads**|**bigint**|Минимальное число серверных страниц считывает, что этот триггер когда-либо выполнялся во время одного выполнения.<br /><br /> Область **применения**: масштабирование базы данных SQL Azure|  
|**max_page_server_reads**|**bigint**|Максимальное количество операций чтения сервера, которое этот триггер выполнял во время одного выполнения.<br /><br /> Область **применения**: масштабирование базы данных SQL Azure|  

  
## <a name="remarks"></a>Комментарии  
 Динамические административные представления в среде [!INCLUDE[ssSDS](../../includes/sssds-md.md)] не могут предоставлять информацию, которая может повлиять на автономность базы данных, или информацию о других базах данных, к которым имеет доступ пользователь. Во избежание раскрытия этой информации все строки, содержащие данные, не принадлежащие подключенному клиенту, отфильтровываются.  

Статистика в представлении обновляется после завершения выполнения запроса.  
  
## <a name="permissions"></a>Разрешения  

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах `Server admin` `Azure Active Directory admin` требуется учетная запись или. Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   
  
## <a name="examples"></a>Примеры  
 В следующем примере возвращаются сведения о пяти первых триггерах, идентифицируемых по среднему затраченному времени.  
  
```sql  
SELECT TOP 5 d.object_id, d.database_id, DB_NAME(database_id) AS 'database_name',   
    OBJECT_NAME(object_id, database_id) AS 'trigger_name', d.cached_time,  
    d.last_execution_time, d.total_elapsed_time,   
    d.total_elapsed_time/d.execution_count AS [avg_elapsed_time],   
    d.last_elapsed_time, d.execution_count  
FROM sys.dm_exec_trigger_stats AS d  
ORDER BY [total_worker_time] DESC;  
```  
  
## <a name="see-also"></a>См. также:  
[Динамические административные представления и функции, связанные с выполнением &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
[sys.dm_exec_sql_text &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
[sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
[sys.dm_exec_procedure_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)   
[sys.dm_exec_cached_plans (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)  
  
  
