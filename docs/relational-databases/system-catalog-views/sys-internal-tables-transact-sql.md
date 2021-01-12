---
description: sys.internal_tables (Transact-SQL)
title: sys.internal_tables (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.internal_tables
- internal_tables
- sys.internal_tables_TSQL
- internal_tables_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- internal tables
- sys.internal_tables catalog view
ms.assetid: a5821c70-f150-4676-8476-3a31f7403dca
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ecc601c3eec509258cdac61a9c9216077f36f712
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094522"
---
# <a name="sysinternal_tables-transact-sql"></a>sys.internal_tables (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает одну строку для каждого объекта какой-либо внутренней таблицы. Внутренние таблицы создаются [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] автоматически для поддержки различных функций. Например, при создании первичного XML-индекса [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] автоматически создает внутреннюю таблицу для сохранения разобранных данных XML-документа. Внутренние таблицы отображаются в схеме **sys** каждой базы данных и имеют уникальные, созданные системой имена, указывающие на их функции, например **xml_index_nodes_2021582240_32001** или **queue_messages_1977058079**  
  
 Данные внутренних таблиц недоступны для пользователей, а их схема является жесткой и неизменной. Невозможно ссылаться на имена внутренних таблиц в инструкциях языка [!INCLUDE[tsql](../../includes/tsql-md.md)]. Например, нельзя выполнить инструкцию, такую как SELECT \* FROM *\<sys.internal_table_name>* . Однако можно обращаться с запросами к представлениям каталогов для просмотра метаданных внутренних таблиц.  
  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**\<Columns inherited from sys.objects>**||Список столбцов, наследуемых этим представлением, см. в разделе [sys. objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).|  
|**internal_type**|**tinyint**|Тип внутренней таблицы:<br /><br /> 3 = **query_disk_store_query_hints**<br /><br /> 4 = **query_disk_store_query_template_parameterization**<br /><br /> 6 = **query_disk_store_wait_stats**<br /><br /> 201 = **queue_messages**<br /><br /> 202 = **xml_index_nodes**<br /><br /> 203 = **fulltext_catalog_freelist**<br /><br /> 205 = **query_notification**<br /><br /> 206 = **service_broker_map**<br /><br /> 207 = **extended_indexes** (например, пространственный индекс)<br /><br /> 208 = **filestream_tombstone**<br /><br /> 209 = **CHANGE_TRACKING**<br /><br /> 210 = **tracked_committed_transactions**<br /><br /> 220 = **contained_features**<br /><br /> 225 = **filetable_updates**<br /><br /> 236 = **selective_xml_index_node_table**<br /><br /> 240 = **query_disk_store_query_text**<br /><br /> 241 = **query_disk_store_query**<br /><br /> 242 = **query_disk_store_plan**<br /><br /> 243 = **query_disk_store_runtime_stats**<br /><br /> 244 = **query_disk_store_runtime_stats_interval**<br /><br /> 245 = **query_context_settings**|  
|**internal_type_desc**|**nvarchar(60)**|Описание типа внутренней таблицы:<br /><br /> QUERY_DISK_STORE_QUERY_HINTS<br /><br /> QUERY_DISK_STORE_QUERY_TEMPLATE_PARAMETERIZATION<br /><br /> QUERY_DISK_STORE_WAIT_STATS<br /><br /> QUEUE_MESSAGES<br /><br /> XML_INDEX_NODES<br /><br /> FULLTEXT_CATALOG_FREELIST<br /><br /> FULLTEXT_CATALOG_MAP<br /><br /> QUERY_NOTIFICATION<br /><br /> SERVICE_BROKER_MAP<br /><br /> EXTENDED_INDEXES<br /><br /> FILESTREAM_TOMBSTONE<br /><br /> CHANGE_TRACKING<br /><br /> TRACKED_COMMITTED_TRANSACTIONS<br /><br /> CONTAINED_FEATURES<br /><br /> FILETABLE_UPDATES<br /><br /> SELECTIVE_XML_INDEX_NODE_TABLE<br /><br /> QUERY_DISK_STORE_QUERY_TEXT<br /><br /> QUERY_DISK_STORE_QUERY<br /><br /> QUERY_DISK_STORE_PLAN<br /><br /> QUERY_DISK_STORE_RUNTIME_STATS<br /><br /> QUERY_DISK_STORE_RUNTIME_STATS_INTERVAL<br /><br /> QUERY_CONTEXT_SETTINGS|  
|**parent_id**|**int**|Идентификатор родителя, независимо от того, находится он в пределах области видимости схемы или нет. Принимает значение 0, если родитель отсутствует.<br /><br /> **queue_messages**  =  **object_id** очереди<br /><br /> **xml_index_nodes**  =  **object_id** XML-индекса<br /><br /> **fulltext_catalog_freelist**  =  **fulltext_catalog_id** полнотекстового каталога<br /><br /> **fulltext_index_map**  =  **object_id** полнотекстового индекса<br /><br /> **query_notification** или **service_broker_map** = 0<br /><br /> **extended_indexes**  =  **object_id** расширенного индекса, например пространственного индекса<br /><br /> **object_id** таблицы, для которой включено отслеживание таблиц = **CHANGE_TRACKING**|  
|**parent_minor_id**|**int**|Вспомогательный идентификатор родителя.<br /><br /> **xml_index_nodes**  =  **index_id** XML-индекса<br /><br /> **extended_indexes**  =  **index_id** расширенного индекса, например пространственного индекса<br /><br /> 0 = **queue_messages**, **fulltext_catalog_freelist**, **fulltext_index_map**, **query_notification**, **service_broker_map** или **CHANGE_TRACKING**|  
|**lob_data_space_id**|**int**|Ненулевое значение — идентификатор пространства данных (файловая группа или схема секционирования), хранящего данные больших объектов (LOB) для этой таблицы.|  
|**filestream_data_space_id**|**int**|Зарезервировано для будущего использования.|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="remarks"></a>Комментарии  
 Внутренние таблицы размещаются в той же файловой группе, что и родительская сущность. С помощью запроса к каталогу, проиллюстрированного далее в примере Е, можно узнать количество страниц, занимаемых внутренними таблицами под хранение данных «в строке», «вне строки» и данных больших объектов (LOB).  
  
 Для возврата данных об использовании места для внутренних таблиц можно использовать системную процедуру [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md) . **sp_spaceused** сообщает внутреннее табличное пространство следующими способами:  
  
-   При указании имени запроса базовая внутренняя таблица, связанная с запросом, находится по ссылке и сообщается объем пространства, занятого ей.  
  
-   Страницы, используемые внутренними таблицами XML-индексов, пространственных индексов и полнотекстовых индексов, включаются в столбец **index_size** . Если указано имя таблицы или индексированного представления, то страницы для XML-индексов, пространственных индексов и полнотекстовых индексов для этого объекта включаются в столбцы **reserved** и **index_size**.  
  
## <a name="examples"></a>Примеры  
 Следующие примеры демонстрируют, как обратиться с запросом к метаданным внутренней таблицы с помощью представлений каталога.  
  
### <a name="a-show-internal-tables-that-inherit-columns-from-the-sysobjects-catalog-view"></a>A. Показывает внутренние таблицы, наследующие столбцы от представления каталога sys.objects  
  
```  
SELECT * FROM sys.objects WHERE type = 'IT';  
```  
  
### <a name="b-return-all-internal-table-metadata-including-that-which-is-inherited-from-sysobjects"></a>Б. Возвращает все метаданные внутренней таблицы (в том числе те, которые наследуются от представления каталога sys.objects)  
  
```  
SELECT * FROM sys.internal_tables;  
```  
  
### <a name="c-return-internal-table-columns-and-column-data-types"></a>В. Возвращает столбцы и типы данных столбцов внутренней таблицы  
  
```  
SELECT SCHEMA_NAME(itab.schema_id) AS schema_name  
    ,itab.name AS internal_table_name  
    ,typ.name AS column_data_type   
    ,col.*  
FROM sys.internal_tables AS itab  
JOIN sys.columns AS col ON itab.object_id = col.object_id  
JOIN sys.types AS typ ON typ.user_type_id = col.user_type_id  
ORDER BY itab.name, col.column_id;  
```  
  
### <a name="d-return-internal-table-indexes"></a>Г. Возвращает индексы внутренней таблицы  
  
```  
SELECT SCHEMA_NAME(itab.schema_id) AS schema_name  
    , itab.name AS internal_table_name  
    , idx.*  
FROM sys.internal_tables AS itab  
JOIN sys.indexes AS idx ON itab.object_id = idx.object_id  
ORDER BY itab.name, idx.index_id;  
```  
  
### <a name="e-return-internal-table-statistics"></a>Д. Возвращает статистику внутренней таблицы  
  
```  
SELECT SCHEMA_NAME(itab.schema_id) AS schema_name  
    ,itab.name AS internal_table_name  
    , s.*  
FROM sys.internal_tables AS itab  
JOIN sys.stats AS s ON itab.object_id = s.object_id  
ORDER BY itab.name, s.stats_id;  
```  
  
### <a name="f-return-internal-table-partition-and-allocation-unit-information"></a>Е. Возвращает информацию о секциях и единицах распределения внутренней таблицы  
  
```  
SELECT SCHEMA_NAME(itab.schema_id) AS schema_name  
    ,itab.name AS internal_table_name  
    ,idx.name AS heap_or_index_name  
    ,p.*  
    ,au.*  
FROM sys.internal_tables AS itab  
JOIN sys.indexes AS idx  
--     JOIN to the heap or the clustered index  
    ON itab.object_id = idx.object_id AND idx.index_id IN (0,1)  
JOIN   sys.partitions AS p   
    ON p.object_id = idx.object_id AND p.index_id = idx.index_id  
JOIN   sys.allocation_units AS au  
--     IN_ROW_DATA (type 1) and ROW_OVERFLOW_DATA (type 3) => JOIN to partition's Hobt  
--     else LOB_DATA (type 2) => JOIN to the partition ID itself.  
ON au.container_id =    
    CASE au.type   
        WHEN 2 THEN p.partition_id   
        ELSE p.hobt_id   
    END  
ORDER BY itab.name, idx.index_id;  
```  
  
### <a name="g-return-internal-table-metadata-for-xml-indexes"></a>Ж. Возвращает метаданные внутренней таблицы для XML-индексов  
  
```  
SELECT t.name AS parent_table  
    ,t.object_id AS parent_table_id  
    ,it.name AS internal_table_name  
    ,it.object_id AS internal_table_id  
    ,xi.name AS primary_XML_index_name  
    ,xi.index_id as primary_XML_index_id  
FROM sys.internal_tables AS it  
JOIN sys.tables AS t   
    ON it.parent_id = t.object_id  
JOIN sys.xml_indexes AS xi   
    ON it.parent_id = xi.object_id  
    AND it.parent_minor_id  = xi.index_id  
WHERE it.internal_type_desc = 'XML_INDEX_NODES';  
GO  
```  
  
### <a name="h-return-internal-table-metadata-for-service-broker-queues"></a>З. Возвращает метаданные внутренней таблицы для очередей компонента Service Broker  
  
```  
SELECT q.name AS queue_name  
    ,q.object_id AS queue_id  
    ,it.name AS internal_table_name  
    ,it.object_id AS internal_table_id  
FROM sys.internal_tables AS it  
JOIN sys.service_queues  AS  q ON it.parent_id = q.object_id  
WHERE it.internal_type_desc = 'QUEUE_MESSAGES';  
GO  
```  
  
## <a name="i-return-internal-table-metadata-for-all-service-broker-services"></a>И. Возвращает метаданные внутренней таблицы для всех служб компонента Service Broker  
  
```  
SELECT *   
FROM tempdb.sys.internal_tables   
WHERE internal_type_desc = 'SERVICE_BROKER_MAP';  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  
  
  
