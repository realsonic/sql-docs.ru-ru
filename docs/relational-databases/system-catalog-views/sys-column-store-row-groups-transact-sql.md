---
description: sys.column_store_row_groups (Transact-SQL)
title: sys.column_store_row_groups (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 10/28/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.column_store_row_groups_TSQL
- column_store_row_groups
- sys.column_store_row_groups
- column_store_row_groups_TSQL
- deleted bitmap
dev_langs:
- TSQL
helpviewer_keywords:
- sys.column_store_row_groups catalog view
ms.assetid: 76e7fef2-d1a4-4272-a2bb-5f5dcd84aedc
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: bb30e3c829dcdabde66fd1a330ec617eea7e5b34
ms.sourcegitcommit: 7791bd2ba339edc5cd2078a6537c8f6bfe72a19b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2021
ms.locfileid: "98564472"
---
# <a name="syscolumn_store_row_groups-transact-sql"></a>sys.column_store_row_groups (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  Предоставляет сведения о кластеризованных индексах columnstore по отдельным сегментам, что позволяет администратору принимать решения о сопровождении системы. **sys.column_store_row_groups** содержит столбец для общего числа физически хранимых строк (включая те, которые помечены как удаленные) и столбец для числа строк, помеченных как удаленные. Используйте **sys.column_store_row_groups** , чтобы определить, какие группы строк имеют высокий процент удаленных строк и должны быть перестроены.  
   
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**object_id**|int|Идентификатор таблицы, для которой определен индекс.|  
|**index_id**|int|Идентификатор индекса таблицы, в которой содержится этот индекс columnstore.|  
|**partition_number**|int|Идентификатор секции таблицы, содержащей идентификатор row_group_id группы строк. Partition_number можно использовать для соединения этого динамического административного представления с представлением sys.partitions.|  
|**row_group_id**|int|Номер группы строк, связанный с этой группой строк. Он уникален внутри секции.<br /><br /> -1 = Заключительный фрагмент таблицы в памяти.|  
|**delta_store_hobt_id**|BIGINT|Hobt_id для открытой группы строк в разностном хранилище.<br /><br /> Значение NULL, если группа строк не находится в разностном хранилище.<br /><br /> Значение NULL для заключительного фрагмента таблицы в памяти.|  
|**state**|tinyint|Идентификатор, связанный с параметром state_description.<br /><br /> 0 = INVISIBLE<br /><br /> 1 = OPEN;<br /><br /> 2 = CLOSED<br /><br /> 3 = COMPRESSED <br /><br /> 4 = ЗАХОРОНЕНИЕ|  
|**state_description**|nvarchar(60)|Описание сохраняемого состояния группы строк:<br /><br /> Невидимый — скрытый сжатый сегмент в процессе построения из данных в разностном хранилище. Операции чтения будут использовать разностное хранилище до момента построения скрытого сжатого сегмента. Когда новый сегмент станет видимым, исходное разностное хранилище будет удалено.<br /><br /> ОТКРЫТЬ — группа строк для чтения и записи, которая принимает новые записи. Открытая группа строк остается в формате rowstore и не сжимается в формат columnstore.<br /><br /> ЗАКРЫТо — группа строк, которая была заполнена, но еще не сжата процессом перемещения кортежей.<br /><br /> СЖАТЫЙ — группа строк, которая была заполнена и сжата.|  
|**total_rows**|BIGINT|Общее число строк, которые физически хранятся в группе строк. Некоторые из строк могли быть удалены, но хранятся и дальше. Максимальное количество строк в группе — 1 048 576 (FFFFF в шестнадцатеричном формате).|  
|**deleted_rows**|BIGINT|Общее число строк в группе строк, которые отмечены как удаленные. Это значение всегда равно 0 для групп строк DELTA.|  
|**size_in_bytes**|BIGINT|Размер в байтах всех данных в этой группе строк (не включая метаданные или общие словари), как для групп строк DELTA и COLUMNSTORE.|  
  
## <a name="remarks"></a>Комментарии  
 Возвращает одну строку для каждой группы строк columnstore для каждой таблицы с кластеризованным или некластеризованным индексом columnstore.  
  
 Используйте **sys.column_store_row_groups** , чтобы определить количество строк, включаемых в группу строк, и размер группы строк.  
  
 Если количество удаленных строк в группе становится большим по отношению к общему числу строк, таблица становится менее эффективной. Перестройте индекс columnstore, чтобы сократить размер таблицы и уменьшить число дисковых операций ввода-вывода, необходимых для чтения таблицы. Для перестроения индекса columnstore используйте параметр **REBUILD** инструкции **ALTER INDEX** .  
  
 Обновляемый columnstore сначала вставляет новые данные в **открытый** группы строк, который находится в формате rowstore, а также иногда называется разностной таблицей.  После заполнения открытого группы строк его состояние меняется на **Closed**. Закрытый группы строк сжимается в формате columnstore с помощью перемещения кортежей, а состояние изменяется на **сжатое**.  Процесс перемещения кортежей — это фоновый процесс, который периодически активируется и проверяет, существуют ли закрытые группы строк, готовые к сжатию в группу строк columnstore.  Процесс перемещения кортежей также освобождает все группы строк, в которых удалены все строки. Освобожденные групп строк помечены как **захоронение**. Для немедленного запуска перемещения кортежей используйте параметр **реорганизовать** инструкции **ALTER INDEX** .  
  
 Когда группа строк columnstore заполняется, она сжимаются и прекращает принимать новые строки. Когда строки удаляются из сжатой группы, они сохраняются в ней, но отмечаются как удаленные. Обновления сжатой группы реализуются как удаление из сжатой группы и вставка в открытую группу.  
  
## <a name="permissions"></a>Разрешения  
 Возвращает сведения для таблицы, если пользователь имеет `VIEW DEFINITION` разрешение на таблицу.  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Примеры  
 Следующий пример соединяет таблицу **sys.column_store_row_groups** с другими системными таблицами для получения сведений о конкретных таблицах. Вычисляемый столбец `PercentFull` — это оценка эффективности группы строк. Чтобы найти сведения об одной таблице, удалите дефисы в комментариях перед предложением **WHERE** и укажите имя таблицы.  
  
```sql  
SELECT i.object_id, object_name(i.object_id) AS TableName,   
i.name AS IndexName, i.index_id, i.type_desc,   
CSRowGroups.*,   
100*(total_rows - ISNULL(deleted_rows,0))/total_rows AS PercentFull    
FROM sys.indexes AS i  
JOIN sys.column_store_row_groups AS CSRowGroups  
    ON i.object_id = CSRowGroups.object_id  
AND i.index_id = CSRowGroups.index_id   
--WHERE object_name(i.object_id) = '<table_name>'   
ORDER BY object_name(i.object_id), i.name, row_group_id;  
```  
  
## <a name="see-also"></a>См. также  
 [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Запросы к системному каталогу SQL Server вопросы и ответы](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [sys.columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.all_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-all-columns-transact-sql.md)   
 [sys.computed_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-computed-columns-transact-sql.md)   
 [Руководство по индексам columnstore](~/relational-databases/indexes/columnstore-indexes-overview.md)     
 [sys.column_store_dictionaries &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-dictionaries-transact-sql.md)   
 [sys.column_store_segments (Transact-SQL)](../../relational-databases/system-catalog-views/sys-column-store-segments-transact-sql.md)  
  
  

