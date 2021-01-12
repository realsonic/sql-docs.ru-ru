---
description: sys.column_store_dictionaries (Transact-SQL)
title: sys.column_store_dictionaries (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.column_store_dictionaries_TSQL
- column_store_dictionaries
- column_store_dictionaries_TSQL
- sys.column_store_dictionaries
dev_langs:
- TSQL
helpviewer_keywords:
- sys.column_store_dictionaries catalog view
ms.assetid: 56efd563-2f72-4caf-94e3-8a182385c173
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 86c749ab8fdad447c477dd4f424cab3ab1d6b086
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092527"
---
# <a name="syscolumn_store_dictionaries-transact-sql"></a>sys.column_store_dictionaries (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Содержит по одной строке для каждого словаря, используемого в индексах columnstore, оптимизированных для памяти xVelocity. Словари используются для кодирования некоторых, но не всех типов данных, поэтому не все столбцы в индексе columnstore имеют словари. Словарь может существовать в качестве основного словаря (для всех сегментов) и, возможно, для других вспомогательных словарей, используемых для подмножества сегментов столбца.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**hobt_id**|**bigint**|Идентификатор индекса куча или сбалансированное дерево (HoBT) для таблицы, которая имеет этот индекс columnstore.|  
|**column_id**|**int**|Идентификатор столбца columnstore, начинающийся с 1. Первый столбец имеет идентификатор 1, второй столбец имеет идентификатор = 2 и т. д.|  
|**dictionary_id**|**int**|Можно использовать два вида словарей — глобальные и локальные, связанные с сегментом столбца. Dictionary_id 0 представляет глобальный словарь, который совместно используется во всех сегментах столбцов (по одному для каждой группы строк) для этого столбца.|  
|**version**|**int**|Версия формата словаря.|  
|**type**|**int**|Тип словаря:<br /><br /> 1 — хэш-словарь, содержащий значения **int**<br /><br /> 2 — не используется<br /><br /> 3 — хэш-словарь, содержащий строковые значения<br /><br /> 4. хэш-словарь, содержащий значения **float**<br /><br /> Дополнительные сведения о словарях см. в разделе [инструкции по индексам columnstore](~/relational-databases/indexes/columnstore-indexes-overview.md).|  
|**last_id**|**int**|Последний идентификатор данных в словаре.|  
|**entry_count**|**bigint**|Количество записей в словаре.|  
|**on_disk_size**|**bigint**|Размер словаря в байтах.|  
|**partition_id**|**bigint**|Указывает идентификатор секции. Уникален в базе данных.|  
  
## <a name="permissions"></a>Разрешения  
Необходимо разрешение `VIEW DEFINITION` для таблицы. Следующие столбцы возвращают значение null, если пользователь также не имеет `SELECT` разрешения: last_id, ENTRY_COUNT, data_ptr.  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Запросы к системному каталогу SQL Server вопросы и ответы](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [sys.columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.all_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-all-columns-transact-sql.md)   
 [sys.computed_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-computed-columns-transact-sql.md)   
 [Руководство по индексам columnstore](~/relational-databases/indexes/columnstore-indexes-overview.md)   
 [Руководство по индексам columnstore](~/relational-databases/indexes/columnstore-indexes-overview.md)   
 [sys.column_store_segments (Transact-SQL)](../../relational-databases/system-catalog-views/sys-column-store-segments-transact-sql.md)  
  
  

