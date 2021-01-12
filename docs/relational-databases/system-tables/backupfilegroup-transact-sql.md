---
description: backupfilegroup (Transact-SQL)
title: backupfilegroup (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- backupfilegroup_TSQL
- backupfilegroup
dev_langs:
- TSQL
helpviewer_keywords:
- filegroups [SQL Server], backupfilegroup system table
- backupfilegroup system table
ms.assetid: d26e8fbe-f5c5-4e10-b2bd-0d8e16ea21f9
author: cawrites
ms.author: chadam
ms.openlocfilehash: cd2f3ac634f737772691c3fdb7a1d4ab4d736123
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102745"
---
# <a name="backupfilegroup-transact-sql"></a>backupfilegroup (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Содержит по одной строке для каждой файловой группы во время резервного копирования. **backupfilegroup** хранится в базе данных **msdb** .  
  
> [!NOTE]  
>  В таблице **backupfilegroup** показана конфигурация файловой группы базы данных, а не резервный набор. Чтобы определить, включен ли файл в резервный набор данных, используйте столбец **is_present** таблицы [backupfile](../../relational-databases/system-tables/backupfile-transact-sql.md) .  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**backup_set_id**|**int**|Резервный набор данных, содержащий эту файловую группу.|  
|**name**|**sysname**|Имя файловой группы.|  
|**filegroup_id**|**int**|Идентификатор файловой группы, уникальный в пределах базы данных. Соответствует **data_space_id** в **sys. FILEGROUP**.|  
|**filegroup_guid**|**uniqueidentifier**|Глобальный уникальный идентификатор файловой группы. Может иметь значение NULL.|  
|**type**|**char(2)**|Тип содержимого, может принимать одно из двух значений:<br /><br /> FG = файловая группа «Строки»<br /><br /> SL = файловая группа журнала сервера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|**type_desc**|**nvarchar(60)**|Описание типа функции, может принимать одно из двух значений:<br /><br /> ROWS_FILEGROUP<br /><br /> SQL_LOG_FILEGROUP|  
|**is_default**|**bit**|Файловая группа по умолчанию; используется, если в инструкции CREATE TABLE или CREATE INDEX не указана ни одна из файловых групп.|  
|**is_readonly**|**bit**|1 = Файловая группа доступна только для чтения.|  
|**log_filegroup_guid**|**uniqueidentifier**|Может иметь значение NULL.|  
  
## <a name="remarks"></a>Комментарии  
  
> [!IMPORTANT]  
>  Одно и то же имя файловой группы может использоваться в разных базах данных, однако у каждой файловой группы есть свой идентификатор GUID. Таким образом, **(backup_set_id, filegroup_guid)** — уникальный ключ, определяющий файловую группу в **backupfilegroup**.  
  
 Инструкция RESTORE VERIFYONLY из *backup_device* with LOADHISTORY заполняет столбцы таблицы **backupmediaset** соответствующими значениями из заголовка набора носителей.  
  
 Чтобы уменьшить количество строк в этой таблице и в других таблицах резервного копирования и журнала, выполните хранимую процедуру [sp_delete_backuphistory](../../relational-databases/system-stored-procedures/sp-delete-backuphistory-transact-sql.md) .  
  
## <a name="see-also"></a>См. также:  
 [Резервное копирование и восстановление таблиц &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)   
 [backupfile (Transact-SQL)](../../relational-databases/system-tables/backupfile-transact-sql.md)   
 [backupmediafamily (Transact-SQL)](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)   
 [backupmediaset (Transact-SQL)](../../relational-databases/system-tables/backupmediaset-transact-sql.md)   
 [backupset (Transact-SQL)](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [Системные таблицы (Transact-SQL)](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
