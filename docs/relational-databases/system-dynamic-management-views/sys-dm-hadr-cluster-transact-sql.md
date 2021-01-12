---
description: sys.dm_hadr_cluster (Transact-SQL)
title: sys.dm_hadr_cluster (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 01/31/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_hadr_cluster
- dm_hadr_cluster_HADR
- sys.dm_hadr_cluster_TSQL
- dm_hadr_cluster
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.dm_hadr_cluster catalog view
- Availability Groups [SQL Server], WSFC clusters
ms.assetid: 13ce70e4-9d43-4a80-a826-099e6213bf85
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: aa289a58047e1108e94a6b70f86eb366807f5af1
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097681"
---
# <a name="sysdm_hadr_cluster-transact-sql"></a>sys.dm_hadr_cluster (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Если узел отказоустойчивой кластеризации Windows Server (WSFC), на котором размещен экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , включенный для, [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] имеет кворум WSFC, **sys.dm_hadr_cluster** возвращает строку, которая предоставляет имя кластера и сведения о кворуме. Если узел WSFC не набирает кворума, строка не возвращается.  
 > [!TIP]
 > Начиная с версии [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] , это динамическое административное представление поддерживает Always on экземпляры отказоустойчивого кластера в дополнение к группам доступности Always on.

|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**cluster_name**|**nvarchar(128)**|Имя кластера WSFC, содержащего экземпляры [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], в которых включена поддержка [!INCLUDE[ssHADR](../../includes/sshadr-md.md)].|  
|**quorum_type**|**tinyint**|Тип кворума, используемый этим кластером WSFC. Одно из следующих значений:<br /><br /> 0 = большинство узлов. Эта конфигурация кворума может выдержать отказ половины узлов (с округлением вверх) минус одного. Например, в кластере из семи узлов такая конфигурация кворума выдерживает отказ трех узлов.<br /><br /> 1 = большинство узлов и дисков. Если следящий диск остается доступным, то эта конфигурация кворума может выдержать отказ половины узлов (с округлением вверх). Например, кластер из шести узлов, в котором следящий диск остается в режиме «в сети», может выдержать отказы трех узлов. Если произошел отказ следящего диска или он оказался в режиме «вне сети», то такая конфигурация может выдержать отказ половины узлов (с округлением вверх) минус одного. Например, кластер из шести узлов, в котором произошел отказ следящего диска, может выдержать отказы двух (3-1=2) узлов.<br /><br /> 2 = большинство узлов и общих папок. Такая конфигурация кворума работает аналогично большинству дисков и узлов, но роль следящего диска в ней играет следящая общая папка.<br /><br /> 3 = нет большинства: только диск. Если диск кворума доступен в сети, то такая конфигурация кворума может выдержать отказ всех узлов, кроме одного.<br /><br /> 4 = неизвестный кворум. Неизвестный кворум для кластера.<br /><br /> 5 = облачный следящий сервер. Кластер использует Microsoft Azure для арбитража кворума. Если облачный следящий сервер доступен, кластер может поддерживать сбой половины узлов (округление).|  
|**quorum_type_desc**|**varchar(50)**|Описание **quorum_type**, одно из следующих:<br /><br /> NODE_MAJORITY<br /><br /> NODE_AND_DISK_MAJORITY<br /><br /> NODE_AND_FILE_SHARE_MAJORITY<br /><br /> NO_MAJORITY:_DISK_ONLY <br /><br /> UNKNOWN_QUORUM <br /><br /> CLOUD_WITNESS|  
|**quorum_state**|**tinyint**|Состояние кворума WSFC, одно из следующих значений:<br /><br /> 0 = неизвестное состояние кворума<br /><br /> 1 = нормальный кворум<br /><br /> 2 = принудительный кворум|  
|**quorum_state_desc**|**varchar(50)**|Описание **quorum_state**, одно из следующих:<br /><br /> UNKNOWN_QUORUM_STATE<br /><br /> NORMAL_QUORUM<br /><br /> FORCED_QUORUM|  
  
## <a name="permissions"></a>Разрешения  
 необходимо разрешение VIEW SERVER STATE на сервере.  
  
## <a name="see-also"></a>См. также:  
 [Always On динамические административные представления и функции групп доступности &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [Представления каталога групп доступности AlwaysOn (Transact-SQL)](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Мониторинг групп доступности &#40;&#41;Transact-SQL ](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [sys.dm_hadr_cluster_members (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md)  
  
  
