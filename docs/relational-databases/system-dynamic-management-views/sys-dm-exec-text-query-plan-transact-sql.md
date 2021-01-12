---
description: sys.dm_exec_text_query_plan (Transact-SQL)
title: sys.dm_exec_text_query_plan (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 10/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_exec_text_query_plan
- sys.dm_exec_text_query_plan_TSQL
- dm_exec_text_query_plan_TSQL
- sys.dm_exec_text_query_plan
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_text_query_plan dynamic management function
ms.assetid: 9d5e5f59-6973-4df9-9eb2-9372f354ca57
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a085041b90652dbdaa3408b0780338df88322751
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096582"
---
# <a name="sysdm_exec_text_query_plan-transact-sql"></a>sys.dm_exec_text_query_plan (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Возвращает инструкцию Showplan в текстовом формате для пакета [!INCLUDE[tsql](../../includes/tsql-md.md)] или для определенной инструкции в пакете. План запроса, указанный дескриптором плана, может кэшироваться или выполняться в данный момент. Эта функция, возвращающая табличное значение, аналогична [sys.dm_exec_query_plan &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md), но имеет следующие отличия.  
  
-   Вывод плана запроса возвращается в текстовом формате.  
-   Размер вывода плана запроса не ограничен.  
-   Можно указать отдельные инструкции в пакете.  
  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
sys.dm_exec_text_query_plan   
(   
    plan_handle   
    , { statement_start_offset | 0 | DEFAULT }  
        , { statement_end_offset | -1 | DEFAULT }  
)  
```  
  
## <a name="arguments"></a>Аргументы  
*plan_handle*  
Токен, однозначно определяющий план выполнения запроса для пакета, который был выполнен, а его план находится в кэше планов или в данный момент выполняется. *plan_handle* имеет тип **varbinary (64)**.   

*Plan_handle* можно получить из следующих объектов DMO: 
  
-   [sys.dm_exec_cached_plans (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)  
  
-   [sys.dm_exec_query_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
  
-   [sys.dm_exec_requests (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  

-   [sys.dm_exec_procedure_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)  

-   [sys.dm_exec_trigger_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)  
  
*statement_start_offset* | 0 | ПАРАМЕТРЫ  
Начальная позиция запроса, который описывает строка, в соответствующем тексте пакета или сохраняемом объекте, в байтах. *statement_start_offset* имеет **тип int**. Значение 0 указывает на начало пакета. Значение по умолчанию — 0.  
  
Начальное смещение инструкции можно получить из следующих объектов DMO:  
  
-  [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
  
-  [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
  
*statement_end_offset* | -1 | ПАРАМЕТРЫ  
Конечная позиция запроса, который описывает строка, в соответствующем тексте пакета или сохраняемом объекте, в байтах.  
  
*statement_start_offset* имеет **тип int**.  
  
Значение -1 обозначает конец пакета. Значение по умолчанию — -1.  
  
## <a name="table-returned"></a>Возвращаемая таблица  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**DBID**|**smallint**|Идентификатор базы данных, в контексте которой выполнялась компиляция инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)], соответствующей данному плану. Для нерегламентированных и подготовленных инструкций SQL это идентификатор базы данных, в которой происходила компиляция инструкции.<br /><br /> Столбец может содержать значение NULL.|  
|**objectid**|**int**|Идентификатор объекта (например хранимой процедуры или определяемой пользователем функции) для этого плана запроса. Для нерегламентированных и подготовленных пакетов этот столбец содержит значение **NULL**.<br /><br /> Столбец может содержать значение NULL.|  
|**number**|**smallint**|Целое число нумерованных хранимых процедур. Например, группа процедур для приложения **orders** может иметь имена вида **orderproc;1**, **orderproc;2** и так далее. Для нерегламентированных и подготовленных пакетов этот столбец содержит значение **NULL**.<br /><br /> Столбец может содержать значение NULL.|  
|**Шифрование**|**bit**|Указывает, зашифрована ли соответствующая хранимая процедура.<br /><br /> 0 = не зашифрована<br /><br /> 1 = зашифрована<br /><br /> Столбец не может содержать значение NULL.|  
|**query_plan**|**nvarchar(max)**|Содержит представление Showplan времени компиляции для плана выполнения запроса, указанного в *plan_handle*. Инструкция Showplan имеет текстовый формат. Для каждого пакета, содержащего, например нерегламентированные инструкции языка [!INCLUDE[tsql](../../includes/tsql-md.md)], вызовы хранимых процедур и вызовы определяемых пользователем функций, формируется один план.<br /><br /> Столбец может содержать значение NULL.|  
  
## <a name="remarks"></a>Комментарии  
 При следующих условиях вывод инструкции Showplan не возвращается в столбец **plan** возвращаемой таблицы для функции **sys.dm_exec_text_query_plan**.  
  
-   Если план запроса, указанный с помощью *plan_handle* , был исключен из кэша планов, то **query_plan** столбец возвращаемой таблицы имеет значение null. Например, такое условие может возникнуть при наличии задержки между принятием и использованием дескриптора плана функции **sys.dm_exec_text_query_plan**.  
  
-   Некоторые инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] не кэшируются, к ним относятся инструкции массовых операций, а также инструкции, содержащие строковые литералы размером более 8 КБ. Для таких инструкций нельзя получить инструкцию Showplan с помощью функции **sys.dm_exec_text_query_plan**, потому что они не существуют в кэше.  
  
-   Если [!INCLUDE[tsql](../../includes/tsql-md.md)] пакет или хранимая процедура содержит вызов пользовательской функции или вызов динамического SQL, например с помощью exec (*String*), скомпилированный XML Showplan для определяемой пользователем функции не включается в таблицу, возвращенную **sys.dm_exec_text_query_plan** для пакета или хранимой процедуры. Вместо этого необходимо выполнить отдельный вызов **sys.dm_exec_text_query_plan** для *plan_handle* , соответствующего определяемой пользователем функции.  
  
Если нерегламентированный запрос использует [простую](../../relational-databases/query-processing-architecture-guide.md#SimpleParam) или [принудительную параметризацию](../../relational-databases/query-processing-architecture-guide.md#ForcedParam), столбец **query_plan** будет содержать только текст инструкции, а не реальный план запроса. Чтобы вернуть план запроса, вызовите функцию **sys.dm_exec_text_query_plan** для дескриптора плана подготовленного параметризированного запроса. Можно определить параметризацию запроса посредством ссылки на столбец **sql** представления [sys.syscacheobjects](../../relational-databases/system-compatibility-views/sys-syscacheobjects-transact-sql.md) или текстовый столбец динамического административного представления [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md).  
  
## <a name="permissions"></a>Разрешения  
 Чтобы выполнить функцию **sys.dm_exec_text_query_plan**, пользователь должен быть членом предопределенной роли сервера **sysadmin** или иметь разрешение VIEW SERVER STATE на сервере.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-retrieving-the-cached-query-plan-for-a-slow-running-transact-sql-query-or-batch"></a>A. Получение кэшированного плана запроса для медленно выполняемого запроса или пакета Transact-SQL  
 Если запрос или пакет [!INCLUDE[tsql](../../includes/tsql-md.md)] выполняется длительное время при определенном соединении с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], то для определения причины задержки необходимо получить план выполнения для этого запроса или пакета. В следующем примере показано, как получить инструкцию Showplan для медленно выполняемого запроса или пакета.  
  
> [!NOTE]  
> Чтобы выполнить этот пример, замените значения *session_id* и *plan_handle* значениями, характерными для сервера.  
  
 Сначала получите идентификатор серверного процесса (SPID) для процесса, выполняющего запрос или пакет, при помощи хранимой процедуры `sp_who`:  
  
```sql  
USE master;  
GO  
EXEC sp_who;  
GO  
```  
  
 Результирующий набор, возвращаемый процедурой `sp_who`, показывает, что идентификатор SPID равен `54`. Идентификатор SPID можно использовать с динамическим административным представлением `sys.dm_exec_requests` для получения дескриптора плана при помощи следующего запроса:  
  
```sql  
USE master;  
GO  
SELECT * FROM sys.dm_exec_requests  
WHERE session_id = 54;  
GO  
```  
  
 Таблица, возвращаемая представлением **sys.dm_exec_requests** указывает, что дескриптором плана для медленно выполняемого запроса или пакета является `0x06000100A27E7C1FA821B10600`. Следующий пример возвращает план запроса для указанного дескриптора плана и использует значения по умолчанию 0 и -1 для возвращения всех инструкций в запросе или пакете.  
  
```sql  
USE master;  
GO  
SELECT query_plan   
FROM sys.dm_exec_text_query_plan (0x06000100A27E7C1FA821B10600,0,-1);  
GO  
```  
  
### <a name="b-retrieving-every-query-plan-from-the-plan-cache"></a>Б. Получение плана каждого запроса из кэша планов  
 Чтобы получить моментальный снимок всех планов запроса, хранимых в кэше планов, необходимо получить дескрипторы планов для всех запросов, хранящихся в кэше, запросив динамическое административное представление `sys.dm_exec_cached_plans`. Дескрипторы планов хранятся в столбце `plan_handle` представления `sys.dm_exec_cached_plans`. Затем воспользуйтесь оператором CROSS APPLY для передачи дескрипторов плана в функцию `sys.dm_exec_text_query_plan`, как показано ниже. Выходные данные инструкции Showplan для каждого плана, находящегося в кэше планов, находятся в `query_plan` столбце возвращаемой таблицы.  
  
```sql  
USE master;  
GO  
SELECT *   
FROM sys.dm_exec_cached_plans AS cp   
CROSS APPLY sys.dm_exec_text_query_plan(cp.plan_handle, DEFAULT, DEFAULT);  
GO  
```  
  
### <a name="c-retrieving-every-query-plan-for-which-the-server-has-gathered-query-statistics-from-the-plan-cache"></a>В. Получение всех планов запросов, для которых сервер собирал статистику запросов, из кэша планов  
 Чтобы получить моментальный снимок всех планов запроса, для которых сервером была собрана статистика и которые в настоящий момент находятся в кэше планов, необходимо получить дескрипторы планов в кэше, запросив динамическое административное представление `sys.dm_exec_query_stats`. Дескрипторы планов хранятся в столбце `plan_handle` представления `sys.dm_exec_query_stats`. Затем воспользуйтесь оператором CROSS APPLY для передачи дескрипторов плана в функцию `sys.dm_exec_text_query_plan`, как показано ниже. Вывод инструкции Showplan для каждого плана находится в столбце `query_plan` возвращаемой таблицы.  
  
```sql  
USE master;  
GO  
SELECT * FROM sys.dm_exec_query_stats AS qs   
CROSS APPLY sys.dm_exec_text_query_plan(qs.plan_handle, qs.statement_start_offset, qs.statement_end_offset);  
GO  
```  
  
### <a name="d-retrieving-information-about-the-top-five-queries-by-average-cpu-time"></a>Г. Получение сведений о первых пяти запросах по среднему времени ЦП  
 Следующий пример возвращает планы запросов и среднее время ЦП для пяти первых запросов. Функция **sys.dm_exec_text_query_plan** указывает значения по умолчанию 0 и -1 для возврата всех инструкций пакета в плане запроса.  
  
```sql  
SELECT TOP 5 total_worker_time/execution_count AS [Avg CPU Time],  
Plan_handle, query_plan   
FROM sys.dm_exec_query_stats AS qs  
CROSS APPLY sys.dm_exec_text_query_plan(qs.plan_handle, 0, -1)  
ORDER BY total_worker_time/execution_count DESC;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [sys.dm_exec_query_plan &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)  
