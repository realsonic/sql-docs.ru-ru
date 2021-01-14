---
description: sys.dm_exec_requests (Transact-SQL)
title: sys.dm_exec_requests (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 10/01/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_exec_requests_TSQL
- sys.dm_exec_requests
- dm_exec_requests_TSQL
- dm_exec_requests
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_requests dynamic management view
ms.assetid: 4161dc57-f3e7-4492-8972-8cfb77b29643
author: pmasl
ms.author: pelopes
ms.reviewer: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a1ad13d4cdbcf8820a318a87f4ef9f52c488a406
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171266"
---
# <a name="sysdm_exec_requests-transact-sql"></a>sys.dm_exec_requests (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Возвращает сведения о каждом запросе, который выполняется в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Дополнительные сведения о запросах см. в разделе [руководств по архитектуре потоков и задач](../../relational-databases/thread-and-task-architecture-guide.md).
   
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|session_id|**smallint**|Идентификатор сеанса, к которому относится данный запрос. Не допускает значение NULL.|  
|request_id|**int**|Идентификатор запроса. Уникален в контексте сеанса. Не допускает значение NULL.|  
|start_time|**datetime**|Метка времени поступления запроса. Не допускает значение NULL.|  
|status|**nvarchar(30)**|Состояние запроса. Оно может быть одним из следующих.<br /><br /> История<br />Запущен<br />Готово к запуску<br />В режиме ожидания<br />Приостановлена<br /><br /> Не допускает значение NULL.|  
|.|**nvarchar(32)**|Тип выполняемой в данный момент команды. Основные типы команд:<br /><br /> SELECT<br />INSERT<br />UPDATE<br />DELETE<br />BACKUP LOG<br />BACKUP DATABASE<br />DBCC<br />FOR<br /><br /> Текст запроса можно получить при помощи функции sys.dm_exec_sql_text, передав ей значение столбца sql_handle. Внутренние системные процессы устанавливают команду в соответствии с выполняемой задачей. Например:<br /><br /> LOCK MONITOR;<br />CHECKPOINTLAZY;<br />WRITER.<br /><br /> Не допускает значение NULL.|  
|sql_handle|**varbinary (64)**|Токен, однозначно определяющий пакет или хранимую процедуру, частью которой является запрос. Допускает значение NULL.| 
|statement_start_offset|**int**|Указывает, что в байтах, начиная с 0, начальной позицией выполняемой в данный момент инструкции для выполняемого в данный момент пакета или сохраненного объекта. Можно использовать вместе с `sql_handle` `statement_end_offset` функциями, и `sys.dm_exec_sql_text` динамической функцией управления, чтобы получить текущую выполняемую инструкцию для запроса. Допускает значение NULL.|  
|statement_end_offset|**int**|Указывает в байтах, начиная с 0, конечную точку выполняемой в данный момент инструкции для выполняемого в данный момент пакета или сохраненного объекта. Можно использовать вместе с `sql_handle` `statement_start_offset` функциями, и `sys.dm_exec_sql_text` динамической функцией управления, чтобы получить текущую выполняемую инструкцию для запроса. Допускает значение NULL.|  
|plan_handle|**varbinary (64)**|Токен, однозначно определяющий план выполнения запроса для выполняемого в данный момент пакета. Допускает значение NULL.|  
|database_id|**smallint**|Идентификатор базы данных, к которой выполняется запрос. Не допускает значение NULL.|  
|user_id|**int**|Идентификатор пользователя, отправившего данный запрос. Не допускает значение NULL.|  
|connection_id|**uniqueidentifier**|Идентификатор соединения, по которому поступил запрос. Допускает значение NULL.|  
|blocking_session_id|**smallint**|Идентификатор сеанса, блокирующего данный запрос. Если этот столбец имеет значение NULL или равен 0, запрос не блокируется или сведения о сеансе блокирующего сеанса недоступны (или не могут быть идентифицированы).<br /><br /> -2 = Блокирующий ресурс принадлежит потерянной распределенной транзакции.<br /><br /> -3 = Блокирующий ресурс принадлежит отложенной транзакции восстановления.<br /><br /> -4 = Идентификатор сеанса владельца кратковременной блокировки определить на данный момент не удалось из-за переходов между внутренними состояниями кратковременной блокировки.|  
|wait_type|**nvarchar(60)**|Если запрос в настоящий момент блокирован, в столбце содержится тип ожидания. Допускает значение NULL.<br /><br /> Дополнительные сведения о типах ожиданий см. в разделе [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md).|  
|wait_time|**int**|Если запрос в настоящий момент блокирован, в столбце содержится продолжительность текущего ожидания (в миллисекундах). Не допускает значение NULL.|  
|last_wait_type|**nvarchar(60)**|Если запрос был блокирован ранее, в столбце содержится тип последнего ожидания. Не допускает значение NULL.|  
|wait_resource|**nvarchar(256)**|Если запрос в настоящий момент блокирован, в столбце указан ресурс, освобождения которого ожидает запрос. Не допускает значение NULL.|  
|open_transaction_count|**int**|Число транзакций, открытых для данного запроса. Не допускает значение NULL.|  
|open_resultset_count|**int**|Число результирующих наборов, открытых для данного запроса. Не допускает значение NULL.|  
|transaction_id|**bigint**|Идентификатор транзакции, в которой выполняется запрос. Не допускает значение NULL.|  
|context_info|**varbinary(128)**|Значение CONTEXT_INFO сеанса. Допускает значение NULL.|  
|percent_complete|**real**|Процент завершения работы для следующих команд.<br /><br /> ALTER INDEX REORGANIZE<br />параметра AUTO_SHRINK с ALTER DATABASE<br />BACKUP DATABASE<br />DBCC CHECKDB<br />DBCC CHECKFILEGROUP<br />DBCC CHECKTABLE<br />DBCC INDEXDEFRAG<br />DBCC SHRINKDATABASE<br />DBCC SHRINKFILE<br />RECOVERY<br />RESTORE DATABASE<br />ROLLBACK<br />TDE ENCRYPTION<br /><br /> Не допускает значение NULL.|  
|estimated_completion_time|**bigint**|Только для внутреннего использования. Не допускает значение NULL.|  
|cpu_time|**int**|Время ЦП (в миллисекундах), затраченное на выполнение запроса. Не допускает значение NULL.|  
|total_elapsed_time|**int**|Общее время, истекшее с момента поступления запроса (в миллисекундах). Не допускает значение NULL.|  
|scheduler_id|**int**|Идентификатор планировщика, который планирует данный запрос. Не допускает значение NULL.|  
|task_address|**varbinary(8)**|Адрес блока памяти, выделенного для задачи, связанной с этим запросом. Допускает значение NULL.|  
|reads|**bigint**|Число операций чтения, выполненных данным запросом. Не допускает значение NULL.|  
|writes|**bigint**|Число операций записи, выполненных данным запросом. Не допускает значение NULL.|  
|logical_reads|**bigint**|Число логических операций чтения, выполненных данным запросом. Не допускает значение NULL.|  
|text_size|**int**|Установка параметра TEXTSIZE для данного запроса. Не допускает значение NULL.|  
|язык|**nvarchar(128)**|Установка языка для данного запроса. Допускает значение NULL.|  
|date_format|**nvarchar (3)**|Установка параметра DATEFORMAT для данного запроса. Допускает значение NULL.|  
|date_first|**smallint**|Установка параметра DATEFIRST для данного запроса. Не допускает значение NULL.|  
|quoted_identifier|**bit**|1 = Параметр QUOTED_IDENTIFIER для запроса включен (ON). В противном случае — 0.<br /><br /> Не допускает значение NULL.|  
|arithabort|**bit**|1 = Параметр ARITHABORT для запроса включен (ON). В противном случае — 0.<br /><br /> Не допускает значение NULL.|  
|ansi_null_dflt_on|**bit**|1 = Параметр ANSI_NULL_DFLT_ON для запроса включен (ON). В противном случае — 0.<br /><br /> Не допускает значение NULL.|  
|ansi_defaults|**bit**|1 = Параметр ANSI_DEFAULTS для запроса включен (ON). В противном случае — 0.<br /><br /> Не допускает значение NULL.|  
|ansi_warnings|**bit**|1 = Параметр ANSI_WARNINGS для запроса включен (ON). В противном случае — 0.<br /><br /> Не допускает значение NULL.|  
|ansi_padding|**bit**|1 = Параметр ANSI_PADDING для запроса включен (ON).<br /><br /> В противном случае — 0.<br /><br /> Не допускает значение NULL.|  
|ansi_nulls|**bit**|1 = Параметр ANSI_NULLS для запроса включен (ON). В противном случае — 0.<br /><br /> Не допускает значение NULL.|  
|concat_null_yields_null|**bit**|1 = Параметр CONCAT_NULL_YIELDS_NULL для запроса включен (ON). В противном случае — 0.<br /><br /> Не допускает значение NULL.|  
|transaction_isolation_level|**smallint**|Уровень изоляции, с которым создана транзакция для данного запроса. Не допускает значение NULL.<br /> 0 = не указан;<br /> 1 = читать незафиксированные;<br /> 2 = читать зафиксированные;<br /> 3 = повторяемые результаты;<br /> 4 = сериализуемые;<br /> 5 = моментальный снимок.|  
|lock_timeout|**int**|Время ожидания блокировки для данного запроса (в миллисекундах). Не допускает значение NULL.|  
|deadlock_priority|**int**|Значение параметра DEADLOCK_PRIORITY для данного запроса. Не допускает значение NULL.|  
|row_count|**bigint**|Число строк, возвращенных клиенту по данному запросу. Не допускает значение NULL.|  
|prev_error|**int**|Последняя ошибка, происшедшая при выполнении запроса. Не допускает значение NULL.|  
|nest_level|**int**|Текущий уровень вложенности кода, выполняемого для данного запроса. Не допускает значение NULL.|  
|granted_query_memory|**int**|Число страниц, выделенных для выполнения поступившего запроса. Не допускает значение NULL.|  
|executing_managed_code|**bit**|Указывает, выполняет ли данный запрос в настоящее время код объекта среды CLR (например, процедуры, типа или триггера). Этот флаг установлен в течение всего времени, когда объект среды CLR находится в стеке, даже когда из среды вызывается код [!INCLUDE[tsql](../../includes/tsql-md.md)]. Не допускает значение NULL.|  
|group_id|**int**|Идентификатор группы рабочей нагрузки, которой принадлежит этот запрос. Не допускает значение NULL.|  
|query_hash|**Binary (8)**|Двоичное хэш-значение рассчитывается для запроса и используется для идентификации запросов с аналогичной логикой. Можно использовать хэш запроса для определения использования статистических ресурсов для запросов, которые отличаются только своими литеральными значениями.|  
|query_plan_hash|**Binary (8)**|Двоичное хэш-значение рассчитывается для плана выполнения запроса и используется для идентификации аналогичных планов выполнения запросов. Можно использовать хэш плана запроса для нахождения совокупной стоимости запросов со схожими планами выполнения.|  
|statement_sql_handle|**varbinary (64)**|**Область применения**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздних версий.<br /><br /> Описатель SQL отдельного запроса.<br /><br />Этот столбец имеет значение NULL, если хранилище запросов не включено для базы данных. |  
|statement_context_id|**bigint**|**Область применения**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздних версий.<br /><br /> Необязательный внешний ключ для sys.query_context_settings.<br /><br />Этот столбец имеет значение NULL, если хранилище запросов не включено для базы данных. |  
|dop |**int** |**Область применения**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] и более поздних версий.<br /><br /> Степень параллелизма запроса. |  
|parallel_worker_count |**int** |**Область применения**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] и более поздних версий.<br /><br /> Количество зарезервированных параллельных рабочих ролей, если это параллельный запрос.  |  
|external_script_request_id |**uniqueidentifier** |**Область применения**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] и более поздних версий.<br /><br /> Идентификатор запроса внешнего скрипта, связанный с текущим запросом. |  
|is_resumable |**bit** |**Область применения**: [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] и более поздних версий.<br /><br /> Указывает, является ли запрос возобновляемой операцией с индексом. |  
|page_resource |**Binary (8)** |**Область применения**: [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]<br /><br /> 8-байтовое шестнадцатеричное представление ресурса страницы, если `wait_resource` столбец содержит страницу. Дополнительные сведения см. в разделе [sys.fn_PageResCracker](../../relational-databases/system-functions/sys-fn-pagerescracker-transact-sql.md). |  
|page_server_reads|**bigint**|Область **применения**: масштабирование базы данных SQL Azure<br /><br /> Число операций чтения сервера страниц, выполненных этим запросом. Не допускает значение NULL.|  
| &nbsp; | &nbsp; | &nbsp; |

