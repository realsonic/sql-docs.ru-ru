---
description: sys.dm_exec_query_resource_semaphores (Transact-SQL)
title: sys.dm_exec_query_resource_semaphores (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_exec_query_resource_semaphores_TSQL
- dm_exec_query_resource_semaphores_TSQL
- sys.dm_exec_query_resource_semaphores
- dm_exec_query_resource_semaphores
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_resource_semaphores dynamic management view
ms.assetid: e43a2aa9-dd52-4c89-911e-1a7d05f7ffbb
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0a9ec322e5f19cab5e8506e5e0dbc05a3e55e900
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096576"
---
# <a name="sysdm_exec_query_resource_semaphores-transact-sql"></a>sys.dm_exec_query_resource_semaphores (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает сведения о текущем состоянии семафора запроса-ресурса в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. **sys.dm_exec_query_resource_semaphores** предоставляет общее состояние памяти для выполнения запросов и позволяет определить, может ли система получить доступ к достаточному объему памяти. Это представление дополняет сведения о памяти, полученные от [sys.dm_os_memory_clerks](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md) , чтобы получить полную картину состояния памяти сервера. **sys.dm_exec_query_resource_semaphores** возвращает одну строку для обычного семафора ресурса и другую строку для семафора ресурса малого запроса. Существует два требования для семафора небольшого запроса:  
  
-   Запрошенный объем памяти должен быть меньше 5 МБ.  
  
-   Стоимость запроса должна быть меньше 3 единиц затрат  
  
> [!NOTE]  
>  Чтобы вызвать эту функцию из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте имя **sys.dm_pdw_nodes_exec_query_resource_semaphores**.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**resource_semaphore_id**|**smallint**|Неуникальный идентификатор семафора ресурса. 0 для обычного семафора ресурса и 1 для семафора ресурса малого запроса.|  
|**target_memory_kb**|**bigint**|Предоставляет использование назначения в килобайтах.|  
|**max_target_memory_kb**|**bigint**|Максимально возможное назначение в килобайтах. Значение NULL для семафора ресурса малого запроса.|  
|**total_memory_kb**|**bigint**|Объем памяти, занимаемый семафором ресурса в килобайтах. Если система находится в нехватке памяти или часто назначается минимальный объем памяти, это значение может быть больше значения **target_memory_kb** или **max_target_memory_kb** . Общий объем памяти — это сумма объемов доступной и выделенной памяти.|  
|**available_memory_kb**|**bigint**|Объем памяти, доступный для нового выделения в килобайтах.|  
|**granted_memory_kb**|**bigint**|Общий объем выделенной памяти.|  
|**used_memory_kb**|**bigint**|Физически используемая часть объема выделенной памяти в килобайтах.|  
|**grantee_count**|**int**|Количество активных запросов, необходимых для выделения.|  
|**waiter_count**|**int**|Количество запросов, ожидающих предоставлений, которые будут удовлетворены.|  
|**timeout_error_count**|**bigint**|Общее количество ошибок времени ожидания с момента запуска сервера. Значение NULL для семафора ресурса малого запроса.|  
|**forced_grant_count**|**bigint**|Общее количество минимальных принудительных предоставлений памяти с момента запуска сервера. Значение NULL для семафора ресурса малого запроса.|  
|**pool_id**|**int**|Идентификатор пула ресурсов, к которому принадлежит данный семафор ресурса.|  
|**pdw_node_id**|**int**|**Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение.|  
  
## <a name="permissions"></a>Разрешения  

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах `Server admin` `Azure Active Directory admin` требуется учетная запись или. Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   
  
## <a name="remarks"></a>Комментарии  
 Запросы, использующие динамические административные представления, которые содержат предложение ORDER BY или статистические функции, могут увеличить потребление памяти и таким образом устранить неполадки.  
  
 Используйте **sys.dm_exec_query_resource_semaphores** для устранения неполадок, но не включайте их в приложения, которые будут использовать будущие версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Регулятор ресурсов позволяет администратору базы данных распределять ресурсы сервера между пулами ресурсов, используя до 64 пулов. В [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версиях каждый пул ведет себя как небольшой независимый экземпляр сервера и требует двух семафоров.  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления и функции, связанные с выполнением &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_exec_query_memory_grants (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)  
  
  


