---
title: Инфраструктура профилирования запросов | Документация Майкрософт
description: Сведения о том, как ядро СУБД SQL Server обращается к сведениям среды выполнения о планах выполнения запросов, чтобы изучить рабочую нагрузку и способ использования ресурсов.
ms.custom: ''
ms.date: 04/23/2019
ms.prod: sql
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- query plans [SQL Server]
- execution plans [SQL Server]
- query profiling
- lightweight query profiling
- lightweight profiling
- lwp
ms.assetid: 07f8f594-75b4-4591-8c29-d63811d7753e
author: pmasl
ms.author: pelopes
manager: amitban
ms.openlocfilehash: 57372929f190ff2fe32e7688d16acc75fafc9700
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171556"
---
# <a name="query-profiling-infrastructure"></a>Инфраструктура профилирования запросов
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] предоставляет возможность доступа к сведениям среды выполнения о планах выполнения запросов. При возникновении проблемы с производительностью одним из самых важных действий является получение сведений о том, какая рабочая нагрузка выполняется в данный момент и каким образом происходит управление ресурсами. Для осуществления этого важно иметь доступ к [действительному плану выполнения](../../relational-databases/performance/display-an-actual-execution-plan.md).

Несмотря на то, что для доступности действительного плана запроса необходимо дождаться завершения выполнения запроса, [динамическая статистика запросов](../../relational-databases/performance/live-query-statistics.md) может анализировать процесс выполнения запроса в режиме реального времени, по мере передачи управления от одного [оператора плана запроса](../../relational-databases/showplan-logical-and-physical-operators-reference.md) другому. Динамический план запроса отображает общий ход выполнения запроса и текущую статистику выполнения на уровне оператора, например число полученных строк, затраченное время, ход выполнения оператора и т. д. Так как эти данные доступны в режиме реального времени и, чтобы их увидеть, не нужно дожидаться завершения запроса, такая статистика чрезвычайно полезна для отладки проблем с производительностью запросов, таких как долгое или "бесконечное" выполнение запросов.

## <a name="the-standard-query-execution-statistics-profiling-infrastructure"></a>Стандартная инфраструктура профилирования статистики выполнения запросов

*Инфраструктуру профилей статистики выполнения запросов*, или стандартное профилирование, необходимо включить для сбора сведений о планах выполнения, а именно числе строк, использовании ЦП и операциях ввода-вывода. Следующие методы сбора сведений о плане выполнения для **целевого сеанса** используют стандартную инфраструктуру профилирования:

- [SET STATISTICS XML](../../t-sql/statements/set-statistics-xml-transact-sql.md) 
- [SET STATISTICS PROFILE](../../t-sql/statements/set-statistics-profile-transact-sql.md)
- [Динамическая статистика запросов](../../relational-databases/performance/live-query-statistics.md)

> [!NOTE]
> Режим *Включить динамическую статистику запросов* в [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] использует стандартную инфраструктуру профилирования.    
> В более поздних версиях [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], если включена [упрощенная инфраструктура профилирования](#lwp), именно она используется для динамической статистики запросов вместо обычного профилирования при просмотре через [Монитор активности](../../relational-databases/performance-monitor/activity-monitor.md) или прямые запросы [sys.dm_exec_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md) динамического административного представления. 

Следующие методы сбора сведений о плане выполнения глобально для **всех сеансов** используют стандартную инфраструктуру профилирования:

-  Расширенное событие **_query_post_execution_showplan_* _. Сведения о включении расширенных событий см. в статье [Monitor System Activity Using Extended Events](../../relational-databases/extended-events/monitor-system-activity-using-extended-events.md).  
- Событие трассировки _ *Showplan XML** в [трассировке SQL](../../relational-databases/sql-trace/sql-trace.md) и [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md). Дополнительные сведения об этом событии трассировки см. в статье [Showplan XML, класс событий](../../relational-databases/event-classes/showplan-xml-event-class.md).

При выполнении сеанса расширенного события, использующего событие *query_post_execution_showplan*, также заполняется динамическое административное представление [sys.dm_exec_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md), которое включает динамическую статистику запросов для всех сеансов при помощи [монитора активности](../../relational-databases/performance-monitor/activity-monitor.md) или прямого запроса динамического административного представления. Дополнительные сведения см. в статье [Live Query Statistics](../../relational-databases/performance/live-query-statistics.md).

## <a name="the-lightweight-query-execution-statistics-profiling-infrastructure"></a><a name="lwp"></a> Упрощенная инфраструктура профилирования статистики выполнения запросов

Начиная с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 и [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] была введена новая *упрощенная инфраструктура профилирования статистики выполнения запросов*, или **упрощенное профилирование**. 

> [!NOTE]
> Хранимые процедуры, скомпилированные в собственном коде, не поддерживаются в упрощенном профилировании.  

### <a name="lightweight-query-execution-statistics-profiling-infrastructure-v1"></a>Упрощенная инфраструктура профилирования статистики выполнения запросов версии 1

**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] с пакетом обновления 2 по [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]). 
  
