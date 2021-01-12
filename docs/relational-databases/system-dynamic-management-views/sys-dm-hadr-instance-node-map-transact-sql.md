---
description: sys.dm_hadr_instance_node_map (Transact-SQL)
title: sys.dm_hadr_instance_node_map (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.sys.dm_hadr_instance_node_map_TSQL
- sys.sys.dm_hadr_instance_node_map
- sys.dm_hadr_instance_node_map_TSQL
- sys.dm_hadr_instance_node_map
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], WSFC
- sys.sys.dm_hadr_instance_node_map dynamic management view
ms.assetid: ccfaf62c-9f87-43cf-a5e7-8942e91dd041
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a89e98b9c7a66478367306fe5683f564df4b09bf
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092854"
---
# <a name="sysdm_hadr_instance_node_map-transact-sql"></a>sys.dm_hadr_instance_node_map (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Для каждого экземпляра, на котором [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] размещена реплика доступности, присоединенная к группе доступности Always on, возвращает имя узла отказоустойчивого кластера Windows Server (WSFC), на котором размещен экземпляр сервера. Это динамическое административное представление может использоваться следующим образом.  
  
-   Динамическое административное представление может оказаться полезным для обнаружения группы доступности с несколькими репликами доступности, размещенными на одном узле WSFC, поскольку такая конфигурация, которая может возникнуть после отработки отказа FCI в том случае, если группа доступности сконфигурирована неверно, не поддерживается. Дополнительные сведения см. в разделе [Отказоустойчивая кластеризация и группы доступности AlwaysOn (SQL Server)](../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md).  
  
-   Когда несколько экземпляров SQL Server размещаются на одном узле WSFC, DLL-библиотека ресурсов через это динамическое административное представление определяет экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], к которому следует подключаться.  
   
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**ag_resource_id**|**nvarchar(256)**|Уникальный идентификатор группы доступности в виде ресурса в WSFC.|  
|**instance_name**|**nvarchar(256)**|Имя экземпляра *сервера* / — экземпляр сервера, на котором размещена реплика группы доступности.|  
|**node_name**|**nvarchar(256)**|Имя узла WSFC.|  
  
## <a name="permissions"></a>Разрешения  
 необходимо разрешение VIEW SERVER STATE на сервере.  
  
## <a name="see-also"></a>См. также:  
 [Always On динамические административные представления и функции групп доступности &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [Представления каталога групп доступности AlwaysOn (Transact-SQL)](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Мониторинг групп доступности &#40;&#41;Transact-SQL ](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Группы доступности AlwaysOn (SQL Server)](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  
  
