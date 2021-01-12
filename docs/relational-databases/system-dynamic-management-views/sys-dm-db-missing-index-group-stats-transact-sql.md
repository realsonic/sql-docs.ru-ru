---
description: sys.dm_db_missing_index_group_stats (Transact-SQL)
title: sys.dm_db_missing_index_group_stats (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_db_missing_index_group_stats_TSQL
- sys.dm_db_missing_index_group_stats
- dm_db_missing_index_group_stats_TSQL
- dm_db_missing_index_group_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_missing_index_group_stats dynamic management view
- missing indexes feature [SQL Server], sys.dm_db_missing_index_group_stats dynamic management view
ms.assetid: c2886986-9e07-44ea-a350-feeac05ee4f4
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a1c1e0236316970d953d5267d22e5598b0cbb215
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097776"
---
# <a name="sysdm_db_missing_index_group_stats-transact-sql"></a>sys.dm_db_missing_index_group_stats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает сводку сведений о группах отсутствующих индексов, за исключением пространственных индексов.  
  
 Динамические административные представления в среде [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] не могут предоставлять информацию, которая может повлиять на автономность базы данных, или информацию о других базах данных, к которым имеет доступ пользователь. Во избежание раскрытия этой информации все строки, содержащие данные, не принадлежащие подключенному клиенту, отфильтровываются.  
    
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**group_handle**|**int**|Идентифицирует группу отсутствующих индексов. Этот идентификатор уникален в пределах сервера.<br /><br /> Другие столбцы содержат сведения обо всех запросах, для которых индекс в группе считается отсутствующим.<br /><br /> Группа индексов содержит только один индекс.|  
|**unique_compiles**|**bigint**|Число компиляций и повторных компиляций, которые получают преимущества от этой группы отсутствующих индексов. Компиляции и повторные компиляции многих различных запросов могут влиять на значение этого столбца.|  
|**user_seeks**|**bigint**|Количество операций поиска по запросам пользователя, для которых мог бы использоваться рекомендованный индекс в группе.|  
|**user_scans**|**bigint**|Количество операций просмотра по запросам пользователя, для которых мог бы использоваться рекомендованный индекс в группе.|  
|**last_user_seek**|**datetime**|Дата и время последней операции поиска по запросам пользователя, для которых мог бы использоваться рекомендованный индекс в группе.|  
|**last_user_scan**|**datetime**|Дата и время последней операции просмотра по запросам пользователя, для которых мог бы использоваться рекомендованный индекс в группе.|  
|**avg_total_user_cost**|**float**|Средняя стоимость запросов пользователя, которая могла быть уменьшена с помощью индекса в группе.|  
|**avg_user_impact**|**float**|Средний процент выигрыша, который могли получить запросы пользователя, если создать эту группу отсутствующих индексов. Значение показывает, что стоимость запроса в среднем уменьшится на этот процент, если создать эту группу отсутствующих индексов.|  
|**system_seeks**|**bigint**|Количество операций поиска по запросам системы, таким как запросы auto stats, для которых мог бы использоваться рекомендованный индекс в группе. Дополнительные сведения см. в разделе [класс событий auto stats](../../relational-databases/event-classes/auto-stats-event-class.md).|  
|**system_scans**|**bigint**|Количество операций просмотра по запросам системы, для которых мог бы использоваться рекомендованный индекс в группе.|  
|**last_system_seek**|**datetime**|Дата и время последней операции системного поиска по запросам системы, для которых мог бы использоваться рекомендованный индекс в группе.|  
|**last_system_scan**|**datetime**|Дата и время последней операции системного просмотра по запросам системы, для которых мог бы использоваться рекомендованный индекс в группе.|  
|**avg_total_system_cost**|**float**|Средняя стоимость системных запросов, которая могла быть уменьшена с помощью индекса в группе.|  
|**avg_system_impact**|**float**|Средний процент выигрыша, который могли получить запросы системы, если создать эту группу отсутствующих индексов. Значение показывает, что стоимость запроса в среднем уменьшится на этот процент, если создать эту группу отсутствующих индексов.|  
  
## <a name="remarks"></a>Комментарии  
 Сведения, возвращаемые представлением **sys.dm_db_missing_index_group_stats**, обновляются при каждом выполнении запроса, а не при каждой компиляции или повторной компиляции запроса. Статистика использования не сохраняется и хранится только до перезапуска [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Администраторы базы данных должны периодически делать резервные копии сведений об отсутствующих индексах, если необходимо сохранить статистику использования после перезагрузки сервера.  

  >[!NOTE]
  >Результирующий набор для этого динамического административного представления ограничен 600 строк. Каждая строка содержит один отсутствующий индекс. Если у вас больше 600 отсутствующих индексов, следует устранить существующие отсутствующие индексы, чтобы можно было просмотреть новые.
  
## <a name="permissions"></a>Разрешения  
 Для выполнения запроса к этому динамическому административному представлению пользователям должно быть предоставлено разрешение VIEW SERVER STATE или любое другое, подразумевающее разрешение VIEW SERVER STATE.  
  
## <a name="examples"></a>Примеры  
 В следующих примерах показано использование динамического административного представления **sys.dm_db_missing_index_group_stats**.  
  
### <a name="a-find-the-10-missing-indexes-with-the-highest-anticipated-improvement-for-user-queries"></a>A. Поиск 10 отсутствующих индексов с самым высоким ожидаемым улучшением производительности пользовательских запросов  
 Следующий запрос определяет, какие 10 отсутствующих индексов окажут самое высокое ожидаемое совокупное улучшение производительности запросов пользователя в порядке убывания.  
  
```  
SELECT TOP 10 *  
FROM sys.dm_db_missing_index_group_stats  
ORDER BY avg_total_user_cost * avg_user_impact * (user_seeks + user_scans)DESC;  
```  
  
### <a name="b-find-the-individual-missing-indexes-and-their-column-details-for-a-particular-missing-index-group"></a>Б. Поиск отдельных отсутствующих индексов и подробных сведений об их столбцах для определенной группы отсутствующих индексов  
 Следующий запрос определяет, какие отсутствующие индексы составляют отдельную группу отсутствующих индексов, и отображает подробные сведения об их столбцах. Для данного примера дескриптор группы отсутствующих индексов равен 24.  
  
```  
SELECT migs.group_handle, mid.*  
FROM sys.dm_db_missing_index_group_stats AS migs  
INNER JOIN sys.dm_db_missing_index_groups AS mig  
    ON (migs.group_handle = mig.index_group_handle)  
INNER JOIN sys.dm_db_missing_index_details AS mid  
    ON (mig.index_handle = mid.index_handle)  
WHERE migs.group_handle = 24;  
```  
  
 Этот запрос предоставляет имя базы данных, схемы и таблицы, в которой отсутствует индекс. Он также предоставляет имена столбцов, которые должны использоваться для ключа индекса. При написании инструкции CREATE INDEX DDL для реализации отсутствующих индексов сначала перечислите столбцы равенства, а затем столбцы неравенства в \<*table_name*> предложении ON инструкции CREATE INDEX. Включенные столбцы должны быть перечислены в предложении INCLUDE инструкции CREATE INDEX. Чтобы определить эффективный порядок столбцов равенства, расположите их на основе их выборности, перечисляя наиболее выбираемые столбцы первыми (крайние левые в списке столбцов).  
  
## <a name="see-also"></a>См. также:  
 [sys.dm_db_missing_index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-columns-transact-sql.md)   
 [sys.dm_db_missing_index_details &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md)   
 [sys.dm_db_missing_index_groups &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)  
  
  