Начиная с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 и [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] потребление ресурсов при сборе сведений о планах выполнения было снижено путем введения упрощенного профилирования. В отличие от стандартного, упрощенное профилирование не собирает сведения о ЦП среды выполнения. Однако упрощенное профилирование по-прежнему собирает сведения о количестве строк и сведения об использовании операций ввода-вывода.

Также было добавлено новое расширенное событие **_query_thread_profile_* _, использующее упрощенное профилирование. Это расширенное событие предоставляет статистику выполнения по операторам, позволяя получить больше сведений о производительности каждого узла и потока. Ниже приведен пример сеанса, использующего это расширенное событие.

```sql
CREATE EVENT SESSION [NodePerfStats] ON SERVER
ADD EVENT sqlserver.query_thread_profile(
  ACTION(sqlos.scheduler_id,sqlserver.database_id,sqlserver.is_system,
    sqlserver.plan_handle,sqlserver.query_hash_signed,sqlserver.query_plan_hash_signed,
    sqlserver.server_instance_name,sqlserver.session_id,sqlserver.session_nt_username,
    sqlserver.sql_text))
ADD TARGET package0.ring_buffer(SET max_memory=(25600))
WITH (MAX_MEMORY=4096 KB,
  EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,
  MAX_DISPATCH_LATENCY=30 SECONDS,
  MAX_EVENT_SIZE=0 KB,
  MEMORY_PARTITION_MODE=NONE,
  TRACK_CAUSALITY=OFF,
  STARTUP_STATE=OFF);
```

> [!NOTE]
> Дополнительные сведения о снижении потребления ресурсов профилированием запросов см. в записи блога [Developers Choice: Query progress - anytime, anywhere](/archive/blogs/sql_server_team/query-progress-anytime-anywhere) (Выбор разработчика: ход выполнения запроса — всегда и везде). 

При выполнении сеанса расширенного события, использующего событие _query_thread_profile*, также заполняется динамическое административное представление [sys.dm_exec_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md) с помощью упрощенного профилирования, которое включает динамическую статистику запросов для всех сеансов при помощи [монитора активности](../../relational-databases/performance-monitor/activity-monitor.md) или прямого запроса динамического административного представления.

### <a name="lightweight-query-execution-statistics-profiling-infrastructure-v2"></a>Упрощенная инфраструктура профилирования статистики выполнения запросов версии 2

**Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] с пакетом обновления 1 по [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) 

