---
description: sys.query_store_wait_stats (Transact-SQL)
title: sys.query_store_wait_stats (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 11/19/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.query_store_wait_stats
- query_store_wait_stats
dev_langs:
- TSQL
helpviewer_keywords:
- query_store_wait_stats catalog view
- sys.query_store_wait_stats catalog view
ms.assetid: ccf7a57c-314b-450c-bd34-70749a02784a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d3df8f87d73ac8ca25fe2e7e83d7bbc3d0da01f2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097973"
---
# <a name="sysquery_store_wait_stats-transact-sql"></a>sys.query_store_wait_stats (Transact-SQL)

[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]

  Содержит сведения о времени ожидания для запроса.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**wait_stats_id**|**bigint**|Идентификатор строки, представляющей статистику ожидания для plan_id, runtime_stats_interval_id, execution_type и wait_category. Он уникален только за прошлые интервалы статистики времени выполнения. Для текущего активного интервала может существовать несколько строк, представляющих статистику ожидания для плана, на который ссылается plan_id, с типом выполнения, представленным execution_type, и категорией ожидания, представленной wait_category. Как правило, одна строка представляет статистику ожидания, записанную на диск, а другие (s) — состояние в памяти. Таким образом, чтобы получить фактическое состояние для каждого интервала, необходимо объединить метрики, группировать по plan_id, runtime_stats_interval_id, execution_type и wait_category. |  
|**plan_id**|**bigint**|Внешний ключ. Присоединение к [sys.query_store_plan &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md).|  
|**runtime_stats_interval_id**|**bigint**|Внешний ключ. Присоединение к [sys.query_store_runtime_stats_interval &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md).|  
|**wait_category**|**tinyint**|Типы ожидания распределяются по категориям в таблице ниже, а затем время ожидания вычисляется по этим категориям ожидания. Для различных категорий ожидания требуется другой анализ для устранения проблемы, но типы ожидания из одной категории ведут к аналогичным действиям по устранению неполадок, а предоставление соответствующего запроса в дополнение к ожиданиям — отсутствие элемента, чтобы успешно завершить большую часть такого исследования.|
|**wait_category_desc**|**nvarchar(128)**|Текстовое описание поля "Категория ожидания" см. в таблице ниже.|
|**execution_type**|**tinyint**|Определяет тип выполнения запроса:<br /><br /> 0 — регулярное выполнение (успешно завершено)<br /><br /> 3. инициированное клиентом прерывание выполнения<br /><br /> 4 — выполнение прерванного исключения|  
|**execution_type_desc**|**nvarchar(128)**|Текстовое описание поля "тип выполнения":<br /><br /> 0 — обычный<br /><br /> 3 — прервано<br /><br /> 4 — исключение|  
|**total_query_wait_time_ms**|**bigint**|Общее `CPU wait` время для плана запроса в пределах интервала агрегирования и категории ожидания (отображается в миллисекундах).|
|**avg_query_wait_time_ms**|**float**|Средняя длительность ожидания для плана запроса на выполнение в пределах интервала агрегирования и категории ожидания (отображается в миллисекундах).|
|**last_query_wait_time_ms**|**bigint**|Длительность последнего ожидания для плана запроса в пределах интервала агрегирования и категории ожидания (отображается в миллисекундах).|
|**min_query_wait_time_ms**|**bigint**|Минимальное `CPU wait` время для плана запроса в пределах интервала агрегирования и категории ожидания (отображается в миллисекундах).|
|**max_query_wait_time_ms**|**bigint**|Максимальное `CPU wait` время для плана запроса в пределах интервала агрегирования и категории ожидания (отображается в миллисекундах).|
|**stdev_query_wait_time_ms**|**float**|`Query wait` стандартное отклонение длительности для плана запроса в пределах интервала агрегирования и категории ожидания (отображается в миллисекундах).|

## <a name="wait-categories-mapping-table"></a>Таблица сопоставления категорий ожидания

"%" используется в качестве подстановочного знака
  
|Целое значение|Категория ожидания|Типы ожидания включают в категорию|  
|-----------------|---------------|-----------------|  
|**0**|**Unknown**|Неизвестно |  
|**1**|**ЦП**|SOS_SCHEDULER_YIELD|
|**2**|**Рабочий поток**|THREADPOOL|
|**3**|**Блокировка**|LCK_M_%|
|**4**|**Кратковременная блокировка**|LATCH_%|
|**5**|**Кратковременная блокировка буфера**|PAGELATCH_%|
|**6**|**Операции ввода-вывода буфера**|PAGEIOLATCH_%|
|**7**|**Компиляция** _|RESOURCE_SEMAPHORE_QUERY_COMPILE|
|_ *8**|**SQL CLR**|CLR%, SQLCLR%|
|**9**|**Зеркальное отображение**|ДБМИРРОР%|
|**10**|**Транзакция**|ТРАНЗАКЦИИ%, DTC%, TRAN_MARKLATCH_%, MSQL_XACT_%, TRANSACTION_MUTEX|
|**11**|**Выключен**|SLEEP_%, LAZYWRITER_SLEEP, SQLTRACE_BUFFER_FLUSH, SQLTRACE_INCREMENTAL_FLUSH_SLEEP, SQLTRACE_WAIT_ENTRIES, FT_IFTS_SCHEDULER_IDLE_WAIT, XE_DISPATCHER_WAIT, REQUEST_FOR_DEADLOCK_SEARCH, LOGMGR_QUEUE, ONDEMAND_TASK_QUEUE, CHECKPOINT_QUEUE, XE_TIMER_EVENT|
|**12**|**PreEmptive**|PREEMPTIVE_%|
|**13**|**Service Broker**|BROKER_% **(но не BROKER_RECEIVE_WAITFOR)**|
|**14**|**Ввод-вывод журнала транзакций**|LOGMGR, ЛОГБУФФЕР, LOGMGR_RESERVE_APPEND, LOGMGR_FLUSH, LOGMGR_PMM_LOG, CHKPT, WRITELOG|
|**15**|**Сетевые операции ввода-вывода**|ASYNC_NETWORK_IO, NET_WAITFOR_PACKET, PROXY_NETWORK_IO, EXTERNAL_SCRIPT_NETWORK_IOF|
|**16**|**Parallelism**|CXPACKET, EXCHANGE, HT%, BMP%, BP%|
|**17**|**Память**|RESOURCE_SEMAPHORE, КМЕМСРЕАД, КМЕМПАРТИТИОНЕД, EE_PMOLOCK, MEMORY_ALLOCATION_EXT, RESERVED_MEMORY_ALLOCATION_EXT, MEMORY_GRANT_UPDATE|
|**стр**|**Ожидание пользователя**|WAITFOR, WAIT_FOR_RESULTS, BROKER_RECEIVE_WAITFOR|
|**стр**|**Трассировка**|ТРАЦЕВРИТЕ, SQLTRACE_LOCK, SQLTRACE_FILE_BUFFER, SQLTRACE_FILE_WRITE_IO_COMPLETION, SQLTRACE_FILE_READ_IO_COMPLETION, SQLTRACE_PENDING_BUFFER_WRITERS, SQLTRACE_SHUTDOWN, QUERY_TRACEOUT, TRACE_EVTNOTIFF|
|**20**|**Полнотекстовый поиск**|FT_RESTART_CRAWL, ПОЛНОТЕКСТОВЫЙ СБОР, MSSEARCH, FT_METADATA_MUTEX, FT_IFTSHC_MUTEX, FT_IFTSISM_MUTEX, FT_IFTS_RWLOCK, FT_COMPROWSET_RWLOCK, FT_MASTER_MERGE, FT_PROPERTYLIST_CACHE, FT_MASTER_MERGE_COORDINATOR, PWAIT_RESOURCE_SEMAPHORE_FT_PARALLEL_QUERY_SYNC|
|**открыт**|**Другие операции ввода-вывода диска**|ASYNC_IO_COMPLETION, IO_COMPLETION, БАККУПИО, WRITE_COMPLETION, IO_QUEUE_LIMIT, IO_RETRY|
|**22**|**Репликация**|SE_REPL_%, REPL_%, HADR_% **(но не HADR_THROTTLE_LOG_RATE_GOVERNOR)**, PWAIT_HADR_%, REPLICA_WRITES, FCB_REPLICA_WRITE, FCB_REPLICA_READ, PWAIT_HADRSIM|
|**23**|**Регулятор частоты ведения журнала**|LOG_RATE_GOVERNOR, POOL_LOG_RATE_GOVERNOR, HADR_THROTTLE_LOG_RATE_GOVERNOR, INSTANCE_LOG_RATE_GOVERNOR|

Категория ожидания **компиляции** в настоящее время не поддерживается.

## <a name="permissions"></a>Разрешения

 Требуется разрешение `VIEW DATABASE STATE`.  
  
## <a name="see-also"></a>См. также:

- [sys.database_query_store_options (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)
- [sys.query_context_settings (Transact-SQL)](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)
- [sys.query_store_plan (Transact-SQL)](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)
- [sys.query_store_query (Transact-SQL)](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)
- [sys.query_store_query_text (Transact-SQL)](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)
- [sys.query_store_runtime_stats_interval (Transact-SQL)](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)
- [Мониторинг производительности с использованием хранилища запросов](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)
- [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)
- [Хранимые процедуры хранилища запросов &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)  
