---
description: sys.dm_exec_background_job_queue (Transact-SQL)
title: sys.dm_exec_background_job_queue (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_exec_background_job_queue
- sys.dm_exec_background_job_queue_TSQL
- dm_exec_background_job_queue_TSQL
- sys.dm_exec_background_job_queue
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_background_job_queue dynamic management function
ms.assetid: 05d9884f-b74c-4e3c-a23b-c90c1ea5ef02
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 07490d9e2a05e65fa90dd76cbdf51647f000bb3a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092951"
---
# <a name="sysdm_exec_background_job_queue-transact-sql"></a>sys.dm_exec_background_job_queue (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает строку для каждого задания обработчика запросов, запланированного для асинхронного (фонового) выполнения.  
  
> **ПРИМЕЧАНИЕ.** Чтобы вызвать эту функцию из **[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]** или **[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]** , используйте имя **sys.dm_pdw_nodes_exec_background_job_queue**.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**time_queued**|**datetime**|Время добавления задания в очередь.|  
|**job_id**|**int**|Идентификатор задания.|  
|**database_id**|**int**|База данных, в которой должно быть выполнено задание.|  
|**object_id1**|**int**|Значение зависит от типа задания. Дополнительные сведения см. в разделе "Примечания".|  
|**object_id2**|**int**|Значение зависит от типа задания. Дополнительные сведения см. в разделе "Примечания".|  
|**object_id3**|**int**|Значение зависит от типа задания. Дополнительные сведения см. в разделе "Примечания".|  
|**object_id4**|**int**|Значение зависит от типа задания. Дополнительные сведения см. в разделе "Примечания".|  
|**error_code**|**int**|Код ошибки, если задание возвращается в очередь из-за сбоя. Это значение равно NULL, если задание приостановлено, не извлечено из очереди или завершено.|  
|**request_type**|**smallint**|Тип запроса задания.|  
|**retry_count**|**smallint**|Число возвратов задания в очередь из-за отсутствия ресурсов или по другим причинам.|  
|**in_progress**|**smallint**|Показывает, начато ли выполнение задания:<br /><br /> 1 = выполнение начато;<br /><br /> 0 = задание пока еще ожидает.|  
|**session_id**|**smallint**|Идентификатор сеанса.|  
|**pdw_node_id**|**int**|**Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение.|  
  
## <a name="permissions"></a>Разрешения

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах `Server admin` `Azure Active Directory admin` требуется учетная запись или. Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   
  
## <a name="remarks"></a>Комментарии  
 Это представление возвращает сведения только для заданий асинхронного обновления статистики. Дополнительные сведения о асинхронном обновлении статистики см. в разделе [Statistics](../../relational-databases/statistics/statistics.md).  
  
 Значения **object_id1** до **object_id4** зависят от типа запроса задания. Значение этих столбцов при разных типах заданий указано в следующей таблице.  
  
|Тип запроса|object_id1|object_id2|object_id3|object_id4|  
|------------------|-----------------|-----------------|-----------------|-----------------|  
|Асинхронное обновление статистики|Идентификатор таблицы или представления|Идентификатор статистики|Не используется|Не используется|  
  
## <a name="examples"></a>Примеры  
 В следующем примере возвращается целый ряд активных асинхронных заданий в фоновой очереди для каждой базы данных экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```  
SELECT DB_NAME(database_id) AS [Database], COUNT(*) AS [Active Async Jobs]  
FROM sys.dm_exec_background_job_queue  
WHERE in_progress = 1  
GROUP BY database_id;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Динамические административные представления и функции, связанные с выполнением &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Статистика](../../relational-databases/statistics/statistics.md)   
 [KILL STATS JOB (Transact-SQL)](../../t-sql/language-elements/kill-stats-job-transact-sql.md)  
  
  



