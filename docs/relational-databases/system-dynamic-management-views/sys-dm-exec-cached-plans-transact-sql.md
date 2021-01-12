---
description: sys.dm_exec_cached_plans (Transact-SQL)
title: sys.dm_exec_cached_plans (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_exec_cached_plans
- dm_exec_cached_plans
- dm_exec_cached_plans_TSQL
- sys.dm_exec_cached_plans_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_cached_plans dynamic management view
ms.assetid: 95b707d3-3a93-407f-8e88-4515d4f2039d
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 46e65114623c1c1c8744643e62cf6f355f5f0475
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092877"
---
# <a name="sysdm_exec_cached_plans-transact-sql"></a>sys.dm_exec_cached_plans (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает строку для каждого плана запроса, кэшируемого [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для более быстрого выполнения запросов. Можно использовать динамическое административное представление для поиска кэшированных планов запросов, кэшированного текста запросов, объема памяти, занимаемого кэшированными планами и счетчика повторного использования кэшированных планов.  
  
 Динамические административные представления в среде [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] не могут предоставлять информацию, которая может повлиять на автономность базы данных, или информацию о других базах данных, к которым имеет доступ пользователь. Во избежание раскрытия этой информации все строки, содержащие данные, не принадлежащие подключенному клиенту, отфильтровываются. Кроме того, значения в столбцах **memory_object_address** и **pool_id** фильтруются. значение столбца установлено в NULL.  
  
> [!NOTE]  
>  Чтобы вызвать эту функцию из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте имя **sys.dm_pdw_nodes_exec_cached_plans**.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|bucketid|**int**|Идентификатор сегмента хэша, в который кэшируется запись. Значение указывает диапазон от 0 до значения размера хэш-таблицы для типа кэша.<br /><br /> Для кэшей SQL Plans и Object Plans размер хэш-таблицы может достигать 10007 на 32-разрядных версиях систем и 40009 — на 64-разрядных. Для кэша Bound Trees размер хэш-таблицы может достигать 1009 на 32-разрядных версиях систем и 4001 на 64-разрядных. Для кэша расширенных хранимых процедур размер хэш-таблицы может достигать 127 на 32-разрядных и 64-разрядных версиях систем.|  
|refcounts|**int**|Число объектов кэша, ссылающихся на данный объект кэша. Значение **refcounts** для записи должно быть не меньше 1, чтобы размещаться в кэше.|  
|usecounts|**int**|Количество повторений поиска объекта кэша. Остается без увеличения, если параметризованные запросы обнаруживают план в кэше. Может быть увеличен несколько раз при использовании инструкции showplan.|  
|size_in_bytes|**int**|Число байтов, занимаемых объектом кэша.|  
|memory_object_address|**varbinary(8)**|Адрес памяти кэшированной записи. Это значение можно использовать с представлением [sys.dm_os_memory_objects](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql.md), чтобы проанализировать распределение памяти кэшированного плана, и с представлением [sys.dm_os_memory_cache_entries](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-cache-entries-transact-sql.md) для определения затрат на кэширование записи.|  
|cacheobjtype|**nvarchar (34)**|Тип объекта в кэше. Он может иметь одно из следующих значений:<br /><br /> Compiled Plan (скомпилированный план)<br /><br /> Compiled Plan Stub (заглушка скомпилированного плана)<br /><br /> Parse Tree (дерево синтаксического анализа)<br /><br /> Extended Proc (расширенные процедуры)<br /><br /> CLR Compiled Func (скомпилированная функция CLR)<br /><br /> CLR Compiled Proc (скомпилированная процедура CLR)|  
|objtype|**nvarchar (16)**|Тип объекта. Ниже приведены возможные значения и соответствующие им описания.<br /><br /> Proc: хранимая процедура<br />Подготовлено: подготовленная инструкция<br />Прямое соединение: нерегламентированный запрос. Относится к [!INCLUDE[tsql](../../includes/tsql-md.md)] отправке как события языка с помощью программы **osql** или **sqlcmd** , а не как удаленных вызовов процедур.<br />Реплпрок: процедура-фильтр репликации<br />Триггер: триггер<br />Представление: Просмотр<br />По умолчанию: по умолчанию<br />Усртаб: пользовательская таблица<br />Систаб: системная таблица<br />Проверка: ПРОВЕРОЧное ограничение<br />Правило: правило|  
|plan_handle|**varbinary (64)**|Идентификатор плана в оперативной памяти. Этот идентификатор является временным и константным, только пока план сохраняется в кэше. Это значение можно использовать со следующими функциями динамического управления:<br /><br /> [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)<br /><br /> [sys.dm_exec_query_plan](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)<br /><br /> [sys.dm_exec_plan_attributes](../../relational-databases/system-dynamic-management-views/sys-dm-exec-plan-attributes-transact-sql.md)|  
|pool_id|**int**|Идентификатор пула ресурсов, для которого подсчитывается использование памяти для плана.|  
|pdw_node_id|**int**|**Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение.|  
  
 <sup>1</sup>  
  
## <a name="permissions"></a>Разрешения

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах `Server admin` `Azure Active Directory admin` требуется учетная запись или. Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   

## <a name="examples"></a>Примеры  
  
### <a name="a-returning-the-batch-text-of-cached-entries-that-are-reused"></a>A. Возвращение текста пакета повторно используемых кэшированных записей  
 Следующий пример возвращает SQL-текст всех кэшированных записей, использованных более одного раза.  
  
```sql  
SELECT usecounts, cacheobjtype, objtype, text   
FROM sys.dm_exec_cached_plans   
CROSS APPLY sys.dm_exec_sql_text(plan_handle)   
WHERE usecounts > 1   
ORDER BY usecounts DESC;  
GO  
```  
  
### <a name="b-returning-query-plans-for-all-cached-triggers"></a>Б. Возвращение планов запросов для всех кэшированных триггеров  
 Следующий пример возвращает планы запросов для кэшированных триггеров.  
  
```sql  
SELECT plan_handle, query_plan, objtype   
FROM sys.dm_exec_cached_plans   
CROSS APPLY sys.dm_exec_query_plan(plan_handle)   
WHERE objtype ='Trigger';  
GO  
```  
  
### <a name="c-returning-the-set-options-with-which-the-plan-was-compiled"></a>В. Возвращение параметров SET, с которыми был скомпилирован план  
 Следующий пример возвращает параметры SET, с использованием которых был скомпилирован план. `sql_handle`Для плана также возвращается. Оператор PIVOT используется для вывода `set_options` атрибутов и в `sql_handle` виде столбцов, а не строк. Дополнительные сведения о значении, возвращаемом в `set_options` , см. в разделе [sys.dm_exec_plan_attributes &#40;TRANSACT-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-plan-attributes-transact-sql.md).  
  
```sql  
SELECT plan_handle, pvt.set_options, pvt.sql_handle  
FROM (  
      SELECT plan_handle, epa.attribute, epa.value   
      FROM sys.dm_exec_cached_plans   
      OUTER APPLY sys.dm_exec_plan_attributes(plan_handle) AS epa  
      WHERE cacheobjtype = 'Compiled Plan'  
      ) AS ecpa   
PIVOT (MAX(ecpa.value) FOR ecpa.attribute IN ("set_options", "sql_handle")) AS pvt;  
GO  
```  
  
### <a name="d-returning-the-memory-breakdown-of-all-cached-compiled-plans"></a>Г. Возвращение распределения памяти всех кэшированных скомпилированных планов  
 Следующий пример возвращает распределение памяти, используемой всеми скомпилированными планами в кэше.  
  
```sql  
SELECT plan_handle, ecp.memory_object_address AS CompiledPlan_MemoryObject,   
    omo.memory_object_address, type, page_size_in_bytes   
FROM sys.dm_exec_cached_plans AS ecp   
JOIN sys.dm_os_memory_objects AS omo   
    ON ecp.memory_object_address = omo.memory_object_address   
    OR ecp.memory_object_address = omo.parent_address  
WHERE cacheobjtype = 'Compiled Plan';  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Динамические административные представления и функции, связанные с выполнением &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_exec_query_plan &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)   
 [sys.dm_exec_plan_attributes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-plan-attributes-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
 [sys.dm_os_memory_objects &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql.md)   
 [sys.dm_os_memory_cache_entries &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-cache-entries-transact-sql.md)   
 [FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md)  
  
  


