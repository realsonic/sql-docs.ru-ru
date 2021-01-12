---
description: sys.databases (Transact-SQL)
title: sys. databases (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/08/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- databases
- databases_TSQL
- sys.databases
- sys.databases_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.databases catalog view
ms.assetid: 46c288c1-3410-4d68-a027-3bbf33239289
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5794b2720340fac295720ac2a4ab05a7906a9226
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093174"
---
# <a name="sysdatabases-transact-sql"></a>sys.databases (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Содержит одну строку для каждой базы данных в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
Если база данных не является `ONLINE` или `AUTO_CLOSE` имеет значение, `ON` а база данных закрыта, то значения некоторых столбцов могут быть `NULL` . Если база данных имеет значение `OFFLINE` , соответствующая строка не видна пользователям с низким уровнем привилегий. Чтобы просмотреть соответствующую строку, если база данных имеет значение `OFFLINE` , пользователь должен иметь по крайней мере `ALTER ANY DATABASE` разрешение уровня сервера или `CREATE DATABASE` разрешение в `master` базе данных.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Имя базы данных, уникальное внутри экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или на сервере [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|  
|**database_id**|**int**|Идентификатор базы данных, уникальный внутри экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или на сервере [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|  
|**source_database_id**|**int**|Не NULL = идентификатор базы данных-источника данного моментального снимка базы данных.<br /> NULL = моментальный снимок не базы данных.|  
|**owner_sid**|**varbinary(85)**|SID (идентификатор безопасности) внешнего владельца базы данных, зарегистрированного на сервере. Сведения о том, кто может владеть базой данных, см. в разделе **ALTER AUTHORIZATION для баз данных** статьи [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md).|  
|**create_date**|**datetime**|Дата создания или переименования базы данных. Для **базы данных tempdb** это значение изменяется при каждом перезапуске сервера.|  
|**compatibility_level**|**tinyint**|Целое число, соответствующее версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], с которой поведение совместимо:<br /><br /><table border="0"><tr><td>**Значение**</td><td>**Относится к**</td></tr><tr><td>70</td><td>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] от 7,0 до [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]</td></tr><tr><td>80</td><td>[!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] сквозь [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]</td></tr><tr><td>90</td><td>[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] сквозь [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]</td></tr><tr><td>100</td><td>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].</td></tr><tr><td>110</td><td>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].</td></tr><tr><td>120</td><td>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].</td></tr><tr><td>130</td><td>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].</td></tr><tr><td>140</td><td>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].</td></tr><tr><td>150</td><td>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].</td></tr></table>|  
|**collation_name**|**sysname**|Параметры сортировки для базы данных. Действует как параметры сортировки по умолчанию для базы данных.<br /> NULL — база данных не находится в режиме «в сети», либо параметр AUTO_CLOSE установлен в ON, и база данных закрыта.|  
|**user_access**|**tinyint**|Установка доступа пользователя:<br /> 0 = указано MULTI_USER.<br /> 1 = указано SINGLE_USER;<br /> 2 = указан RESTRICTED_USER.|  
|**user_access_desc**|**nvarchar(60)**|Описание задания доступа пользователя.|  
|**is_read_only**|**bit**|1 = база данных находится в режиме READ_ONLY<br /> 0 = база данных находится в режиме READ_WRITE|  
|**is_auto_close_on**|**bit**|1 = параметр AUTO_CLOSE находится в состоянии ON<br /> 0 = параметр AUTO_CLOSE находится в состоянии OFF|  
|**is_auto_shrink_on**|**bit**|1 = параметр AUTO_SHRINK находится в состоянии ON<br /> 0 = параметр AUTO_SHRINK находится в состоянии OFF|  
|**state**|**tinyint**|**Значение**<br /> 0 = ONLINE <br /> 1 = RESTORING <br /> 2 = восстановление <sup>1</sup><br /> 3 = RECOVERY_PENDING <sup>1</sup><br /> 4 = SUSPECT <br /> 5 = ЭКСТРЕННая <sup>1</sup><br /> 6 = АВТОНОМный режим <sup>1</sup><br /> 7 = копирование <sup>2</sup> <br /> 10 = OFFLINE_SECONDARY <sup>2</sup> <br /><br /> **Примечание.** Для Always On баз данных запросите `database_state` `database_state_desc` столбцы или [sys.dm_hadr_database_replica_states](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md).<br /><br /><sup>1</sup> **применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] ) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]<br /><sup>2</sup> **применяется к**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)][!INCLUDE[ssGeoDR](../../includes/ssgeodr-md.md)]|  
|**state_desc**|**nvarchar(60)**|Описание состояния базы данных. См. раздел State.|  
|**is_in_standby**|**bit**|База данных доступна только для чтения для журнала восстановления.|  
|**is_cleanly_shutdown**|**bit**|1 = база данных закрыта верно; восстановление при запуске не требуется<br /> 0 = база данных закрыта неверно; требуется восстановление при запуске|  
|**is_supplemental_logging_enabled**|**bit**|1 = SUPPLEMENTAL_LOGGING в состоянии ON<br /> 0 = SUPPLEMENTAL_LOGGING в состоянии OFF|  
|**snapshot_isolation_state**|**tinyint**|Состояние транзакций изоляции моментальных снимков, задаваемое при помощи параметра ALLOW_SNAPSHOT_ISOLATION.<br /> 0 = изоляция моментальных снимков в состоянии OFF (по умолчанию). Изоляция моментальных снимков запрещена.<br /> 1 = изоляция моментальных снимков в состоянии ON. Изоляция моментальных снимков разрешена.<br /> 2 = изоляция моментальных снимков в состоянии перехода в состояние OFF. Для всех транзакций записываются изменения. Нельзя запустить новые транзакции, использующие изоляцию моментальных снимков. База данных находится в состоянии перехода в состояние OFF до тех пор, пока все транзакции, активные при выполнении инструкции ALTER DATABASE, не будут завершены.<br /> 3 = изоляция моментальных снимков в состоянии перехода в состояние ON. Для новых транзакций записываются изменения. Транзакции не могут использовать изоляцию моментальных снимков до тех пор, пока состояние изоляции моментальных снимков не перейдет в 1 (ON). База данных находится в состоянии перехода в состояние ON до тех пор, пока все транзакции, активные при выполнении инструкции ALTER DATABASE, не будут завершены.|  
|**snapshot_isolation_state_desc**|**nvarchar(60)**|Описание состояния транзакций изоляции моментальных снимков, задаваемое при помощи параметра ALLOW_SNAPSHOT_ISOLATION.|  
|**is_read_committed_snapshot_on**|**bit**|1 = параметр READ_COMMITTED_SNAPSHOT установлен в значение ON. Операции чтения с уровнем изоляции read-committed основаны на просмотре моментальных снимков и не запрашивают блокировок.<br /> 0 = параметр READ_COMMITTED_SNAPSHOT установлен в значение OFF (по умолчанию). Операции чтения с уровнем изоляции read-committed используют разделяемые блокировки.|  
|**recovery_model**|**tinyint**|Выбранная модель восстановления:<br /> 1 = FULL<br /> 2 = BULK_LOGGED<br /> 3 = SIMPLE|  
|**recovery_model_desc**|**nvarchar(60)**|Описание выбранной модели восстановления.|  
|**page_verify_option**|**tinyint**|Значение параметра PAGE_VERIFY:<br /> 0 = нет<br /> 1 = TORN_PAGE_DETECTION<br /> 2 = CHECKSUM|  
|**page_verify_option_desc**|**nvarchar(60)**|Описание значения параметра PAGE_VERIFY.|  
|**is_auto_create_stats_on**|**bit**|1 = AUTO_CREATE_STATISTICS в состоянии ON<br /> 0 = AUTO_CREATE_STATISTICS в состоянии OFF|  
|**is_auto_create_stats_incremental_on**|**bit**|Указывает параметр по умолчанию для добавочной обработки автоматической статистики.<br /> 0 = автоматическое создание статистики не добавочно<br /> 1 = автоматическое создание статистики по возможности добавочно<br /> **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]).|  
|**is_auto_update_stats_on**|**bit**|1 = AUTO_UPDATE_STATISTICS в состоянии ON<br /> 0 = AUTO_UPDATE_STATISTICS в состоянии OFF|  
|**is_auto_update_stats_async_on**|**bit**|1 = AUTO_UPDATE_STATISTICS_ASYNC в состоянии ON<br /> 0 = AUTO_CREATE_STATISTICS_ASYNC в состоянии OFF|  
|**is_ansi_null_default_on**|**bit**|1 = ANSI_NULL_DEFAULT в состоянии ON<br /> 0 = ANSI_NULL_DEFAULT в состоянии OFF|  
|**is_ansi_nulls_on**|**bit**|1 = ANSI_NULLS в состоянии ON<br /> 0 = ANSI_NULLS в состоянии OFF|  
|**is_ansi_padding_on**|**bit**|1 = ANSI_PADDING в состоянии ON<br /> 0 = ANSI_PADDING в состоянии OFF|  
|**is_ansi_warnings_on**|**bit**|1 = ANSI_WARNINGS в состоянии ON<br /> 0 = ANSI_WARNINGS в состоянии OFF|  
|**is_arithabort_on**|**bit**|1 = ARITHABORT в состоянии ON<br /> 0 = ARITHABORT в состоянии OFF|  
|**is_concat_null_yields_null_on**|**bit**|1 = CONCAT_NULL_YIELDS_NULL в состоянии ON<br /> 0 = CONCAT_NULL_YIELDS_NULL в состоянии OFF|  
|**is_numeric_roundabort_on**|**bit**|1 = NUMERIC_ROUNDABORT в состоянии ON<br /> 0 = NUMERIC_ROUNDABORT в состоянии OFF|  
|**is_quoted_identifier_on**|**bit**|1 = QUOTED_IDENTIFIER в состоянии ON<br /> 0 = QUOTED_IDENTIFIER в состоянии OFF|  
|**is_recursive_triggers_on**|**bit**|1 = RECURSIVE_TRIGGERS в состоянии ON<br /> 0 = RECURSIVE_TRIGGERS в состоянии OFF|  
|**is_cursor_close_on_commit_on**|**bit**|1 = CURSOR_CLOSE_ON_COMMIT в состоянии ON<br /> 0 = CURSOR_CLOSE_ON_COMMIT в состоянии OFF|  
|**is_local_cursor_default**|**bit**|1 = CURSOR_DEFAULT соответствует локальному курсору<br /> 0 = CURSOR_DEFAULT соответствует глобальному курсору|  
|**is_fulltext_enabled**|**bit**|1 = полнотекстовый режим включен для данной базы данных<br /> 0 = полнотекстовый режим отключен для данной базы данных|  
|**is_trustworthy_on**|**bit**|1 = база данных помечена как надежная<br /> 0 = база данных не помечена как надежная<br /> По умолчанию для восстановленных или присоединенных баз данных не включена надежность.|  
|**is_db_chaining_on**|**bit**|1 = межбазовые цепочки владения в состоянии ON<br /> 0 = межбазовые цепочки владения в состоянии OFF|  
|**is_parameterization_forced**|**bit**|1 = параметризация в состоянии FORCED<br /> 0 = параметризация в состоянии SIMPLE|  
|**is_master_key_encrypted_by_server**|**bit**|1 = база данных имеет главный ключ шифрования<br /> 0 = база данных не имеет главного ключа шифрования|  
|**is_query_store_on**|**bit**|1 = хранилище запросов включено для этой базы данных. Проверьте [sys.database_query_store_options](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md) , чтобы просмотреть состояние хранилища запросов.<br /> 0 = хранилище запросов не включено<br /> **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]).|  
|**is_published**|**bit**|1 = база данных является базой данных публикации в топологии репликации транзакций или моментальных снимков<br /> 0 = не является базой данных публикации|  
|**is_subscribed**|**bit**|Данный столбец не используется. Он всегда возвращает 0, независимо от состояния подписчика базы данных.|  
|**is_merge_published**|**bit**|1 = база данных является базой данных публикации в топологии репликации слиянием<br /> 0 = база данных не является базой данных публикации в топологии репликации слиянием|  
|**is_distributor**|**bit**|1 = база данных является базой данных распространителя в топологии репликации<br /> 0 = база данных не является базой данных распространителя в топологии репликации|  
|**is_sync_with_backup**|**bit**|1 = база данных помечена для синхронизации с резервной копией при помощи репликации<br /> 0 = база данных не помечена для синхронизации с резервной копией при помощи репликации|  
|**service_broker_guid**|**uniqueidentifier**|Идентификатор компонента Service Broker для данной базы данных. Используется как целевой экземпляр **broker_instance** в таблице маршрутизации.|  
|**is_broker_enabled**|**bit**|1 = брокер в этой базе данных в данный момент отправляет и принимает сообщения.<br /> 0 = все отправленные сообщения останутся в очереди передачи, а полученные сообщения не будут помещены в очередь в этой базе данных.<br /> По умолчанию в восстановленных или прикрепленных базах данных брокер отключен. Исключением является зеркальное отображение базы данных, при котором брокер включается после отработки отказа.|  
|**log_reuse_wait**|**tinyint**|Повторное использование пространства журнала транзакций в настоящее время ожидает одного из следующих элементов в последней контрольной точке. Более подробное объяснение этих значений см. [в журнале транзакций](../../relational-databases/logs/the-transaction-log-sql-server.md).<br /> **Значение**<br /> 0 = ничего<br /> 1 = контрольная точка (если база данных использует модель восстановления и имеет оптимизированную для памяти файловую группу данных, то должен отобразиться `log_reuse_wait` столбец, указывающий на значение `checkpoint` или `xtp_checkpoint` ) <sup>1</sup><br /> 2 = резервная копия журнала <sup>1</sup><br /> 3 = активное резервное копирование или восстановление <sup>1</sup><br /> 4 = активная транзакция <sup>1</sup><br /> 5 = зеркальное отображение базы данных <sup>1</sup><br /> 6 = репликация <sup>1</sup><br /> 7 = создание моментального снимка базы данных <sup>1</sup><br /> 8 = просмотр журнала <br /> 9 = Always On вторичная реплика групп доступности применяет записи журнала транзакций этой базы данных к соответствующей базе данных-получателю. <sup>2</sup><br /> 9 = другое (временное) <sup>3</sup><br /> 10 = только для внутреннего использования <sup>2</sup><br /> 11 = только для внутреннего использования <sup>2</sup><br /> 12 = только для внутреннего использования <sup>2</sup><br /> 13 = самая старая страница <sup>2</sup><br /> 14 = второй <sup>2</sup><br />  16 = XTP_CHECKPOINT (если база данных использует модель восстановления и имеет оптимизированную для памяти файловую группу данных, то должен отобразиться столбец, `log_reuse_wait` указывающий `checkpoint` или `xtp_checkpoint` ) <sup>4</sup><br /><br /><sup>1</sup> **применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] )<br /><sup>2</sup> **применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] )<br /><sup>3</sup> **применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (до и включительно [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] )<br /><sup>4</sup> **применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] )|  
|**log_reuse_wait_desc**|**nvarchar(60)**|Описание повторного использования места в журнале транзакций, ожидаемого в настоящее время по состоянию на последнюю контрольную точку.|  
|**is_date_correlation_on**|**bit**|1 = DATE_CORRELATION_OPTIMIZATION в состоянии ON<br /> 0 = DATE_CORRELATION_OPTIMIZATION в состоянии OFF|  
|**is_cdc_enabled**|**bit**|1 = в базе данных включена система отслеживания измененных данных. Дополнительные сведения см. в разделе [sys.sp_cdc_enable_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-db-transact-sql.md).|  
|**is_encrypted**|**bit**|Указывает, зашифрована ли база данных (отражает состояние, Последнее заданное с помощью `ALTER DATABASE SET ENCRYPTION` предложения). Может иметь одно из следующих значений:<br /> 1 = зашифрована<br /> 0 = не зашифрована.<br /> Дополнительные сведения о шифровании баз данных см. в статье [Прозрачное шифрование данных (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md).<br /> Если база данных находится в процессе расшифровки, `is_encrypted` показывает значение 0. Состояние процесса шифрования можно просмотреть с помощью [sys.dm_database_encryption_keys](../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md) динамического административного представления.|  
|**is_honor_broker_priority_on**|**bit**|Указывает, учитывает ли база данных приоритеты диалога (отражает состояние Last, установленное с помощью `ALTER DATABASE SET HONOR_BROKER_PRIORITY` предложения). Может иметь одно из следующих значений:<br /> 1 = HONOR_BROKER_PRIORITY имеет значение ON;<br /> 0 = HONOR_BROKER_PRIORITY имеет значение OFF.<br /> По умолчанию восстановленные или присоединенные базы данных имеют приоритет компонента Service Broker.|  
|**replica_id**|**uniqueidentifier**|Уникальный идентификатор локальной реплики доступности [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] группы доступности, если таковая имеется, частью которой является база данных.<br /> NULL = база данных не является частью реплики доступности в группе доступности.<br /> **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**group_database_id**|**uniqueidentifier**|Уникальный идентификатор базы данных в Always On группе доступности (при наличии), в которой участвует база данных. **group_database_id** одинаковы для этой базы данных в первичной реплике и на каждой вторичной реплике, в которой база данных была присоединена к группе доступности.<br /> NULL = база данных не является частью реплики доступности в любой группе доступности.<br /> **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**resource_pool_id**|**int**|Идентификатор пула ресурсов, сопоставленного с этой базой данных. Этот пул ресурсов управляет общим объемом памяти, доступным оптимизированным для памяти таблицам из этой базы данных.<br /> **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] )|  
|**default_language_lcid**|**smallint**|Указывает идентификатор локали (lcid) языка по умолчанию автономной базы данных.<br /> **Примечание.** Функции в качестве [параметра конфигурации сервера «Настройка языка по умолчанию](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md) » для `sp_configure` . Это значение равно **null** для неавтономной базы данных.<br /> **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**default_language_name**|**nvarchar(128)**|Указывает язык по умолчанию автономной базы данных.<br /> Это значение равно **null** для неавтономной базы данных.<br /> **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**default_fulltext_language_lcid**|**int**|Указывает код локали языка полнотекстового поиска по умолчанию для автономной базы данных.<br /> **Примечание.** Функции по умолчанию [настроить параметр конфигурации сервера Full-Text Language по умолчанию](../../database-engine/configure-windows/configure-the-default-full-text-language-server-configuration-option.md) для `sp_configure` . Это значение равно **null** для неавтономной базы данных.<br /> **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**default_fulltext_language_name**|**nvarchar(128)**|Указывает язык полнотекстового поиска по умолчанию автономной базы данных.<br /> Это значение равно **null** для неавтономной базы данных.<br /> **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**is_nested_triggers_on**|**bit**|Указывает, разрешены ли вложенные триггеры в автономной базе данных.<br /> 0 = вложенные триггеры не разрешены<br /> 1 = вложенные триггеры разрешены<br /> **Примечание.** Функции, как [Настройка параметра конфигурации сервера nested triggers](../../database-engine/configure-windows/configure-the-nested-triggers-server-configuration-option.md) для `sp_configure` . Это значение равно **null** для неавтономной базы данных. Дополнительные сведения см. в разделе [sys.configуратионс &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) .<br /> **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**is_transform_noise_words_on**|**bit**|Указывает, должны ли преобразовываться пропускаемые слова в автономной базе данных.<br /> 0 = пропускаемые слова не должны преобразовываться.<br /> 1 = пропускаемые слова должны преобразовываться.<br /> **Примечание.** Функции в качестве [параметра конфигурации сервера transform noise words](../../database-engine/configure-windows/transform-noise-words-server-configuration-option.md) для `sp_configure` . Это значение равно **null** для неавтономной базы данных. Дополнительные сведения см. в разделе [sys.configуратионс &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) .<br /> **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] )|  
|**two_digit_year_cutoff**|**smallint**|Указывает числовое значение в диапазоне от 1753 до 9999, представляющее пороговый год для интерпретации года, обозначенного двумя цифрами, в виде года, обозначенного четырьмя цифрами.<br /> **Примечание.** Функции в качестве [параметра конфигурации сервера "Настройка двух цифр года отсечки](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md) " `sp_configure` . Это значение равно **null** для неавтономной базы данных. Дополнительные сведения см. в разделе [sys.configуратионс &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) .<br /> **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**containment**|**TINYINT NOT NULL**|Указывает состояние включения базы данных.<br />  0 = автономная работа базы данных отключена. **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]<br /> 1 = база данных находится в частичных вложениях: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] )|  
|**containment_desc**|**nvarchar (60) NOT NULL**|Указывает состояние включения базы данных.<br /> NONE = прежняя версия базы данных (нулевое включение)<br /> PARTIAL = частично автономная база данных<br /> **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**target_recovery_time_in_seconds**|**int**|Предполагаемое время восстановления базы данных в секундах. Допускает значение NULL.<br /> **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**delayed_durability**|**int**|Параметр отложенной устойчивости:<br /> 0 = ОТКЛЮЧЕНО<br /> 1 = РАЗРЕШЕНО<br /> 2 = ПРИНУДИТЕЛЬНО<br /> Дополнительные сведения см. в разделе [Управление устойчивостью транзакций](../../relational-databases/logs/control-transaction-durability.md).<br /> **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|  
|**delayed_durability_desc**|**nvarchar(60)**|Параметр отложенной устойчивости:<br /> DISABLED<br /> ALLOWED<br /> FORCED<br /> **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|  
|**is_memory_optimized_elevate_to_snapshot_on**|**bit**|К таблицам с оптимизацией для памяти доступ производится с использованием изоляции SNAPSHOT, когда в TRANSACTION ISOLATION LEVEL установлен более низкий уровень изоляции — READ COMMITTED или READ UNCOMMITTED.<br /> 1 = минимальный уровень изоляции — SNAPSHOT.<br /> 0 = уровень изоляции не повышается.|  
|**is_federation_member**|**bit**|Указывает, является ли база данных членом федерации.<br /> **Область применения**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**is_remote_data_archive_enabled**|**bit**|Указывает, растягивается ли база данных.<br /> 0 = база данных не поддерживает Stretch.<br /> 1 = база данных поддерживает Stretch.<br /> **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] )<br /> Дополнительные сведения см. в разделе [Stretch Database](../../sql-server/stretch-database/stretch-database.md).|  
|**is_mixed_page_allocation_on**|**bit**|Указывает, могут ли таблицы и индексы в базе данных выделять начальные страницы из смешанных экстентов.<br /> 0 = таблицы и индексы в базе данных всегда распределяют начальные страницы из однородных экстентов.<br /> 1 = таблицы и индексы в базе данных могут распределять начальные страницы из смешанных экстентов.<br /> Дополнительные сведения см. в разделе `SET MIXED_PAGE_ALLOCATION` параметр [инструкции ALTER database SET &#40;&#41;TRANSACT-SQL ](../../t-sql/statements/alter-database-transact-sql-set-options.md).<br /> **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] )|  
|**is_temporal_history_retention_enabled**|**bit**|Указывает, включена ли задача очистки политики временного хранения.<br /><br />1 = временное хранение включено<br />0 = временное хранение отключено<br />**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|
|**catalog_collation_type**|**int**|Параметр сортировки каталога:<br />0 = DATABASE_DEFAULT<br />2 = SQL_Latin_1_General_CP1_CI_AS<br /> **Область применения**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|
|**catalog_collation_type_desc**|**nvarchar(60)**|Параметр сортировки каталога:<br />DATABASE_DEFAULT<br />SQL_Latin_1_General_CP1_CI_AS<br /> **Область применения**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|
|**physical_database_name**|**nvarchar(128)**|Для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] — физическое имя базы данных. Для [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] — Общий идентификатор для баз данных на сервере. <br />**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|
|**is_result_set_caching_on**|**bit**|Указывает, включено ли кэширование результирующего набора.<br />1 = кэширование результирующего набора включено<br />0 = кэширование результирующего набора отключено<br />**Применимо к**: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] Gen2. Пока эта функция будет извлечена во все регионы, проверьте версию, развернутую в экземпляре, и последние [заметки о выпуске Azure синапсе](/azure/synapse-analytics/sql-data-warehouse/release-notes-10-0-10106-0) и [расписание обновления Gen2](/azure/synapse-analytics/sql-data-warehouse/gen2-migration-schedule) для доступности функций.|
|**is_accelerated_database_recovery_on**|**bit**|Указывает, включено ли быстрое восстановление базы данных (ADR).<br />1 = ADR включен<br />0 = ADR отключен<br />**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|
|**is_tempdb_spill_to_remote_store**|**bit**|Указывает, включен ли сброс базы данных tempdb в удаленное хранилище.<br />1 = включен<br />0 = отключен<br />**Применимо к**: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] Gen2. Пока эта функция будет извлечена во все регионы, проверьте версию, развернутую в экземпляре, и последние [заметки о выпуске Azure синапсе](/azure/synapse-analytics/sql-data-warehouse/release-notes-10-0-10106-0) и [расписание обновления Gen2](/azure/synapse-analytics/sql-data-warehouse/gen2-migration-schedule) для доступности функций.|
|**is_stale_page_detection_on**|**bit**|Указывает, включено ли обнаружение устаревшей страницы.<br />1 = Обнаружение устаревших страниц включено<br />0 = обнаружение устаревших страниц отключено<br />**Применимо к**: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] Gen2. Пока эта функция будет извлечена во все регионы, проверьте версию, развернутую в экземпляре, и последние [заметки о выпуске Azure синапсе](/azure/synapse-analytics/sql-data-warehouse/release-notes-10-0-10106-0) и [расписание обновления Gen2](/azure/synapse-analytics/sql-data-warehouse/gen2-migration-schedule) для доступности функций.|
|**is_memory_optimized_enabled**|**bit**|Указывает, включены ли для базы данных определенные функции In-Memory, например [гибридный буферный пул](../../database-engine/configure-windows/hybrid-buffer-pool.md). Не отражает состояние доступности или конфигурации выполняющейся [в памяти OLTP](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md). <br />1 = функции, оптимизированные для памяти, включены<br />0 = функции, оптимизированные для памяти, отключены<br />**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|
  
