---
description: sys.dm_exec_query_plan (Transact-SQL)
title: sys.dm_exec_query_plan (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_exec_query_plan_TSQL
- sys.dm_exec_query_plan
- dm_exec_query_plan
- sys.dm_exec_query_plan_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_plan dynamic management function
ms.assetid: e26f0867-9be3-4b2e-969e-7f2840230770
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 18d6e4e8e3c6fe097676d1bf65f337610127566a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094130"
---
# <a name="sysdm_exec_query_plan-transact-sql"></a>sys.dm_exec_query_plan (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Возвращает события инструкции Showplan в XML-формате для пакета, указанного в дескрипторе плана. План, указанный в дескрипторе плана может быть кэширован или выполняться в данный момент.  
  
Схема XML для инструкции Showplan опубликована и доступна на [веб-сайте Майкрософт](https://go.microsoft.com/fwlink/?linkid=43100&clcid=0x409). Эта схема также доступна в папке установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
sys.dm_exec_query_plan(plan_handle)  
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
  
## <a name="table-returned"></a>Возвращаемая таблица  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**DBID**|**smallint**|Идентификатор базы данных, в контексте которой выполнялась компиляция инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)], соответствующей данному плану. Для нерегламентированных и подготовленных инструкций SQL это идентификатор базы данных, в которой происходила компиляция инструкции.<br /><br /> Столбец может содержать значение NULL.|  
|**objectid**|**int**|Идентификатор объекта (например хранимой процедуры или определяемой пользователем функции) для этого плана запроса. Для нерегламентированных и подготовленных пакетов этот столбец содержит значение **NULL**.<br /><br /> Столбец может содержать значение NULL.|  
|**number**|**smallint**|Целое число нумерованных хранимых процедур. Например, группа процедур для приложения **orders** может иметь имена вида **orderproc;1**, **orderproc;2** и так далее. Для нерегламентированных и подготовленных пакетов этот столбец содержит значение **NULL**.<br /><br /> Столбец может содержать значение NULL.|  
|**Шифрование**|**bit**|Указывает, зашифрована ли соответствующая хранимая процедура.<br /><br /> 0 = не зашифрована<br /><br /> 1 = зашифрована<br /><br /> Столбец не может содержать значение NULL.|  
|**query_plan**|**xml**|Содержит представление Showplan времени компиляции для плана выполнения запроса, указанного в *plan_handle*. Представление Showplan имеет формат XML. Для каждого пакета, содержащего, например нерегламентированные инструкции языка [!INCLUDE[tsql](../../includes/tsql-md.md)], вызовы хранимых процедур и вызовы определяемых пользователем функций, формируется один план.<br /><br /> Столбец может содержать значение NULL.|  
  
## <a name="remarks"></a>Комментарии  
 При следующих условиях вывод инструкции Showplan не возвращается в столбец **query_plan** возвращаемой таблицы для функции **sys.dm_exec_query_plan**.  
  
-   Если план запроса, указанный с помощью *plan_handle* , был исключен из кэша планов, то **query_plan** столбец возвращаемой таблицы имеет значение null. Например, такое условие может возникнуть при наличии задержки между принятием и использованием дескриптора плана функцией **sys.dm_exec_query_plan**.  
  
-   Некоторые инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] не кэшируются, к ним относятся инструкции массовых операций, а также инструкции, содержащие строковые литералы размером более 8 КБ. Для таких инструкций нельзя получить представление Showplan в формате XML, используя функцию **sys.dm_exec_query_plan**, если пакет не выполняется в данный момент, потому что они не существуют в кэше.  
  
-   Если [!INCLUDE[tsql](../../includes/tsql-md.md)] пакет или хранимая процедура содержит вызов пользовательской функции или вызов динамического SQL, например с помощью exec (*String*), скомпилированный XML Showplan для определяемой пользователем функции не включается в таблицу, возвращенную **sys.dm_exec_query_plan** для пакета или хранимой процедуры. Вместо этого необходимо выполнить отдельный вызов **sys.dm_exec_query_plan** для маркера плана, соответствующего определяемой пользователем функции.  
  
 Если нерегламентированный запрос использует простую или принудительную параметризацию, столбец **query_plan** будет содержать только текст инструкции, а не фактический план запроса. Чтобы вернуть план запроса, вызовите функцию **sys.dm_exec_query_plan** для дескриптора плана подготовленного параметризированного запроса. Можно определить параметризацию запроса посредством ссылки на столбец **sql** представления [sys.syscacheobjects](../../relational-databases/system-compatibility-views/sys-syscacheobjects-transact-sql.md) или текстовый столбец динамического административного представления [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md).  
  
> [!NOTE] 
> Из-за ограничения количества вложенных уровней, разрешенных в типе данных **XML** , **sys.dm_exec_query_plan** не может возвращать планы запросов, которые соответствуют или превышают 128 уровней вложенных элементов. В более ранних версиях [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] это условие предназначалось для предотвращения возврата плана запроса и формирования ошибки 6335. В [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] пакетах обновления 2 (SP2) и более поздних версиях столбец **query_plan** возвращает значение null.   
> Для возврата выходных данных плана запроса в текстовом формате можно использовать функцию динамического управления [sys.dm_exec_text_query_plan &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-text-query-plan-transact-sql.md) .  
  
## <a name="permissions"></a>Разрешения  
 Для выполнения **sys.dm_exec_query_plan** пользователь должен быть членом предопределенной роли сервера **sysadmin** или иметь `VIEW SERVER STATE` разрешение на сервере.  
  
## <a name="examples"></a>Примеры  
 В следующих примерах показано использование динамического административного представления **sys.dm_exec_query_plan**.  
  
 Чтобы просмотреть представление Showplan в формате XML, необходимо выполнить следующие запросы в редакторе запросов среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], а затем щелкнуть элемент **ShowPlanXML** в столбце **query_plan** таблицы, возвращаемой функцией **sys.dm_exec_query_plan**. Представление Showplan в формате XML отображается на сводной панели среды [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Чтобы сохранить XML Showplan в файл, щелкните правой кнопкой мыши **ShowPlanXML** в столбце **query_plan** , выберите команду **сохранить результаты как**, назовите файл в формате \<*file_name*> . sqlplan, например миксмлшовплан. sqlplan.  
  
### <a name="a-retrieve-the-cached-query-plan-for-a-slow-running-transact-sql-query-or-batch"></a>A. Получение кэшированного плана запроса для медленно выполняемого запроса или пакета Transact-SQL  
 Планы запросов для различных типов пакетов [!INCLUDE[tsql](../../includes/tsql-md.md)], в том числе нерегламентированных пакетов, хранимых процедур и определяемых пользователем функций, кэшируются в области памяти, называемой кэшем планов. Каждый кэшированный план запроса идентифицируется при помощи уникального идентификатора, дескриптора плана. Чтобы получить план выполнения для определенного запроса или пакета [!INCLUDE[tsql](../../includes/tsql-md.md)], можно указать дескриптор плана при помощи динамического административного представления **sys.dm_exec_query_plan**.  
  
 Если запрос или пакет [!INCLUDE[tsql](../../includes/tsql-md.md)] выполняется длительное время при определенном соединении с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], то для определения причины задержки необходимо получить план выполнения для этого запроса или пакета. В следующем примере показано, как получить представление Showplan в формате XML для медленно выполняемого запроса или пакета.  
  
