---
description: sys.dm_exec_background_job_queue_stats (Transact-SQL)
title: sys.dm_exec_background_job_queue_stats (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_exec_background_job_queue_stats
- sys.dm_exec_background_job_queue_stats
- dm_exec_background_job_queue_stats_TSQL
- sys.dm_exec_background_job_queue_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_background_job_queue_stats dynamic management view
ms.assetid: 27f62ab5-46c4-417e-814d-8d6437034d1c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3383742972ea96af500514b89e33cfdb58085a0d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099827"
---
# <a name="sysdm_exec_background_job_queue_stats-transact-sql"></a>sys.dm_exec_background_job_queue_stats (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает строку, в которой предоставляются статистические данные для каждого задания обработчика запросов, передаваемого для асинхронного (фонового) выполнения.  
  
> [!NOTE]  
>  Чтобы вызвать эту функцию из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте имя **sys.dm_pdw_nodes_exec_background_job_queue_stats**.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**queue_max_len**|**int**|Максимальная длина очереди.|  
|**enqueued_count**|**int**|Количество запросов, успешно поставленных в очередь.|  
|**started_count**|**int**|Количество запросов, начавших выполнение.|  
|**ended_count**|**int**|Количество запросов, обслуженных до успешного или неудачного завершения.|  
|**failed_lock_count**|**int**|Количество закончившихся ошибкой запросов из-за состязания блокировок или из-за взаимоблокировки.|  
|**failed_other_count**|**int**|Количество запросов, закончившихся ошибкой по другим причинам.|  
|**failed_giveup_count**|**int**|Количество закончившихся ошибкой запросов из-за достижения предельного значения количества повторных попыток.|  
|**enqueue_failed_full_count**|**int**|Количество неудачных попыток постановки в очередь из-за ее заполненности.|  
|**enqueue_failed_duplicate_count**|**int**|Количество дублированных попыток постановки в очередь.|  
|**elapsed_avg_ms**|**int**|Среднее время с момента начала выполнения запроса (в миллисекундах).|  
|**elapsed_max_ms**|**int**|Время с момента начала выполнения самого длинного запроса (в миллисекундах).|  
|**pdw_node_id**|**int**|**Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение.|  
  
## <a name="remarks"></a>Комментарии  
 Это представление возвращает сведения только для заданий асинхронного обновления статистики. Дополнительные сведения о асинхронном обновлении статистики см. в разделе [Statistics](../../relational-databases/statistics/statistics.md).  
  
## <a name="permissions"></a>Разрешения

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах `Server admin` `Azure Active Directory admin` требуется учетная запись или. Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   

## <a name="examples"></a>Примеры  
  
### <a name="a-determining-the-percentage-of-failed-background-jobs"></a>A. Определение процента неудачно завершенных фоновых заданий  
 В следующем примере возвращается процент фоновых заданий, завершившихся со сбоем, для всех выполненных запросов.  
  
```  
SELECT   
        CASE ended_count WHEN 0   
                THEN 'No jobs ended'   
                ELSE CAST((failed_lock_count + failed_giveup_count + failed_other_count) / CAST(ended_count AS float) * 100 AS varchar(20))   
        END AS [Percent Failed]  
FROM sys.dm_exec_background_job_queue_stats;  
GO  
```  
  
### <a name="b-determining-the-percentage-of-failed-enqueue-attempts"></a>Б. Определение процента неудачно завершенных попыток постановки в очередь  
 В следующем примере возвращается процент попыток постановки в очередь, завершившихся со сбоем, для всех выполненных запросов.  
  
```  
SELECT   
        CASE enqueued_count WHEN 0   
                THEN 'No jobs posted'   
                ELSE CAST((enqueue_failed_full_count + enqueue_failed_duplicate_count) / CAST(enqueued_count + enqueue_failed_full_count + enqueue_failed_duplicate_count AS float) * 100 AS varchar(20))   
        END AS [Percent Enqueue Failed]  
FROM sys.dm_exec_background_job_queue_stats;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Динамические административные представления и функции, связанные с выполнением (Transact-SQL)](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


