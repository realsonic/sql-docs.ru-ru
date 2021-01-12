---
description: backupset (Transact-SQL)
title: Backup (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 12/24/2020
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- backupset
- backupset_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- backupset system table
- backup media [SQL Server], backupset system table
- backup sets [SQL Server]
ms.assetid: 6ff79bbf-4acf-4f75-926f-38637ca8a943
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 87d509f0a49668bd354fd1132bb3501c6d2bdbcd
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098715"
---
# <a name="backupset-transact-sql"></a>backupset (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Содержит по одной строке для каждого резервного набора данных. *Резервный набор данных* содержит резервную копию, полученную в результате отдельной успешной операции резервного копирования. Инструкции RESTORE, RESTORE FILELISTONLY, RESTORE HEADERONLY и RESTORE VERIFYONLY выполняются над отдельным резервным набором данных в рамках набора носителей на указанном устройстве или устройствах резервного копирования.  
  
 Эта таблица хранится в базе данных **msdb** .  

|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**backup_set_id**|**int**|Уникальный идентификационный номер резервного набора, который определяет резервный набор. Удостоверение, первичный ключ.|  
|**backup_set_uuid**|**uniqueidentifier**|Уникальный идентификационный номер резервного набора, который определяет резервный набор.|  
|**media_set_id**|**int**|Уникальный идентификационный номер набора носителей, который определяет набор носителей, содержащий резервный набор данных. Ссылается на **backupmediaset (media_set_id)**.|  
|**first_family_number**|**tinyint**|Номер семейства носителя, с которого начинается резервный набор данных. Может иметь значение NULL.|  
|**first_media_number**|**smallint**|Номер носителя, с которого начинается резервный набор данных. Может иметь значение NULL.|  
|**last_family_number**|**tinyint**|Номер семейства носителя, которым заканчивается резервный набор данных. Может иметь значение NULL.|  
|**last_media_number**|**smallint**|Номер носителя, которым заканчивается резервный набор данных. Может иметь значение NULL.|  
|**catalog_family_number**|**tinyint**|Номер семейства носителя, содержащего начало каталога резервного набора данных. Может иметь значение NULL.|  
|**catalog_media_number**|**smallint**|Номер носителя, содержащего начало каталога резервного набора данных. Может иметь значение NULL.|  
|**position**|**int**|Позиция резервного набора данных, используемая в операции восстановления для поиска соответствующего резервного набора данных и файлов. Может иметь значение NULL. Дополнительные сведения см. в разделе файл в [BACKUP &#40;&#41;Transact-SQL ](../../t-sql/statements/backup-transact-sql.md).|  
|**expiration_date**|**datetime**|Дата и время окончания срока действия для резервного набора. Может иметь значение NULL.|  
|**software_vendor_id**|**int**|Идентификационный номер поставщика программного обеспечения, выполняющего запись заголовка резервного носителя. Может иметь значение NULL.|  
|**name**|**nvarchar(128)**|Имя резервного набора. Может иметь значение NULL.|  
|**description**|**nvarchar(255)**|Описание резервного набора данных. Может иметь значение NULL.|  
|**user_name**|**nvarchar(128)**|Имя пользователя, выполняющего операцию резервного копирования. Может иметь значение NULL.|  
|**software_major_version**|**tinyint**|[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]основной номер версии. Может иметь значение NULL.|  
|**software_minor_version**|**tinyint**|Дополнительный номер версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Может иметь значение NULL.|  
|**software_build_version**|**smallint**|Номер сборки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Может иметь значение NULL.|  
|**time_zone**|**smallint**|Разница между местным временем (где выполняется операция резервного копирования) и временем в 15-минутном скоординированном времени (UTC) с использованием сведений о часовом поясе во время начала операции резервного копирования. Может принимать значения от -48 до +48 включительно. Значение 127 соответствует неизвестному значению. Например, -20 — время на восточном побережье США (Eastern Standard Time, EST), отстоящее на пять часов вперед от UTC. Может иметь значение NULL.|  
|**mtf_minor_version**|**tinyint**|Дополнительный номер версии [!INCLUDE[msCoName](../../includes/msconame-md.md)] Tape Format. Может иметь значение NULL.|  
|**first_lsn**|**numeric(25,0)**|Регистрационный номер транзакции в журнале для первой или самой ранней записи журнала в резервном наборе данных. Может иметь значение NULL.|  
|**last_lsn**|**numeric(25,0)**|Регистрационный номер транзакции в журнале для следующей записи журнала после резервного набора данных. Может иметь значение NULL.|  
|**checkpoint_lsn**|**numeric(25,0)**|Регистрационный номер транзакции в журнале для записи, с которой должна начинаться операция повтора. Может иметь значение NULL.|  
|**database_backup_lsn**|**numeric(25,0)**|Регистрационный номер транзакции в журнале для последней полной резервной копии базы данных. Может иметь значение NULL.<br /><br /> **database_backup_lsn** — это "начало контрольной точки", которое активируется при запуске резервного копирования. Этот номер LSN будет совпадать с **first_lsn** , если резервная копия предпринимается, когда база данных бездействует и репликация не настроена.|  
|**database_creation_date**|**datetime**|Дата и время изначального создания базы данных. Может иметь значение NULL.|  
|**backup_start_date**|**datetime**|Дата и время начала операции резервного копирования. Может иметь значение NULL.|  
|**backup_finish_date**|**datetime**|Дата и время окончания операции резервного копирования. Может иметь значение NULL.|  
|**type**|**char(1)**|Тип резервного копирования. Возможны следующие варианты:<br /><br /> D = база данных<br /><br /> I = разностное копирование базы данных;<br /><br /> L = журнал<br /><br /> F = копирование файла или файловой группы;<br /><br /> G = разностное копирование файла;<br /><br /> P = частичное копирование;<br /><br /> Q = частичное разностное копирование.<br /><br /> Может иметь значение NULL.|  
|**sort_order**|**smallint**|Порядок сортировки на сервере, выполняющем операцию резервного копирования. Может иметь значение NULL. Дополнительные сведения о порядках сортировки и параметрах сортировки см. в разделе [Параметры сортировки и поддержка Юникода](../../relational-databases/collations/collation-and-unicode-support.md).|  
|**code_page**|**smallint**|Кодовая страница на сервере, выполняющем операцию резервного копирования. Может иметь значение NULL. Дополнительные сведения о кодовых страницах см. в разделе [Параметры сортировки и поддержка Юникода](../../relational-databases/collations/collation-and-unicode-support.md).|  
|**compatibility_level**|**tinyint**|Настройка уровня совместимости для базы данных. Возможны следующие варианты:<br /><br /> 90 = [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]<br /><br /> 100 = [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]<br /><br /> 110 = [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]<br /><br /> 120 = [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]<br /><br /> Может иметь значение NULL.<br /><br /> Дополнительные сведения об уровнях совместимости см. в разделе [Уровень совместимости ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).|  
|**database_version**|**int**|Номер версии базы данных. Может иметь значение NULL.|  
|**backup_size**|**numeric(20,0)**|Размер резервного набора данных в байтах. Может иметь значение NULL. Для резервных копий VSS backup_size является оценочным значением.|  
|**database_name**|**nvarchar(128)**|Имя базы данных, участвовавшей в операции резервного копирования. Может иметь значение NULL.|  
|**server_name**|**nvarchar(128)**|Имя сервера, выполняющего операцию резервного копирования [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Может иметь значение NULL.|  
|**machine_name**|**nvarchar(128)**|Имя компьютера, на котором выполняется [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Может иметь значение NULL.|  
|**flags**|**int**|В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] столбец **flags** является устаревшим и заменяется следующими битовыми столбцами:<br /><br /> **has_bulk_logged_data** <br /> **is_snapshot** <br /> **is_readonly** <br /> **is_single_user** <br /> **has_backup_checksums** <br /> **is_damaged** <br /> **begins_log_chain** <br /> **has_incomplete_metadata** <br /> **is_force_offline** <br /> **is_copy_only**<br /><br /> Может иметь значение NULL.<br /><br /> В резервных наборах данных, созданных в предыдущих версиях [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], биты-флаги означают:<br />1 = резервная копия содержит минимум записанных в журнал данных; <br />2 = использовано предложение WITH SNAPSHOT; <br />4 = база данных во время резервного копирования была доступна только для чтения.<br />8 = база данных во время резервного копирования находилась в однопользовательском режиме.|  
|**unicode_locale**|**int**|Локаль Юникод. Может иметь значение NULL.|  
|**unicode_compare_style**|**int**|Стиль сравнения Юникод. Может иметь значение NULL.|  
|**collation_name**|**nvarchar(128)**|Имя параметров сортировки. Может иметь значение NULL.|  
|**Is_password_protected**|**bit**|Определяет, защищен ли резервный набор данных<br /><br /> паролем:<br /><br /> 0 = не защищен<br /><br /> 1 = защищен|  
|**recovery_model**|**nvarchar(60)**|Модель восстановления базы данных:<br /><br /> FULL<br /><br /> BULK-LOGGED;<br /><br /> ПРОСТОЙ|  
|**has_bulk_logged_data**|**bit**|1 = резервная копия содержит данные неполного журнала и массовых изменений.|  
|**is_snapshot**|**bit**|1 = резервная копия была создана с использованием параметра SNAPSHOT.|  
|**is_readonly**|**bit**|1 = база данных во время резервного копирования была доступна только для чтения.|  
|**is_single_user**|**bit**|1 = база данных во время резервного копирования находилась в однопользовательском режиме.|  
|**has_backup_checksums**|**bit**|1 = резервная копия содержит контрольные суммы резервных копий.|  
|**is_damaged**|**bit**|1 = при создании резервной копии было обнаружено повреждение базы данных. Было указано продолжать операцию резервного копирования, несмотря на ошибки.|  
|**begins_log_chain**|**bit**|1 = это первая резервная копия журналов в непрерывной цепочке. Цепочка журналов начинается с первой резервной копии журналов, выполненной после создания базы данных или переключения от простой модели восстановления к полной или модели восстановления с неполным протоколированием.|  
|**has_incomplete_metadata**|**bit**|1 = резервная копия заключительного фрагмента журнала с неполными метаданными. Дополнительные сведения см. в статье [Резервные копии заключительного фрагмента журнала (SQL Server)](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).|  
|**is_force_offline**|**bit**|1 = база данных была переведена в режим вне сети посредством параметра NORECOVERY при создании резервной копии.|  
|**is_copy_only**|**bit**|1 = резервная копия только для копирования. Дополнительные сведения см. в разделе [Резервные копии только для копирования (SQL Server)](../../relational-databases/backup-restore/copy-only-backups-sql-server.md).|  
|**first_recovery_fork_guid**|**uniqueidentifier**|Идентификатор начальной вилки восстановления. Это соответствует **значение firstrecoveryforkid** RESTORE HEADERONLY.<br /><br /> Для резервных копий данных **first_recovery_fork_guid** равно **last_recovery_fork_guid**.|  
|**last_recovery_fork_guid**|**uniqueidentifier**|Идентификатор конечной вилки восстановления. Это соответствует **RecoveryForkID** RESTORE HEADERONLY.<br /><br /> Для резервных копий данных **first_recovery_fork_guid** равно **last_recovery_fork_guid**.|  
|**fork_point_lsn**|**numeric(25,0)**|Если **first_recovery_fork_guid** не равно **last_recovery_fork_guid**, это регистрационный номер в журнале для точки ветвления. В противном случае значение равно NULL.|  
|**database_guid**|**uniqueidentifier**|Уникальный идентификатор базы данных. Это соответствует **биндингид** RESTORE HEADERONLY. При восстановлении базы данных назначается новое значение.|  
|**family_guid**|**uniqueidentifier**|Уникальный идентификатор оригинальной базы данных в момент создания. Это значение остается неизменным при восстановлении базы данных, даже если ей присваивается другое имя.|  
|**differential_base_lsn**|**numeric(25,0)**|Основной регистрационный номер транзакции в журнале для разностного резервного копирования. Для разностной резервной копии с одной на основе; изменения с номерами LSN больше или равными **differential_base_lsn** включаются в разностную резервную копию.<br /><br /> Для многобазовой разностной резервной копии значение равно NULL, а базовый номер LSN должен быть определен на уровне файла (см. раздел [backupfile &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfile-transact-sql.md)).<br /><br /> Для неразностных типов резервного копирования значение всегда равно NULL.|  
|**differential_base_guid**|**uniqueidentifier**|Для разностной резервной копии с одной основой значение является уникальным идентификатором базовой копии для разностного копирования.<br /><br /> Для многобазового разностного резервного копирования значение равно NULL, а базовая копия для разностного копирования должна быть определена на файловом уровне.<br /><br /> Для неразностных типов резервного копирования значение равно NULL.|  
|**compressed_backup_size**|**Numeric (20, 0)**|Общее число байт в резервной копии, хранящейся на диске.<br /><br /> Чтобы вычислить коэффициент сжатия, используйте **compressed_backup_size** и **backup_size**.<br /><br /> При обновлении базы данных **msdb** этому параметру присваивается значение null. Это означает резервное копирование без сжатия.|  
|**key_algorithm**|**nvarchar(32)**|Алгоритм шифрования резервной копии. Значение NO_Encryption указывает, что резервная копия не была зашифрована.|  
|**encryptor_thumbprint**|**varbinary(20)**|Отпечаток шифратора, который будет использоваться для поиска сертификата или асимметричного ключа в базе данных. Если резервная копия не была зашифрована, это значение равно NULL.|  
|**encryptor_type**|**nvarchar(32)**|Тип используемого шифратора: сертификат или асимметричный ключ. . Если резервная копия не была зашифрована, это значение равно NULL.|  
  
## <a name="remarks"></a>Комментарии
- Инструкция RESTORE VERIFYONLY из *backup_device* with LOADHISTORY заполняет столбец таблицы **backupmediaset** соответствующими значениями из заголовка набора носителей.  
- Чтобы уменьшить количество строк в этой таблице и в других таблицах резервного копирования и журнала, выполните хранимую процедуру [sp_delete_backuphistory](../../relational-databases/system-stored-procedures/sp-delete-backuphistory-transact-sql.md) .  
- Для SQL Управляемый экземпляр, в таблице резервных копий отображается только журнал резервного копирования для инициированных пользователем [резервных копий только для копирования](../../relational-databases/backup-restore/copy-only-backups-sql-server.md). Таблица резервных копий не отображает журнал резервного копирования для автоматических резервных копий, выполняемых службой. 
  
## <a name="see-also"></a>См. также:  
 [Резервное копирование и восстановление таблиц &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)   
 [backupfile (Transact-SQL)](../../relational-databases/system-tables/backupfile-transact-sql.md)   
 [backupfilegroup (Transact-SQL)](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)   
 [backupmediafamily (Transact-SQL)](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)   
 [backupmediaset (Transact-SQL)](../../relational-databases/system-tables/backupmediaset-transact-sql.md)   
 [Возможные ошибки носителей во время резервного копирования и восстановления (SQL Server)](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md)   
 [Наборы носителей, семейства носителей и резервные наборы данных (SQL Server)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   
 [Модели восстановления (SQL Server)](../../relational-databases/backup-restore/recovery-models-sql-server.md)   
 [RESTORE HEADERONLY (Transact-SQL)](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)   
 [Резервное копирование и восстановление таблиц &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)  
  
  
