---
description: sys.dm_exec_query_statistics_xml (Transact-SQL)
title: sys.dm_exec_query_statistics_xml (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 11/16/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sys.dm_exec_query_statistics_xml
- sys.dm_exec_query_statistics_xml_TSQL
- dm_exec_query_statistics_xml_TSQL
- dm_exec_query_statistics_xml
helpviewer_keywords:
- sys.dm_exec_query_statistics_xml management view
ms.assetid: fdc7659e-df41-488e-b2b5-0d79734dfecb
author: pmasl
ms.author: pelopes
ms.openlocfilehash: 9bfce6f56bdf39597def73884f12ca06f6eb56b7
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170276"
---
# <a name="sysdm_exec_query_statistics_xml-transact-sql"></a>sys.dm_exec_query_statistics_xml (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Возвращает план выполнения запроса для запросов в реальном режиме. Используйте это динамическое административное представление для получения XML-документа Showplan с временной статистикой. 

## <a name="syntax"></a>Синтаксис

```
sys.dm_exec_query_statistics_xml(session_id)  
``` 

## <a name="arguments"></a>Аргументы 
*session_id*  
 Идентификатор сеанса, в котором будет выполняться поиск пакета. *session_id* имеет **smallint**. *session_id* можно получить из следующих объектов DMO:  
  
-   [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
  
-   [sys.dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)  
  
-   [sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/sys-dm-exec-connections-transact-sql.md)  

## <a name="table-returned"></a>Возвращаемая таблица

|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|
|session_id|**smallint**|Идентификатор сеанса. Не допускает значения NULL.|
|request_id|**int**|Идентификатор запроса. Не допускает значения NULL.|
|sql_handle|**varbinary (64)**|Токен, однозначно определяющий пакет или хранимую процедуру, частью которой является запрос. Допускает значение NULL.|
|plan_handle|**varbinary (64)**|Токен, однозначно определяющий план выполнения запроса для выполняемого в данный момент пакета. Допускает значение NULL.|
|query_plan|**xml**|Содержит представление среды выполнения Showplan плана выполнения запроса, указанного в *plan_handle* содержащего частичную статистику. Представление Showplan имеет формат XML. Для каждого пакета, содержащего, например нерегламентированные инструкции языка [!INCLUDE[tsql](../../includes/tsql-md.md)], вызовы хранимых процедур и вызовы определяемых пользователем функций, формируется один план. Допускает значение NULL.|

## <a name="remarks"></a>Примечания
Эта системная функция доступна начиная с [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] версии с пакетом обновления 1 (SP1). См. статью KB [3190871](https://support.microsoft.com/help/3190871)

Эта системная функция работает как в **стандартной** и **упрощенной** инфраструктуре профилирования статистики выполнения запросов. Дополнительные сведения см. в разделе [Инфраструктура профилирования запросов](../../relational-databases/performance/query-profiling-infrastructure.md).  

В следующих случаях выходные данные Showplan не возвращаются в столбце **query_plan** возвращаемой таблицы для **sys.dm_exec_query_statistics_xml**:  
  
-   Если план запроса, соответствующий указанному *session_id* , больше не исполняется, **query_plan** столбец возвращаемой таблицы имеет значение null. Например, это условие может возникать, если задержка между моментом записи обработчика плана и ее использованием с **sys.dm_exec_query_statistics_xml**.  
    
Из-за ограничения количества вложенных уровней, разрешенных в типе данных **XML** , **sys.dm_exec_query_statistics_xml** не может возвращать планы запросов, которые соответствуют или превышают 128 уровней вложенных элементов. В более ранних версиях [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] это условие предназначалось для предотвращения возврата плана запроса и формирования ошибки 6335. В [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] пакетах обновления 2 (SP2) и более поздних версиях столбец **query_plan** возвращает значение null.   

## <a name="permissions"></a>Разрешения  
В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение на сервере.  
На [!INCLUDE[ssSDS](../../includes/sssds-md.md)] уровнях Premium требуется `VIEW DATABASE STATE` разрешение в базе данных. На [!INCLUDE[ssSDS](../../includes/sssds-md.md)] уровнях Standard и Basic требуется **Администратор сервера** или учетная запись **администратора Azure Active Directory** .

## <a name="examples"></a>Примеры  
  
### <a name="a-looking-at-live-query-plan-and-execution-statistics-for-a-running-batch"></a>A. Просмотр динамического плана запроса и статистики выполнения для выполняемого пакета  
 В следующем примере запрос **sys.dm_exec_requests** для поиска интересного запроса и копирования его `session_id` из выходных данных.  
  
```sql  
SELECT * FROM sys.dm_exec_requests;  
GO  
```  
  
 Затем, чтобы получить динамический план запроса и статистику выполнения, используйте скопированную `session_id` с помощью системной функции **sys.dm_exec_query_statistics_xml**.  
  
```sql  
--Run this in a different session than the session in which your query is running.
SELECT * FROM sys.dm_exec_query_statistics_xml(< copied session_id >);  
GO  
```   

 Или в сочетании для всех выполняющихся запросов.  
  
```sql  
--Run this in a different session than the session in which your query is running.
SELECT * FROM sys.dm_exec_requests
CROSS APPLY sys.dm_exec_query_statistics_xml(session_id);  
GO  
```   
  
## <a name="see-also"></a>См. также:
  [Флаги трассировки](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)  
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Динамические административные представления, связанные с базами данных &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  

