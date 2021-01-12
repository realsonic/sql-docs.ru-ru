---
description: sys.dm_os_tasks (Transact-SQL)
title: sys.dm_os_tasks (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_os_tasks
- sys.dm_os_tasks_TSQL
- dm_os_tasks_TSQL
- dm_os_tasks
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_tasks dynamic management view
ms.assetid: 180a3c41-e71b-4670-819d-85ea7ef98bac
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c823b4edbe62e23ec0127fa5066ed46762b402e3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096541"
---
# <a name="sysdm_os_tasks-transact-sql"></a>sys.dm_os_tasks (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает одну строку для каждой активной задачи в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Задача — это базовая единица выполнения в SQL Server. К примерам задач относятся запрос, имя входа, выход из системы и системные задачи, такие как действие очистки фантомов, активность контрольных точек, модуль записи журнала, параллельное действие повтора. Дополнительные сведения о задачах см. в разделе [руководств по архитектуре потоков и задач](../../relational-databases/thread-and-task-architecture-guide.md).
  
> [!NOTE]  
> Чтобы вызвать эту функцию из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте имя **sys.dm_pdw_nodes_os_tasks**.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**task_address**|**varbinary(8)**|Адрес объекта в памяти.|  
|**task_state**|**nvarchar(60)**|Состояние задачи. Оно может быть одним из следующих.<br /><br /> Ожидание: ожидание рабочего потока.<br /><br /> ГОТОВ к запуску: готово к запуску, но ожидает получения такта.<br /><br /> ВЫПОЛНЯЕТСЯ: в данный момент выполняется в планировщике.<br /><br /> SUSPENDED: имеет рабочий, но ожидает события.<br /><br /> Готово: завершено.<br /><br /> СПИНЛУП: завис в спин-блокировке.|  
|**context_switches_count**|**int**|Число переключений контекста планировщика, которые задача уже выполнила.|  
|**pending_io_count**|**int**|Количество физических операций ввода-вывода, выполняемых этой задачей.|  
|**pending_io_byte_count**|**bigint**|Суммарное количество байт, обработанных в операциях ввода-вывода, выполняемых этой задачей.|  
|**pending_io_byte_average**|**int**|Среднее количество байт, обработанных в операциях ввода-вывода, выполняемых этой задачей.|  
|**scheduler_id**|**int**|Идентификатор родительского планировщика. Это дескриптор сведений планировщика для этой задачи. Дополнительные сведения см. в разделе [sys.dm_os_schedulers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md).|  
|**session_id**|**smallint**|Идентификатор сеанса, связанного с задачей.|  
|**exec_context_id**|**int**|Идентификатор контекста выполнения, связанного с задачей.|  
|**request_id**|**int**|Идентификатор запроса задачи. Дополнительные сведения см. в разделе [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).|  
|**worker_address**|**varbinary(8)**|Адрес в памяти для исполнителя, выполняющего задачу.<br /><br /> NULL = задача либо ожидает готовности исполнителя, либо выполнение задачи только что завершилось.<br /><br /> Дополнительные сведения см. в разделе [sys.dm_os_workers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md).|  
|**host_address**|**varbinary(8)**|Адрес сервера в памяти.<br /><br /> 0 = задача создавалась без помощи сервера. Помогает идентифицировать сервер, использованный для создания этой задачи.<br /><br /> Дополнительные сведения см. в разделе [sys.dm_os_hosts &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-hosts-transact-sql.md).|  
|**parent_task_address**|**varbinary(8)**|Адрес в памяти задачи, являющейся родительской задачей объекта.|  
|**pdw_node_id**|**int**|**Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение.|  
  
## <a name="permissions"></a>Разрешения
В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах `Server admin` `Azure Active Directory admin` требуется учетная запись или. Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   

## <a name="examples"></a>Примеры  
  
### <a name="a-monitoring-parallel-requests"></a>A. Наблюдение за параллельными запросами  
 Для запросов, которые выполняются параллельно, вы увидите несколько строк для одного сочетания ( \<**session_id**> , \<**request_id**> ). Используйте следующий запрос, чтобы найти [параметр конфигурации сервера max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) для всех активных запросов.  
  
> [!NOTE]  
>  **Request_id** уникален в пределах сеанса.  
  
```sql  
SELECT  
    task_address,  
    task_state,  
    context_switches_count,  
    pending_io_count,  
    pending_io_byte_count,  
    pending_io_byte_average,  
    scheduler_id,  
    session_id,  
    exec_context_id,  
    request_id,  
    worker_address,  
    host_address  
  FROM sys.dm_os_tasks  
  ORDER BY session_id, request_id;  
```  
  
### <a name="b-associating-session-ids-with-windows-threads"></a>Б. Сопоставление идентификатора сеанса с потоками Windows  
 Можно использовать следующий запрос для сопоставления значения идентификатора сеанса со значением идентификатора потока Windows. Затем можно наблюдать за производительностью потока в системном мониторе Windows. Следующий запрос не возвращает сведений для сеансов, находящихся в ждущем режиме.  
  
```sql  
SELECT STasks.session_id, SThreads.os_thread_id  
  FROM sys.dm_os_tasks AS STasks  
  INNER JOIN sys.dm_os_threads AS SThreads  
    ON STasks.worker_address = SThreads.worker_address  
  WHERE STasks.session_id IS NOT NULL  
  ORDER BY STasks.session_id;  
GO  
```  

## <a name="see-also"></a>См. также:  
[SQL Server динамические административные представления, связанные с операционной системой &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)    
[Руководство по архитектуре потоков и задач](../../relational-databases/thread-and-task-architecture-guide.md)     
  


