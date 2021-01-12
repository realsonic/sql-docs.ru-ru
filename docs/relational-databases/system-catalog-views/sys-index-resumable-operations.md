---
description: sys.index_resumable_operations (Transact-SQL)
title: sys.index_resumable_operations (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 11/12/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.index_resumable_operations_TSQL
- sys.indexes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.indexes
- sys.index_resumable_operations
ms.assetid: ''
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 97b02afea0dc4756783ad5197a24b5ca5d952282
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094533"
---
# <a name="sysindex_resumable_operations-transact-sql"></a>sys.index_resumable_operations (Transact-SQL)

[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]
**sys.index_resumable_operations** — это системное представление, которое отслеживает и проверяет текущее состояние выполнения возобновляемого перестроения индекса или создания.  
Область **применения**: SQL Server (2017 и более поздние версии) и база данных SQL Azure.
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Идентификатор объекта, которому принадлежит этот индекс (не допускает значения NULL).|  
|**index_id**|**int**|Идентификатор индекса (не допускает значения NULL). **index_id** уникален только в пределах объекта.|
|**name**|**sysname**|Имя индекса. **имя** уникально только в пределах объекта.|  
|**sql_text**|**nvarchar(max)**|Текст инструкции DDL T-SQL|
|**last_max_dop**|**smallint**|Использован последний MAX_DOP (по умолчанию = 0)|
|**partition_number**|**int**|Номер секции в индексе или куче-владельце. Для несекционированных таблиц и индексов, если выполняется перестроение всех секций, значение этого столбца равно NULL.|
|**state**|**tinyint**|Операционное состояние для возобновляемого индекса:<br /><br />0 = работает<br /><br />1 = приостановка|
|**state_desc**|**nvarchar(60)**|Описание операционного состояния возобновляемого индекса (запущенного или приостановленного)|  
|**start_time**|**datetime**|Время начала операции с индексами (не допускает значения NULL)|
|**last_pause_time**|**datatime**| Время последней паузы операции с индексом (Nullable). Значение NULL, если операция выполняется и никогда не приостанавливается.|
|**total_execution_time;**|**int**|Общее время выполнения от времени начала в минутах (не допускает значения NULL)|
|**percent_complete**|**real**|Выполнение операции с индексами в% (не допускает значения NULL).|
|**page_count**|**bigint**|Общее число страниц индекса, выделенных операцией построения индекса для новых и сопоставленных индексов (не допускает значения NULL).

## <a name="permissions"></a>Разрешения

[!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  

## <a name="example"></a>Пример

 Перечисление всех операций создания или перестроения индекса, которые находятся в состоянии приостановки.

```sql
SELECT * FROM  sys.index_resumable_operations WHERE STATE = 1;  
```

## <a name="see-also"></a>См. также:

- [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md)
- [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md)
- [Представления каталога](catalog-views-transact-sql.md)
- [Представления каталога объектов](object-catalog-views-transact-sql.md)
- [sys.indexes](sys-xml-indexes-transact-sql.md)
- [sys.index_columns](sys-index-columns-transact-sql.md)
- [sys.xml_indexes](sys-xml-indexes-transact-sql.md)
- [sys.objects](sys-index-columns-transact-sql.md)
- [sys.key_constraints](sys-key-constraints-transact-sql.md)
- [sys.filegroups](sys-filegroups-transact-sql.md)
- [sys.partition_schemes](sys-partition-schemes-transact-sql.md)
- [Часто задаваемые вопросы о запросах к системному каталогу сервера SQL Server](querying-the-sql-server-system-catalog-faq.md)
