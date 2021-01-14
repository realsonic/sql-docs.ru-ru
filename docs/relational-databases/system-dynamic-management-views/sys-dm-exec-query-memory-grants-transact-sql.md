---
description: sys.dm_exec_query_memory_grants (Transact-SQL)
title: sys.dm_exec_query_memory_grants (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 05/19/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_exec_query_memory_grants_TSQL
- sys.dm_exec_query_memory_grants
- sys.dm_exec_query_memory_grants_TSQL
- dm_exec_query_memory_grants
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_memory_grants dynamic management view
ms.assetid: 2c417747-2edd-4e0d-8a9c-e5f445985c1a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 147bf57a568460c0b1df68fc4e232432bef3b834
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170286"
---
# <a name="sysdm_exec_query_memory_grants-transact-sql"></a>sys.dm_exec_query_memory_grants (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает сведения обо всех запросах, которые были запрошены и ожидают предоставления памяти или предоставил предоставление памяти. Запросы, не требующие предоставления памяти, не будут отображаться в этом представлении. Например, операции сортировки и хэш-соединений имеют доступ к памяти для выполнения запросов, а запросы без предложения **ORDER BY** не будут предоставлять память.  
  
 Динамические административные представления в среде [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] не могут предоставлять информацию, которая может повлиять на автономность базы данных, или информацию о других базах данных, к которым имеет доступ пользователь. Во избежание раскрытия этой информации все строки, содержащие данные, не принадлежащие подключенному клиенту, отфильтровываются. Кроме того, значения в столбцах **scheduler_id**, **wait_order**, **pool_id**, **group_id** фильтруются. значение столбца установлено в NULL.  
  
> [!NOTE]  
> Чтобы вызвать эту функцию из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте имя **sys.dm_pdw_nodes_exec_query_memory_grants**.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**session_id**|**smallint**|Идентификатор (SPID) сеанса, в котором выполняется данный запрос.|  
|**request_id**|**int**|Идентификатор запроса. Уникален в контексте сеанса.|  
|**scheduler_id**|**int**|Идентификатор планировщика, который планирует данный запрос.|  
|**dop**|**smallint**|Степень параллелизма данного запроса.|  
|**request_time**|**datetime**|Дата и время обращения запроса за предоставлением памяти.|  
|**grant_time**|**datetime**|Дата и время, когда запросу была предоставлена память. Возвращает значение NULL, если память еще не была предоставлена.|  
|**requested_memory_kb**|**bigint**|Общий объем запрошенной памяти в килобайтах.|  
|**granted_memory_kb**|**bigint**|Общий объем фактически предоставленной памяти в килобайтах. Может быть значение NULL, если память еще не была предоставлена. Обычно это значение должно быть одинаковым с **requested_memory_kb**. Для создания индекса сервер может разрешить дополнительное предоставление по требованию памяти, объем которой выходит за рамки изначально предоставленной памяти.|  
|**required_memory_kb**|**bigint**|Минимальный объем памяти в килобайтах (КБ), необходимый для выполнения данного запроса. **requested_memory_kb** имеет то же значение или больше, чем это значение.|  
|**used_memory_kb**|**bigint**|Используемый в данный момент объем физической памяти (в килобайтах).|  
|**max_used_memory_kb**|**bigint**|Максимальный объем используемой до данного момента физической памяти в килобайтах.|  
|**query_cost**|**float**|Ожидаемая стоимость запроса.|  
|**timeout_sec**|**int**|Время ожидания данного запроса в секундах до отказа от обращения за предоставлением памяти.|  
|**resource_semaphore_id**|**smallint**|Неуникальный идентификатор семафора ресурса, которого ожидает данный запрос.<br /><br /> **Примечание.** Этот идентификатор уникален в версиях [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , предшествующих [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] . Данное изменение может повлиять на устранение проблем в запросах. Дополнительные сведения см. в подразделе «Замечания» далее в этом разделе.|  
|**queue_id**|**smallint**|Идентификатор ожидающей очереди, в которой данный запрос ожидает предоставления памяти. Значение NULL, если память уже предоставлена.|  
|**wait_order**|**int**|Последовательный порядок ожидающих запросов в указанной очереди **queue_id**. Это значение может измениться для данного запроса, если другие запросы получают доступ к памяти или превышено время ожидания. Значение NULL, если память уже предоставлена.|  
|**is_next_candidate**|**bit**|Является следующим кандидатом на предоставление памяти.<br /><br /> 1 = да<br /><br /> 0 = нет<br /><br /> NULL = память уже предоставлена.|  
|**wait_time_ms**|**bigint**|Время ожидания в миллисекундах. Значение NULL, если память уже предоставлена.|  
|**plan_handle**|**varbinary (64)**|Идентификатор для данного плана запроса. Чтобы извлечь фактический план XML, используйте представление **sys.dm_exec_query_plan**.|  
|**sql_handle**|**varbinary (64)**|Идентификатор текста [!INCLUDE[tsql](../../includes/tsql-md.md)] для данного запроса. Чтобы получить фактический текст [!INCLUDE[tsql](../../includes/tsql-md.md)], используйте представление **sys.dm_exec_sql_text**.|  
|**group_id**|**int**|Идентификатор группы рабочей нагрузки, в которой выполняется данный запрос.|  
|**pool_id**|**int**|Идентификатор пула ресурсов, к которому принадлежит данная группа рабочей нагрузки.|  
|**is_small**|**tinyint**|Значение 1 означает, что для данной операции предоставления памяти используется малый семафор ресурса. Значение 0 означает использование обычного семафора.|  
|**ideal_memory_kb**|**bigint**|Объем, в килобайтах (КБ), предоставленной памяти, необходимый для размещения всех данных в физической памяти. Основывается на оценке количества элементов.|  
|**pdw_node_id**|**int**|Идентификатор узла, на котором находится данное распределение.<br /><br /> **Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] |  
|**reserved_worker_count**|**bigint**|Количество зарезервированных [рабочих потоков](../../relational-databases/thread-and-task-architecture-guide.md#sql-server-task-scheduling).<br /><br />**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. |  
|**used_worker_count**|**bigint**|Число [рабочих потоков](../../relational-databases/thread-and-task-architecture-guide.md#sql-server-task-scheduling) , используемых в данный момент.<br /><br />**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|  
|**max_used_worker_count**|**bigint**|Максимальное число [рабочих потоков](../../relational-databases/thread-and-task-architecture-guide.md#sql-server-task-scheduling) , использованных до этого момента.<br /><br />**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|  
|**reserved_node_bitmap**|**bigint**|Битовая карта узлов NUMA, в которых зарезервированы [рабочие потоки](../../relational-databases/thread-and-task-architecture-guide.md#sql-server-task-scheduling) .<br /><br />**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|  
  
## <a name="permissions"></a>Разрешения  

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] необходимо разрешение `VIEW DATABASE STATE` для базы данных.   
   
## <a name="remarks"></a>Примечания  
 Обычный сценарий отладки для времени ожидания запроса может выглядеть следующим образом:  
  
-   Проверьте общее состояние системной памяти с помощью представлений [sys.dm_os_memory_clerks](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md), [sys.dm_os_sys_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) и различных счетчиков производительности.  
  
-   Проверьте резервирование памяти в процессе выполнения запроса в представлении **sys.dm_os_memory_clerks**, где `type = 'MEMORYCLERK_SQLQERESERVATIONS'`.  
  
-   Проверьте наличие запросов, ожидающих <sup>1</sup> для грантов, с помощью **sys.dm_exec_query_memory_grants**.  
  
    ```sql  
    --Find all queries waiting in the memory queue  
    SELECT * FROM sys.dm_exec_query_memory_grants where grant_time is null  
    ```  
    
    <sup>1</sup> В этом случае типом ожидания, как правило, является RESOURCE_SEMAPHORE. Дополнительные сведения см. в разделе [sys.dm_os_wait_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md). 
  
-   Поиск в кэше для запросов с предоставлением памяти с помощью [sys.dm_exec_cached_plans &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md) и [sys.dm_exec_query_plan &#40;transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)  
  
    ```sql  
    -- retrieve every query plan from the plan cache  
    USE master;  
    GO  
    SELECT * FROM sys.dm_exec_cached_plans cp CROSS APPLY sys.dm_exec_query_plan(cp.plan_handle);  
    GO  
    ```  
  
-   Дополнительно изучите требующие большого объема памяти запросы с помощью представления [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).  
  
    ```sql  
    --Find top 5 queries by average CPU time  
    SELECT TOP 5 total_worker_time/execution_count AS [Avg CPU Time],  
      plan_handle, query_plan   
    FROM sys.dm_exec_query_stats AS qs  
    CROSS APPLY sys.dm_exec_query_plan(qs.plan_handle)  
    ORDER BY total_worker_time/execution_count DESC;  
    GO  
    ```  
  
-   Если неконтролируемый запрос подозрителен, изучите план выполнения из представления [sys.dm_exec_query_plan](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md) и текст пакета из представления [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md).  
  
 Запросы, использующие динамические административные представления, включающие `ORDER BY` или агрегаты, могут увеличить потребление памяти и, таким же, приводят к проблеме, которую они устраняют.  
  
 Регулятор ресурсов позволяет администратору базы данных распределять ресурсы сервера между пулами ресурсов, используя до 64 пулов. Начиная с [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] , каждый пул ведет себя как небольшой независимый экземпляр сервера и требует 2 семафора. Число строк, возвращаемых из **sys.dm_exec_query_resource_semaphores** , может доставлять до 20 раз больше, чем строк, возвращаемых в [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .  
  
## <a name="see-also"></a>См. также:  
 [sys.dm_exec_query_resource_semaphores &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-resource-semaphores-transact-sql.md)     
 [sys.dm_os_wait_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md)     
 [Динамические административные представления и функции, связанные с выполнением &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)    
 [Руководство по архитектуре потоков и задач](../../relational-databases/thread-and-task-architecture-guide.md)   
  
