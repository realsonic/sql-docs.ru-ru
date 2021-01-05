---
title: Отслеживание групп доступности с помощью Transact-SQL (T-SQL)
description: Используйте инструкции Transact-SQL SELECT для отслеживания групп доступности Always On, их реплик и баз данных.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- dynamic management views [SQL Server], AlwaysOn Availability Groups
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], listeners
- Availability Groups [SQL Server], databases
- catalog views [SQL Server], AlwaysOn Availability Groups
ms.assetid: 881a34de-8461-4811-8c62-322bf7226bed
author: cawrites
ms.author: chadam
ms.openlocfilehash: a3a3110540340da3dcb05c660e8c0027a0ba6d97
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642998"
---
# <a name="monitor-availability-groups-transact-sql"></a>Отслеживание групп доступности (Transact-SQL)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Для мониторинга групп доступности и реплик доступности, а также связанных баз данных с помощью [!INCLUDE[tsql](../../../includes/tsql-md.md)]в [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] предусмотрен набор представлений каталога, динамических административных представлений и свойств сервера. С помощью инструкций [!INCLUDE[tsql](../../../includes/tsql-md.md)] SELECT можно использовать представления для отслеживания групп доступности, их реплик и баз данных. Сведения, возвращаемые по данной группе доступности, зависят от наличия подключения к экземпляру [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , на котором размещена первичная или вторичная реплика.  
  
> [!TIP]  
>  Многие из этих представлений можно объединять с помощью их столбцов ID, что позволяет возвращать сведения из нескольких представлений в одном запросе.  
  
  
##  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] требуется разрешение VIEW ANY DEFINITION на экземпляре сервера. [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] необходимо разрешение VIEW SERVER STATE на сервере.  
  
##  <a name="monitoring-the-always-on-availability-groups-feature-on-a-server-instance"></a><a name="AoAgFeatureOnSI"></a> Мониторинг функции групп доступности AlwaysOn в экземпляре сервера  
 Для наблюдения за компонентом [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] на экземпляре сервера пользуйтесь следующей встроенной функцией.  
  
 Функция[SERVERPROPERTY](../../../t-sql/functions/serverproperty-transact-sql.md)  
 Возвращает сведения о свойствах сервера о том, включен ли [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] , а также (если включен) был ли он запущен на экземпляре сервера.  
  
 **Имена столбцов**. IsHadrEnabled, HadrManagerStatus  
  
##  <a name="monitoring-availability-groups-on-the-wsfc-cluster"></a><a name="WSFC"></a> Мониторинг групп доступности на кластере WSFC  
 Для мониторинга отказоустойчивой кластеризации Windows Server (WSFC), на котором размещается локальный экземпляр сервера с поддержкой [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], используются следующие представления:  
  
 [sys.dm_hadr_cluster](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-transact-sql.md)  
 Если на узле отказоустойчивой кластеризации Windows Server (WSFC), где размещен экземпляр SQL Server с включенным [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] , имеется кворум WSFC, **sys.dm_hadr_cluster** возвращает строку, которая предоставляет имя кластера и сведения о форуме. Если узел WSFC не набирает кворум, строки не возвращаются.  
  
 **Имена столбцов:** cluster_name, quorum_type, quorum_type_desc, quorum_state, quorum_state_desc  
  
 [sys.dm_hadr_cluster_members](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md)  
 Если на узле WSFC, где размещается локальный экземпляр SQL Server с включенной поддержкой AlwaysOn, имеется кворум WSFC, он возвращает строку для каждого из участников, составляющих кворум, и состояние каждого из них.  
  
 **Имена столбцов:** member_name, member_type, member_type_desc, member_state, member_state_desc, number_of_quorum_votes  
  
 [sys.dm_hadr_cluster_networks](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-networks-transact-sql.md)  
 Возвращает по строке для каждого из элементов, участвующих в конфигурации подсети группы доступности. Это динамическое административное представление можно использовать для проверки виртуального сетевого IP-адреса, настроенного для каждой из реплик доступности.  
  
 **Имена столбцов:** member_name, network_subnet_ip, network_subnet_ipv4_mask, network_subnet_prefix_length, is_public, is_ipv4  
  
 **Первичный ключ:** member_name + network_subnet_IP + network_subnet_prefix_length  
  
 [sys.dm_hadr_instance_node_map](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-instance-node-map-transact-sql.md)  
 Для каждого экземпляра SQL Server, где размещена реплика доступности, подключенная к группе доступности AlwaysOn, возвращается имя узла отказоустойчивой кластеризации Windows Server (WSFC), где размещен экземпляр сервера. Это динамическое административное представление может использоваться следующим образом.  
  
