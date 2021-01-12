---
description: sys.dm_geo_replication_link_status (база данных SQL Azure)
title: sys.dm_geo_replication_link_status
titleSuffix: Azure SQL Database
ms.date: 01/28/2019
ms.service: sql-database
ms.reviewer: ''
ms.topic: conceptual
f1_keywords:
- dm_geo_replication_link_status
- dm_geo_replication_link_status_TSQL
- sys.dm_geo_replication_link_status
- sys.dm_geo_replication_link_status_TSQL
helpviewer_keywords:
- dm_geo_replication_link_status dynamic management view
- sys.dm_geo_replication_link_status dynamic management view
ms.assetid: d763d679-470a-4c21-86ab-dfe98d37e9fd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.custom: seo-dt-2019
ms.openlocfilehash: 2134a7890d23a7a080a576f8337e750bee2563eb
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092820"
---
# <a name="sysdm_geo_replication_link_status-azure-sql-database"></a>sys.dm_geo_replication_link_status (база данных SQL Azure)

[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Содержит по одной строке для каждой связи репликации между первичной и базой данных-получателем в партнерстве георепликации. Это относится как к базам данных-источникам, так и к базам данных-получателям. Если какая-либо база данных-источник задействована в более чем одном постоянном канале репликации, эта таблица содержит по строке на каждое такое отношение репликации. Это представление создается во всех базах данных, в том числе в логической базе данных master. Однако запрос этого представления для логической базы данных master возвращает пустой набор.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|link_guid|**uniqueidentifier**|Уникальный идентификатор канала репликации.|  
|partner_server|**sysname**|Имя сервера базы данных SQL, содержащего связанную базу данных.|  
|partner_database|**sysname**|Имя связанной базы данных на связанном сервере базы данных SQL.|  
|last_replication|**datetimeoffset**|Метка времени для подтверждения последней транзакции получателем на основе часов базы данных-источника. Это значение доступно только в базе данных-источнике.|  
|replication_lag_sec|**int**|Разница во времени в секундах между значением last_replication и меткой времени фиксации этой транзакции на основе базы данных источника, основанной на тактах.  Это значение доступно только в базе данных-источнике.|  
|replication_state|**tinyint**|Состояние георепликации для этой базы данных, одно из следующих:<br /><br /> 1 = заполнение. Выполняется заполнение целевого объекта георепликации, но две базы данных еще не синхронизированы. Пока заполнение не завершится, вы не сможете подключиться к базе данных-получателю. Удаление базы данных-получателя с сервера-источника приведет к отмене операции заполнения.<br /><br /> 2 = перехватить. База данных-получатель находится в состоянии согласованности транзакций и постоянно синхронизируется с базой данных-источником.<br /><br /> 4 = приостановлено. Это неактивная связь непрерывного копирования. Это состояние обычно означает, что доступной для Interlink полосы пропускания недостаточно для уровня активности транзакций в базе данных-источнике. Однако связь непрерывного копирования не повреждена.|  
|replication_state_desc|**nvarchar(256)**|PENDING<br /><br /> SEEDING<br /><br /> CATCH_UP|  
|роль|**tinyint**|Роль георепликации, одна из следующих:<br /><br /> 0 = основной. Database_id ссылается на базу данных-источник в партнерстве георепликации.<br /><br /> 1 = вторичный.  Database_id ссылается на базу данных-источник в партнерстве георепликации.|  
|role_desc|**nvarchar(256)**|PRIMARY<br /><br /> SECONDARY|  
|secondary_allow_connections|**tinyint**|Вторичный тип, один из следующих:<br /><br /> 0 = прямые подключения не разрешены для базы данных-получателя, и база данных недоступна для чтения.<br /><br /> 2 = все соединения разрешены для базы данных в реплике-получателе REPL; икатион для доступа только для чтения.|  
|secondary_allow_connections_desc|**nvarchar(256)**|Нет<br /><br /> Все|  
|last_commit|**datetimeoffset**|Время последней фиксации транзакции в базе данных. При получении в базе данных источника указывает время последней фиксации в базе данных источника. При получении в базе данных-получателе указывает время последней фиксации в базе данных-получателе. При получении в базе данных-получателе, когда первичная реплика канала репликации находится в недоступном виде, она указывает до того, как будет перехвачена дополнительная точка.|
  
> [!NOTE]  
>  Если отношение репликации завершается удалением базы данных-получателя (раздел 4,2), строка для этой базы данных в представлении **sys.dm_geo_replication_link_status** исчезает.  
  
## <a name="permissions"></a>Разрешения  
 Любая учетная запись с view_database_state разрешением может запрашивать **sys.dm_geo_replication_link_status**.  
  
## <a name="example"></a>Пример  
 Отображение времени отдержек репликации и последней репликации баз данных-получателей.  
  
```  
SELECT   
     link_guid  
   , partner_server  
   , last_replication  
   , replication_lag_sec   
FROM sys.dm_geo_replication_link_status;  
```  
  
## <a name="see-also"></a>См. также:  
 [ALTER DATABASE &#40;база данных SQL Azure&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [sys.geo_replication_links &#40;базе данных SQL Azure&#41;](../../relational-databases/system-dynamic-management-views/sys-geo-replication-links-azure-sql-database.md)   
 [sys.dm_operation_status &#40;базе данных SQL Azure&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database.md)   
 [sp_wait_for_database_copy_sync](../system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync.md)
