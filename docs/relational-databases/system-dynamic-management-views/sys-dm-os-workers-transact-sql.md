---
description: sys.dm_os_workers (Transact-SQL)
title: sys.dm_os_workers (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_workers_TSQL
- sys.dm_os_workers_TSQL
- dm_os_workers
- sys.dm_os_workers
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_workers dynamic management view
ms.assetid: 4d5d1e52-a574-4bdd-87ae-b932527235e8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d93b2be18167232798b3d8de6ce1ca67f9e8c9d6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095139"
---
# <a name="sysdm_os_workers-transact-sql"></a>sys.dm_os_workers (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает строку для каждого исполнителя в системе. Дополнительные сведения о рабочих ролях см. в разделе " [архитектура потоков и задач](../../relational-databases/thread-and-task-architecture-guide.md)". 
  
> [!NOTE]  
>  Чтобы вызвать эту функцию из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте имя **sys.dm_pdw_nodes_os_workers**.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|worker_address|**varbinary(8)**|Адрес памяти исполнителя.|  
|status|**int**|Только для внутреннего использования.|  
|is_preemptive|**bit**|1 = исполнитель работает по расписанию с вытеснением. Любой исполнитель, запускающий внешний код, работает по расписанию с вытеснением.|  
|is_fiber|**bit**|1 = исполнитель работает с использованием упрощенных пулов. Дополнительные сведения см. в подразделе [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).|  
|is_sick|**bit**|1 = работа исполнителя приостановлена при попытке получить спин-блокировку. Такое значение этого параметра может указывать на проблему конфликта в связи с объектом, к которому часто запрашивается доступ.|  
|is_in_cc_exception|**bit**|1 = исполнитель обрабатывает исключение, не являющееся исключением [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|is_fatal_exception|**bit**|Указывает на получение этим исполнителем неустранимого исключения.|  
|is_inside_catch|**bit**|1 = исполнитель в настоящий момент обрабатывает исключение.|  
|is_in_polling_io_completion_routine|**bit**|1 = исполнитель в настоящий момент выполняет подпрограмму завершения для незавершенного ввода-вывода. Дополнительные сведения см. в разделе [sys.dm_io_pending_io_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-io-pending-io-requests-transact-sql.md).|  
|context_switch_count|**int**|Количество переключений контекста планировщика, выполненных этим исполнителем.|  
|pending_io_count|**int**|Количество физических операций ввода-вывода, выполненных этим исполнителем.|  
|pending_io_byte_count|**bigint**|Общее число байтов для всех незавершенных физических вводов-выводов для этого исполнителя.|  
|pending_io_byte_average|**int**|Среднее число байтов для всех физических вводов-выводов для этого исполнителя.|  
|wait_started_ms_ticks|**bigint**|Момент времени, в [ms_ticks](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md), когда этот рабочий процесс перешел в состояние SUSPENDED. Вычитание этого значения из ms_ticks в [sys.dm_os_sys_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) возвращает число миллисекунд, в течение которых Рабочая роль была в состоянии ожидания.|  
|wait_resumed_ms_ticks|**bigint**|Момент времени, в [ms_ticks](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md), когда этот рабочий процесс перешел в состояние готовности к запуску. Вычитание этого значения из ms_ticks в [sys.dm_os_sys_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) возвращает число миллисекунд, в течение которых Рабочая роль находится в готовой очереди.|  
|task_bound_ms_ticks|**bigint**|Момент времени, в [ms_ticks](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md), когда задача привязана к этому рабочему процессу.|  
|worker_created_ms_ticks|**bigint**|Момент времени, в [ms_ticks](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md), при создании рабочей роли.|  
|exception_num|**int**|Номер ошибки последнего исключения, возникшего у этого исполнителя.|  
|exception_severity|**int**|Серьезность последнего исключения, возникшего у этого исполнителя.|  
|exception_address|**varbinary(8)**|Адрес кода, откуда было получено исключение|  
|affinity|**bigint**|Сходство рабочих потоков. Соответствует сходству потока в [sys.dm_os_threads &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-threads-transact-sql.md).|  
|Состояние|**nvarchar(60)**|Состояние исполнителя. Может иметь одно из следующих значений:<br /><br /> INIT = исполнитель в настоящий момент инициализируется.<br /><br /> RUNNING = исполнитель в настоящий момент выполняется, в режиме без приоритетного прерывания или с приоритетным прерыванием.<br /><br /> RUNNABLE = исполнитель готов к запуску в соответствии с планировщиком.<br /><br /> SUSPENDED = исполнитель в настоящий момент приостановлен, находится в режиме ожидания сигнала от события.|  
|start_quantum|**bigint**|Время начала текущего выполнения этого исполнителя (в миллисекундах).|  
|end_quantum|**bigint**|Время окончания текущего выполнения этого исполнителя (в миллисекундах).|  
|last_wait_type|**nvarchar(60)**|Тип последнего ожидания. Список типов ожидания см. в разделе [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md).|  
|return_code|**int**|Возвращенное значение от последнего ожидания. Может иметь одно из следующих значений:<br /><br /> 0 =SUCCESS<br /><br /> 3 = DEADLOCK<br /><br /> 4 = PREMATURE_WAKEUP<br /><br /> 258 = TIMEOUT|  
|quantum_used|**bigint**|Только для внутреннего использования.|  
|max_quantum|**bigint**|Только для внутреннего использования.|  
|boost_count|**int**|Только для внутреннего использования.|  
|tasks_processed_count|**int**|Количество задач, обработанных этим исполнителем.|  
|fiber_address|**varbinary(8)**|Адрес памяти волокна, с которым связан этот исполнитель.<br /><br /> NULL = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не настроен для использования упрощенных пулов.|  
|task_address|**varbinary(8)**|Адрес памяти текущей задачи. Дополнительные сведения см. в разделе [sys.dm_os_tasks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md).|  
|memory_object_address|**varbinary(8)**|Адрес памяти объекта памяти исполнителя. Дополнительные сведения см. в разделе [sys.dm_os_memory_objects &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql.md).|  
|thread_address|**varbinary(8)**|Адрес памяти потока, связанного с этим исполнителем. Дополнительные сведения см. в разделе [sys.dm_os_threads &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-threads-transact-sql.md).|  
|signal_worker_address|**varbinary(8)**|Адрес памяти исполнителя, который последним подавал сигнал этому объекту. Дополнительные сведения см. в разделе [sys.dm_os_workers](../../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md).|  
|scheduler_address|**varbinary(8)**|Адрес памяти планировщика. Дополнительные сведения см. в разделе [sys.dm_os_schedulers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md).|  
|processor_group|**smallint**|Сохраняет идентификатор группы процессоров, назначенный данному потоку.|  
|pdw_node_id|**int**|**Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение.|  
  
## <a name="remarks"></a>Комментарии  
 Если значением состояния исполнителя является RUNNING, и исполнитель выполняется в режиме без приоритетного прерывания, адрес исполнителя совпадает со значением active_worker_address в sys.dm_os_schedulers.  
  
 Когда исполнитель, ожидающий события, получает сигнал, он помещается в начало очереди готовых к работе исполнителей. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] разрешает делать это тысячу раз подряд, после чего исполнитель помещается в конец очереди. Перемещение исполнителя в конец очереди имеет некоторые последствия для производительности.  
  
## <a name="permissions"></a>Разрешения
В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
На [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] уровнях Premium требуется `VIEW DATABASE STATE` разрешение в базе данных. На [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] уровнях Standard и Basic требуется  `Server Admin` членство в роли или `Azure Active Directory admin` учетная запись.   

## <a name="examples"></a>Примеры  
 Можно воспользоваться следующим запросом, чтобы определить, как долго исполнитель находился в состояниях SUSPENDED и RUNNABLE.  
  
```sql
SELECT   
    t1.session_id,  
    CONVERT(varchar(10), t1.status) AS status,  
    CONVERT(varchar(15), t1.command) AS command,  
    CONVERT(varchar(10), t2.state) AS worker_state,  
    w_suspended =   
      CASE t2.wait_started_ms_ticks  
        WHEN 0 THEN 0  
        ELSE   
          t3.ms_ticks - t2.wait_started_ms_ticks  
      END,  
    w_runnable =   
      CASE t2.wait_resumed_ms_ticks  
        WHEN 0 THEN 0  
        ELSE   
          t3.ms_ticks - t2.wait_resumed_ms_ticks  
      END  
  FROM sys.dm_exec_requests AS t1  
  INNER JOIN sys.dm_os_workers AS t2  
    ON t2.task_address = t1.task_address  
  CROSS JOIN sys.dm_os_sys_info AS t3  
  WHERE t1.scheduler_id IS NOT NULL;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
 session_id status     command         worker_state w_suspended w_runnable  
 ---------- ---------- --------------- ------------ ----------- --------------------  
 4          background LAZY WRITER     SUSPENDED    688         688  
 6          background LOCK MONITOR    SUSPENDED    4657        4657
 19         background BRKR TASK       SUSPENDED    603820344   603820344  
 14         background BRKR EVENT HNDL SUSPENDED    63583641    63583641  
 51         running    SELECT          RUNNING      0           0  
 2          background RESOURCE MONITO RUNNING      0           603825954  
 3          background LAZY WRITER     SUSPENDED    422         422  
 7          background SIGNAL HANDLER  SUSPENDED    603820485   603820485  
 13         background TASK MANAGER    SUSPENDED    603824704   603824704  
 18         background BRKR TASK       SUSPENDED    603820407   603820407  
 9          background TRACE QUEUE TAS SUSPENDED    454         454  
 52         suspended  SELECT          SUSPENDED    35094       35094  
 1          background RESOURCE MONITO RUNNING      0           603825954  
```

 На выходе, если переменные `w_runnable` и `w_suspended` равны, они представляют время, которое исполнитель находится в состоянии SUSPENDED. Иначе переменная `w_runnable` представляет время, проведенное исполнителем в состоянии RUNNABLE. На выходе, сеанс `52` находится в состоянии `SUSPENDED` в течение `35,094` миллисекунд.  
  
## <a name="see-also"></a>См. также:  
[SQL Server динамические административные представления, связанные с операционной системой &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)       
[Руководство по архитектуре обработки запросов](../../relational-databases/query-processing-architecture-guide.md#DOP)       
[Руководство по архитектуре потоков и задач](../../relational-databases/thread-and-task-architecture-guide.md)    
