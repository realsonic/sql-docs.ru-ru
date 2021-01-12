---
description: sys.server_audits (Transact-SQL)
title: sys.server_audits (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 04/05/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- server_audits_TSQL
- sys.server_audits_TSQL
- sys.server_audits
- server_audits
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_audits catalog view
ms.assetid: c2c4a000-1127-46a8-b1e9-947fd1136e1e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 3bb5b8b56cb2fec01b6fe655f3f51841cc7ebb21
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093099"
---
# <a name="sysserver_audits-transact-sql"></a>sys.server_audits (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Содержит по одной строке для каждого аудита [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в экземпляре сервера. Дополнительные сведения см. в статье [Подсистема аудита SQL Server (ядро СУБД)](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**audit_id**|**int**|Идентификатор аудита.|  
|**name**|**sysname**|Имя аудита.|  
|**audit_guid**|**uniqueidentifier**|Идентификатор GUID для аудита, который используется для перечисления аудитов с&#124;рядовыми серверными спецификациями аудита базы данных во время операций запуска сервера и подключения к базе данных.|  
|**create_date**|**datetime**|Дата и время создания аудита в формате UTC.|  
|**modify_date**|**datetime**|Дата и время последнего изменения аудита в формате UTC.|  
|**principal_id**|**int**|Идентификатор владельца аудита, зарегистрированного на сервере.|  
|**type**|**char(2)**|Тип аудита:<br /><br /> Журнал событий безопасности SL-NT<br /><br /> AL — журнал событий приложений NT<br /><br /> FL — файл в файловой системе|  
|**type_desc**|**nvarchar(60)**|ЖУРНАЛ БЕЗОПАСНОСТИ<br /><br /> ЖУРНАЛ ПРИЛОЖЕНИЙ<br /><br /> FILE|  
|**on_failure**|**tinyint**|В случае ошибки записи данных о действии:<br /><br /> 0 — продолжить<br /><br /> 1. Завершение работы экземпляра сервера<br /><br /> 2 — сбой операции|  
|**on_failure_desc**|**nvarchar(60)**|В случае ошибки записи данных о действии:<br /><br /> CONTINUE<br /><br /> SHUTDOWN SERVER INSTANCE<br /><br /> FAIL_OPERATION|  
|**is_state_enabled**|**tinyint**|0 — отключено<br /><br /> 1 — включено.|  
|**queue_delay**|**int**|Максимальное время ожидания перед записью на диск, в миллисекундах. Если 0, то аудит гарантирует запись, прежде чем событие может быть продолжено.|  
|**предикат**|**nvarchar (3000)**|Выражение предиката, применяемое к событию.|  
  
## <a name="permissions"></a>Разрешения  
 Участники с разрешением **ALTER ANY SERVER AUDIT** или **View ANY DEFINITION** имеют доступ к этому представлению каталога. Кроме того, участнику не должно быть запрещено разрешение **View ANY DEFINITION** .  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также  
 [CREATE SERVER AUDIT (Transact-SQL)](../../t-sql/statements/create-server-audit-transact-sql.md)   
 [ALTER SERVER AUDIT (Transact-SQL)](../../t-sql/statements/alter-server-audit-transact-sql.md)   
 [DROP SERVER AUDIT (Transact-SQL)](../../t-sql/statements/drop-server-audit-transact-sql.md)   
 [CREATE SERVER AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/create-server-audit-specification-transact-sql.md)   
 [ALTER SERVER AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/alter-server-audit-specification-transact-sql.md)   
 [DROP SERVER AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/drop-server-audit-specification-transact-sql.md)   
 [CREATE DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/create-database-audit-specification-transact-sql.md)   
 [ALTER DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/alter-database-audit-specification-transact-sql.md)   
 [DROP DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/drop-database-audit-specification-transact-sql.md)   
 [ALTER AUTHORIZATION (Transact-SQL)](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sys.fn_get_audit_file (Transact-SQL)](../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md)   
 [sys.server_file_audits (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)   
 [sys.server_audit_specifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)   
 [sys.server_audit_specification_details (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-audit-specification-details-transact-sql.md)   
 [sys.database_audit_specifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)   
 [sys.database_audit_specification_details (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)   
 [sys.dm_server_audit_status (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)   
 [sys.dm_audit_actions (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)   
 [sys.dm_audit_class_type_map (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-audit-class-type-map-transact-sql.md)   
 [Создание аудита сервера и спецификации аудита сервера](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
  
