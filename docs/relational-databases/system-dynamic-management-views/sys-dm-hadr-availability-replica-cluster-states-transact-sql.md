---
description: sys.dm_hadr_availability_replica_cluster_states (Transact-SQL)
title: sys.dm_hadr_availability_replica_cluster_states (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_hadr_availability_replica_cluster_states_TSQL
- dm_hadr_availability_replica_cluster_states
- sys.dm_hadr_availability_replica_cluster_states
- dm_hadr_availability_replica_cluster_states_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], WSFC clusters
- sys.dm_hadr_availability_replica_cluster_states dynamic management view
ms.assetid: 2e0dd780-6a71-4f4b-b7f7-6e063bec71d6
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 79ed4be2df381b2b64c6a69a2beedaef92d38dd7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092822"
---
# <a name="sysdm_hadr_availability_replica_cluster_states-transact-sql"></a>sys.dm_hadr_availability_replica_cluster_states (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает по строке для каждой реплики доступности AlwaysOn (независимо от состояния соединения) во всех группах доступности AlwaysOn (независимо от расположения реплики) в кластере WSFC.  
  
##  <a name="connected_state"></a>  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**replica_id**|**uniqueidentifier**|Уникальный идентификатор реплики доступности.|  
|**replica_server_name**|**nvarchar(256)**|Имя экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], на котором размещена реплика.|  
|**group_id**|**uniqueidentifier**|Уникальный идентификатор группы доступности.|  
|**join_state**|**tinyint**|0 = не присоединена.<br /><br /> 1 = присоединенная, изолированная<br /><br /> 2 = присоединена, экземпляр отказоустойчивого кластера.|  
|**join_state_desc**|**nvarchar(60)**|NOT_JOINED<br /><br /> JOINED_STANDALONE<br /><br /> JOINED_FAILOVER_CLUSTER_INSTANCE|  
  
## <a name="security"></a>Безопасность  
  
### <a name="permissions"></a>Разрешения  
 необходимо разрешение VIEW SERVER STATE на сервере.  
  
## <a name="see-also"></a>См. также:  
 [Отслеживание групп доступности (Transact-SQL)](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
  