> [!NOTE]  
>  Чтобы выполнить этот пример, замените значения *session_id* и *plan_handle* значениями, характерными для сервера.  
  
 Сначала получите идентификатор серверного процесса (SPID) для процесса, выполняющего запрос или пакет, при помощи хранимой процедуры `sp_who`:  
  
```sql  
USE master;  
GO  
exec sp_who;  
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
  
 Таблица, возвращаемая **sys.dm_exec_requests** указывает, что обработчик плана для замедляют выполнение запроса или пакета имеет значение `0x06000100A27E7C1FA821B10600` , которое можно указать в качестве аргумента *plan_handle* с `sys.dm_exec_query_plan` целью получения плана выполнения в формате XML, как показано ниже. План выполнения в формате XML для медленно выполняемых запросов или пакетов содержится в столбце **query_plan** таблицы, возвращаемой функцией `sys.dm_exec_query_plan`.  
  
```sql  
USE master;  
GO  
SELECT * 
FROM sys.dm_exec_query_plan (0x06000100A27E7C1FA821B10600);  
GO  
```  
  
### <a name="b-retrieve-every-query-plan-from-the-plan-cache"></a>Б. Получение плана каждого запроса из кэша планов  
 Чтобы получить моментальный снимок всех планов запроса, хранимых в кэше планов, необходимо получить дескрипторы планов для всех запросов, хранящихся в кэше, запросив динамическое административное представление `sys.dm_exec_cached_plans`. Дескрипторы планов хранятся в столбце `plan_handle` представления `sys.dm_exec_cached_plans`. Затем воспользуйтесь оператором CROSS APPLY для передачи дескрипторов плана в функцию `sys.dm_exec_query_plan`, как показано ниже. Вывод инструкции Showplan в формате XML для каждого плана, находящегося в кэше планов, находится в столбце `query_plan` возвращаемой таблицы.  
  
```sql  
USE master;  
GO  
SELECT * 
FROM sys.dm_exec_cached_plans AS cp 
CROSS APPLY sys.dm_exec_query_plan(cp.plan_handle);  
GO  
```  
  
### <a name="c-retrieve-every-query-plan-for-which-the-server-has-gathered-query-statistics-from-the-plan-cache"></a>В. Получение всех планов запросов, для которых сервер собрал статистику запросов из кэша планов  
 Чтобы получить моментальный снимок всех планов запроса, для которых сервером была собрана статистика и которые в настоящий момент находятся в кэше планов, необходимо получить дескрипторы планов в кэше, запросив динамическое административное представление `sys.dm_exec_query_stats`. Дескрипторы планов хранятся в столбце `plan_handle` представления `sys.dm_exec_query_stats`. Затем воспользуйтесь оператором CROSS APPLY для передачи дескрипторов плана в функцию `sys.dm_exec_query_plan`, как показано ниже. Вывод инструкции Showplan в формате XML для каждого плана, который находится в кэше планов и для которого сервер собирал статистику, находится в столбце `query_plan` возвращаемой таблицы.  
  
```sql  
USE master;  
GO  
SELECT * 
FROM sys.dm_exec_query_stats AS qs 
CROSS APPLY sys.dm_exec_query_plan(qs.plan_handle);  
GO  
```  
  
### <a name="d-retrieve-information-about-the-top-five-queries-by-average-cpu-time"></a>Г. Получение сведений о первых пяти запросах по среднему времени ЦП  
 Следующий пример возвращает планы и среднее время ЦП для пяти первых запросов.  
  
```sql  
SELECT TOP 5 total_worker_time/execution_count AS [Avg CPU Time],  
   plan_handle, query_plan   
FROM sys.dm_exec_query_stats AS qs  
CROSS APPLY sys.dm_exec_query_plan(qs.plan_handle)  
ORDER BY total_worker_time/execution_count DESC;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [sys.dm_exec_cached_plans (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)   
 [sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
 [sys.dm_exec_requests (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [sp_who (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)   
 [Справочник по логическим и физическим операторам Showplan](../../relational-databases/showplan-logical-and-physical-operators-reference.md)   
 [sys.dm_exec_text_query_plan &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-text-query-plan-transact-sql.md)  
  
  