-   Динамическое административное представление может оказаться полезным для обнаружения группы доступности с несколькими репликами доступности, размещенными на одном узле WSFC, поскольку такая конфигурация, которая может возникнуть после отработки отказа FCI в том случае, если группа доступности сконфигурирована неверно, не поддерживается.  
  
-   Когда несколько экземпляров SQL Server размещаются на одном узле WSFC, DLL-библиотека ресурсов через это динамическое административное представление определяет экземпляр SQL Server, к которому следует подключаться.  
  
 **Имена столбцов:** ag_resource_id, instance_name, node_name  
  
 [sys.dm_hadr_name_id_map](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-name-id-map-transact-sql.md)  
 Показывает сопоставление групп доступности AlwaysOn, к которым подключен текущий экземпляр SQL Server, и трех уникальных идентификаторов: идентификатора группы доступности, идентификатора ресурсов WSCF и идентификатора группы WSFC. Цель такого сопоставления состоит в обработке сценария, в ходе которого ресурс/группа WSFC переименовывается.  
  
 **Имена столбцов:** ag_name, ag_id, ag_resource_id, ag_group_id  
  
> [!NOTE]  
>  См. также описание команд **sys.dm_hadr_availability_replica_cluster_nodes** и **sys.dm_hadr_availability_replica_cluster_states** в разделе [Мониторинг реплик доступности](#AvReplicas) и описание команд **sys.availability_databases_cluster** и **sys.dm_hadr_database_replica_cluster_states** в разделе [Мониторинг баз данных доступности](#AvDbs) далее в этой статье.  
  
 Сведения о кластерах WSFC и [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] см. в разделах [Отказоустойчивая кластеризация Windows Server (WSFC) с SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md) и [Отказоустойчивая кластеризация и группы доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md).  
  
##  <a name="monitoring-availability-groups"></a><a name="AvGroups"></a> Monitoring Availability Groups  
 Для мониторинга групп доступности, для которых на экземпляре сервера размещена реплика доступности, используются следующие представления.  
  
 [sys.availability_groups](../../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)  
 Возвращает строку для каждой группы доступности, для которых в локальном экземпляре [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] размещена реплика доступности. Каждая строка содержит кэшированную копию метаданных группы доступности.  
  
 **Имена столбцов:** group_id, name, resource_id, resource_group_id, failure_condition_level, health_check_timeout, automated_backup_preference, automated_backup_preference_desc  
  
 [sys.availability_groups_cluster](../../../relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md)  
 Возвращает строку для каждой группы доступности в кластере WSFC. Каждая строка содержит метаданные группы доступности из отказоустойчивой кластеризации Windows Server (WSFC).  
  
 **Имена столбцов:** group_id, name, resource_id, resource_group_id, failure_condition_level, health_check_timeout, automated_backup_preference, automated_backup_preference_desc  
  
 [sys.dm_hadr_availability_group_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md)  
 Возвращает по строке для каждой из групп доступности, у которых имеется реплика доступности на локальном экземпляре [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Каждая строка отображает состояния работоспособности определенной группы доступности.  
  
 **Имена столбцов:** group_id, primary_replica, primary_recovery_health, primary_recovery_health_desc, secondary_recovery_health, secondary_recovery_health_desc, synchronization_health, synchronization_health_desc  
  
##  <a name="monitoring-availability-replicas"></a><a name="AvReplicas"></a> Мониторинг реплик доступности  
 Для мониторинга групп доступности используются следующие представления и системная функция.  
  
 [sys.availability_replicas](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)  
 Возвращает строку для каждой реплики доступности, для которой в локальном экземпляре [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] размещена реплика доступности.  
  
 **Имена столбцов:** replica_id, group_id, replica_metadata_id, replica_server_name, owner_sid, endpoint_url, availability_mode, availability_mode_desc, failover_mode, failover_mode_desc, session_timeout, primary_role_allow_connections, primary_role_allow_connections_desc, secondary_role_allow_connections, secondary_role_allow_connections_desc, create_date, modify_date, backup_priority, read_only_routing_url  
  
 [sys.availability_read_only_routing_lists](../../../relational-databases/system-catalog-views/sys-availability-read-only-routing-lists-transact-sql.md)  
 Возвращает строку для списка маршрутизации только для чтения для каждой реплики доступности в группе доступности AlwaysOn в отказоустойчивом кластере WSFC.  
  
 **Имена столбцов:** replica_id, routing_priority, read_only_replica_id  
  
 [sys.dm_hadr_availability_replica_cluster_nodes](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-nodes-transact-sql.md)  
 Возвращает по строке для каждой из реплик доступности (независимо от состояния соединения) в группах доступности AlwaysOn в отказоустойчивой кластеризации Windows Server (WSFC).  
  
 **Имена столбцов:** group_name, replica_server_name, node_name  
  
 [sys.dm_hadr_availability_replica_cluster_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md)  
 Возвращает по строке для каждой из реплик (вне зависимости от состояния соединения) во всех группах доступности AlwaysOn (вне зависимости от расположения реплики) в отказоустойчивой кластеризации Windows Server (WSFC).  
  
 **Имена столбцов:** replica_id, replica_server_name, group_id, join_state, join_state_desc  
  
 [sys.dm_hadr_availability_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md)  
 Возвращает строку с состоянием каждой локальной реплики доступности и для каждой удаленной реплики доступности, входящей в ту же группу доступности.  
  
 **Имена столбцов:** replica_id, group_id, is_local, role, role_desc, operational_state, operational_state_desc, connected_state, connected_state_desc, recovery_health, recovery_health_desc, synchronization_health, synchronization_health_desc, last_connect_error_number, last_connect_error_description и last_connect_error_timestamp  
  
 [sys.fn_hadr_backup_is_preferred_replica](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md)  
 Определяет, является ли текущая реплика предпочитаемой резервной репликой отработки.  
  
> [!NOTE]  
>  Сведения о счетчиках производительности для реплик доступности (объект производительности **SQLServer:Availability Replica**  ) см. в разделе [SQL Server, реплика доступности](../../../relational-databases/performance-monitor/sql-server-availability-replica.md).  
  
##  <a name="monitoring-availability-databases"></a><a name="AvDbs"></a> Мониторинг баз данных доступности  
 Для мониторинга баз данных доступности используйте следующие представления.  
  
 [sys.availability_databases_cluster](../../../relational-databases/system-catalog-views/sys-availability-databases-cluster-transact-sql.md)  
 Содержит одну строку для каждой базы данных в экземпляре SQL Server, которая является частью групп доступности AlwaysOn в кластере, независимо от того, присоединена ли база данных локальных копий к группе доступности или нет.  
  
> [!NOTE]  
>  При добавлении базы данных в группу доступности база данных-источник автоматически присоединяется к группе. Базы данных-получатели необходимо подготовить на каждой из вторичных реплик до того, как их можно будет присоединить к группе доступности.  
  
 **Имена столбцов:** group_id, group_database_id, database_name  
  
 [sys.databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
 Содержит одну строку для каждой базы данных в экземпляре [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Если база данных принадлежит к реплике доступности, то в строке для этой базы данных отображается идентификатор GUID реплики и уникальный идентификатор базы данных внутри группы доступности.  
  
 **[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] :** replica_id, group_database_id  
  
 [sys.dm_hadr_auto_page_repair](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-auto-page-repair-transact-sql.md)  
 Возвращает строку для каждой попытки автоматического восстановления страниц во всех базах данных доступности в реплике доступности, размещенной в группе доступности на экземпляре сервера. Это представление содержит строки, связанные с последними попытками автоматического восстановления страниц в определенной базе данных-источнике или получателе, количество которых ограничено числом в 100 строк на каждую базу данных. По достижении максимального значения строка для следующей попытки автоматического восстановления страниц заменяет одну из существующих записей.  
  
 **Имена столбцов:** database_id, file_id, page_id, error_type, page_status, modification_time  
  
 [sys.dm_hadr_database_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)  
 Возвращает по строке для каждой из баз данных, участвующих в любой группе доступности, реплика доступности которой размещена на локальном экземпляре [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
 **Имена столбцов:** database_id, group_id, replica_id, group_database_id, is_local, synchronization_state, synchronization_state_desc, is_commit_participant, synchronization_health, synchronization_health_desc, database_state, database_state_desc, is_suspended, suspend_reason, suspend_reason_desc, recovery_lsn, truncation_lsn, last_sent_lsn, last_sent_time, last_received_lsn, last_received_time, last_hardened_lsn, last_hardened_time, last_redone_lsn, last_redone_time, log_send_queue_size, log_send_rate, redo_queue_size, redo_rate, filestream_send_rate, end_of_log_lsn, last_commit_lsn, last_commit_time, low_water_mark_for_ghosts  
  
 [sys.dm_hadr_database_replica_cluster_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md)  
 Возвращает строку с информацией, помогающей составить представление о работоспособности баз данных доступности каждой из групп доступности в отказоустойчивой кластеризации Windows Server (WSFC). Динамическое административное представление удобно использовать при планировании или при отработке отказа либо при поиске вторичной реплики в группе доступности, которая не дает усекать журнал данной базы данных-источника.  
  
 **Имена столбцов:** replica_id, group_database_id, database_name, is_failover_ready, is_pending_secondary_suspend, is_database_joined, recovery_lsn, truncation_lsn  
  
> [!NOTE]  
>  Расположение первичной реплики — авторитетный источник для группы доступности.  
  
> [!NOTE]  
>  Сведения о счетчиках производительности [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] для баз данных доступности (объект производительности **SQLServer:Database Replica** ) см. в разделе [SQL Server, реплика базы данных](../../../relational-databases/performance-monitor/sql-server-database-replica.md). Для мониторинга активности журнала транзакций для баз данных доступности пользуйтесь следующими счетчиками объекта производительности **SQLServer:Databases**: **Время записи журнала на диск (мс)** , **Записей журнала на диск/с**, **Неудачных обращений к кэшу пула журнала/с**, **Операций чтения диска пула журнала/с** и **Запросов пула журнала/с**. Дополнительные сведения см. в статье [SQL Server, Databases Object](../../../relational-databases/performance-monitor/sql-server-databases-object.md).  
  
##  <a name="monitoring-availability-group-listeners"></a><a name="AGlisteners"></a> Мониторинг прослушивателей группы доступности  
 Для мониторинга прослушивателей группы доступности в подсети кластера WSFC используйте следующие представления:  
  
 [sys.availability_group_listener_ip_addresses](../../../relational-databases/system-catalog-views/sys-availability-group-listener-ip-addresses-transact-sql.md)  
 Возвращает строку для каждого совместимого виртуального IP-адреса, который в настоящее время включен для прослушивателя группы доступности.  
  
 **Имена столбцов:** listener_id, IP-адрес, ip_subnet_mask, is_dhcp, network_subnet_ip, network_subnet_prefix_length, network_subnet_ipv4_mask, state, state_desc  
  
 [sys.availability_group_listeners](../../../relational-databases/system-catalog-views/sys-availability-group-listeners-transact-sql.md)  
 Для любой выбранной группы доступности возвращает либо ноль строк, указывая, что с группой доступности не связано ни одного сетевого имени, либо отдельную строку для каждой конфигурации прослушивателя группы доступности в кластере WSFC.  
  
 **Имена столбцов:** group_id, listener_id, dns_name, порт, is_conformant, ip_configuration_string_from_cluster  
  
 [sys.dm_tcp_listener_states](../../../relational-databases/system-dynamic-management-views/sys-dm-tcp-listener-states-transact-sql.md)  
 Возвращает строку, содержащую сведения о динамическом состоянии для каждого прослушивателя TCP.  
  
 **Имена столбцов:** listener_id, ip_address, is_ipv4, port, type, type_desc, state, state_desc, start_time  
  
 **Первичный ключ:** listener_id  
  
 Сведения о прослушивателях групп доступности см. в разделе [Прослушиватели группы доступности, подключения клиентов и отработка отказа приложений (SQL Server)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
 **Задачи по мониторингу групп доступности AlwaysOn:**  
  
-   [Использование сведений обозревателя объектов для мониторинга групп доступности (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-object-explorer-details-to-monitor-availability-groups.md)  
  
-   [Просмотр свойств группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/view-availability-group-properties-sql-server.md)  
  
-   [Просмотр свойств реплики доступности (SQL Server)](../../../database-engine/availability-groups/windows/view-availability-replica-properties-sql-server.md)  
  
-   [Просмотр свойств прослушивателя группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/view-availability-group-listener-properties-sql-server.md)  
  
 **Справочник по мониторингу групп доступности AlwaysOn (Transact-SQL):**  
  
-   [SERVERPROPERTY (Transact-SQL)](../../../t-sql/functions/serverproperty-transact-sql.md)  
  
-   [sys.availability_group_listener_ip_addresses (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-availability-group-listener-ip-addresses-transact-sql.md)  
  
-   [sys.availability_group_listeners (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-availability-group-listeners-transact-sql.md)  
  
-   [sys.availability_databases_cluster (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-availability-databases-cluster-transact-sql.md)  
  
-   [sys.availability_groups (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)  
  
-   [sys.availability_read_only_routing_lists (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-availability-read-only-routing-lists-transact-sql.md)  
  
-   [sys.availability_replicas (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)  
  
-   [sys.dm_hadr_availability_replica_cluster_nodes (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-nodes-transact-sql.md)  
  
-   [sys.dm_hadr_availability_replica_cluster_states (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md)  
  
-   [sys.database_mirroring_endpoints (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)  
  
-   [sys.dm_hadr_auto_page_repair (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-auto-page-repair-transact-sql.md)  
  
-   [sys.dm_hadr_availability_group_states (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md)  
  
-   [sys.dm_hadr_availability_replica_cluster_states (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md)  
  
-   [sys.dm_hadr_availability_replica_states (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md)  
  
-   [sys.dm_hadr_database_replica_states (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)  
  
-   [sys.dm_hadr_database_replica_cluster_states (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md)  
  
-   [sys.dm_hadr_cluster (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-transact-sql.md)  
  
-   [sys.dm_hadr_cluster_members (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md)  
  
-   [sys.dm_hadr_cluster_networks (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-networks-transact-sql.md)  
  
-   [sys.dm_hadr_database_replica_cluster_states (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md)  
  
-   [sys.dm_hadr_database_replica_states (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)  
  
-   [sys.dm_hadr_instance_node_map (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-instance-node-map-transact-sql.md)  
  
-   [sys.dm_hadr_name_id_map (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-name-id-map-transact-sql.md)  
  
-   [sys.dm_os_performance_counters (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)  
  
-   [sys.dm_tcp_listener_states (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-tcp-listener-states-transact-sql.md)  
  
-   [sys.fn_hadr_backup_is_preferred_replica  (Transact-SQL)](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md)  
  
 **Счетчики производительности AlwaysOn:**  
  
-   [SQL Server, реплика доступности](../../../relational-databases/performance-monitor/sql-server-availability-replica.md)  
  
-   [SQL Server, реплика базы данных](../../../relational-databases/performance-monitor/sql-server-database-replica.md)  
  
-   [SQL Server, объект Databases](../../../relational-databases/performance-monitor/sql-server-databases-object.md)  
  
 **Управление на основе политик для групп доступности AlwaysOn**  
  
-   [Использование политик AlwaysOn для просмотра состояния группы доступности (SQL Server)](../../../database-engine/availability-groups/windows/use-always-on-policies-to-view-the-health-of-an-availability-group-sql-server.md)  
  
-   [Использование панели мониторинга AlwaysOn (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
## <a name="see-also"></a>См. также:  
 [Группы доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Отслеживание групп доступности (SQL Server)](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)  
  
  
