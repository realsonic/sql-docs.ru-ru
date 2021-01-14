---
description: sys.dm_db_column_store_row_group_physical_stats (Transact-SQL)
title: sys.dm_db_column_store_row_group_physical_stats (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 05/05/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_db_column_store_row_group_physical_stats_TSQL
- sys.dm_db_column_store_row_group_physical_stats
- dm_db_column_store_row_group_physical_stats
- dm_db_column_store_row_group_physical_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dm_db_column_store_row_group_physical_stats
- sys.dm_db_column_store_row_group_physical_stats dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b47d4bd97718bab9b25c6cb6d7772b08cd7251f2
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172126"
---
# <a name="sysdm_db_column_store_row_group_physical_stats-transact-sql"></a>sys.dm_db_column_store_row_group_physical_stats (Transact-SQL)


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Предоставляет текущие сведения на уровне группы строк о всех индексах columnstore в текущей базе данных.  

Это расширяет представление каталога [sys.column_store_row_groups &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-column-store-row-groups-transact-sql.md).  

|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Идентификатор базовой таблицы.|  
|**index_id**|**int**|ИДЕНТИФИКАТОР этого индекса columnstore в *object_id* таблице.|  
|**partition_number**|**int**|Идентификатор секции таблицы, содержащей *row_group_id*. Partition_number можно использовать для соединения этого динамического административного представления с представлением sys.partitions.|  
|**row_group_id**|**int**|Идентификатор этой группы строк. Для секционированных таблиц значение value является уникальным в пределах секции.<br /><br /> -1 для хвоста в памяти.|  
|**delta_store_hobt_id**|**bigint**|Hobt_id для группы строк в разностном хранилище.<br /><br /> Значение NULL, если группа строк не находится в разностном хранилище.<br /><br /> Значение NULL для заключительного фрагмента таблицы в памяти.|  
|**state**|**tinyint**|ИДЕНТИФИКАЦИОНный номер, связанный *state_description*.<br /><br /> 0 = INVISIBLE<br /><br /> 1 = OPEN;<br /><br /> 2 = CLOSED<br /><br /> 3 = COMPRESSED<br /><br /> 4 = ЗАХОРОНЕНИЕ<br /><br /> Сжатие является единственным состоянием, которое применяется к таблицам в памяти.|  
|**state_desc**|**nvarchar(60)**|Описание состояния группы строк:<br /><br /> 0 — невидимая — группа строк, которая строится. Пример: <br />Группа строк в columnstore невидима во время сжатия данных. После завершения сжатия параметр метаданных изменяет состояние группы строк columnstore с невидимого на сжатый и состояние группы строк deltastore с "ЗАКРЫТо" на "ЗАХОРОНЕНие".<br /><br /> 1 — OPEN — группа строк deltastore, которая принимает новые строки. Открытая группа строк остается в формате rowstore и не сжимается в формат columnstore.<br /><br /> 2 — ЗАКРЫТо — группа строк в разностном хранилище, которая содержит максимальное количество строк, и ожидает, пока процесс перемещения кортежа сжимает его в columnstore.<br /><br /> 3 — сжатый — группа строк, которая сжимается с помощью сжатия columnstore и хранится в columnstore.<br /><br /> 4 — ЗАХОРОНЕНие — группа строк, которая ранее была в deltastore и больше не используется.|  
|**total_rows**|**bigint**|Количество строк, физически хранящихся в группе строк. Для сжатых групп строк. Включает строки, помеченные как удаленные.|  
|**deleted_rows**|**bigint**|Количество строк, физически хранящихся в сжатой группе строк и помеченных для удаления.<br /><br /> Для групп строк, которые находятся в разностном хранилище, значение равно 0.|  
|**size_in_bytes**|**bigint**|Объединенный размер всех страниц в данной группе строк (в байтах). Этот размер не включает размер, необходимый для хранения метаданных или общих словарей.|  
|**trim_reason**|**tinyint**|Причина, по которой СЖАТая группа строк была активирована, меньше максимального количества строк.<br /><br /> 0 — UNKNOWN_UPGRADED_FROM_PREVIOUS_VERSION<br /><br /> 1 — NO_TRIM<br /><br /> 2 — BULKLOAD<br /><br /> 3 — REORG<br /><br /> 4 — DICTIONARY_SIZE<br /><br /> 5 — MEMORY_LIMITATION<br /><br /> 6 — RESIDUAL_ROW_GROUP<br /><br /> 7 — STATS_MISMATCH<br /><br /> 8 — ПЕРЕБРОСКИ<br /><br /> 9 — AUTO_MERGE|  
|**trim_reason_desc**|**nvarchar(60)**|Описание *trim_reason*.<br /><br /> 0-UNKNOWN_UPGRADED_FROM_PREVIOUS_VERSION: произошла ошибка при обновлении предыдущей версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> 1 — NO_TRIM: группа строк не обрезается. Группа строк была сжата до 1 048 576 строк.  Число строк может быть меньше, если подмножество строк было удалено после закрытия разностного группы строк<br /><br /> 2 — BULKLOAD: размер пакета массовой загрузки ограничен количеством строк.<br /><br /> 3 — REORG: принудительное сжатие в составе команды REORG.<br /><br /> 4 — DICTIONARY_SIZE: размер словаря слишком велик для сжатия всех строк вместе.<br /><br /> 5 — MEMORY_LIMITATION: недостаточно свободной памяти для сжатия всех строк.<br /><br /> 6-RESIDUAL_ROW_GROUP: закрывается как часть последней группы строк со строками < 1 000 000 во время операции построения индекса<br /><br /> 7 — STATS_MISMATCH: только для columnstore в таблице в памяти. Если статистика неправильно указывает >= 1 000 000 уточняющих строк в заключительном фрагменте, но было обнаружено меньшее число, сжатая группы строк будет содержать < 1 000 000 строк.<br /><br /> 8-ПЕРЕБРОСКИ: только для columnstore в таблице в памяти. Если в хвосте имеется > 1 000 000 подходящих строк, то оставшиеся строки оставшихся пакетов сжимаются, если число составляет от 100 КБ до 1 000 000<br /><br /> 9 — AUTO_MERGE: операция слияния кортежей, выполняемая в фоновом режиме, объединяет один или несколько групп строк в этот группы строк.|  
|**transition_to_compressed_state**|tinyint|Показывает, как этот группы строк был перемещен из deltastore в сжатое состояние в columnstore.<br /><br /> 1 — NOT_APPLICABLE<br /><br /> 2 — INDEX_BUILD<br /><br /> 3 — TUPLE_MOVER<br /><br /> 4 — REORG_NORMAL<br /><br /> 5 — REORG_FORCED<br /><br /> 6 — BULKLOAD<br /><br /> 7. СЛИЯНИЕ|  
|**transition_to_compressed_state_desc**|nvarchar(60)| 1 — NOT_APPLICABLE — операция не применяется к deltastore. Или группы строк был сжат до обновления до, в этом [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] случае журнал не сохраняется.<br /><br /> 2 — INDEX_BUILD — при создании индекса или перестроении индекса сжат группы строк.<br /><br /> 3 — TUPLE_MOVER — компонент перемещения кортежей, работающий в фоновом режиме, сжимает группы строк. Перемещение кортежей происходит после того, как состояние группы строк изменится с OPEN на CLOSED.<br /><br /> 4 — REORG_NORMAL — операция реорганизации, ALTER INDEX... REORG, перемещен закрытый группы строк из deltastore в columnstore. Это произошло до того, как в процессе перемещения по кортежам пришло время на перемещение группы строк.<br /><br /> 5 — REORG_FORCED — этот группы строк был открыт в deltastore и был принудительно передан в columnstore до того, как он имел полное число строк.<br /><br /> 6. BULKLOAD. операция массовой загрузки сжимает группы строк напрямую без использования deltastore.<br /><br /> 7 — MERGE — операция слияния объединяет один или несколько групп строк в этот группы строк, а затем выполняет сжатие columnstore.|  
|**has_vertipaq_optimization**|bit|Оптимизация VertiPaq улучшает сжатие columnstore путем изменения порядка строк в группы строк для достижения более высокого сжатия. В большинстве случаев такая оптимизация выполняется автоматически. В двух случаях оптимизация VertiPaq не используется:<br/>  а. Если Дельта-группы строк перемещается в columnstore и имеется один или несколько некластеризованных индексов в индексе columnstore, то оптимизация VertiPaq пропускается для сворачивания изменений в индексе сопоставления.<br/> b. для индексов columnstore в таблицах, оптимизированных для памяти. <br /><br /> 0 = нет<br /><br /> 1 = да|  
|**поколения**|BIGINT|Создание группы строк, связанной с этой группой строк.|  
|**created_time**|datetime2|Время создания группы строк.<br /><br /> NULL — для индекса columnstore в таблице в памяти.|  
|**closed_time**|datetime2|Время, когда группы строк был закрыт.<br /><br /> NULL — для индекса columnstore в таблице в памяти.|  
| &nbsp; | &nbsp; | &nbsp; |

## <a name="results"></a>Результаты  
 Возвращает по одной строке для каждого группы строк в текущей базе данных.  
  
## <a name="permissions"></a>Разрешения  
Требуется `CONTROL` разрешение на таблицу и `VIEW DATABASE STATE` разрешение в базе данных.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-calculate-fragmentation-to-decide-when-to-reorganize-or-rebuild-a-columnstore-index"></a>A. Вычислите фрагментацию, чтобы решить, когда следует реорганизовать или перестроить индекс columnstore.  
 Для индексов columnstore процент удаленных строк является хорошей мерой для фрагментации в группы строк. Если фрагментация составляет 20% или более, удалите удаленные строки. Дополнительные примеры см. в разделе [реорганизация и перестроение индексов](~/relational-databases/indexes/reorganize-and-rebuild-indexes.md).  
  
 В этом примере объединяются **sys.dm_db_column_store_row_group_physical_stats** с другими системными таблицами, а затем столбец вычисляется `Fragmentation` как оценка эффективности каждой группы строк в текущей базе данных. Чтобы найти сведения об одной таблице, удалите дефисы в комментариях перед предложением **WHERE** и укажите имя таблицы.  
  
```sql  
SELECT i.object_id,   
    object_name(i.object_id) AS TableName,   
    i.name AS IndexName,   
    i.index_id,   
    i.type_desc,   
    CSRowGroups.*,  
    100*(ISNULL(deleted_rows,0))/NULLIF(total_rows,0) AS 'Fragmentation'
FROM sys.indexes AS i  
JOIN sys.dm_db_column_store_row_group_physical_stats AS CSRowGroups  
    ON i.object_id = CSRowGroups.object_id AND i.index_id = CSRowGroups.index_id   
-- WHERE object_name(i.object_id) = 'table_name'   
ORDER BY object_name(i.object_id), i.name, row_group_id;  
```  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)      
 [Архитектура индексов columnstore](../../relational-databases/sql-server-index-design-guide.md#columnstore_index)         
 [Запросы к системному каталогу SQL Server вопросы и ответы](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [sys.columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.all_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-all-columns-transact-sql.md)   
 [sys.computed_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-computed-columns-transact-sql.md)  
 [sys.column_store_dictionaries &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-dictionaries-transact-sql.md)   
 [sys.column_store_segments (Transact-SQL)](../../relational-databases/system-catalog-views/sys-column-store-segments-transact-sql.md)  
  
