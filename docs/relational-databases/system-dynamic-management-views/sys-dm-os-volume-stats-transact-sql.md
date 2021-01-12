---
description: sys.dm_os_volume_stats (Transact-SQL)
title: sys.dm_os_volume_stats (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 09/03/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_volume_stats_TSQL
- dm_os_volume_stats
- sys.dm_os_volume_stats
- sys.dm_os_volume_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_volume_stats dynamic management function
ms.assetid: fa1c58ad-8487-42ad-956c-983f2229025f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 32deca4f06709806d90ee7d25e25105f191ad23e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098829"
---
# <a name="sysdm_os_volume_stats-transact-sql"></a>sys.dm_os_volume_stats (Transact-SQL)
[!INCLUDE[tsql-appliesto-2008R2SP1-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-2008R2sp1-xxxx-xxxx-xxx-md.md)]

  Возвращает сведения о томе (каталоге) операционной системы, в котором хранятся указанные базы данных и файлы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Используйте эту функцию динамического управления для проверки атрибутов физического диска или для получения сведений об объеме свободного пространства в каталоге.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
sys.dm_os_volume_stats (database_id, file_id)  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Аргументы  
 *database_id*  
 Идентификатор базы данных. Аргумент *database_id* имеет тип **int** и не имеет значения по умолчанию. Не может быть NULL.  
  
 *file_id*  
 Идентификатор файла. *file_id* имеет **тип int** и не имеет значения по умолчанию. Не может быть NULL.  
  
## <a name="table-returned"></a>Возвращаемая таблица  
  
||||  
|-|-|-|  
|**Столбец**|**Data type**|**Описание**|  
|**database_id**|**int**|Идентификатор базы данных. Не может иметь значение NULL.|  
|**file_id**|**int**|Идентификатор файла. Не может иметь значение NULL.|  
|**volume_mount_point**|**nvarchar(512)**|Точка подключения, с которой ассоциирован корень тома. Может возвращать пустую строку. Возвращает значение NULL в операционной системе Linux.|  
|**volume_id**|**nvarchar(512)**|Идентификатор тома операционной системы. Может возвращать пустую строку. Возвращает значение NULL в операционной системе Linux.|  
|**logical_volume_name**|**nvarchar(512)**|Логическое имя тома. Может возвращать пустую строку. Возвращает значение NULL в операционной системе Linux.|  
|**file_system_type**|**nvarchar(512)**|Тип файловой системы тома (например, NTFS, FAT, RAW). Может возвращать пустую строку. Возвращает значение NULL в операционной системе Linux.|  
|**total_bytes**|**bigint**|Общий размер тома в байтах. Не может иметь значение NULL.|  
|**available_bytes**|**bigint**|Доступное свободное место на томе. Не может иметь значение NULL.|  
|**supports_compression**|**tinyint**|Указывает, поддерживает ли том сжатие на уровне операционной системы. Не может иметь значение NULL в Windows и возвращает значение NULL в операционной системе Linux.|  
|**supports_alternate_streams**|**tinyint**|Указывает, поддерживает ли том дополнительные потоки. Не может иметь значение NULL в Windows и возвращает значение NULL в операционной системе Linux.|  
|**supports_sparse_files**|**tinyint**|Указывает, поддерживает ли том разреженные файлы.  Не может иметь значение NULL в Windows и возвращает значение NULL в операционной системе Linux.|  
|**is_read_only**|**tinyint**|Указывает, помечен ли том как доступный только для чтения. Не может иметь значение NULL.|  
|**is_compressed**|**tinyint**|Указывает, сжат ли том в настоящее время. Не может иметь значение NULL в Windows и возвращает значение NULL в операционной системе Linux.|  
|**incurs_seek_penalty**|**tinyint**|Указывает тип хранилища, поддерживающего этот том. Возможны следующие значения:<br /><br />0: не выполнять поиск на этом томе, как правило, если устройство хранения PMM или SSD<br /><br />1: Поиск штрафов на этом томе, обычно если устройство хранения является HDD<br /><br />2: тип хранилища не может быть определен, если том расположен по UNC-пути или подключенным общим папкам<br /><br />NULL: тип хранилища не может быть определен в операционной системе Linux<br /><br />**Применимо к:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] )|  
  
## <a name="security"></a>Безопасность  
  
### <a name="permissions"></a>Разрешения  
 Требуется разрешение `VIEW SERVER STATE`.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-return-total-space-and-available-space-for-all-database-files"></a>A. Получение сведений об общем и доступном пространстве для всех файлов баз данных  
 В следующем примере выполняется возврат значения общего и доступного пространства (в байтах) для всех файлов баз данных в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
SELECT f.database_id, f.file_id, volume_mount_point, total_bytes, available_bytes  
FROM sys.master_files AS f  
CROSS APPLY sys.dm_os_volume_stats(f.database_id, f.file_id);  
```  
  
### <a name="b-return-total-space-and-available-space-for-the-current-database"></a>Б. Получение сведений об общем и доступном пространстве для текущей базы данных  
 В следующем примере выполняется возврат значения общего и доступного пространства (в байтах) для файлов текущей базы данных.  
  
```sql  
SELECT database_id, f.file_id, volume_mount_point, total_bytes, available_bytes  
FROM sys.database_files AS f  
CROSS APPLY sys.dm_os_volume_stats(DB_ID(f.name), f.file_id);  
```  
  
## <a name="see-also"></a>См. также:  
 [sys.master_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)   
 [sys.database_files (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)  
  
  