[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 включает переработанную версию упрощенного профилирования с минимальным потреблением ресурсов. Упрощенное профилирование можно также включить глобально с помощью [флага трассировки 7412](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) в версиях, указанных выше в поле *Применимо к*. Новая функция динамического управления [sys.dm_exec_query_statistics_xml](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-statistics-xml-transact-sql.md) введена для возвращения плана выполнения запроса для активных запросов.

Начиная с [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 CU3 и [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU11, если упрощенное профилирование не включено глобально, можно использовать [указание запроса USE HINT](../../t-sql/queries/hints-transact-sql-query.md#use_hint) с новым аргументом **QUERY_PLAN_PROFILE** для включения упрощенного профилирования на уровне запроса для любого сеанса. После завершения запроса, содержащего это новое указание, также выводится новое расширенное событие **_query_plan_profile_* _, предоставляющее действительный план выполнения в формате XML, аналогично расширенному событию _query_post_execution_showplan*. 

> [!NOTE]
> Расширенное событие *query_plan_profile* также использует упрощенное профилирование, даже если указание запроса отсутствует. 

Пример сеанса с расширенным событием *query_plan_profile* можно настроить, как показано ниже:

```sql
CREATE EVENT SESSION [PerfStats_LWP_Plan] ON SERVER
ADD EVENT sqlserver.query_plan_profile(
  ACTION(sqlos.scheduler_id,sqlserver.database_id,sqlserver.is_system,
    sqlserver.plan_handle,sqlserver.query_hash_signed,sqlserver.query_plan_hash_signed,
    sqlserver.server_instance_name,sqlserver.session_id,sqlserver.session_nt_username,
    sqlserver.sql_text))
ADD TARGET package0.ring_buffer(SET max_memory=(25600))
WITH (MAX_MEMORY=4096 KB,
  EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,
  MAX_DISPATCH_LATENCY=30 SECONDS,
  MAX_EVENT_SIZE=0 KB,
  MEMORY_PARTITION_MODE=NONE,
  TRACK_CAUSALITY=OFF,
  STARTUP_STATE=OFF);
```

### <a name="lightweight-query-execution-statistics-profiling-infrastructure-v3"></a>Упрощенная инфраструктура профилирования статистики выполнения запросов версии 3

**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

[!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] включают в себя заново переработанную версию упрощенного профилирования, собирающего данные о количестве строк для всех выполнений. Упрощенное профилирование включено в [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] по умолчанию. Начиная с версии [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)], флаг трассировки 7412 не действует. Упрощенное профилирование можно отключить на уровне базы данных с помощью конфигурации уровня базы данных [LIGHTWEIGHT_QUERY_PROFILING](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md): `ALTER DATABASE SCOPED CONFIGURATION SET LIGHTWEIGHT_QUERY_PROFILING = OFF;`.

Появилась новая функция динамического управления [sys.dm_exec_query_plan_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-stats-transact-sql.md), которая возвращает эквивалент последнего известного действительного плана выполнения для большинства запросов и называется *статистика плана последнего запроса*. Ее можно включить на уровне базы данных с помощью конфигурации уровня базы данных [LAST_QUERY_PLAN_STATS](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md): `ALTER DATABASE SCOPED CONFIGURATION SET LAST_QUERY_PLAN_STATS = ON;`.

Новое расширенное событие *query_post_execution_plan_profile* служит для сбора эквивалента действительного плана выполнения на основе упрощенного, а не стандартного профилирования, как в случае с событием *query_post_execution_showplan*. [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] также предлагает это событие, начиная с CU14. Пример сеанса с расширенным событием *query_post_execution_plan_profile* можно настроить, как показано ниже.

```sql
CREATE EVENT SESSION [PerfStats_LWP_All_Plans] ON SERVER
ADD EVENT sqlserver.query_post_execution_plan_profile(
  ACTION(sqlos.scheduler_id,sqlserver.database_id,sqlserver.is_system,
    sqlserver.plan_handle,sqlserver.query_hash_signed,sqlserver.query_plan_hash_signed,
    sqlserver.server_instance_name,sqlserver.session_id,sqlserver.session_nt_username,
    sqlserver.sql_text))
ADD TARGET package0.ring_buffer(SET max_memory=(25600))
WITH (MAX_MEMORY=4096 KB,
  EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,
  MAX_DISPATCH_LATENCY=30 SECONDS,
  MAX_EVENT_SIZE=0 KB,
  MEMORY_PARTITION_MODE=NONE,
  TRACK_CAUSALITY=OFF,
  STARTUP_STATE=OFF);
```

#### <a name="example-1---extended-event-session-using-standard-profiling"></a>Пример 1. Сеанс расширенных событий на основе стандартного профилирования

```sql
CREATE EVENT SESSION [QueryPlanOld] ON SERVER 
ADD EVENT sqlserver.query_post_execution_showplan(
    ACTION(sqlos.task_time, sqlserver.database_id, 
    sqlserver.database_name, sqlserver.query_hash_signed, 
    sqlserver.query_plan_hash_signed, sqlserver.sql_text))
ADD TARGET package0.event_file(SET filename = N'C:\Temp\QueryPlanStd.xel')
WITH (MAX_MEMORY=4096 KB, EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS, 
    MAX_DISPATCH_LATENCY=30 SECONDS, MAX_EVENT_SIZE=0 KB, 
    MEMORY_PARTITION_MODE=NONE, TRACK_CAUSALITY=OFF, STARTUP_STATE=OFF);
```

#### <a name="example-2---extended-event-session-using-lightweight-profiling"></a>Пример 2. Сеанс расширенных событий на основе упрощенного профилирования

```sql
CREATE EVENT SESSION [QueryPlanLWP] ON SERVER 
ADD EVENT sqlserver.query_post_execution_plan_profile(
    ACTION(sqlos.task_time, sqlserver.database_id, 
    sqlserver.database_name, sqlserver.query_hash_signed, 
    sqlserver.query_plan_hash_signed, sqlserver.sql_text))
ADD TARGET package0.event_file(SET filename=N'C:\Temp\QueryPlanLWP.xel')
WITH (MAX_MEMORY=4096 KB, EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS, 
    MAX_DISPATCH_LATENCY=30 SECONDS, MAX_EVENT_SIZE=0 KB, 
    MEMORY_PARTITION_MODE=NONE, TRACK_CAUSALITY=OFF, STARTUP_STATE=OFF);
```

## <a name="query-profiling-infrastruture-usage-guidance"></a>Руководство по использованию инфраструктуры профилирования запросов
В приведенной ниже таблице перечислены действия по включению стандартного или упрощенного профилирования глобально (на уровне сервера) или в одном сеансе. В ней также приведены сведения о минимальных версиях, поддерживающих это действие. 

|Область|Стандартное профилирование|Упрощенное профилирование|
|---------------|---------------|---------------|
|Global|Сеанс xEvent с расширенным событием `query_post_execution_showplan`. Минимальная версия: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|Флаг трассировки 7412. Минимальная версия: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] с пакетом обновления 1 (SP1)|
|Global|Система "Трассировка SQL" и SQL Server Profiler с событием трассировки `Showplan XML`. Минимальная версия: SQL Server 2000|Сеанс xEvent с расширенным событием `query_thread_profile`. Минимальная версия: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] с пакетом обновления 2 (SP2)|
|Global|-|Сеанс xEvent с расширенным событием `query_post_execution_plan_profile`. Минимальная версия: [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] с накопительным пакетом обновления 14 (CU14) и [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]|
|Сеанс|Используйте `SET STATISTICS XML ON`. Минимальная версия: SQL Server 2000|Используйте указание запроса `QUERY_PLAN_PROFILE`, а также сеанс xEvent с расширенным событием `query_plan_profile`. Минимальная версия: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] с пакетом обновления 2 (SP2) и накопительным пакетом обновления 3 (CU3) и [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] с накопительным пакетом обновления 11 (CU11)|
|Сеанс|Используйте `SET STATISTICS PROFILE ON`. Минимальная версия: SQL Server 2000|-|
|Сеанс|Нажмите кнопку [Статистика активных запросов](../../relational-databases/performance/live-query-statistics.md) в SSMS. Минимальная версия: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] с пакетом обновления 2 (SP2)|-|

