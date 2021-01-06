---
title: Динамические административные представления и представления системного каталога для групп доступности
description: Коллекция динамических административных представлений и представлений каталога, помогающих отслеживать и диагностировать работоспособность группы доступности Always On.
ms.custom: seo-lt-2019
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: troubleshooting
ms.assetid: 4320a4a4-6183-462b-8bda-e7424e7cb706
author: rothja
ms.author: jroth
ms.openlocfilehash: a2a69aa016ae258375c6095bbf38e58ddd75b981
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643598"
---
# <a name="dynamic-management-views-and-system-catalog-views-always-on-availability-groups"></a>Динамические административные представления и представления системного каталога (группы доступности AlwaysOn)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Этот раздел содержит некоторые типичные запросы в динамических административных представлениях AlwaysOn, которые можно использовать для мониторинга и диагностики групп доступности.  
  
> [!TIP]  
>  В панели мониторинга AlwaysOn можно легко настроить графический интерфейс для отображения множества динамических административных представлений для реплик доступности и баз данных доступности, щелкнув заголовок таблицы правой кнопкой мыши и выбрав представление, которое нужно отобразить или скрыть.  
  
 Дополнительные сведения о динамических административных представлениях для групп доступности см. в разделе [Динамические представления управления и функции, связанные с группами доступности AlwaysOn (Transact-SQL)](~/relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md). Дополнительные сведения о представлениях каталога для групп доступности см. в разделе [Представления каталога групп доступности AlwaysOn (Transact-SQL)](~/relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md).  
  
## <a name="check-the-wsfc-cluster-node-configuration"></a>Проверка параметров конфигурации для узла кластера WSFC  
 Приведенный ниже запрос Transact-SQL (T-SQL) возвращает состояние всех узлов в текущем кластере отказоустойчивой кластеризации Windows Server (WSFC).  
  
```sql  
use master  
go  
select * from sys.dm_hadr_cluster_members  
go  
```  
  
 Этот результирующий набор сообщает состояние каждого узла элемента текущего кластера WSFC. Если кворум определен как **Большинство узлов и общих папок**, сообщается даже общая папка. Можно просмотреть состояние каждого узла, включая его вес при голосовании (значение [number_of_quorum_votes](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md)).  
  
## <a name="explore-the-cluster-network"></a>Просмотр сети кластера  
 Приведенный ниже запрос возвращает конфигурацию сети для текущего кластера WSFC.  
  
```sql  
select * from sys.dm_hadr_cluster_networks  
```  
  
 Результирующий набор содержит по одной строке для каждого сетевого адаптера в кластере WSFC. Например, в кластере из двух узлов, содержащем два сетевых адаптера на каждом узле, этот запрос возвращает четыре строки.  
  
## <a name="explore-the-availability-groups"></a>Просмотр групп доступности  
 Приведенный ниже запрос возвращает сведения о группе доступности.  
  
```sql  
select primary_replica, primary_recovery_health_desc, synchronization_health_desc from sys.dm_hadr_availability_group_states  
go  
select * from sys.availability_groups  
go  
select * from sys.availability_groups_cluster  
go  
```  
  
 Динамические административные представления [sys.dm_hadr_availability_group_states (Transact-SQL)](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md), [sys.availability_groups (Transact-SQL)](~/relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md) и [sys.availability_groups_cluster](~/relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md) возвращают сведения о группах доступности в текущем кластере WSFC. Фактически, кажется, что [sys.availability_groups (Transact-SQL)](~/relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md) и [sys.availability_groups_cluster](~/relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md) возвращают одинаковые сведения.  
  
 Однако [sys.availability_groups_cluster](~/relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md) сообщает метаданные группы доступности, хранящиеся в кластере, а [sys.availability_groups (Transact-SQL)](~/relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md) — метаданные группы доступности, кэшированные в пространстве процесса SQL Server. Кроме того, два этих динамических административных представления сообщают сведения о конфигурации, тогда как [sys.dm_hadr_availability_group_states (Transact-SQL)](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md) сообщает текущее состояние работоспособности для групп доступности.  
  
