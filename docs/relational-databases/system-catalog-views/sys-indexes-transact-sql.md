---
description: sys.indexes (Transact-SQL)
title: sys. indexes (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 02/12/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.indexes
- indexes
- sys.indexes_TSQL
- indexes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.indexes catalog view
ms.assetid: 066bd9ac-6554-4297-88fe-d740de1f94a8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a06c0570f55a46ae29d994f5eed8b34b95d72b39
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094572"
---
# <a name="sysindexes-transact-sql"></a>sys.indexes (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Содержит строку для каждого индекса или кучи табличного объекта, такого как таблица, представление или функция с табличным значением.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Идентификатор объекта, которому принадлежит данный индекс.|  
|**name**|**sysname**|Имя индекса. **имя** уникально только в пределах объекта.<br /><br /> NULL = куча.|  
|**index_id**|**int**|Идентификатор индекса. **index_id** уникален только в пределах объекта.<br /><br /> 0 = куча;<br /><br /> 1 или 5 = кластеризованный индекс (дерево B, columnstore)<br /><br /> > 1 и <> 5 = некластеризованный индекс|  
|**type**|**tinyint**|Тип индекса:<br /><br /> 0 = куча;<br /><br /> 1 = кластеризованный<br /><br /> 2 = некластеризованный.<br /><br /> 3 = XML.<br /><br /> 4 = пространственный.<br /><br /> 5 = кластеризованный индекс columnstore. **Область применения**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздних версий.<br /><br /> 6 = некластеризованный индекс columnstore. **Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.<br /><br /> 7 = некластеризованный хэш-индекс. **Область применения**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздних версий.|  
|**type_desc**|**nvarchar(60)**|Описание типа индекса.<br /><br /> HEAP<br /><br /> CLUSTERED<br /><br /> NONCLUSTERED<br /><br /> XML<br /><br /> SPATIAL<br /><br /> CLUSTERed COLUMNSTORE — **применяется к**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздним версиям.<br /><br /> Некластеризованный COLUMNSTORE — **применяется к**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздним версиям.<br /><br /> Некластеризованный хэш: некластеризованные хэш-индексы поддерживаются только для таблиц, оптимизированных для памяти. Представление sys.hash_indexes отображает текущие хэш-индексы и свойства хэша. Дополнительные сведения см. в разделе [sys.hash_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-hash-indexes-transact-sql.md). **Область применения**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздних версий.|  
|**is_unique**|**bit**|1 = индекс уникален.<br /><br /> 0 = индекс не уникален.<br /><br /> Всегда равен 0 для кластеризованных индексов columnstore.|  
|**data_space_id**|**int**|Идентификатор пространства данных этого индекса. Пространством данных может быть или файловая группа, или схема секционирования.<br /><br /> 0 = **object_id** является возвращающей табличное значение функцией или индексом в памяти.|  
|**ignore_dup_key**|**bit**|1 = параметр IGNORE_DUP_KEY имеет значение ON.<br /><br /> 0 = параметр IGNORE_DUP_KEY имеет значение OFF.|  
|**is_primary_key**|**bit**|1 = индекс является частью ограничения PRIMARY KEY.<br /><br /> Всегда равен 0 для кластеризованных индексов columnstore.|  
|**is_unique_constraint**|**bit**|1 = индекс является частью ограничения UNIQUE.<br /><br /> Всегда равен 0 для кластеризованных индексов columnstore.|  
|**fill_factor**|**tinyint**|> 0 = процент FILLFACTOR, используемый при создании или перестроении индекса.<br /><br /> 0 = значение по умолчанию.<br /><br /> Всегда равен 0 для кластеризованных индексов columnstore.|  
|**is_padded**|**bit**|1 = параметр PADINDEX имеет значение ON.<br /><br /> 0 = параметр PADINDEX имеет значение OFF.<br /><br /> Всегда равен 0 для кластеризованных индексов columnstore.|  
|**is_disabled**|**bit**|1 = индекс отключен.<br /><br /> 0 = индекс не отключен.|  
|**is_hypothetical**|**bit**|1 = индекс является гипотетическим и не может быть использован непосредственно как путь доступа к данным. Гипотетические индексы содержат статистику уровня столбцов.<br /><br /> 0 = индекс не является гипотетическим.|  
|**allow_row_locks**|**bit**|1 = индекс допускает блокировки строк.<br /><br /> 0 = индекс не допускает блокировки строк.<br /><br /> Всегда равен 0 для кластеризованных индексов columnstore.|  
|**allow_page_locks**|**bit**|1 = индекс допускает блокировки страниц.<br /><br /> 0 = индекс не допускает блокировки страниц.<br /><br /> Всегда равен 0 для кластеризованных индексов columnstore.|  
|**has_filter**|**bit**|1 = индекс с фильтром; содержит строки, удовлетворяющие определению фильтра.<br /><br /> 0 = индекс без фильтра.|  
|**filter_definition**|**nvarchar(max)**|Выражение для подмножества строк, включенного в фильтруемый индекс.<br /><br /> NULL для кучи, нефильтрованного индекса или недостаточных разрешений на таблицу.|  
|**auto_created**|**bit**|1 = индекс был создан автоматической настройкой.<br /><br />0 = индекс был создан пользователем.
|**optimize_for_sequential_key**|**bit**|1 = для индекса включена оптимизация вставки последней страницы.<br><br>0 = значение по умолчанию. В индексе отключена оптимизация вставки последней страницы.|

> [!NOTE]
> Бит **optimize_for_sequential_key** поддерживается только в версиях SQL Server 2019 CTP 3,1 и выше.
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Примеры  
 В следующем примере возвращаются все индексы для таблицы `Production.Product` в [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] базе данных.  
  
```  
  
SELECT i.name AS index_name  
    ,i.type_desc  
    ,is_unique  
    ,ds.type_desc AS filegroup_or_partition_scheme  
    ,ds.name AS filegroup_or_partition_scheme_name  
    ,ignore_dup_key  
    ,is_primary_key  
    ,is_unique_constraint  
    ,fill_factor  
    ,is_padded  
    ,is_disabled  
    ,allow_row_locks  
    ,allow_page_locks  
FROM sys.indexes AS i  
INNER JOIN sys.data_spaces AS ds ON i.data_space_id = ds.data_space_id  
WHERE is_hypothetical = 0 AND i.index_id <> 0   
AND i.object_id = OBJECT_ID('Production.Product');  
GO  
  
```  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sys.index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)   
 [sys.xml_indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-xml-indexes-transact-sql.md)   
 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.key_constraints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-key-constraints-transact-sql.md)   
 [sys.filegroups (Transact-SQL)](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)   
 [sys.partition_schemes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-partition-schemes-transact-sql.md)   
 [Запросы к системному каталогу SQL Server вопросы и ответы](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [Выполняющаяся в памяти OLTP (оптимизация в памяти)](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
