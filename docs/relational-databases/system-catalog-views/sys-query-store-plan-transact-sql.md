---
description: sys.query_store_plan (Transact-SQL)
title: sys.query_store_plan (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 01/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- QUERY_STORE_PLAN_TSQL
- SYS.QUERY_STORE_PLAN
- SYS.QUERY_STORE_PLAN_TSQL
- QUERY_STORE_PLAN
dev_langs:
- TSQL
helpviewer_keywords:
- query_store_plan catalog view
- sys.query_store_plan catalog view
ms.assetid: b4d05439-6360-45db-b1cd-794f4a64935e
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1b13c6f42eb967c6ef5d01e189860c9de3204646
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095475"
---
# <a name="sysquery_store_plan-transact-sql"></a>sys.query_store_plan (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Содержит сведения о каждом плане выполнения, связанном с запросом.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|
|**plan_id**|**bigint**|Первичный ключ.|  
|**query_id**|**bigint**|Внешний ключ. Присоединение к [sys.query_store_query &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md).|  
|**plan_group_id**|**bigint**|Идентификатор группы планов. Для запросов курсоров обычно требуется несколько планов (заполнение и получение). Заполнять и выборке собранные планы находятся в одной группе.<br /><br /> 0 означает, что план не входит в группу.|  
|**engine_version**|**nvarchar(32)**|Версия подсистемы, используемой для компиляции плана в формате **"основной. дополнительный. сборка. Редакция"** .|  
|**compatibility_level**|**smallint**|Уровень совместимости базы данных, на который ссылается запрос.|  
|**query_plan_hash**|**Binary (8)**|Хэш MD5 отдельного плана.|  
|**query_plan**|**nvarchar(max)**|Инструкция Showplan XML для плана запроса.|  
|**is_online_index_plan**|**bit**|План использовался при построении индекса в режиме «в сети». <br/>**Примечание.** Azure синапсе Analytics всегда возвращает ноль (0).|  
|**is_trivial_plan**|**bit**|План является тривиальным планом (выходные данные на этапе 0 оптимизатора запросов). <br/>**Примечание.** Azure синапсе Analytics всегда возвращает ноль (0).|  
|**is_parallel_plan**|**bit**|План является параллельным. <br/>**Примечание.** Azure синапсе Analytics всегда будет возвращать один (1).|  
|**is_forced_plan**|**bit**|План помечается как принудительный, когда пользователь выполняет хранимую процедуру **sys.sp_query_store_force_plan**. Принудительный механизм не *гарантирует* , что именно этот план будет использоваться для запроса, на который ссылается **query_id**. Форсирование плана вызывает повторную компиляцию запроса и, как правило, создает тот же или аналогичный план для плана, на который ссылается **plan_id**. Если форсирование плана не выполняется, **force_failure_count** увеличивается, а **last_force_failure_reason** заполняется причиной сбоя. <br/>**Примечание.** Azure синапсе Analytics всегда возвращает ноль (0).|  
|**is_natively_compiled**|**bit**|План включает оптимизированные для обработки в памяти процедуры. (0 = FALSE, 1 = TRUE). <br/>**Примечание.** Azure синапсе Analytics всегда возвращает ноль (0).|  
|**force_failure_count**|**bigint**|Число неудачных попыток принудительного выполнения плана. Его можно увеличить, только если запрос перекомпилируется (*не при каждом выполнении*). Он сбрасывается в 0 каждый раз, **is_plan_forced** меняется с **false** на **true**. <br/>**Примечание.** Azure синапсе Analytics всегда возвращает ноль (0).|  
|**last_force_failure_reason**|**int**|Причина сбоя принудительного применения плана.<br /><br /> 0: нет ошибки, в противном случае — номер ошибки, вызвавшей сбой принудительного выполнения<br /><br /> 8637: ONLINE_INDEX_BUILD<br /><br /> 8683: INVALID_STARJOIN<br /><br /> 8684: TIME_OUT<br /><br /> 8689: NO_DB<br /><br /> 8690: HINT_CONFLICT<br /><br /> 8691: SETOPT_CONFLICT<br /><br /> 8694: DQ_NO_FORCING_SUPPORTED<br /><br /> 8698: NO_PLAN<br /><br /> 8712: NO_INDEX<br /><br /> 8713: VIEW_COMPILE_FAILED<br /><br /> \<other value>: GENERAL_FAILURE <br/>**Примечание.** Azure синапсе Analytics всегда возвращает ноль (0).|  
|**last_force_failure_reason_desc**|**nvarchar(128)**|Текстовое описание last_force_failure_reason_desc.<br /><br /> ONLINE_INDEX_BUILD: запрос пытается изменить данные, в то время как Целевая таблица имеет индекс, который строится в режиме "в сети"<br /><br /> INVALID_STARJOIN: Plan содержит недопустимую спецификацию Старжоин<br /><br /> TIME_OUT: оптимизатор превысил количество разрешенных операций при поиске плана, указанного принудительным планом<br /><br /> NO_DB: база данных, указанная в плане, не существует<br /><br /> HINT_CONFLICT: не удается скомпилировать запрос, так как возник конфликт плана с указанием запроса<br /><br /> DQ_NO_FORCING_SUPPORTED: не удается выполнить запрос, так как планирование конфликтов с использованием распределенных запросов или полнотекстовых операций.<br /><br /> NO_PLAN: обработчику запросов не удалось создать план запроса, так как принудительный план не может быть проверен на допустимость для запроса<br /><br /> NO_INDEX: индекс, указанный в плане, больше не существует<br /><br /> VIEW_COMPILE_FAILED: не удалось принудительно выполнить план запроса из-за проблемы в индексированном представлении, на которое ссылается план<br /><br /> GENERAL_FAILURE: Общая ошибка принудительного выполнения (не рассматривается по причинам выше) <br/>**Примечание.** Azure синапсе Analytics всегда будет возвращать *значение None*.|  
|**count_compiles**|**bigint**|Планирование статистики компиляции.|  
|**initial_compile_start_time**|**datetimeoffset**|Планирование статистики компиляции.|  
|**last_compile_start_time**|**datetimeoffset**|Планирование статистики компиляции.|  
|**last_execution_time**|**datetimeoffset**|Время последнего выполнения ссылается на последнее время окончания запроса или плана.|  
|**avg_compile_duration**|**float**|Планирование статистики компиляции. <br/>**Примечание.** Azure синапсе Analytics всегда возвращает ноль (0).|  
|**last_compile_duration**|**bigint**|Планирование статистики компиляции. <br/>**Примечание.** Azure синапсе Analytics всегда возвращает ноль (0).|  
|**plan_forcing_type**|**int**|Тип форсирования плана.<br /><br />0: НЕТ<br /><br />1: ВРУЧНУЮ<br /><br />2: АВТО|  
|**plan_forcing_type_desc**|**nvarchar(60)**|Текстовое описание plan_forcing_type.<br /><br />Нет: нет форсирования плана<br /><br />ВРУЧНУЮ: планирование принудительно пользователем<br /><br />AUTO: планирование принудительно по автоматической настройке|  

## <a name="plan-forcing-limitations"></a>Ограничения принудительного использования планов
Хранилище запросов имеет механизм, позволяющий оптимизатору запросов принудительно применить определенный план выполнения. Но существуют некоторые ограничения, которые могут препятствовать применению плана. 

Во-первых, когда план содержит следующие конструкции:
* Инструкция INSERT BULK.
* ссылка на внешнюю таблицу;
* распределенный запрос или полнотекстовые операции;
* использование глобальных запросов. 
* Динамические курсоры или наборы ключей 
* Недопустимая спецификация соединения типа "звезда" 

> [!NOTE]
> База данных SQL Azure и SQL Server 2019 поддерживают Форсирование планов для статических и быстрых курсоров.

Во-вторых, когда объекты, от которых зависит план, больше не доступны:
* база данных (если база данных, где план был создан изначально, больше не существует);
* индекс (больше не существует или отключен).

Наконец, проблемы с самим планом:
* недопустим для запроса;
* оптимизатор запросов превысил количество разрешенных операций;
* неправильно сформированный XML-код плана.

## <a name="permissions"></a>Разрешения  
 Требуется разрешение **View Database State** .  
  
## <a name="see-also"></a>См. также:  
 [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.query_context_settings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)   
 [sys.query_store_query &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)   
 [sys.query_store_query_text &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)   
 [sys.query_store_runtime_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql.md)   
 [sys.query_store_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)  
 [sys.query_store_runtime_stats_interval &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)   
 [Мониторинг производительности с использованием хранилища запросов](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Хранимые процедуры хранилища запросов &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)  
  
  
