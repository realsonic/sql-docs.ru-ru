---
description: sys.database_files (Transact-SQL)
title: sys.database_files (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 09/19/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.database_files
- sys.database_files_TSQL
- database_files
- database_files_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_files catalog view
ms.assetid: 0f5b0aac-c17d-4e99-b8f7-d04efc9edf44
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 603b9d6fd9a2f15a4592a251dd2b9df6ddb44bfa
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093247"
---
# <a name="sysdatabase_files-transact-sql"></a>sys.database_files (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Содержит по одной строке для каждого из файлов базы данных, в которых она хранится. Это представление на каждую базу данных.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**file_id**|**int**|Идентификатор файла в базе данных.|  
|**file_guid**|**uniqueidentifier**|Идентификатор GUID файла.<br /><br /> NULL = база данных была обновлена с более ранней версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (допустима для SQL Server 2005 и более ранних версий).|  
|**type**|**tinyint**|Тип файла:<br/><br /> 0 = строки<br /><br/> 1 = журнал.<br/><br /> 2 = FILESTREAM.<br /><br /> 3 = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> 4 = полнотекстовый|  
|**type_desc**|**nvarchar(60)**|Описание типа файла:<br /><br /> ROWS <br /><br /> LOG<br /><br /> FILESTREAM<br /><br /> FULLTEXT|  
|**data_space_id**|**int**|Значение может быть больше или равно 0. Значение, равное 0, представляет файл журнала базы данных, а значение больше 0 представляет идентификатор файловой группы, в которой хранится этот файл данных.|  
|**name**|**sysname**|Логическое имя файла в базе данных.|  
|**physical_name**|**nvarchar(260)**|Имя файла в операционной системе. Если база данных размещена на [вторичной реплике, доступной для чтения](../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)AlwaysOn, **physical_name** указывает расположение файла базы данных первичной реплики. Для правильного расположения файла базы данных-получателя, доступной для чтения, запросите [sys.sysалтфилес](../../relational-databases/system-compatibility-views/sys-sysaltfiles-transact-sql.md).|  
|**state**|**tinyint**|Состояние файла:<br /><br /> 0 = ONLINE<br /><br /> 1 = RESTORING<br /><br /> 2 = RECOVERING<br /><br /> 3 = RECOVERY_PENDING<br /><br /> 4 = SUSPECT<br /><br /> 5 = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> 6 = OFFLINE<br /><br /> 7 = DEFUNCT|  
|**state_desc**|**nvarchar(60)**|Описание состояния файла:<br /><br /> ONLINE<br /><br /> RESTORING<br /><br /> RECOVERING<br /><br /> RECOVERY_PENDING<br /><br /> SUSPECT<br /><br /> OFFLINE<br /><br /> DEFUNCT<br /><br /> Дополнительные сведения см. в разделе [Состояния файлов](../../relational-databases/databases/file-states.md).|  
|**size**|**int**|Текущий размер файла в страницах по 8 КБ.<br /><br /> 0 = не определено.<br /><br /> Для моментального снимка базы данных аргумент size отражает максимальное пространство, которое моментальный снимок может использовать только для файла.<br /><br /> Для контейнеров файловой группы FILESTREAM размер отражает текущий используемый размер контейнера.|  
|**max_size**|**int**|Максимальный размер файла в страницах по 8 КБ:<br /><br /> 0 = Увеличение размера запрещено.<br /><br /> -1 = размер файла может увеличиваться до полного заполнения диска.<br /><br /> 268435456 = файл журнала может увеличиваться до 2 ТБ.<br /><br /> Для контейнеров файловой группы FILESTREAM max_size отражает максимальный размер контейнера.<br /><br /> Обратите внимание, что базы данных, которые были обновлены с неограниченным размером файла журнала, будут сообщать-1 о максимальном размере файла журнала.|  
|**квот**|**int**|0 = Файл имеет фиксированный размер и не будет увеличиваться.<br /><br /> >0 = файл будет автоматически расти.<br /><br /> Если значение is_percent_growth = 0, шаг увеличения размера указывается в единицах по 8 КБ, с округлением до ближайших 64 КБ.<br /><br /> Если значение аргумента is_percent_growth = 1, шаг увеличения размера выражается в процентах от общего размера.|  
|**is_media_read_only**|**bit**|1 = файл находится на носителе только для чтения.<br /><br /> 0 = файл размещен на носителе для чтения-записи.|  
|**is_read_only**|**bit**|1 = файл помечен как файл только для чтения.<br /><br /> 0 = файл помечен для чтения-записи.|  
|**is_sparse**|**bit**|1 = разреженный файл.<br /><br /> 0 = неразреженный файл.<br /><br /> Дополнительные сведения см. в разделе [Просмотр размера разреженного файла снимка базы данных (Transact-SQL)](../../relational-databases/databases/view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql.md).|  
|**is_percent_growth**|**bit**|1 = размер файла увеличивается в процентах.<br /><br /> 0 = абсолютное увеличение размера в страницах.|  
|**is_name_reserved**|**bit**|1 = имя удаленного файла (name или physical_name) доступно для использования только после следующего резервного копирования журнала. После того как файлы удалены из базы данных, логические имена остаются в зарезервированном состоянии до следующего резервного копирования журнала. Этот столбец является важным только в случае использования модели полного восстановления и модели восстановления с неполным протоколированием.|  
|**create_lsn**|**numeric(25,0)**|Регистрационный номер транзакции в журнале (LSN), на котором создан файл.|  
|**drop_lsn**|**numeric(25,0)**|Номер LSN, с которым файл удален.<br /><br /> 0 = имя файла недоступно для повторного использования.|  
|**read_only_lsn**|**numeric(25,0)**|Номер LSN, на котором файловая группа, содержащая файл, изменила тип с «для чтения и записи» на «только для чтения» (самое последнее изменение).|  
|**read_write_lsn**|**numeric(25,0)**|Номер LSN, на котором файловая группа, содержащая файл, изменила тип с «только для чтения» на «для чтения и записи» (самое последнее изменение).|  
|**differential_base_lsn**|**numeric(25,0)**|Основа для разностных резервных копий. Экстенты данных, измененных после того, как этот номер LSN будет включен в разностную резервную копию.|  
|**differential_base_guid**|**uniqueidentifier**|Уникальный идентификатор базовой резервной копии, на которой будет основываться разностная резервная копия.|  
|**differential_base_time**|**datetime**|Время, соответствующее differential_base_lsn.|  
|**redo_start_lsn**|**numeric(25,0)**|Номер LSN, с которого должен начаться следующий накат.<br /><br /> Равно NULL за исключением случаев, когда значение аргумента state = RESTORING или значение аргумента state = RECOVERY_PENDING.|  
|**redo_start_fork_guid**|**uniqueidentifier**|Уникальный идентификатор точки вилки восстановления. Значение аргумента first_fork_guid следующей восстановленной резервной копии журнала должно совпадать с этим значением. Представляет текущее состояние файла.|  
|**redo_target_lsn**|**numeric(25,0)**|Номер LSN, на котором накат в режиме «в сети» по данному файлу может остановиться.<br /><br /> Равно NULL за исключением случаев, когда значение аргумента state = RESTORING или значение аргумента state = RECOVERY_PENDING.|  
|**redo_target_fork_guid**|**uniqueidentifier**|Вилка восстановления, на которой файл может быть восстановлен. Используется в паре с redo_target_lsn.|  
|**backup_lsn**|**numeric(25,0)**|Номер LSN самых новых данных или разностная резервная копия файла.|  
  
> [!NOTE]  
>  При удалении или перестройке больших индексов либо удалении или усечении больших таблиц компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] откладывает фактическое освобождение страниц и связанных блокировок до момента фиксации транзакции. Отложенные операции удаления не освобождают выделенное место немедленно. Следовательно, значения, полученные из sys.database_files сразу после удаления или усечения больших объектов, могут не соответствовать фактическому размеру свободного места на диске.  
  
## <a name="permissions"></a>Разрешения  
 Необходимо быть членом роли **public**. Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  

## <a name="examples"></a>Примеры  
Следующая инструкция возвращает имя, размер файла и объем пустого пространства для каждого файла базы данных.

```
SELECT name, size/128.0 FileSizeInMB,
size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0 
   AS EmptySpaceInMB
FROM sys.database_files;
```
Дополнительные сведения об использовании см [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] . в разделе [Определение размера базы данных в базе данных SQL Azure 12](/archive/blogs/sqlcat/determining-database-size-in-azure-sql-database-v12) в блоге группы консультирования клиентов SQL.
  
## <a name="see-also"></a>См. также:  
 [Представления каталогов баз данных и файлов (Transact-SQL)](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)   
 [Состояния файлов](../../relational-databases/databases/file-states.md)   
 [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.master_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)   
 [Database Files and Filegroups](../../relational-databases/databases/database-files-and-filegroups.md)   
 [sys.data_spaces (Transact-SQL)](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)  
  