## <a name="permissions"></a>Разрешения

 Если вызывающий объект не `sys.databases` является владельцем базы данных, а база данных не `master` или `tempdb` , то минимальные разрешения, необходимые для просмотра соответствующей строки, `ALTER ANY DATABASE` либо `VIEW ANY DATABASE` разрешение уровня сервера, либо `CREATE DATABASE` разрешение в `master` базе данных. База данных, к которой подключен вызывающий объект, всегда может быть просмотрена в `sys.databases` .  
  
> [!IMPORTANT]  
> По умолчанию роль public имеет `VIEW ANY DATABASE` разрешение, что позволяет всем именам входа просматривать сведения о базе данных. Чтобы заблокировать имя входа от возможности обнаружения базы данных, `REVOKE` `VIEW ANY DATABASE` разрешения `public` или `DENY` `VIEW ANY DATABASE` разрешения для отдельных имен входа.  
  
## <a name="azure-sql-database-remarks"></a>Примечания к базе данных SQL Azure

В [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] этом представлении доступно в `master` базе данных и в пользовательских базах данных. В `master` базе данных это представление возвращает сведения о `master` базе данных и всех пользовательских базах данных на сервере. В пользовательской базе данных это представление возвращает сведения только по текущей базе данных и базе данных master.  
  
 Воспользуйтесь представлением `sys.databases` в базе данных `master` на сервере [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], где создается новая база данных. После запуска копирования базы данных можно запрашивать `sys.databases` и `sys.dm_database_copies` представления из `master` базы данных целевого сервера, чтобы получить дополнительные сведения о ходе копирования.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-query-the-sysdatabases-view"></a>A. Запрос к представлению sys.databases

