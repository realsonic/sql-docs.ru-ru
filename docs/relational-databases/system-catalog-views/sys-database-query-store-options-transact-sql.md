---
title: sys.database_query_store_options (Transact-SQL)
description: sys.database_query_store_options (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- DATABASE_QUERY_STORE_OPTIONS_TSQL
- DATABASE_QUERY_STORE_OPTIONS
- SYS.DATABASE_QUERY_STORE_OPTIONS_TSQL
- SYS.DATABASE_QUERY_STORE_OPTIONS
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_query_store_options catalog view
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.custom: ''
ms.date: 1/8/2021
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7735f03de4231cc46871a3470535a53ca9d3f942
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171926"
---
# <a name="sysdatabase_query_store_options-transact-sql"></a>sys.database_query_store_options (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Возвращает параметры хранилища запросов для этой базы данных.  
  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] и выше), [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**desired_state**|**smallint**|Указывает режим требуемой операции хранилища запросов, явно заданный пользователем.<br /> 0 = выключен. <br /> 1 = READ_ONLY<br /> 2 = READ_WRITE|  
|**desired_state_desc**|**nvarchar(60)**|Текстовое описание требуемого режима работы хранилища запросов:<br />OFF<br />READ_ONLY<br />READ_WRITE|  
|**actual_state**|**smallint**|Указывает режим работы хранилища запросов. В дополнение к списку требуемых состояний, требуемых для пользователя, фактическое состояние может быть состоянием ошибки.<br /> 0 = выключен. <br /> 1 = READ_ONLY<br /> 2 = READ_WRITE<br /> 3 = ОШИБКА|  
|**actual_state_desc**|**nvarchar(60)**|Текстовое описание реального режима работы хранилища запросов.<br />OFF<br />READ_ONLY<br />READ_WRITE<br />ошибка<br /><br /> Существуют ситуации, когда фактическое состояние отличается от желаемого состояния:<br />— Если база данных имеет режим "только для чтения" или размер хранилища запросов превышает настроенную квоту, хранилище запросов может действовать в режиме только для чтения, даже если пользователь указал параметр "чтение и запись".<br />— В экстремальных сценариях хранилище запросов может вводить состояние ошибки из-за внутренних ошибок. Начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] , если это происходит, хранилище запросов можно восстановить, выполнив `sp_query_store_consistency_check` хранимую процедуру в затронутой базе данных. Если работа `sp_query_store_consistency_check` не работает или используется [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] , необходимо очистить данные, выполнив `ALTER DATABASE [YourDatabaseName] SET QUERY_STORE CLEAR ALL;`|  
|**readonly_reason**|**int**|Если **desired_state_desc** READ_WRITE и **actual_state_desc** READ_ONLY, **readonly_reason** возвращает битовую карту, чтобы указать, почему хранилище запросов находится в режиме только для чтения.<br /><br /> **1** — база данных находится в режиме только для чтения<br /><br /> **2** — база данных находится в однопользовательском режиме<br /><br /> **4** . база данных находится в аварийном режиме<br /><br /> **8** — база данных является вторичной репликой (применяется для Always on и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] георепликации). Это значение можно эффективно наблюдать только на вторичных репликах, доступных **для чтения**<br /><br /> **65536** -хранилище запросов достигло предельного размера, установленного `MAX_STORAGE_SIZE_MB` параметром. Дополнительные сведения об этом параметре см. в разделе [Параметры ALTER DATABASE SET (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md).<br /><br /> **131072** -количество различных инструкций в хранилище запросов достигло предела внутренней памяти. Рассмотрите возможность удаления ненужных запросов или обновления до более высокого уровня служб, чтобы обеспечить передачу хранилища запросов в режим чтения и записи.<br /><br /><br /> **262144** -размер элементов в памяти, ожидающих сохранения на диске, достигло предела внутренней памяти. Хранилище запросов будет временно работать в режиме только для чтения, пока элементы в памяти не будут сохранены на диске. <br /><br /><br /> **524288** -база данных достигла предельного размера диска. Хранилище запросов является частью пользовательской базы данных, поэтому, если для базы данных больше нет свободного места, это означает, что хранилище запросов больше не может расти.<br /><br /> <br /> Сведения о том, как переключить режим операций хранилища запросов на чтение и запись, см. в разделе **Проверка хранилища запросов на сбор данных о запросах** [с использованием хранилища запросов](../../relational-databases/performance/best-practice-with-the-query-store.md#Verify).|  
|**current_storage_size_mb**|**bigint**|Размер хранилища запросов на диске в мегабайтах.|  
|**flush_interval_seconds**|**bigint**|Период регулярного сброса данных хранилища запросов на диск за считанные секунды. Значение по умолчанию — **900** (15 минут).<br /><br /> Измените с помощью `ALTER DATABASE <database> SET QUERY_STORE (DATA_FLUSH_INTERVAL_SECONDS  = <interval>)` инструкции.|  
|**interval_length_minutes**|**bigint**|Интервал агрегирования статистики в минутах. Произвольные значения не допускаются. Используйте один из следующих элементов: 1, 5, 10, 15, 30, 60 и 1440 минут. Значение по умолчанию — **60** минут.|  
|**max_storage_size_mb**|**bigint**|Максимальный размер диска для хранилища запросов в мегабайтах (МБ). Значение по умолчанию — **100** МБ до [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] и **1 ГБ** , начиная с [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] .<br />Для [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] выпуска Premium значение по умолчанию — 1 ГБ, а для [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] выпуска Basic — 10 МБ.<br /><br /> Измените с помощью `ALTER DATABASE <database> SET QUERY_STORE (MAX_STORAGE_SIZE_MB = <size>)` инструкции.|  
|**stale_query_threshold_days**|**bigint**|Число дней, в течение которых данные запроса хранятся в хранилище запросов. Значение по умолчанию — **30**. Задайте значение 0, чтобы отключить политику хранения.<br />Для выпуска [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] Basic Edition значение по умолчанию — семь дней.<br /><br /> Измените с помощью `ALTER DATABASE <database> SET QUERY_STORE ( CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = <value> ) )` инструкции.|  
|**max_plans_per_query**|**bigint**|Ограничивает максимальное количество хранимых планов. Значение по умолчанию — **200**. Если достигнуто максимальное значение, хранилище запросов прекращает запись новых планов для этого запроса. Значение 0 устраняет ограничение в отношении количества захваченных планов.<br /><br /> Измените с помощью `ALTER DATABASE<database> SET QUERY_STORE (MAX_PLANS_PER_QUERY = <n>)` инструкции.|  
|**query_capture_mode**|**smallint**|Текущий активный режим записи запросов:<br /><br /> **1** = все — записываются все запросы. Это значение конфигурации по умолчанию для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ( [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] и более поздних версий).<br /><br /> 2 = Автоматический сбор соответствующих запросов на основе числа выполнений и потребления ресурсов. Это значение конфигурации по умолчанию для [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].<br /><br /> 3 = NONE — отключить запись новых запросов. Хранилище запросов будет продолжать сбор статистики компиляции и времени выполнения для запросов, которые уже были записаны. Используйте эту конфигурацию с осторожностью, так как вы можете пропустить важные запросы. <br /><br /> 4 = CUSTOM — позволяет дополнительный контроль над политикой отслеживания запросов с помощью [параметров QUERY_CAPTURE_POLICY](../../t-sql/statements/alter-database-transact-sql-set-options.md#SettingOptions).<br /> **Область применения**: [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] и более поздних версий.|  
|**query_capture_mode_desc**|**nvarchar(60)**|Текстовое описание фактического режима записи хранилища запросов:<br /><br /> ALL (по умолчанию для [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] )<br /><br /> **Авто** (по умолчанию для [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] )<br /><br /> None <br /><br /> CUSTOM|  
|**capture_policy_execution_count**|**int**|Параметр НАСТРАИВАЕМой политики режима записи запросов. Определяет количество выполнений запроса в течение ознакомительного периода. Значение по умолчанию равно 30.<br />**Область применения**: [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] и более поздних версий.| 
|**capture_policy_total_compile_cpu_time_ms**|**bigint**|Параметр НАСТРАИВАЕМой политики режима записи запросов. Определяет общее время ЦП, затраченное на компиляцию, которое запрос использовал за ознакомительный период. Значение по умолчанию — 1000.<br /> **Область применения**: [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] и более поздних версий.|
|**capture_policy_total_execution_cpu_time_ms**|**bigint**|Параметр НАСТРАИВАЕМой политики режима записи запросов. Определяет общее время ЦП, затраченное на выполнение, которое запрос использовал за ознакомительный период. Значение по умолчанию — 100.<br /> **Область применения**: [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] и более поздних версий.|
|**capture_policy_stale_threshold_hours**|**int**|Параметр НАСТРАИВАЕМой политики режима записи запросов. Определяет период интервала ознакомления для определения того, нужно ли записать запрос. Значение по умолчанию — 24 часа.<br /> **Область применения**: [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] и более поздних версий.|
|**size_based_cleanup_mode**|**smallint**|Определяет, будет ли автоматически активирована очистка, когда общий объем данных приблизится к верхней границе ограничения.<br /><br /> 0 = очистка на основе размера не будет автоматически активирована.<br /><br /> **1** = Очистка на основе авторазмера будет автоматически активирована, если размер на диске достигнет **90%** *max_storage_size_mb*. Это значение конфигурации по умолчанию.<br /><br />Эта очистка сначала удаляет самые дешевые и самые старые запросы. Она останавливается при достижении примерно **80%** *max_storage_size_mb* .|  
|**size_based_cleanup_mode_desc**|**nvarchar(60)**|Текстовое описание фактического режима очистки на основе размера хранилища запросов:<br /><br /> OFF <br /> **Авто** (по умолчанию)|  
|**wait_stats_capture_mode**|**smallint**|Определяет, выполняет ли хранилище запросов сбор статистики ожидания: <br /><br /> 0 = выключен. <br /> **1** = включено<br /> **Область применения**: [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] и более поздних версий.|
|**wait_stats_capture_mode_desc**|**nvarchar(60)**|Текстовое описание фактического режима записи статистики ожидания: <br /><br /> OFF <br /> **Вкл** . (по умолчанию)<br /> **Область применения**: [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] и более поздних версий.|
|**actual_state_additional_info**|**nvarchar (8000)**|В настоящее время не используется. Может быть реализован в будущем.|
  
## <a name="permissions"></a>Разрешения  
 Требуется разрешение `VIEW DATABASE STATE`.  
  
## <a name="see-also"></a>См. также:  
 [sys.query_context_settings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)   
 [sys.query_store_plan &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)   
 [sys.query_store_query &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)   
 [sys.query_store_query_text &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)   
 [sys.query_store_runtime_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql.md)   
 [sys.query_store_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)  
 [sys.query_store_runtime_stats_interval &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)   
 [Мониторинг производительности с использованием хранилища запросов](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sys.fn_stmt_sql_handle_from_sql_stmt &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-stmt-sql-handle-from-sql-stmt-transact-sql.md)   
 [Хранимые процедуры хранилища запросов &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)  