## <a name="remarks"></a>Примечания 
Чтобы выполнить код, внешний по отношению к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (например, расширенную хранимую процедуру или распределенный запрос), поток должен выйти из-под управления планировщика, работающего в режиме без вытеснения. Для этого исполнитель переходит в режим с вытеснением. Значения времени, возвращаемые этим динамическим административным представлением, не включают время, затраченное в режиме с вытеснением.

При выполнении параллельных запросов в [режиме строки](../../relational-databases/query-processing-architecture-guide.md#row-mode-execution) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] назначает рабочий поток для координации рабочих потоков, ответственных за выполнение назначенных им задач. В этом динамическом административном отображении для запроса отображается только поток координатора. Столбцы **операций чтения**, **записи**, **logical_reads** и **row_count** **не обновляются** для потока координатора. Столбцы **wait_type**, **wait_time**, **last_wait_type**, **wait_resource** и **granted_query_memory** **обновляются только** для потока координатора. Дополнительные сведения см. в статье [Руководство по архитектуре потоков и задач](../../relational-databases/thread-and-task-architecture-guide.md).

## <a name="permissions"></a>Разрешения
Если пользователь имеет `VIEW SERVER STATE` разрешение на сервере, он увидит все выполняющиеся сеансы на экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ; в противном случае пользователь увидит только текущий сеанс. `VIEW SERVER STATE` не может быть предоставлено в базе данных SQL Azure, поэтому `sys.dm_exec_requests` всегда ограничивается текущим соединением.

В Always-On сценариях, если вторичная реплика имеет значение **только для чтения**, то соединение с базой данных-получателем должно указывать назначение приложения в параметрах строки подключения путем добавления `applicationintent=readonly` . В противном случае проверка доступа для `sys.dm_exec_requests` баз данных в группе доступности не будет пройдена, даже если `VIEW SERVER STATE` имеется разрешение.

  
## <a name="examples"></a>Примеры  
  
### <a name="a-finding-the-query-text-for-a-running-batch"></a>A. Поиск текста запроса для выполнения пакета

 В следующем примере выполняется запрос `sys.dm_exec_requests` для поиска необходимого запроса и из его результата копируется `sql_handle`.  

```sql
SELECT * FROM sys.dm_exec_requests;  
GO  
```  

Затем для получения текста инструкции используйте скопированный `sql_handle` с помощью системной функции `sys.dm_exec_sql_text(sql_handle)`.  

```sql
SELECT * FROM sys.dm_exec_sql_text(< copied sql_handle >);  
GO  
```

### <a name="b-finding-all-locks-that-a-running-batch-is-holding"></a>Б. Поиск всех блокировок, которые содержит выполняемый пакет

В следующем примере запрос **sys.dm_exec_requests** для поиска интересного пакета и его копирования `transaction_id` из выходных данных.

```sql
SELECT * FROM sys.dm_exec_requests;  
GO
```

Затем, чтобы найти сведения о блокировке, используйте скопированную `transaction_id` с системной функцией **sys.dm_tran_locks**.  

```sql
SELECT * FROM sys.dm_tran_locks
WHERE request_owner_type = N'TRANSACTION'
    AND request_owner_id = < copied transaction_id >;
GO  
```

### <a name="c-finding-all-currently-blocked-requests"></a>В. Поиск всех запросов, заблокированных в настоящий момент

В следующем примере запрос **sys.dm_exec_requests** для поиска сведений о заблокированных запросах.  

```sql
SELECT session_id ,status ,blocking_session_id  
    ,wait_type ,wait_time ,wait_resource
    ,transaction_id
FROM sys.dm_exec_requests
WHERE status = N'suspended';  
GO  
```  

### <a name="d-ordering-existing-requests-by-cpu"></a>Г. Упорядочивание существующих запросов по ЦП

```sql
SELECT 
   req.session_id
   , req.start_time
   , cpu_time 'cpu_time_ms'
   , object_name(st.objectid,st.dbid) 'ObjectName' 
   , substring
      (REPLACE
        (REPLACE
          (SUBSTRING
            (ST.text
            , (req.statement_start_offset/2) + 1
            , (
               (CASE statement_end_offset
                  WHEN -1
                  THEN DATALENGTH(ST.text)  
                  ELSE req.statement_end_offset
                  END
                    - req.statement_start_offset)/2) + 1)
       , CHAR(10), ' '), CHAR(13), ' '), 1, 512)  AS statement_text  
FROM sys.dm_exec_requests AS req  
   CROSS APPLY sys.dm_exec_sql_text(req.sql_handle) as ST
   ORDER BY cpu_time desc;
GO
```

## <a name="see-also"></a>См. также:
[Динамические административные представления и функции](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)     
[Динамические административные представления и функции, связанные с выполнением](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)      
[sys.dm_os_memory_clerks](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md)     
[sys.dm_os_sys_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)     
[sys.dm_exec_query_memory_grants](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)    
[sys.dm_exec_query_plan](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)    
[sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)      
[SQL Server, объект статистики SQL](../../relational-databases/performance-monitor/sql-server-sql-statistics-object.md)     
[Руководство по архитектуре обработки запросов](../../relational-databases/query-processing-architecture-guide.md#DOP)       
[Руководство по архитектуре потоков и задач](../../relational-databases/thread-and-task-architecture-guide.md)    
