---
title: Справочник по системным объектам группы доступности
description: Справка по различным системным объектам, которые могут использоваться при работе с группами доступности Always On.
ms.custom: seo-lt-2019
ms.date: 04/03/2010
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: reference
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 340fcfa12f3432a09821132f44e8d58672be8198
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641022"
---
# <a name="always-on-availability-group-system-object-reference"></a>Справочник по системным объектам группы доступности AlwaysOn

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]
    
Этот справочная страница по разным системным объектам, которые могут использоваться при работе с группами доступности AlwaysOn. 

## <a name="system-catalog-views"></a>Представления системного каталога

| Представление системного каталога | Description|
| :------ | :----------------------------- |
| [sys.availability_databases_cluster](../../../relational-databases/system-catalog-views/sys-availability-databases-cluster-transact-sql.md)   | Содержит по строке для каждой базы данных доступности в экземпляре SQL Server, в которой размещена реплика доступности для группы доступности AlwaysOn в кластере для отказоустойчивой кластеризации Windows Server (WSFC), независимо от того, присоединена ли локальная копия базы данных к группе доступности. |
| [sys.availability_group_listener_ip_addresses](../../../relational-databases/system-catalog-views/sys-availability-group-listener-ip-addresses-transact-sql.md)  | Возвращает по строке для каждого IP-адреса, связанного с прослушивателем группы доступности AlwaysOn в кластере WSFC. |
| [sys.availability_group_listeners](../../../relational-databases/system-catalog-views/sys-availability-group-listeners-transact-sql.md)    | Для каждой группы доступности AlwaysOn не возвращает ни одной строки, указывая на то, что с группой доступности не связаны имена сети, либо возвращает по одной строке для каждой конфигурации прослушивателей групп доступности в кластере WSFC.  |
| [sys.availability_groups](../../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)   | Возвращает по строке для каждой группы доступности, для которой в локальном экземпляре SQL Server размещена реплика доступности. Каждая строка содержит кэшированную копию метаданных группы доступности. |
| [sys.availability_groups_cluster](../../../relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md)    | Возвращает по строке для каждой группы доступности AlwaysOn в кластере WSFC. Каждая строка содержит метаданные группы доступности из кластера WSFC. |
| [sys.availability_read_only_routing_lists](../../../relational-databases/system-catalog-views/sys-availability-read-only-routing-lists-transact-sql.md)    | Возвращает строку для списка маршрутизации только для чтения для каждой реплики доступности в группе доступности AlwaysOn в отказоустойчивом кластере WSFC. |
| [sys.availability_replicas](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)    | Возвращает по строке для каждой реплики доступности, принадлежащей любой группе доступности AlwaysOn в отказоустойчивом кластере WSFC. |
| &nbsp; | &nbsp; |

## <a name="system-dynamic-management-views"></a>Системные динамические административные представления


| Системное динамическое административное представление | Description|
| :------ | :----------------------------- |
| [sys.dm_hadr_auto_page_repair](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-auto-page-repair-transact-sql.md)   | Возвращает строку для каждой попытки автоматического восстановления страниц во всех базах данных доступности в реплике доступности, размещенной в группе доступности на экземпляре сервера.  |
| [sys.dm_hadr_availability_group_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md)    | Возвращает по строке для каждой группы доступности AlwaysOn, включающий реплику доступности на локальном экземпляре SQL Server. Каждая строка отображает состояния работоспособности определенной группы доступности. |
| [sys.dm_hadr_availability_replica_cluster_nodes](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-nodes-transact-sql.md)     | Возвращает по строке для каждой реплики доступности (независимо от состояния соединения) в группах доступности AlwaysOn в кластере WSFC. |
| [sys.dm_hadr_availability_replica_cluster_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md)     | Возвращает по строке для каждой реплики доступности AlwaysOn (независимо от состояния соединения) во всех группах доступности AlwaysOn (независимо от расположения реплики) в кластере WSFC. |
| [sys.dm_hadr_availability_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md)    | Возвращает по строке для каждой локальной реплики и каждой удаленной реплики, входящей в ту же группу доступности AlwaysOn, что и локальная реплика. Каждая строка содержит сведения о состоянии конкретной реплики. |
| [sys.dm_hadr_cluster](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-transact-sql.md)    | Возвращает одну строку, в которой предоставляется имя кластера и сведения о кворуме. |
| [sys.dm_hadr_cluster_members](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md)    | Возвращает по одной строке для каждого элемента, который входит в кворум, и сведения о состоянии каждого элемента. |
| [sys.dm_hadr_cluster_networks](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-networks-transact-sql.md)    | Возвращает по строке для каждого из элементов кластера WSFC, участвующего в конфигурации подсети группы доступности.  |
| [sys.dm_hadr_database_replica_cluster_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md)     | Возвращает строку с информацией, помогающей получить полезные сведения о работоспособности баз данных доступности в каждой группе доступности AlwaysOn в кластере WSFC.  |
| [sys.dm_hadr_database_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)    | Возвращает по строке для каждой базы данных, которая входит в группу доступности AlwaysOn, реплика доступности которой размещена в локальном экземпляре SQL Server. |
| [sys.dm_hadr_instance_node_map](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-instance-node-map-transact-sql.md)    | Для каждого экземпляра SQL Server, где размещена реплика доступности, подключенная к группе доступности AlwaysOn, возвращается имя узла WSFC, на котором размещен экземпляр сервера. |
| [sys.dm_hadr_name_id_map](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-name-id-map-transact-sql.md)    | Показывает сопоставление групп доступности AlwaysOn, к которым подключен текущий экземпляр SQL Server, и трех уникальных идентификаторов: идентификатора группы доступности, идентификатора ресурсов WSCF и идентификатора группы WSFC. |
| [sys.dm_tcp_listener_states](../../../relational-databases/system-dynamic-management-views/sys-dm-tcp-listener-states-transact-sql.md)    | Возвращает строку, содержащую сведения о динамическом состоянии для каждого прослушивателя TCP. |
| &nbsp; | &nbsp; |

## <a name="system-functions"></a>Системные функции


| Системная функция | Description|
| :------ | :----------------------------- |
| [sys.fn_hadr_is_primary_replica](../../../relational-databases/system-functions/sys-fn-hadr-is-primary-replica-transact-sql.md)  | Используется для определения, является ли текущая реплика первичной репликой. |
| [sys.fn_hadr_backup_is_preferred_replica](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md)    | Используется для определения, является ли текущая реплика предпочитаемой резервной репликой. |
| [sys.fn_hadr_distributed_ag_replica](../../../relational-databases/system-functions/sys-fn-hadr-distributed-ag-replica-transact-sql.md)    | Используется для сопоставления реплики в распределенной группе доступности с локальной группой доступности. |
| &nbsp; | &nbsp; |


  
## <a name="see-also"></a>См. также:  
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   

  
