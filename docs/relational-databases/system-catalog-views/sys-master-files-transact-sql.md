---
description: sys.master_files (Transact-SQL)
title: sys.master_files (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/10/2016
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.master_files
- master_files_TSQL
- sys.master_files_TSQL
- master_files
dev_langs:
- TSQL
helpviewer_keywords:
- sys.master_files catalog view
ms.assetid: 803b22f2-0016-436b-a561-ce6f023d6b6a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 55b5465b2c43b02247a48478272393f941a4a51d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095546"
---
# <a name="sysmaster_files-transact-sql"></a>sys.master_files (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Содержит по одной строке для каждого файла базы данных, как хранящиеся в базе данных master. Это единственное общесистемное представление.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|database_id|**int**|Идентификатор базы данных, которому принадлежит данный файл. Masterdatabase_id всегда имеет значение 1.|  
|file_id|**int**|Идентификатор файла в базе данных. Параметр file_id первичного файла всегда имеет значение 1.|  
|file_guid|**uniqueidentifier**|Уникальный идентификатор файла.<br /><br /> NULL = база данных была обновлена с более ранней версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (допустима для SQL Server 2005 и более ранних версий).|  
|тип|**tinyint**|Тип файла:<br /><br /> 0 = строки.<br /><br /> 1 = журнал.<br /><br /> 2 = FILESTREAM.<br /><br /> 3 = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> 4 = полнотекстовый каталог (полнотекстовые каталоги в версии, предшествующей [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]; полнотекстовые каталоги, которые обновлены до версии или созданы в версии [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и последующих, сообщают о типе файла 0).|  
|type_desc|**nvarchar(60)**|Описание типа файла:<br /><br /> ROWS<br /><br /> LOG<br /><br /> FILESTREAM<br /><br /> FULLTEXT (полнотекстовые каталоги в версии, предшествующей [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]).|  
|data_space_id|**int**|Идентификатор пространства данных, которому принадлежит этот файл. Пространство данных является файловой группой.<br /><br /> 0 = файлы журнала|  
|name|**sysname**|Логическое имя файла в базе данных.|  
|physical_name|**nvarchar(260)**|Имя файла в операционной системе.|  
|Состояние|**tinyint**|Состояние файла:<br /><br /> 0 = ONLINE<br /><br /> 1 = RESTORING<br /><br /> 2 = RECOVERING<br /><br /> 3 = RECOVERY_PENDING<br /><br /> 4 = SUSPECT<br /><br /> 5 = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> 6 = OFFLINE<br /><br /> 7 = DEFUNCT|  
|state_desc|**nvarchar(60)**|Описание состояния файла:<br /><br /> ONLINE<br /><br /> RESTORING<br /><br /> RECOVERING<br /><br /> RECOVERY_PENDING<br /><br /> SUSPECT<br /><br /> OFFLINE<br /><br /> DEFUNCT<br /><br /> Дополнительные сведения см. в разделе [Состояния файлов](../../relational-databases/databases/file-states.md).|  
|размер;|**int**|Текущий размер файла, в 8 КБ страницах. Для моментального снимка базы данных аргумент size отражает максимальное пространство, которое моментальный снимок может использовать только для файла.<br /><br /> Примечание. это поле заполняется нулевым числом контейнеров FILESTREAM. Запросите представление каталога *sys.database_files* , чтобы получить фактический размер контейнеров FILESTREAM.|  
|max_size|**int**|Максимальный размер файла в страницах по 8 КБ:<br /><br /> 0 = Увеличение размера запрещено.<br /><br /> -1 = размер файла может увеличиваться до полного заполнения диска.<br /><br /> 268435456 = файл журнала может увеличиваться до 2 ТБ.<br /><br /> Примечание. базы данных, которые были обновлены с неограниченным размером файла журнала, будут сообщать-1 о максимальном размере файла журнала.|  
|growth|**int**|0 = Файл имеет фиксированный размер и не будет увеличиваться.<br /><br /> >0 = файл будет автоматически расти.<br /><br /> Если аргумент is_percent_growth имеет значение 0, шаг роста измеряется в страницах по 8 КБ, округленных до ближайших 64 КБ.<br /><br /> Если значение аргумента is_percent_growth = 1, шаг увеличения размера выражается в процентах от общего размера.|  
|is_media_read_onlyF|**bit**|1 = файл находится на носителе только для чтения.<br /><br /> 0 = Файл размещен на носителе, доступно для чтения и записи.|  
|is_read_only|**bit**|1 = файл помечен как файл только для чтения.<br /><br /> 0 = Файл помечен как доступный для чтения и записи.|  
|is_sparse|**bit**|1 = разреженный файл.<br /><br /> 0 = неразреженный файл.<br /><br /> Дополнительные сведения см. в разделе [Просмотр размера разреженного файла снимка базы данных (Transact-SQL)](../../relational-databases/databases/view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql.md).|  
|is_percent_growth|**bit**|1 = размер файла увеличивается в процентах.<br /><br /> 0 = абсолютное увеличение размера в страницах.|  
|is_name_reserved|**bit**|1 = имя удаленного файла, доступно для использования. Резервная копия журнала должна быть получена, перед тем как имя (аргументы name или physical_name) может быть использовано для нового имени файла.<br /><br /> 0 = имя файла, недоступно для использования.|  
|create_lsn|**numeric(25,0)**|Регистрационный номер транзакции в журнале (LSN), на котором создан файл.|  
|drop_lsn|**numeric(25,0)**|Номер LSN, с которым файл удален.|  
|read_only_lsn|**numeric(25,0)**|Номер LSN, на котором файловая группа, содержащая файл, изменила тип с «для чтения и записи» на «только для чтения» (самое последнее изменение).|  
|read_write_lsn|**numeric(25,0)**|Номер LSN, на котором файловая группа, содержащая файл, изменила тип с «только для чтения» на «для чтения и записи» (самое последнее изменение).|  
|differential_base_time|**numeric(25,0)**|Основа для разностных резервных копий. Экстенты данных, измененных после того, как этот номер LSN будет включен в разностную резервную копию.|  
|differential_base_guid|**uniqueidentifier**|Уникальный идентификатор базовой резервной копии, на которой будет основываться разностная резервная копия.|  
|differential_base_time|**datetime**|Время, соответствующее differential_base_lsn.|  
|redo_start_lsn|**numeric(25,0)**|Номер LSN, с которого должен начаться следующий накат.<br /><br /> Равно NULL за исключением случаев, когда значение аргумента state = RESTORING или значение аргумента state = RECOVERY_PENDING.|  
|redo_start_fork_guid|**uniqueidentifier**|Уникальный идентификатор точки вилки восстановления. Значение аргумента first_fork_guid следующей восстановленной резервной копии журнала должно совпадать с этим значением. Это отражает текущее состояние контейнера.|  
|redo_target_lsn|**numeric(25,0)**|Номер LSN, на котором накат в режиме «в сети» по данному файлу может остановиться.<br /><br /> Равно NULL за исключением случаев, когда значение аргумента state = RESTORING или значение аргумента state = RECOVERY_PENDING.|  
|redo_target_fork_guid|**uniqueidentifier**|Вилка восстановления, на которой может быть восстановлен контейнер. Используется в паре с redo_target_lsn.|  
|backup_lsn|**numeric(25,0)**|Номер LSN самых новых данных или разностная резервная копия файла.|  
|credential_id|**int**|Объект `credential_id` , `sys.credentials` используемый для хранения файла. Например, если выполняется [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на виртуальной машине Azure, а файлы базы данных хранятся в хранилище BLOB-объектов Azure, учетные данные настраиваются с учетными данными доступа к месту хранения.|  
  
> [!NOTE]  
>  При удалении или перестройке больших индексов либо удалении или усечении больших таблиц компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] откладывает фактическое освобождение страниц и связанных блокировок до момента фиксации транзакции. Отложенные операции удаления не освобождают выделенное место немедленно. Поэтому значения, возвращаемые sys.master_files сразу после удаления или обрезания большого объекта, могут не отражать доступное пространство на диске.  
  
## <a name="permissions"></a>Разрешения  
 Минимальные разрешения, необходимые для просмотра соответствующих строк — CREATE DATABASE, ALTER ANY DATABASE или VIEW ANY DEFINITION.  
  
## <a name="see-also"></a>См. также:  
 [Представления каталогов баз данных и файлов (Transact-SQL)](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)   
 [Состояния файлов](../../relational-databases/databases/file-states.md)   
 [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.database_files (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [Файлы и файловые группы базы данных](../../relational-databases/databases/database-files-and-filegroups.md)  
  
  
