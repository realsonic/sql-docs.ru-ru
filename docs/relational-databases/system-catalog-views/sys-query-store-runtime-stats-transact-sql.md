---
description: sys.query_store_runtime_stats (Transact-SQL)
title: sys.query_store_runtime_stats (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 07/24/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- SYS.QUERY_STORE_RUNTIME_STATS_TSQL
- QUERY_STORE_RUNTIME_STATS_TSQL
- SYS.QUERY_STORE_RUNTIME_STATS
- QUERY_STORE_RUNTIME_STATS
dev_langs:
- TSQL
helpviewer_keywords:
- query_store_runtime_stats catalog view
- sys.query_store_runtime_stats catalog view
ms.assetid: ccf7a57c-314b-450c-bd34-70749a02784a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8a6f0f21100ca68575c8db8dcde8e70f688d0ef5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098026"
---
# <a name="sysquery_store_runtime_stats-transact-sql"></a>sys.query_store_runtime_stats (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Содержит сведения о статистике выполнения запроса в среде выполнения.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**runtime_stats_id**|**bigint**|Идентификатор строки, представляющей статистику выполнения среды выполнения для **plan_id**, **execution_type** и **runtime_stats_interval_id**. Он уникален только за прошлые интервалы статистики времени выполнения. Для текущего активного интервала может существовать несколько строк, представляющих статистику времени выполнения для плана, на который ссылается **plan_id**, с типом выполнения, представленным **execution_type**. Как правило, одна строка представляет статистику времени выполнения, которая записывается на диск, а другие (s) представляют состояние в памяти. Таким образом, чтобы получить фактическое состояние для каждого интервала, необходимо выполнить статистическую обработку метрик, группирование по **plan_id**, **execution_type** и **runtime_stats_interval_id**.<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**plan_id**|**bigint**|Внешний ключ. Присоединение к [sys.query_store_plan &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md).|  
|**runtime_stats_interval_id**|**bigint**|Внешний ключ. Присоединение к [sys.query_store_runtime_stats_interval &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md).|  
|**execution_type**|**tinyint**|Определяет тип выполнения запроса:<br /><br /> 0 — регулярное выполнение (успешно завершено)<br /><br /> 3. инициированное клиентом прерывание выполнения<br /><br /> 4 — выполнение прерванного исключения|  
|**execution_type_desc**|**nvarchar(128)**|Текстовое описание поля "тип выполнения":<br /><br /> 0 — обычный<br /><br /> 3 — прервано<br /><br /> 4 — исключение|  
|**first_execution_time**|**datetimeoffset**|Время первого выполнения для плана запроса в пределах интервала агрегирования. Это относится к времени окончания выполнения запроса.|  
|**last_execution_time**|**datetimeoffset**|Время последнего выполнения плана запроса в пределах интервала агрегирования. Это относится к времени окончания выполнения запроса.|  
|**count_executions**|**bigint**|Общее число выполнений для плана запроса в пределах интервала статистической обработки.|  
|**avg_duration**|**float**|Средняя длительность плана запроса в пределах интервала агрегирования (в микросекундах).|  
|**last_duration**|**bigint**|Последняя длительность плана запроса в пределах интервала агрегирования (в микросекундах).|  
|**min_duration**|**bigint**|Минимальная длительность плана запроса в пределах интервала агрегирования (в микросекундах).|  
|**max_duration**|**bigint**|Максимальная длительность плана запроса в пределах интервала агрегирования (в микросекундах).|  
|**stdev_duration**|**float**|Стандартное отклонение длительности для плана запроса в пределах интервала агрегирования (в микросекундах).|  
|**avg_cpu_time**|**float**|Среднее время ЦП для плана запроса в пределах интервала агрегирования (отображается в микросекундах).<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**last_cpu_time**|**bigint**|Последнее время ЦП для плана запроса в пределах интервала агрегирования (в микросекундах).<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**min_cpu_time**|**bigint**|Минимальное время ЦП для плана запроса в пределах интервала агрегирования (в микросекундах).<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**max_cpu_time**|**bigint**|Максимальное время ЦП для плана запроса в пределах интервала агрегирования (в микросекундах).<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**stdev_cpu_time**|**float**|Стандартное отклонение времени ЦП для плана запроса в пределах интервала агрегирования (в микросекундах).<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**avg_logical_io_reads**|**float**|Среднее число логических операций чтения ввода-вывода для плана запроса в пределах интервала агрегирования. (выражается числом считанных 8 КБ страниц).<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**last_logical_io_reads**|**bigint**|Последнее число логических операций чтения ввода-вывода для плана запроса в пределах интервала агрегирования. (выражается числом считанных 8 КБ страниц).<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**min_logical_io_reads**|**bigint**|Минимальное число логических операций чтения ввода-вывода для плана запроса в пределах интервала статистической обработки. (выражается числом считанных 8 КБ страниц).<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**max_logical_io_reads**|**bigint**|Максимальное число логических операций чтения ввода-вывода для плана запроса в пределах интервала статистической обработки. (выражается числом считанных 8 КБ страниц).<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**stdev_logical_io_reads**|**float**|Число логических операций чтения ввода-вывода для плана запроса в пределах интервала агрегирования. (выражается числом считанных 8 КБ страниц).<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**avg_logical_io_writes**|**float**|Среднее число логических операций записи ввода-вывода для плана запроса в пределах интервала агрегирования.<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**last_logical_io_writes**|**bigint**|Последнее число логических операций записи ввода-вывода для плана запроса в пределах интервала агрегирования.<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**min_logical_io_writes**|**bigint**|Минимальное число логических операций записи ввода-вывода для плана запроса в пределах интервала статистической обработки.<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**max_logical_io_writes**|**bigint**|Максимальное число логических операций записи ввода-вывода для плана запроса в пределах интервала статистической обработки.<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**stdev_logical_io_writes**|**float**|Число логических операций ввода-вывода при стандартном отклонении для плана запроса в пределах интервала агрегирования.<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**avg_physical_io_reads**|**float**|Среднее число физических операций чтения ввода-вывода для плана запроса в пределах интервала агрегирования (выраженное числом считанных 8 КБ страниц).<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**last_physical_io_reads**|**bigint**|Последнее число физических операций чтения ввода-вывода для плана запроса в пределах интервала агрегирования (выраженное числом считанных 8 КБ страниц).<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**min_physical_io_reads**|**bigint**|Минимальное число физических операций чтения ввода-вывода для плана запроса в пределах интервала агрегирования (выраженное числом считанных 8 КБ страниц).<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**max_physical_io_reads**|**bigint**|Максимальное число физических операций чтения ввода-вывода для плана запроса в пределах интервала агрегирования (выраженное числом считанных 8 КБ страниц).<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**stdev_physical_io_reads**|**float**|Число физических операций ввода-вывода. стандартное отклонение для плана запроса в пределах интервала агрегирования (выраженное числом считанных 8 КБ страниц).<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**avg_clr_time**|**float**|Среднее время CLR для плана запроса в пределах интервала агрегирования (отображается в микросекундах).<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**last_clr_time**|**bigint**|Время последней среды CLR для плана запроса в пределах интервала агрегирования (в микросекундах).<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**min_clr_time**|**bigint**|Минимальное время CLR для плана запроса в пределах интервала агрегирования (в микросекундах).<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**max_clr_time**|**bigint**|Максимальное время CLR для плана запроса в пределах интервала агрегирования (в микросекундах).<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**stdev_clr_time**|**float**|Стандартное отклонение времени CLR для плана запроса в пределах интервала агрегирования (в микросекундах).<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**avg_dop**|**float**|Среднее значение DOP (степень параллелизма) для плана запроса в пределах интервала статистической обработки.<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**last_dop**|**bigint**|Последнее значение DOP (степень параллелизма) для плана запроса в пределах интервала статистической обработки.<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**min_dop**|**bigint**|Минимальное значение DOP (степень параллелизма) для плана запроса в пределах интервала статистической обработки.<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**max_dop**|**bigint**|Максимальный уровень DOP (степень параллелизма) для плана запроса в пределах интервала статистической обработки.<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**stdev_dop**|**float**|Стандартное отклонение DOP (степень параллелизма) для плана запроса в пределах интервала агрегирования.<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**avg_query_max_used_memory**|**float**|Среднее предоставление памяти (отображается как число страниц размером 8 КБ) для плана запроса в пределах интервала агрегирования. Всегда равно 0 для запросов, использующих процедуры, оптимизированные для памяти в собственном виде.<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**last_query_max_used_memory**|**bigint**|Последнее предоставление памяти (отображается как число страниц размером 8 КБ) для плана запроса в пределах интервала агрегирования. Всегда равно 0 для запросов, использующих процедуры, оптимизированные для памяти в собственном виде.<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**min_query_max_used_memory**|**bigint**|Минимальный объем памяти, предоставленный для плана запроса в пределах интервала агрегирования (в виде числа 8 КБ). Всегда равно 0 для запросов, использующих процедуры, оптимизированные для памяти в собственном виде.<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**max_query_max_used_memory**|**bigint**|Максимальный объем выделения памяти (отображается как число страниц размером 8 КБ) для плана запроса в пределах интервала агрегирования. Всегда равно 0 для запросов, использующих процедуры, оптимизированные для памяти в собственном виде.<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**stdev_query_max_used_memory**|**float**|Память предоставляет стандартное отклонение (сообщаемое как число страниц размером 8 КБ) для плана запроса в пределах интервала статистической обработки. Всегда равно 0 для запросов, использующих процедуры, оптимизированные для памяти в собственном виде.<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**avg_rowcount**|**float**|Среднее число возвращенных строк для плана запроса в пределах интервала статистической обработки.<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**last_rowcount**|**bigint**|Число возвращенных строк по последнему выполнению плана запроса в пределах интервала статистической обработки.<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**min_rowcount**|**bigint**|Минимальное число возвращаемых строк для плана запроса в пределах интервала статистической обработки.<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**max_rowcount**|**bigint**|Максимальное число возвращаемых строк для плана запроса в пределах интервала статистической обработки.|  
|**stdev_rowcount**|**float**|Число возвращенных строк со стандартным отклонением для плана запроса в пределах интервала статистической обработки.|
|**avg_log_bytes_used**|**float**|Среднее число байтов в журнале базы данных, используемое планом запроса, в пределах интервала статистической обработки.<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**last_log_bytes_used**|**bigint**|Число байтов в журнале базы данных, используемых при последнем выполнении плана запроса, в пределах интервала статистической обработки.<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**min_log_bytes_used**|**bigint**|Минимальное число байтов в журнале базы данных, используемом планом запроса, в пределах интервала статистической обработки.<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**max_log_bytes_used**|**bigint**|Максимальное число байтов в журнале базы данных, используемое планом запроса, в пределах интервала статистической обработки.<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|
|**stdev_log_bytes_used**|**float**|Стандартное отклонение числа байтов в журнале базы данных, используемого планом запроса, в пределах интервала статистической обработки.<br/>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] всегда будет возвращать ноль (0).|  
|**avg_tempdb_space_used**|**float**|Среднее число страниц, используемых в базе данных tempdb для плана запроса в пределах интервала агрегирования (выраженное в виде числа 8 КБ страниц).<br><br/>**Применимо к:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|**last_tempdb_space_used**|**bigint**|Последнее количество страниц, использованных в базе данных tempdb для плана запроса в пределах интервала агрегирования (выражено в виде числа 8 КБ страниц).<br><br/>**Применимо к:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|**min_tempdb_space_used**|**bigint**|Минимальное число страниц, используемых в базе данных tempdb для плана запроса в пределах интервала агрегирования (выраженное в виде числа 8 КБ страниц).<br><br/>**Применимо к:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|**max_tempdb_space_used**|**bigint**|Максимальное количество страниц, используемых в базе данных tempdb для плана запроса в пределах интервала агрегирования (выраженное в виде числа 8 КБ страниц).<br><br/>**Применимо к:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|**stdev_tempdb_space_used**|**float**|Количество страниц, используемых в стандартном отклонении tempdb для плана запроса в пределах интервала агрегирования (выражено в виде числа 8 КБ страниц).<br><br/>**Применимо к:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|**avg_page_server_io_reads**|**float**|Среднее число операций чтения ввода-вывода сервера на странице для плана запроса в пределах интервала агрегирования (выраженное числом считанных 8 КБ страниц).<br><br/>**Применимо к:** Масштабирование базы данных SQL Azure</br>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] , [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] (не в случае масштабирования) всегда будет возвращать ноль (0).|
|**last_page_server_io_reads**|**bigint**|Последнее число операций чтения ввода-вывода сервера на странице для плана запроса в пределах интервала агрегирования (выражается числом считанных 8 КБ страниц).<br><br/>**Применимо к:** [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] Гипермасштабируемого</br>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] , [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] (не в случае масштабирования) всегда будет возвращать ноль (0).|
|**min_page_server_io_reads**|**bigint**|Минимальное число операций чтения ввода-вывода на сервере для плана запроса в пределах интервала агрегирования (выраженное числом считанных 8 КБ страниц).<br><br/>**Применимо к:** [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] Гипермасштабируемого</br>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] , [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] (не в случае масштабирования) всегда будет возвращать ноль (0).|
|**max_page_server_io_reads**|**bigint**|Максимальное число операций чтения ввода-вывода на сервере для плана запроса в пределах интервала агрегирования (выраженное числом считанных 8 КБ страниц).<br><br/>**Применимо к:** [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] Гипермасштабируемого</br>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] , [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] (не в случае масштабирования) всегда будет возвращать ноль (0).|
|**stdev_page_server_io_reads**|**float**|Число операций ввода-вывода сервера на странице: стандартное отклонение для плана запроса в пределах интервала агрегирования (выраженное числом считанных 8 КБ страниц).<br><br/>**Применимо к:** [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] Гипермасштабируемого</br>**Примечание.** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] , [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] (не в случае масштабирования) всегда будет возвращать ноль (0).|
  
## <a name="permissions"></a>Разрешения  
Требуется разрешение `VIEW DATABASE STATE`.  
  
## <a name="see-also"></a>См. также:  
 [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.query_context_settings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)   
 [sys.query_store_plan &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)   
 [sys.query_store_query &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)   
 [sys.query_store_query_text &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)   
 [sys.query_store_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)  
 [sys.query_store_runtime_stats_interval &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)   
 [Мониторинг производительности с использованием хранилища запросов](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Query Store Stored Procedures (Transact-SQL)](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)   (Хранимые процедуры хранилища запросов (Transact-SQL))  
 [Рекомендации по хранилищу запросов](../../relational-databases/performance/best-practice-with-the-query-store.md)   
  
