---
description: sys.dm_hadr_availability_group_states (Transact-SQL)
title: sys.dm_hadr_availability_group_states (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_hadr_availability_group_states
- sys.dm_hadr_availability_group_states_TSQL
- dm_hadr_availability_group_states_TSQL
- dm_hadr_availability_group_states
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.dm_hadr_availability_group_states dynamic management view
ms.assetid: d18019dd-f8dc-4492-b035-b1a639369b65
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c5cdbf3f295fc8781e25a6df3ae78d63c22a3b6a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092835"
---
# <a name="sysdm_hadr_availability_group_states-transact-sql"></a>sys.dm_hadr_availability_group_states (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает строку для каждой Always On группы доступности, которая владеет репликой доступности на локальном экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Каждая строка отображает состояния работоспособности определенной группы доступности.  
  
> [!NOTE]  
>  Чтобы получить полный список, выполните запрос к представлению каталога [sys.availability_groups](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md) .  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**group_id**|**uniqueidentifier**|Уникальный идентификатор группы доступности.|  
|**primary_replica**|**varchar(128)**|Имя экземпляра сервера, на котором размещена текущая первичная реплика.<br /><br /> NULL = не первичная реплика, или не удается связаться с отказоустойчивым кластером WSFC.|  
|**primary_recovery_health**|**tinyint**|Указывает состояние работоспособности (восстановления) первичной реплики, одно из следующих значений:<br /><br /> 0 = выполняется<br /><br /> 1 = в сети<br /><br /> NULL<br /><br /> Во вторичных репликах столбец **primary_recovery_health** имеет значение null.|  
|**primary_recovery_health_desc**|**nvarchar(60)**|Описание **primary_replica_health**, одно из следующих:<br /><br /> ONLINE_IN_PROGRESS<br /><br /> ONLINE<br /><br /> NULL|  
|**secondary_recovery_health**|**tinyint**|Указывает состояние восстановления реплики вторичной реплики, одно из следующих:<br /><br /> 0 = выполняется<br /><br /> 1 = в сети<br /><br /> NULL<br /><br /> В первичной реплике **secondary_recovery_health** столбец имеет значение null.|  
|**secondary_recovery_health_desc**|**nvarchar(60)**|Описание **secondary_recovery_health**, одно из следующих:<br /><br /> ONLINE_IN_PROGRESS<br /><br /> ONLINE<br /><br /> NULL|  
|**synchronization_health**|**tinyint**|Отражает сводный показатель **synchronization_health** всех реплик доступности в группе доступности. Ниже приведены возможные значения и их описания.<br /><br /> 0: неработоспособен. Ни одна из реплик доступности не имеет работоспособного **synchronization_health** (2 = работоспособный).<br /><br /> 1: частично работоспособен. Некоторые, но не все реплики доступности находятся в исправном состоянии.<br /><br /> 2: исправен. Все реплики доступности находятся в исправном состоянии синхронизации.<br /><br /> Сведения о работоспособности синхронизации реплики см. в столбце **synchronization_health** в [sys.dm_hadr_availability_replica_states &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md).|  
|**synchronization_health_desc**|**nvarchar(60)**|Описание **synchronization_health**, одно из следующих:<br /><br /> NOT_HEALTHY<br /><br /> PARTIALLY_HEALTHY<br /><br /> HEALTHY|  
  
## <a name="security"></a>Безопасность  
  
### <a name="permissions"></a>Разрешения  
 необходимо разрешение VIEW SERVER STATE на сервере.  
  
## <a name="see-also"></a>См. также:  
 [Мониторинг групп доступности &#40;&#41;Transact-SQL ](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Группы доступности AlwaysOn (SQL Server)](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Динамические представления управления и функции, связанные с группами доступности AlwaysOn (Transact-SQL)](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)  
  
  
