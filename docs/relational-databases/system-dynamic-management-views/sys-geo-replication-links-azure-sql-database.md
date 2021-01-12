---
description: sys.geo_replication_links (база данных SQL Azure)
title: sys.geo_replication_links (база данных SQL Azure) | Документация Майкрософт
ms.custom: ''
ms.date: 01/28/2019
ms.service: sql-database
ms.reviewer: ''
ms.topic: conceptual
f1_keywords:
- dm_geo_replication_links_TSQL
- dm_geo_replication_links
- sys.dm_geo_replication_links
- sys.dm_geo_replication_links_TSQL
helpviewer_keywords:
- sys.dm_geo_replication_links dynamic management view
- dm_geo_replication_links dynamic management view
ms.assetid: 58911798-1d60-4f28-87ab-2def2bfc3de7
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.openlocfilehash: f10f318ebba5627957f1555b5979e8993f6f5e02
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093908"
---
# <a name="sysgeo_replication_links-azure-sql-database"></a>sys.geo_replication_links (база данных SQL Azure)

[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Содержит по одной строке для каждой связи репликации между первичной и базой данных-получателем в партнерстве георепликации. Данное представление находится в логической базе данных master.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|database_id|**int**|ИДЕНТИФИКАТОР текущей базы данных в представлении sys. databases.|  
|start_date|**datetimeoffset**|Время в формате UTC в центральном центре обработки данных SQL, когда была инициирована репликация базы данных.|  
|modify_date|**datetimeoffset**|Время в формате UTC при выполнении георепликации базы данных в региональном центре обработки данных SQL. В это время новая база данных будет синхронизирована с базой данных-источником.|  
|link_guid|**uniqueidentifier**|Уникальный идентификатор канала георепликации.|  
|partner_server|**sysname**|Имя сервера базы данных SQL, содержащего геореплицированную базу данных.|  
|partner_database|**sysname**|Имя геореплицированной базы данных на связанном сервере базы данных SQL.|  
|replication_state|**tinyint**|Состояние георепликации для этой базы данных, одно из следующих:<br /><br /> 0 = ожидание. Создание активной базы данных-получателя запланировано, но необходимые шаги подготовки еще не завершены.<br /><br /> 1 = заполнение. Выполняется заполнение целевого объекта георепликации, но две базы данных еще не синхронизированы. Пока заполнение не завершится, вы не сможете подключиться к базе данных-получателю. Удаление базы данных-получателя с сервера-источника приведет к отмене операции заполнения.<br /><br /> 2 = перехватить. База данных-получатель находится в состоянии согласованности транзакций и постоянно синхронизируется с базой данных-источником.|  
|replication_state_desc|**nvarchar(256)**|PENDING<br /><br /> SEEDING<br /><br /> CATCH_UP|  
|роль|**tinyint**|Роль георепликации, одна из следующих:<br /><br /> 0 = основной. Database_id ссылается на базу данных-источник в партнерстве георепликации.<br /><br /> 1 = вторичный.  Database_id ссылается на базу данных-источник в партнерстве георепликации.|  
|role_desc|**nvarchar(256)**|PRIMARY<br /><br /> SECONDARY|  
|secondary_allow_connections|**tinyint**|Вторичный тип, один из следующих:<br /><br /> 0 = Нет. База данных-получатель недоступна до отработки отказа.<br /><br /> 1 = только для чтения. База данных-получатель доступна только для клиентских подключений с ApplicationIntent = ReadOnly.<br /><br /> 2= все. База данных-получатель доступна любому клиентскому подключению.|  
|secondary_allow_connections _desc|**nvarchar(256)**|Нет<br /><br /> Все<br /><br /> Только для чтения|  
  
## <a name="permissions"></a>Разрешения

Это представление доступно в базе данных **master** только для входа субъекта уровня сервера.  
  
## <a name="example"></a>Пример

Отображение всех баз данных с связями георепликации.  

```sql
SELECT
     database_id  
   , start_date  
   , partner_server  
   , partner_database  
   , replication_state  
   , role_desc  
   , secondary_allow_connections_desc
FROM sys.geo_replication_links;  
```

## <a name="see-also"></a>См. также:

 [CREATE DATABASE (база данных SQL Azure)](../../t-sql/statements/alter-database-transact-sql.md)   
 [sys.dm_geo_replication_link_status &#40;базе данных SQL Azure&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database.md)   
 [sys.dm_operation_status &#40;базе данных SQL Azure&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database.md)
