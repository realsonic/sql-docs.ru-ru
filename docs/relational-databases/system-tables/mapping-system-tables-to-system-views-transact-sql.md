---
description: Сопоставление системных таблиц с системными представлениями (Transact-SQL)
title: Сопоставление системных таблиц с системными представлениями (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- catalog views [SQL Server], mapping system tables to
- mapping system tables to system views [SQL Server]
- system tables [SQL Server], mapping to catalog views
ms.assetid: a616fce9-b4c1-49da-87a7-9d6f74911d8f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 86e629050907b1ffbd7a1748082e683db85affbf
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092454"
---
# <a name="mapping-system-tables-to-system-views-transact-sql"></a>Сопоставление системных таблиц с системными представлениями (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  В этом разделе описаны соответствия системных таблиц и функций с системными представлениями и функциями.  
  
 В следующей таблице приведен перечень соответствий между системными таблицами и системными представлениями или функциями в [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
|Системная таблица|Системные представления и функции|Тип представления или функции|  
|------------------|-------------------------------|------------------------------|  
|sysaltfiles|[sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)|Представление каталога|  
|syscacheobjects|[sys.dm_exec_cached_plans](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)<br /><br /> [sys.dm_exec_plan_attributes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-plan-attributes-transact-sql.md)<br /><br /> [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)<br /><br /> [sys.dm_exec_cached_plan_dependent_objects](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plan-dependent-objects-transact-sql.md)|Динамическое административное представление<br /><br /> Динамическое административное представление<br /><br /> Динамическое административное представление<br /><br /> Динамическое административное представление|  
|syscharsets|[sys.syscharsets](../../relational-databases/system-compatibility-views/sys-syscharsets-transact-sql.md)|Представление совместимости|  
|sysconfigures|[sys.configurations](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md)|Представление каталога|  
|syscurconfigs|[sys.configurations](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md)|Представление каталога|  
|sysdatabases|[sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)|Представление каталога|  
|sysdevices|[sys.backup_devices](../../relational-databases/system-catalog-views/sys-backup-devices-transact-sql.md)|Представление каталога|  
|syslanguages|[sys.syslanguages](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md)|Представление совместимости|  
|syslockinfo|[sys.dm_tran_locks](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)|Динамическое административное представление|  
|syslocks|[sys.dm_tran_locks](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)|Динамическое административное представление|  
|syslogins|[sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)<br /><br /> [sys.sql_logins](../../relational-databases/system-catalog-views/sys-sql-logins-transact-sql.md)|Представление каталога|  
|sysmessages|[sys.messages](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)|Представление каталога|  
|sysoledbusers|[sys.linked_logins](../../relational-databases/system-catalog-views/sys-linked-logins-transact-sql.md)|Представление каталога|  
|sysopentapes|[sys.dm_io_backup_tapes](../../relational-databases/system-dynamic-management-views/sys-dm-io-backup-tapes-transact-sql.md)|Динамическое административное представление|  
|sysperfinfo|[sys.dm_os_performance_counters](../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)|Динамическое административное представление|  
|sysprocesses|[sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/sys-dm-exec-connections-transact-sql.md)<br /><br /> [sys.dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)<br /><br /> [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)|Динамическое административное представление<br /><br /> Динамическое административное представление<br /><br /> Динамическое административное представление|  
|sysremotelogins|[sys.remote_logins](../../relational-databases/system-catalog-views/sys-remote-logins-transact-sql.md)|Представление каталога|  
|sysservers|[sys.servers](../../relational-databases/system-catalog-views/sys-servers-transact-sql.md)|Представление каталога|  
  
 В следующей таблице показаны соответствия между системными таблицами или функциями, которые присутствуют в каждой базе данных [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)], и системными представлениями или функциями [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
|Системная таблица или функция|Системное представление или функция|Тип представления или функции|  
|------------------------------|-----------------------------|------------------------------|  
|fn_virtualfilestats|[sys.dm_io_virtual_file_stats](../../relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql.md)|Динамическое административное представление|  
|syscolumns|[sys.columns](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)|Представление каталога|  
|syscomments|[sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)|Представление каталога|  
|sysconstraints|[sys.check_constraints](../../relational-databases/system-catalog-views/sys-check-constraints-transact-sql.md)<br /><br /> [sys.default_constraints](../../relational-databases/system-catalog-views/sys-default-constraints-transact-sql.md)<br /><br /> [sys.key_constraints](../../relational-databases/system-catalog-views/sys-key-constraints-transact-sql.md)<br /><br /> [sys.foreign_keys](../../relational-databases/system-catalog-views/sys-foreign-keys-transact-sql.md)|Представление каталога<br /><br /> Представление каталога<br /><br /> Представление каталога<br /><br /> Представление каталога|  
|sysdepends|[sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)|Представление каталога|  
|sysfilegroups|[sys.filegroups](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)|Представление каталога|  
|sysfiles|[sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)|Представление каталога|  
|sysforeignkeys|[sys.foreign_key_columns](../../relational-databases/system-catalog-views/sys-foreign-key-columns-transact-sql.md)|Представление каталога|  
|sysindexes|[sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)<br /><br /> [sys.partitions](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)<br /><br /> [sys.allocation_units](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)<br /><br /> [sys.dm_db_partition_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql.md)|Представление каталога<br /><br /> Представление каталога<br /><br /> Представление каталога<br /><br /> Динамическое административное представление|  
|sysindexkeys|[sys.index_columns](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)|Представление каталога|  
|sysmembers|[sys.database_role_members](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)|Представление каталога|  
|sysobjects|[sys.objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)|Представление каталога|  
|syspermissions|[sys.database_permissions](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)<br /><br /> [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md)|Представление каталога<br /><br /> Представление каталога|  
|sysprotects|[sys.database_permissions](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)<br /><br /> [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md)|Представление каталога<br /><br /> Представление каталога|  
|sysreferences|[sys.foreign_keys](../../relational-databases/system-catalog-views/sys-foreign-keys-transact-sql.md)|Представление каталога|  
|systypes|[sys.types](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)|Представление каталога|  
|sysusers|[sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)|Представление каталога|  
|sysfulltextcatalogs|[sys.fulltext_catalogs](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md)|Представление каталога|  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Системные таблицы (Transact-SQL)](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