## <a name="remarks"></a>Remarks

> [!IMPORTANT]
> Из-за возможных случайных нарушений прав доступа во время выполнения мониторинга хранимой процедуры, которая ссылается на [sys.dm_exec_query_statistics_xml](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-statistics-xml-transact-sql.md), необходимо установить исправление [4078596 КБ](https://support.microsoft.com/help/4078596) на [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] и [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)].

Начиная с упрощенного профилирования версии 2, благодаря его низкому потреблению ресурсов, любой сервер, у которого нет перегрузки ЦП, может выполнять упрощенное профилирование **непрерывно**. Это позволяет специалистам по работе с базами данных в любое время подключаться к любому запущенному выполнению, например с помощью монитора активности или прямого запроса `sys.dm_exec_query_profiles`, и получать план запроса со статистикой времени выполнения.

Дополнительные сведения о снижении потребления ресурсов профилированием запросов см. в записи блога [Developers Choice: Query progress - anytime, anywhere](https://techcommunity.microsoft.com/t5/SQL-Server/Developers-Choice-Query-progress-anytime-anywhere/ba-p/385004) (Выбор разработчика: ход выполнения запроса — всегда и везде). 

> [!NOTE]
> Расширенные события на основе упрощенного профилирования используют данные стандартного профилирования, если инфраструктура стандартного профилирования уже включена. Допустим, имеется запущенный сеанс расширенных событий `query_post_execution_showplan`, и запускается еще один сеанс для событий `query_post_execution_plan_profile`. Во втором сеансе будут использоваться данные стандартного профилирования.

> [!NOTE]
> В [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] упрощенное профилирование по умолчанию отключено, но оно включается при запуске трассировки XEvent на основе `query_post_execution_plan_profile` и снова отключается при остановке трассировки. Поэтому если трассировка XEvent на основе `query_post_execution_plan_profile` часто запускается и останавливается на экземпляре [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], настоятельно рекомендуется включить упрощенное профилирование на глобальном уровне, установив флаг трассировки 7412, чтобы избежать повторяющихся издержек на включение и отключение профилирования. 

## <a name="see-also"></a>См. также:  
 [Наблюдение и настройка производительности](../../relational-databases/performance/monitor-and-tune-for-performance.md)     
 [Средства контроля и настройки производительности](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)     
 [Открытие монитора активности (среда SQL Server Management Studio)](../../relational-databases/performance-monitor/open-activity-monitor-sql-server-management-studio.md)     
 [Монитор активности](../../relational-databases/performance-monitor/activity-monitor.md)     
 [Мониторинг производительности с использованием хранилища запросов](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)     
 [Мониторинг активности системы с помощью расширенных событий](../../relational-databases/extended-events/monitor-system-activity-using-extended-events.md)      
 [sys.dm_exec_query_statistics_xml](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-statistics-xml-transact-sql.md)     
 [sys.dm_exec_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md)     
 [Флаги трассировки](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)    
 [Справочник по логическим и физическим операторам Showplan](../../relational-databases/showplan-logical-and-physical-operators-reference.md)    
 [Действительный план выполнения](../../relational-databases/performance/display-an-actual-execution-plan.md)    
 [Динамическая статистика запросов](../../relational-databases/performance/live-query-statistics.md)