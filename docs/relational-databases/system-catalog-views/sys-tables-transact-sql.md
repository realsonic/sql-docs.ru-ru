---
description: sys.tables (Transact-SQL)
title: sys. Tables (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- tables_TSQL
- sys.tables_TSQL
- sys.tables
- tables
dev_langs:
- TSQL
helpviewer_keywords:
- sys.tables catalog view
ms.assetid: 8c42eba1-c19f-4045-ac82-b97a5e994090
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0f033d0b414fab40a33a34490a0ef7ff08a2c232
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171136"
---
# <a name="systables-transact-sql"></a>sys.tables (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает по одной строке для каждой пользовательской таблицы в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|\<inherited columns>||Список столбцов, наследуемых этим представлением, см. в разделе [sys. objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).|  
|lob_data_space_id|**int**|Ненулевое значение — это идентификатор пространства данных (файловой группы или схемы секционирования), хранящего данные больших двоичных объектов (LOB) для этой таблицы. Примерами типов данных LOB являются **varbinary (max)**, **varchar (max)**, **Geography** или **XML**.<br /><br /> 0 = таблица не содержит данных LOB.|  
|filestream_data_space_id|**int**|Идентификатор пространства данных для файловой группы FILESTREAM или схемы секционирования, состоящей из файловых групп FILESTREAM.<br /><br /> Чтобы сообщить имя файловой группы FILESTREAM, выполните запрос `SELECT FILEGROUP_NAME (filestream_data_space_id) FROM sys.tables` .<br /><br /> sys.tables можно объединить со следующими представлениями по условию filestream_data_space_id = data_space_id.<br /><br /> -sys. FILEGROUP<br /><br /> — sys.partition_schemes<br /><br /> -sys. indexes<br /><br /> — sys.allocation_units<br /><br /> — sys.fulltext_catalogs<br /><br /> — sys.data_spaces<br /><br /> — sys.destination_data_spaces<br /><br /> — sys.master_files<br /><br /> — sys.database_files<br /><br /> -backupfilegroup (присоединение filegroup_id)|  
|max_column_id_used|**int**|Максимальный идентификатор столбца, когда-либо использованный в таблице.|  
|lock_on_bulk_load|**bit**|Таблица заблокирована при массовой загрузке. Дополнительные сведения см. в разделе [sp_tableoption (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md).|  
|uses_ansi_nulls|**bit**|Таблица была создана при установленном параметре SET ANSI_NULLS = ON.|  
|is_replicated|**bit**|1 = таблица опубликована путем репликации моментальных снимков или транзакций.|  
|has_replication_filter|**bit**|1 = для таблицы имеется фильтр репликации.|  
|is_merge_published|**bit**|1 = таблица опубликована путем репликации слиянием.|  
|is_sync_tran_subscribed|**bit**|1 = таблица используется в немедленно обновляемой подписке.|  
|has_unchecked_assembly_data|**bit**|1 = таблица содержит материализованные данные, зависящие от сборки, определение которых изменилось во время последней операции ALTER ASSEMBLY. Будет сброшено в значение 0 после следующей успешной операции DBCC CHECKDB или DBCC CHECKTABLE.|  
|text_in_row_limit|**int**|Максимальное число байтов, разрешенное для текста в строке.<br /><br /> 0 = параметр текста в строке не установлен. Дополнительные сведения см. в разделе [sp_tableoption (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md).|  
|large_value_types_out_of_row|**bit**|1 = типы больших значений хранятся вне строк. Дополнительные сведения см. в разделе [sp_tableoption (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md).|  
|is_tracked_by_cdc|**bit**|1 = в таблице включена система отслеживания измененных данных. Дополнительные сведения см. в разделе [sys.sp_cdc_enable_table &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md).|  
|lock_escalation|**tinyint**|Значение параметра LOCK_ESCALATION для таблицы.<br /><br /> 0 = TABLE<br /><br /> 1 = DISABLE<br /><br /> 2 = AUTO|  
|lock_escalation_desc|**nvarchar(60)**|Текстовое описание параметра lock_escalation для таблицы. Возможные значения: TABLE, AUTO и DISABLE.|  
|is_filetable|**bit**|**Применимо к**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше, а также [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)].<br /><br /> 1 = таблица имеет тип FileTable.<br /><br /> Дополнительные сведения о таблицах FileTable см. в разделе [Таблицы FileTable (SQL Server)](../../relational-databases/blob/filetables-sql-server.md).|  
|устойчивость|**tinyint**|**Применимо к**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и выше, а также [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)].<br /><br /> Возможные следующие значения.<br /><br /> 0 = SCHEMA_AND_DATA<br /><br /> 1 = SCHEMA_ONLY<br /><br /> Значением по умолчанию является значение 0.|  
|durability_desc|**nvarchar(60)**|**Применимо к**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и выше, а также [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)].<br /><br /> Допустимы следующие значения:<br /><br /> SCHEMA_ONLY<br /><br /> SCHEMA_AND_DATA<br /><br /> Значение SCHEMA_AND_DATA указывает, что таблица является надежной и оптимизированной для памяти. SCHEMA_AND_DATA — это значение по умолчанию для таблиц, оптимизированных для памяти. Значение SCHEMA_ONLY указывает, что табличные данные не будут сохранены после перезапуска базы данных с объектами, оптимизированными для памяти.|  
|is_memory_optimized|**bit**|**Применимо к**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и выше, а также [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)].<br /><br /> Допустимы следующие значения:<br /><br /> 0 = не оптимизировано для памяти.<br /><br /> 1 = оптимизировано для памяти<br /><br /> Значение по умолчанию — 0.<br /><br /> Оптимизированные для памяти таблицы — это хранящиеся в памяти пользовательские таблицы, схемы которых сохраняются на диске аналогично другим пользовательским таблицам. Доступ к оптимизированным для памяти таблицам можно осуществлять из скомпилированных в собственном коде хранимых процедур.|  
|temporal_type|**tinyint**|**Применимо к**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] и выше, а также [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)].<br /><br /> Числовое значение, представляющее тип таблицы:<br /><br /> 0 = NON_TEMPORAL_TABLE<br /><br /> 1 = HISTORY_TABLE<br /><br /> 2 = SYSTEM_VERSIONED_TEMPORAL_TABLE|  
|temporal_type_desc|**nvarchar(60)**|**Применимо к**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] и выше, а также [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)].<br /><br /> Текстовое описание типа таблицы:<br /><br /> NON_TEMPORAL_TABLE<br /><br /> HISTORY_TABLE<br /><br /> SYSTEM_VERSIONED_TEMPORAL_TABLE|  
|history_table_id|**int**|**Применимо к**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] и выше, а также [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)].<br /><br /> Если temporal_type в (2, 4) возвращает object_id таблицы, которая поддерживает исторические данные, в противном случае возвращает значение NULL.|  
|is_remote_data_archive_enabled|**bit**|**Применимо к**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] и более поздним версиям и [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]<br /><br /> Указывает, включена ли таблица с растяжением.<br /><br /> 0 = таблица не поддерживает растяжение.<br /><br /> 1 = таблица поддерживает растяжение.<br /><br /> Дополнительные сведения см. в разделе [Stretch Database](../../sql-server/stretch-database/stretch-database.md).|  
|is_external|**bit**|**Применимо к**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] и более поздним, [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)] и [!INCLUDE[sssdwfull](../../includes/sssdwfull-md.md)] .<br /><br /> Указывает, что таблица является внешней таблицей.<br /><br /> 0 = таблица не является внешней таблицей.<br /><br /> 1 = таблица является внешней таблицей.| 
|history_retention_period|**int**|**Применимо к**: [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]. <br/><br/>Числовое значение, представляющее продолжительность временного периода хранения журнала в единицах, указанных с помощью history_retention_period_unit. |  
|history_retention_period_unit|**int**|**Применимо к**: [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]. <br/><br/>Числовое значение, представляющее тип временной единицы хранения истории. <br /><br />-1: БЕСКОНЕЧНОСТЬ <br /><br />3: ДЕНЬ <br /><br />4: НЕДЕЛЯ <br /><br />5: МЕСЯЦ <br /><br />6: ГОД |  
|history_retention_period_unit_desc|**nvarchar (10)**|**Применимо к**: [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]. <br/><br/>Текстовое описание типа временной единицы срока хранения истории. <br /><br />INFINITE <br /><br />DAY <br /><br />WEEK <br /><br />MONTH <br /><br />YEAR |  
|is_node|**bit**|**Применимо к**: [!INCLUDE[sssql17-md.md](../../includes/sssql17-md.md)] и [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]. <br/><br/>1 = это таблица узла графа. <br /><br />0 = это не таблица узла графа. |  
|is_edge|**bit**|**Применимо к**: [!INCLUDE[sssql17-md.md](../../includes/sssql17-md.md)] и [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]. <br/><br/>1 = это краевая таблица графа. <br /><br />0 = это не краевая таблица графа. |  

## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Примеры  
 В следующем примере возвращаются все пользовательские таблицы, у которых нет первичного ключа.  
  
```  
SELECT SCHEMA_NAME(schema_id) AS schema_name  
    ,name AS table_name   
FROM sys.tables   
WHERE OBJECTPROPERTY(object_id,'TableHasPrimaryKey') = 0  
ORDER BY schema_name, table_name;  
GO  
  
```  
  
В следующем примере показано, как можно предоставить доступ к связанным временным данным.  
   
**Применимо к**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] и выше, а также [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)].
  
```  
SELECT T1.object_id, T1.name as TemporalTableName, SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema,  
T2.name as HistoryTableName, SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema,  
T1.temporal_type_desc  
FROM sys.tables T1  
LEFT JOIN sys.tables T2   
ON T1.history_table_id = T2.object_id  
ORDER BY T1.temporal_type desc  
```  

В следующем примере показано, как можно предоставить сведения о временном хранении журнала.  

**Применимо к**: [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)].  
  
```  
SELECT DB.is_temporal_history_retention_enabled, SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema, 
T1.name as TemporalTableName, SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema, T2.name as HistoryTableName,
T1.history_retention_period, T1.history_retention_period_unit_desc
FROM sys.tables T1  
OUTER APPLY (select is_temporal_history_retention_enabled from sys.databases where name = DB_NAME()) DB
LEFT JOIN sys.tables T2   
ON T1.history_table_id = T2.object_id WHERE T1.temporal_type = 2 
```  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [DBCC CHECKDB &#40;&#41;Transact-SQL ](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)   
 [DBCC CHECKTABLE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md)   
 [Запросы к системному каталогу SQL Server вопросы и ответы](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [Выполняющаяся в памяти OLTP (оптимизация в памяти)](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
