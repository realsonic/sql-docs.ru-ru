---
description: sys.dm_exec_distributed_request_steps (Transact-SQL)
title: sys.dm_exec_distributed_request_steps (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- SYS.DM_EXEC_DISTRIBUTED_REQUEST_STEPS_TSQL
- DM_EXEC_DISTRIBUTED_REQUEST_STEPS_TSQL
- DM_EXEC_DISTRIBUTED_REQUEST_STEPS
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase,views
- PolyBase
- dm_exec_distributed_request_steps
- sys.dm_exec_distributed_request_steps management view
ms.assetid: 1954541d-b716-4e03-8fcc-7022f428e01d
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 57ea8cc29caae50f91c8351bf72af494b000b178
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094085"
---
# <a name="sysdm_exec_distributed_request_steps-transact-sql"></a>sys.dm_exec_distributed_request_steps (Transact-SQL)
[!INCLUDE [sqlserver2016-asa-pdw](../../includes/applies-to-version/sqlserver2016-asa-pdw.md)]

  Содержит сведения обо всех шагах, составляющих данный запрос или запрос Polybase. В нем отображается одна строка для каждого шага запроса.  
  
|Имя столбца|Тип данных|Description|Диапазон|  
|-----------------|---------------|-----------------|-----------|  
|execution_id|**int**|execution_id и step_index сделать ключ для этого представления. Уникальный числовой идентификатор, связанный с запросом.|См. раздел ID в [sys.dm_exec_requests &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).|  
|step_index|**int**|Расположение этого шага в последовательности шагов, составляющих запрос.|от 0 до (n – 1) для запроса с n шагами.|  
|operation_type|**nvarchar(128)**|Тип операции, представленной этим шагом.|"MoveOperation", "OnOperation", "Рандомидоператион", "RemoteOperation", "ReturnOperation", "ShuffleMoveOperation", "Темптаблепропертиесоператион", "Дропдиагностикснотифйоператион", "HadoopShuffleOperation", "HadoopBroadCastOperation", "HadoopRoundRobinOperation"|  
|distribution_type|**nvarchar(32)**|Место исполнения шага.|"Аллкомпутенодес", "Аллдистрибутионс", "ComputeNode", "Distribution", "AllNodes", "Субсетнодес", "Distribution", "unspecifieded".|  
|location_type|**nvarchar(32)**|Место исполнения шага.|"COMPUTE", "Head" или "DMS". Все шаги перемещения данных показывают "DMS".|  
|status|**nvarchar(32)**|Состояние этого шага|"Pending", "работает", "Complete", "Failed", "Ундофаилед", "Пендингканцел", "recommit", "Undone", "Abortd"|  
|error_id|**nvarchar (36)**|Уникальный идентификатор ошибки, связанной с этим шагом, если таковой имеется|См. идентификатор [sys.dm_exec_compute_node_errors &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-node-errors-transact-sql.md), значение null, если ошибка не возникла.|  
|start_time|**datetime**|Время начала выполнения этапа|Меньше или равно текущему времени и больше или равно end_compile_time запроса, к которому относится этот шаг.|  
|end_time|**datetime**|Время, когда выполнение этого шага было завершено, было отменено или завершилось сбоем.|Меньшее или равное текущему времени и больше или равно start_time, установите значение NULL для шагов, выполняемых в данный момент или в очереди.|  
|total_elapsed_time|**int**|Общее количество времени выполнения шага запроса (в миллисекундах)|Между 0 и разностью между end_time и start_time. 0 для шагов в очереди.|  
|row_count|**bigint**|Общее число строк, измененных или возвращенных этим запросом|0 для шагов, которые не изменяют или возвращают данные, количество затронутых строк в противном случае. Задайте значение-1 для шагов DMS.|  
|.|nvarchar(4000)|Содержит полный текст команды этого шага.|Любая допустимая строка запроса для шага. Усекается, если длиннее 4000 символов.|  
  
## <a name="see-also"></a>См. также:  
 [Устранение неполадок в Polybase с помощью динамических административных представлений](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))   
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Динамические административные представления, связанные с базами данных &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