В следующем примере возвращается несколько столбцов, доступных в `sys.databases` представлении.  
  
```sql  
SELECT name, user_access_desc, is_read_only, state_desc, recovery_model_desc  
FROM sys.databases;  
```  
  
### <a name="b-check-the-copying-status-in-sssds"></a>Б. Проверка состояния копирования в продукте [!INCLUDE[ssSDS](../../includes/sssds-md.md)]

В следующем примере выполняется запрос `sys.databases` `sys.dm_database_copies` к представлениям и, чтобы получить сведения об операции копирования базы данных.  
  
**Область применения**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
```sql
-- Execute from the master database.  
SELECT a.name, a.state_desc, b.start_date, b.modify_date, b.percent_complete  
FROM sys.databases AS a  
INNER JOIN sys.dm_database_copies AS b ON a.database_id = b.database_id  
WHERE a.state = 7;  
```

### <a name="c-check-the-temporal-retention-policy-status-in-sssds"></a>В. Проверьте состояние политики временного хранения в [!INCLUDE[ssSDS](../../includes/sssds-md.md)]

В следующем примере запрос `sys.databases` возвращает сведения о том, включена ли задача очистки временного хранения. Имейте в виду, что после временного хранения операция восстановления по умолчанию отключена. Используйте `ALTER DATABASE` , чтобы включить его явным образом.
  
**Область применения**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
```sql  
-- Execute from the master database.  
SELECT a.name, a.is_temporal_history_retention_enabled 
FROM sys.databases AS a;
```  
  
## <a name="next-steps"></a>Дальнейшие действия

- [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)
- [sys.database_mirroring_witnesses (Transact-SQL)](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)
- [sys.database_recovery_status &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-recovery-status-transact-sql.md)
- [Представления каталогов баз данных и файлов (Transact-SQL)](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)
- [sys.dm_database_copies &#40;база данных SQL Azure&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-database-copies-azure-sql-database.md)  