> [!IMPORTANT]  
>  Эту номенклатуру дополняют динамические административные представления, документирующие реплики доступности и базы данных доступности.  
  
## <a name="explore-the-availability-replicas"></a>Просмотр реплик доступности  
 Приведенный ниже запрос возвращает сведения о репликах доступности, определенных в группах доступности.  
  
```sql  
select replica_id, role_desc, connected_state_desc, synchronization_health_desc from sys.dm_hadr_availability_replica_states  
go  
select replica_server_name, replica_id, availability_mode_desc, endpoint_url from sys.availability_replicas  
go  
select replica_server_name, join_state_desc from sys.dm_hadr_availability_replica_cluster_states  
go  
```  
  
 Как и в случае с динамическими административными представлениями группы доступности, имеется три представления, сообщающих о репликах доступности. [sys.dm_hadr_availability_replica_states](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md) сообщает сведения о состоянии для реплик доступности, кэшированных локально в SQL Server, а [sys.dm_hadr_availability_replica_cluster_states](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md) — сведения о состоянии для реплик доступности из кластера WSFC. Наконец, [sys.availability_replicas](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md) сообщает данные о конфигурации для реплик доступности, кэшированных локально в SQL Server.  
  
## <a name="explore-availability-replica-health"></a>Просмотр работоспособности реплик доступности  
 Приведенный ниже запрос возвращает актуальные сведения о работоспособности для реплик доступности.  
  
```sql  
select replica_id, role_desc, recovery_health_desc, synchronization_health_desc from sys.dm_hadr_availability_replica_states  
go  
```  
  
 Сравните результаты запроса на первичной и вторичной репликах и обратите внимание, что на вторичной реплике сведения о работоспособности указываются только для этой реплики и ни для каких других реплик в группе доступности.  
  
## <a name="explore-the-availability-databases"></a>Просмотр баз данных доступности  
 Приведенный ниже запрос возвращает сведения о репликах доступности, определенных в группе доступности. Вы можете заметить изменение в результатах запроса до и после приостановки перемещения данных в базе данных доступности.  
  
```sql
select * from sys.availability_databases_cluster  
go  
select group_database_id, database_name, is_failover_ready  from sys.dm_hadr_database_replica_cluster_states  
go  
select database_id, synchronization_state_desc, synchronization_health_desc, last_hardened_lsn, redo_queue_size, log_send_queue_size from sys.dm_hadr_database_replica_states  
go  
```  
  
 Здесь сведения о базах данных доступности опять сообщают три динамических административных представления. [sys.availability_databases_cluster](~/relational-databases/system-catalog-views/sys-availability-databases-cluster-transact-sql.md) сообщает сведения о конфигурации для баз данных доступности из кластера WSFC. [sys.dm_hadr_database_replica_cluster_states](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md) сообщает сведения о состоянии для реплик базы данных, кэшированных локально в SQL Server. Оно содержит некоторые важные сведения о состоянии, например готовность реплики доступности к переходу на другой ресурс. Наконец, [sys.dm_hadr_database_replica_states](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) является очень подробным результирующим набором, который сообщает сведения об удостоверении и состоянии для каждой базы данных доступности, например сведения о номерах LSN для журналов первичной и вторичной реплик базы данных.  
  
## <a name="explore-availability-database-health"></a>Просмотр работоспособности баз данных доступности  
 Приведенный ниже запрос возвращает сведения о работоспособности для каждой из баз данных в репликах. Вы можете заметить изменение в результатах запроса до и после приостановки перемещения данных в базе данных доступности.  
  
```sql  
select dc.database_name, dr.database_id, dr.synchronization_state_desc,   
dr.suspend_reason_desc, dr.synchronization_health_desc  
from sys.dm_hadr_database_replica_states dr  join sys.availability_databases_cluster dc  
on dr.group_database_id=dc.group_database_id   
where is_local=1  
go  
```  
  
  
