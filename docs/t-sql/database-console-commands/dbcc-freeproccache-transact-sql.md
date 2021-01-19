---
description: DBCC FREEPROCCACHE (Transact-SQL)
title: DBCC FREEPROCCACHE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 10/13/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FREEPROCCACHE_TSQL
- FREEPROCCACHE
- DBCC_FREEPROCCACHE_TSQL
- DBCC FREEPROCCACHE
dev_langs:
- TSQL
helpviewer_keywords:
- freeing procedure cache
- removing procedure cache elements
- deleting procedure cache elements
- DBCC FREEPROCCACHE statement
- procedure cache [SQL Server]
- clearing procedure cache
ms.assetid: 0e09d210-6f23-4129-aedb-3d56b2980683
author: pmasl
ms.author: umajay
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 635cbfe8b21f60bf2bb9cec49efac62c627a6ae4
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172626"
---
# <a name="dbcc-freeproccache-transact-sql"></a>DBCC FREEPROCCACHE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Удаляет все элементы из кэша планов, удаляет заданный план из кэша планов с помощью указания дескриптора плана или дескриптора SQL либо удаляет все записи кэша, связанные с указанным пулом ресурсов.

> [!NOTE]
> DBCC FREEPROCCACHE не очищает статистику выполнения для хранимых процедур, скомпилированных в собственном коде. Кэш процедур не содержит сведения о хранимых процедурах, скомпилированных в собственном коде. Все статистические данные выполнения, полученные при выполнении процедур, появятся в динамических административных представлениях (DMV) статистики выполнения: [sys.dm_exec_procedure_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md) и [sys.dm_exec_query_plan (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md).  
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
Синтаксис для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:

```sql
DBCC FREEPROCCACHE [ ( { plan_handle | sql_handle | pool_name } ) ] [ WITH NO_INFOMSGS ]  
```  

Синтаксис для [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] и :[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]
  
```sql
DBCC FREEPROCCACHE [ ( COMPUTE | ALL ) ] 
     [ WITH NO_INFOMSGS ]   
[;]  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 ( { *plan_handle* | *sql_handle* | *pool_name* } )  
*plan_handle* уникально идентифицирует план запроса для запущенного пакета, план которого хранится в кэше планов. Аргумент *plan_handle* имеет тип **varbinary(64)**, и его можно получить из следующих объектов DMO:  
 -   [sys.dm_exec_cached_plans](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)  
 -   [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
 -   [sys.dm_exec_query_memory_grants](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)  
 -   [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  

*sql_handle* представляет дескриптор SQL очищаемого пакета. Аргумент *sql_handle* имеет тип **varbinary(64)**, и его можно получить из следующих объектов DMO:  
 -   [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
 -   [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
 -   [sys.dm_exec_cursors](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cursors-transact-sql.md)  
 -   [sys.dm_exec_xml_handles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-xml-handles-transact-sql.md)  
 -   [sys.dm_exec_query_memory_grants](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)  

*pool_name* представляет имя пула ресурсов Resource Governor. Аргумент *pool_name* имеет тип **sysname** и может быть получен с помощью запроса к динамическому административному представлению [sys.dm_resource_governor_resource_pools](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md).  
 Чтобы связать группу рабочей нагрузки Resource Governor с пулом ресурсов, запросите динамическое административное представление [sys.dm_resource_governor_workload_groups](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md). Чтобы получить сведения о группе рабочей нагрузки для сеанса, запросите динамическое административное представление [sys.dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md).  

  
 WITH NO_INFOMSGS  
 Подавляет вывод всех информационных сообщений.  
  
 COMPUTE  
 Очистка кэша планов запросов в каждом вычислительном узле. Это значение по умолчанию.  
  
 ALL  
 Очистка кэша планов запросов в каждом вычислительном узле и в управляющем узле.  

> [!NOTE]
> Начиная с версии [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], для очистки кэша процедур (планов) для базы данных в области действия служит инструкция `ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE`.

## <a name="remarks"></a>Комментарии  
Инструкция DBCC FREEPROCCACHE используется для аккуратной очистки кэша планов. Очистка кэша процедур (планов) приводит к исключению всех планов. В результате при выполнении входящих запросов будет компилироваться новый план, а не использоваться существующий план из кэша. 

Это может стать причиной внезапного временного снижения производительности обработки запросов из-за увеличения числа компиляций. Для каждого очищенного хранилища кэша в кэше планов журнал ошибок [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] содержит следующее информационное сообщение:

> `SQL Server has encountered %d occurrence(s) of cachestore flush for the '%s' cachestore (part of plan cache) due to 'DBCC FREEPROCCACHE' or 'DBCC FREESYSTEMCACHE' operations.` 

Это сообщение добавляется в журнал каждые пять минут при сбросе кэша в течение этого интервала времени.

Следующие операции по перенастройке также очищают кэш процедур:  
-   доступ к счетчику контейнеров проверки кэша  
-   доступ к квоте кэша проверки  
-   clr enabled  
-   стоимостный порог для параллелизма  
-   cross db ownership chaining  
-   память для создания индекса  
-   максимальная степень параллелизма  
-   max server memory  
-   max text repl size  
-   максимальное количество рабочих потоков  
-   min memory per query  
-   min server memory  
-   ограничение стоимости регулятора запросов  
-   ожидание запроса  
-   remote query timeout  
-   user options  
  
## <a name="result-sets"></a>Результирующие наборы  
Если предложение WITH NO_INFOMSGS не указано, инструкция DBCC FREEPROCCACHE возвращает: "Выполнение DBCC завершено. Если инструкция DBCC выдает сообщения об ошибках, обратитесь к системному администратору».
  
## <a name="permissions"></a>Разрешения  
Применимо к: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 
- Требует разрешения ALTER SERVER STATE на сервере.  

Применимо к: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]
- Необходимо членство в предопределенной роли сервера DB_OWNER.  

## <a name="general-remarks-for-sssdw-and-sspdw"></a>Общие замечания касательно [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
Несколько команд DBCC FREEPROCCACHE могут выполняться одновременно.
В [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] очистка кэша планов может приводить к временному снижению производительности обработки запросов, так как для входящих запросов компилируется новый план, а не используется существующий план из кэша. 

Команда DBCC FREEPROCCACHE (COMPUTE) приводит к перекомпиляции запросов сервером [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] только в том случае, если они выполняются в вычислительных узлах. Она не приводит к тому, что [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] перекомпилируют план параллельных запросов, созданный в управляющем узле.
Команду DBCC FREEPROCCACHE можно отменить во время выполнения.
  
## <a name="limitations-and-restrictions-for-sssdw-and-sspdw"></a>Ограничения для [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
Команда DBCC FREEPROCCACHE не может выполняться в рамках транзакции.
Команда DBCC FREEPROCCACHE не поддерживается в инструкции EXPLAIN.
  
## <a name="metadata-for-sssdw-and-sspdw"></a>Метаданные для [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
При выполнении команды DBCC FREEPROCCACHE в системное представление sys.pdw_exec_requests добавляется новая строка.

## <a name="examples-ssnoversion"></a>Примеры: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
### <a name="a-clearing-a-query-plan-from-the-plan-cache"></a>A. Очистка плана запроса из кэша планов  
В следующем примере план запроса очищается из кэша планов путем указания дескриптора плана запроса. Чтобы обеспечить наличие запроса-образца в кэше планов, сначала выполните следующий запрос. Динамические административные представления `sys.dm_exec_cached_plans` и `sys.dm_exec_sql_text` запрашиваются для возврата дескриптора плана соответствующего запроса. 

Затем значение дескриптора плана из результирующего набора вставляется в инструкцию `DBCC FREEPROCACHE` для удаления из кэша планов именно этого плана.
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT * FROM Person.Address;  
GO  
SELECT plan_handle, st.text  
FROM sys.dm_exec_cached_plans   
CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS st  
WHERE text LIKE N'SELECT * FROM Person.Address%';  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
plan_handle                                         text  
--------------------------------------------------  -----------------------------  
0x060006001ECA270EC0215D05000000000000000000000000  SELECT * FROM Person.Address;  
  
(1 row(s) affected)
 ```
 
```sql  
-- Remove the specific plan from the cache.  
DBCC FREEPROCCACHE (0x060006001ECA270EC0215D05000000000000000000000000);  
GO  
```  
  
### <a name="b-clearing-all-plans-from-the-plan-cache"></a>Б. Очистка всех планов из кэша планов  
В следующем примере из кэша планов удаляются все элементы. Предложение WITH `NO_INFOMSGS` указывается, чтобы избежать отображения информационного сообщения.
  
```sql  
DBCC FREEPROCCACHE WITH NO_INFOMSGS;  
```  
  
### <a name="c-clearing-all-cache-entries-associated-with-a-resource-pool"></a>В. Очистка всех записей кэша, связанных с пулом ресурсов  
В следующем примере очищаются все записи кэша, связанные с указанным пулом ресурсов. Сначала запрашивается представление `sys.dm_resource_governor_resource_pools` для получения значения аргумента *pool_name*.
  
```sql  
SELECT * FROM sys.dm_resource_governor_resource_pools;  
GO  
DBCC FREEPROCCACHE ('default');  
GO  
```  
  
## <a name="examples-sssdw-and-sspdw"></a>Примеры: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-dbcc-freeproccache-basic-syntax-examples"></a>Г. Примеры базового синтаксиса DBCC FREEPROCCACHE  
В приведенном ниже примере в вычислительных узлах удаляются все существующие кэши планов запросов. Хотя задан контекст UserDbSales, кэши планов запросов в вычислительных узлах будут удалены для всех баз данных. Предложение WITH NO_INFOMSGS блокирует появление информационных сообщений в результатах.  
  
```sql
USE UserDbSales;  
DBCC FREEPROCCACHE (COMPUTE) WITH NO_INFOMSGS;
```  
  
 В следующем примере результаты будут теми же, что и в предыдущем, за исключением того, что в них будут приводиться информационные сообщения.  
  
```sql
USE UserDbSales;  
DBCC FREEPROCCACHE (COMPUTE);  
```  
  
Если информационные сообщения запрошены и выполнение завершилось успешно, результаты запроса будут содержать одну строку для каждого вычислительного узла.
  
### <a name="e-granting-permission-to-run-dbcc-freeproccache"></a>Д. Предоставление разрешения на выполнение DBCC FREEPROCCACHE  
В приведенном ниже примере имени для входа David предоставляется разрешение на выполнение DBCC FREEPROCCACHE.  
  
```sql
GRANT ALTER SERVER STATE TO David; 
GO
```  
  
## <a name="see-also"></a>См. также:  
[DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[Регулятор ресурсов](../../relational-databases/resource-governor/resource-governor.md)  
[ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)
  
  
