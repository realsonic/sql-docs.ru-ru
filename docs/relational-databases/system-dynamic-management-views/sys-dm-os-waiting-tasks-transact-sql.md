---
description: sys.dm_os_waiting_tasks (Transact-SQL)
title: sys.dm_os_waiting_tasks (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_waiting_tasks
- sys.dm_os_waiting_tasks_TSQL
- dm_os_waiting_tasks_TSQL
- sys.dm_os_waiting_tasks
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_waiting_tasks dynamic management view
ms.assetid: ca5e6844-368c-42e2-b187-6e5f5afc8df3
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c02bb3523eda26afb5aac7e4e904cb1aec8dd62b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097642"
---
# <a name="sysdm_os_waiting_tasks-transact-sql"></a>sys.dm_os_waiting_tasks (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает сведения об очереди задач, ожидающих освобождения определенного ресурса. Дополнительные сведения о задачах см. в разделе [руководств по архитектуре потоков и задач](../../relational-databases/thread-and-task-architecture-guide.md).
   
> [!NOTE]  
> Чтобы вызвать эту функцию из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте имя **sys.dm_pdw_nodes_os_waiting_tasks**.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**waiting_task_address**|**varbinary(8)**|Адрес ожидающей задачи.|  
|**session_id**|**smallint**|Идентификатор сеанса, связанного с этой задачей.|  
|**exec_context_id**|**int**|Идентификатор контекста выполнения, связанного с этой задачей.|  
|**wait_duration_ms**|**bigint**|Общее время ожидания для этого типа ожиданий в миллисекундах. Это время является инклюзивным **signal_wait_time**.|  
|**wait_type**|**nvarchar(60)**|Имя типа ожидания.|  
|**resource_address**|**varbinary(8)**|Адрес ресурса, освобождения которого ожидает задача.|  
|**blocking_task_address**|**varbinary(8)**|Задача, которая в настоящий момент блокирует этот ресурс.|  
|**blocking_session_id**|**smallint**|Идентификатор сеанса, блокирующего данный запрос. Если этот столбец содержит значение NULL, то запрос не блокирован или сведения о сеансе блокировки недоступны (или не могут быть идентифицированы).<br /><br /> -2 = Блокирующий ресурс принадлежит потерянной распределенной транзакции.<br /><br /> -3 = Блокирующий ресурс принадлежит отложенной транзакции восстановления.<br /><br /> -4 = Идентификатор сеанса владельца кратковременной блокировки не может быть определен из-за внутренних переходов состояния кратковременной блокировки.|  
|**blocking_exec_context_id**|**int**|Идентификатор контекста выполнения блокирующей задачи.|  
|**resource_description**|**nvarchar (3072)**|Описание используемого ресурса. Дополнительные сведения см. в приведенном ниже списке.|  
|**pdw_node_id**|**int**|**Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение.|  
  
## <a name="resource_description-column"></a>Столбец resource_description  
 Столбец resource_description может иметь следующие значения.  
  
 **Владелец ресурса пула потоков:**  
  
-   ThreadPool ID = планировщик\<hex-address>  
  
 **Владелец ресурса параллельного запроса:**  
  
-   Ексчанжеевент ID = {порт | Pipe} \<hex-address> столбцы waittype = \<exchange-wait-type> NodeId =\<exchange-node-id>  
  
 **Exchange-wait-type:**  
  
-   e_waitNone  
  
-   e_waitPipeNewRow  
  
-   e_waitPipeGetRow  
  
-   e_waitSynchronizeConsumerOpen  
  
-   e_waitPortOpen  
  
-   e_waitPortClose  
  
-   e_waitRange  
  
 **Владелец ресурса блокировки:**  
  
-   \<type-specific-description> ID = \<lock-hex-address> режим блокировки = \<mode> ассоЦиатедобжектид =\<associated-obj-id>  
  
     **\<type-specific-description> может быть:**  
  
    -   Для базы данных: databaselock, подресурс = \<databaselock-subresource> DBID =\<db-id>  
  
    -   Для файла: FileLock ИД \<file-id> ресурса = подресурс = \<filelock-subresource> DBID =\<db-id>  
  
    -   Для объекта: objectlock Локкпартитион = \<lock-partition-id> objID = \<obj-id> Resource = \<objectlock-subresource> DBID =\<db-id>  
  
    -   Для страницы: пажелокк ИД = \<file-id> идентификатор страницы = \<page-id> DBID = \<db-id> Resource =\<pagelock-subresource>  
  
    -   Для ключа: кэйлокк хобтид = \<hobt-id> DBID =\<db-id>  
  
    -   Для ЭКСТЕНТа: екстентлокк ИД = \<file-id> идентификатор страницы = \<page-id> DBID =\<db-id>  
  
    -   Для RID: ридлокк ИД = \<file-id> идентификатор страницы = \<page-id> DBID =\<db-id>  
  
    -   Для приложения: аппликатионлокк hash = \<hash> датабасепринЦипалид = \<role-id> DBID =\<db-id>  
  
    -   Для МЕТАДАННЫХ: metadatalock, подресурс = \<metadata-subresource> ClassID = \<metadatalock-description> DBID =\<db-id>  
  
    -   Для HOBT: хобтлокк хобтид = " \<hobt-id> подресурс = \<hobt-subresource> DBID =\<db-id>  
  
    -   Для ALLOCATION_UNIT: аллокунитлокк хобтид = " \<hobt-id> подресурс = \<alloc-unit-subresource> DBID =\<db-id>  
  
     **\<mode> может быть:**  
  
     Sch-S, Sch-M, S, U, X, IS, IU, IX, SIU, SIX, UIX, BU, RangeS-S, RangeS-U, RangeI-N, RangeI-S, RangeI-U, RangeI-X, RangeX-, RangeX-U, RangeX-X  
  
 **Владелец внешнего ресурса:**  
  
-   External Екстерналресаурце =\<wait-type>  
  
 **Владелец универсального ресурса:**  
  
-   Рабочая область Трансактионинфо Трансактионмутекс =\<workspace-id>  
  
-   Mutex  
  
-   CLRTaskJoin  
  
-   CLRMonitorEvent  
  
-   CLRRWLockEvent  
  
-   resourceWait  
  
 **Владелец ресурса кратковременной блокировки:**  
  
-   \<db-id>:\<file-id>:\<page-in-file>  
  
-   \<GUID>  
  
-   \<latch-class> (\<latch-address>)  
  
## <a name="permissions"></a>Разрешения

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах `Server admin` `Azure Active Directory admin` требуется учетная запись или. Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   
 
## <a name="example"></a>Пример
### <a name="a-identify-tasks-from-blocked-sessions"></a>A. Выявление задач из заблокированных сеансов. 

```sql
SELECT * FROM sys.dm_os_waiting_tasks 
WHERE blocking_session_id IS NOT NULL; 
```   

### <a name="b-view-waiting-tasks-per-connection"></a>Б. Просмотр ожидающих задач на подключение

```sql
SELECT st.text AS [SQL Text], c.connection_id, w.session_id, 
  w.wait_duration_ms, w.wait_type, w.resource_address, 
  w.blocking_session_id, w.resource_description, c.client_net_address, c.connect_time
FROM sys.dm_os_waiting_tasks AS w
INNER JOIN sys.dm_exec_connections AS c ON w.session_id = c.session_id 
CROSS APPLY (SELECT * FROM sys.dm_exec_sql_text(c.most_recent_sql_handle)) AS st 
              WHERE w.session_id > 50 AND w.wait_duration_ms > 0
ORDER BY c.connection_id, w.session_id
GO
```

### <a name="c-view-waiting-tasks-for-all-user-processes-with-additional-information"></a>В. Просмотр ожидающих задач для всех пользовательских процессов с дополнительными сведениями

```sql
SELECT 'Waiting_tasks' AS [Information], owt.session_id,
    owt.wait_duration_ms, owt.wait_type, owt.blocking_session_id,
    owt.resource_description, es.program_name, est.text,
    est.dbid, eqp.query_plan, er.database_id, es.cpu_time,
    es.memory_usage*8 AS memory_usage_KB
FROM sys.dm_os_waiting_tasks owt
INNER JOIN sys.dm_exec_sessions es ON owt.session_id = es.session_id
INNER JOIN sys.dm_exec_requests er ON es.session_id = er.session_id
OUTER APPLY sys.dm_exec_sql_text (er.sql_handle) est
OUTER APPLY sys.dm_exec_query_plan (er.plan_handle) eqp
WHERE es.is_user_process = 1
ORDER BY owt.session_id;
GO
```
  
## <a name="see-also"></a>См. также:  
[SQL Server динамические административные представления, связанные с операционной системой &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)      
[Руководство по архитектуре потоков и задач](../../relational-databases/thread-and-task-architecture-guide.md)     
   
 
