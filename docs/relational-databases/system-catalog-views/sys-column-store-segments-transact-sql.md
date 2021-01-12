---
description: sys.column_store_segments (Transact-SQL)
title: sys.column_store_segments (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 09/24/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- column_store_segments
- sys.column_store_segments_TSQL
- sys.column_store_segments
- column_store_segments_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.column_store_segments catalog view
ms.assetid: 1253448c-2ec9-4900-ae9f-461d6b51b2ea
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 189a5cca5cfac0cce6437ccc256d461140f3c40b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095614"
---
# <a name="syscolumn_store_segments-transact-sql"></a>sys.column_store_segments (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

Возвращает по одной строке для каждого сегмента столбца в индексе columnstore. Для каждого столбца группы строк имеется один сегмент столбца. Например, таблица с 10 групп строк и 34 столбцами возвращает 340 строк. 
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**partition_id**|**bigint**|Указывает идентификатор секции. Уникален в базе данных.|  
|**hobt_id**|**bigint**|Идентификатор индекса куча или сбалансированное дерево (HoBT) для таблицы, которая имеет этот индекс columnstore.|  
|**column_id**|**int**|Идентификатор столбца columnstore.|  
|**segment_id**|**int**|Идентификатор группы строк. Для обеспечения обратной совместимости имя столбца будет по-segment_id, несмотря на то, что это идентификатор группы строк. Можно однозначно идентифицировать сегмент с помощью \<hobt_id, partition_id, column_id> , <segment_id>.|  
|**version**|**int**|Версия формата сегмента столбца.|  
|**encoding_type**|**int**|Тип кодировки, используемой для этого сегмента:<br /><br /> 1 = VALUE_BASED-нестроковый/двоичный без словаря (аналогично 4 с некоторыми внутренними вариациями)<br /><br /> 2 = VALUE_HASH_BASED — нестроковый или двоичный столбец с общими значениями в словаре<br /><br /> 3 = STRING_HASH_BASED-строка/двоичный столбец с общими значениями в словаре<br /><br /> 4 = STORE_BY_VALUE_BASED-не строковый/двоичный без словаря<br /><br /> 5 = STRING_STORE_BY_VALUE_BASED-String/binary без словаря<br /><br /> Дополнительные сведения см. в разделе [Примечания](#remarks).|  
|**row_count**|**int**|Число строк в группе строк.|  
|**has_nulls**|**int**|Значение 1, если сегмент столбца содержит значения NULL.|  
|**base_id**|**bigint**|Идентификатор базового значения, если используется тип кодировки 1. Если тип кодировки 1 не используется, base_id устанавливается в значение-1.|  
|**magnitude**|**float**|Величина, если используется тип кодировки 1. Если тип кодировки 1 не используется, то величина устанавливается равным-1.|  
|**primary_dictionary_id**|**int**|Значение 0 представляет глобальный словарь. Значение-1 указывает на отсутствие глобального словаря, созданного для этого столбца.|  
|**secondary_dictionary_id**|**int**|Ненулевое значение указывает на локальный словарь для этого столбца в текущем сегменте (т. е. группы строк). Значение-1 указывает на отсутствие локального словаря для этого сегмента.|  
|**min_data_id**|**bigint**|Минимальный идентификатор данных в сегменте столбца.|  
|**max_data_id**|**bigint**|Максимальный идентификатор данных в сегменте столбца.|  
|**null_value**|**bigint**|Значение, используемое для представления значений NULL.|  
|**on_disk_size**|**bigint**|Размер сегмента в байтах.|  
  
## <a name="remarks"></a>Комментарии  
Тип кодировки сегмента columnstore выбирается [!INCLUDE[ssde_md](../../includes/ssde_md.md)] с целью достижения минимальных затрат на хранение путем анализа данных сегмента. Если данные в основном отличаются, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] используется кодировка на основе значений. Если данные в основном не отличаются, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] используется кодировка на основе хэша. Вариант кодировки, основанной на строках и значениях, связан с типом сохраняемых данных, будь то символьные данные или двоичные данные. Все кодировки по возможности используют преимущества битовой упаковки и длины выполнения.
 
## <a name="permissions"></a>Разрешения  
 Для всех столбцов требуется по крайней мере `VIEW DEFINITION` разрешение на таблицу. Следующие столбцы возвращают значение null, если пользователь также не имеет `SELECT` разрешения: `has_nulls` , `base_id` , `magnitude` ,, `min_data_id` `max_data_id` и `null_value` .  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  

## <a name="examples"></a>Примеры

### <a name="the-following-query-returns-information-about-segments-of-a-columnstore-index"></a>Следующий запрос возвращает сведения о сегментах индекса columnstore.  
  
```sql  
SELECT i.name, p.object_id, p.index_id, i.type_desc,   
    COUNT(*) AS number_of_segments  
FROM sys.column_store_segments AS s   
INNER JOIN sys.partitions AS p   
    ON s.hobt_id = p.hobt_id   
INNER JOIN sys.indexes AS i   
    ON p.object_id = i.object_id  
WHERE i.type = 5 OR i.type = 6  
GROUP BY i.name, p.object_id, p.index_id, i.type_desc ;  
GO  
```  

## <a name="see-also"></a>См. также:  
 [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Запросы к системному каталогу SQL Server вопросы и ответы](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [sys.columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.all_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-all-columns-transact-sql.md)   
 [sys.computed_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-computed-columns-transact-sql.md)   
 [Руководство по индексам columnstore](~/relational-databases/indexes/columnstore-indexes-overview.md)    
 [sys.column_store_dictionaries &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-dictionaries-transact-sql.md)  
  
 
