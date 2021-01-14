---
description: sys.dm_db_incremental_stats_properties (Transact-SQL)
title: sys.dm_db_incremental_stats_properties (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 12/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_db_incremental_stats_properties
- sys.dm_db_incremental_stats_properties_TSQL
- dm_db_incremental_stats_properties
- dm_db_incremental_stats_properties_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_incremental_stats_properties
ms.assetid: aa0db893-34d1-419c-b008-224852e71307
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 27585efd99f537e6b2f6d3082c341533e8bbea3b
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172096"
---
# <a name="sysdm_db_incremental_stats_properties-transact-sql"></a>sys.dm_db_incremental_stats_properties (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  Возвращает свойства добавочной статистики для указанного объекта базы данных (таблицы) из текущей базы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Использование `sys.dm_db_incremental_stats_properties` (который содержит номер секции) аналогичен `sys.dm_db_stats_properties` , который используется для недобавочной статистики. 
  
  Эта функция была введена в [!INCLUDE[ssSQL14_md](../../includes/sssql14-md.md)] пакет обновления 2 (SP2) и [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] пакет обновления 1 (SP1).
  
## <a name="syntax"></a>Синтаксис  
  
```  
sys.dm_db_incremental_stats_properties (object_id, stats_id)  
```  
  
## <a name="arguments"></a>Аргументы  
 *object_id*  
 Идентификатор объекта текущей базы данных, для которого запрашиваются свойства одной из добавочных статистик. *object_id* имеет **тип int**.  
  
 *stats_id*  
 Идентификатор статистики для указанного аргумента *object_id*. Идентификатор статистики может быть получен из динамического административного представления [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) . *stats_id* имеет тип **int**.  
  
## <a name="table-returned"></a>Возвращаемая таблица  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|object_id|**int**|Идентификатор объекта (таблицы), для которого возвращаются свойства объекта статистики.|  
|stats_id|**int**|Идентификатор объекта статистики. Уникален в пределах таблицы. Дополнительные сведения см. в статье [sys.stats (Transact-SQL)](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md).|
|partition_number|**int**|Номер секции, содержащей часть таблицы.|  
|last_updated|**datetime2**|Дата и время последнего обновления объекта статистики. Дополнительные сведения см. в подразделе [Примечания](#Remarks) на этой странице.|  
|rows|**bigint**|Общее число строк в таблице при последнем обновлении статистики. Если статистика отфильтрована или соответствует отфильтрованному индексу, количество строк может быть меньше, чем количество строк в таблице.|  
|rows_sampled|**bigint**|Общее количество строк, выбранных для статистических вычислений.|  
|steps|**int**|Число шагов в гистограмме. Дополнительные сведения см. в статье [DBCC SHOW_STATISTICS (Transact-SQL)](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md).|  
|unfiltered_rows|**bigint**|Общее количество строк в таблице до применения критерия фильтра (для отфильтрованной статистики). Если статистика не отфильтрована, то unfiltered_rows равно значению, которое возвращается в столбце rows.|  
|modification_counter|**bigint**|Общее количество изменений в начальном столбце статистики (на основе которого строится гистограмма) с момента последнего обновления статистики.<br /><br /> Этот столбец не содержит сведения для таблиц, оптимизированных для памяти.|  
  
## <a name="remarks"></a><a name="Remarks"></a> Замечания  
 `sys.dm_db_incremental_stats_properties` возвращает пустой набор строк, если выполняется любое из следующих условий:  
  
-   `object_id` или `stats_id` имеет значение NULL.   
-   Указанный объект не найден или не соответствует таблице с добавочной статистикой.  
-   Указанный идентификатор статистики не соответствует имеющейся статистике для указанного идентификатора объекта статистики.  
-   Текущий пользователь не имеет разрешений на просмотр объекта статистики.
 
 Это поведение позволяет безопасно использовать представление `sys.dm_db_incremental_stats_properties` при перекрестном применении к строкам в таких представлениях, как `sys.objects` и `sys.stats`. Этот метод может возвращать для статистики свойства, соответствующие каждой секции. Чтобы просмотреть свойства по всем секциям для объединенной статистики, используйте вместо этого sys.dm_db_stats_properties. 

Дата обновления статистики хранится в [большом двоичном объекте статистики](../../relational-databases/statistics/statistics.md#DefinitionQOStatistics) вместе с [гистограммой](../../relational-databases/statistics/statistics.md#histogram) и [вектором плотности](../../relational-databases/statistics/statistics.md#density), а не в метаданных. Если данные не считываются для создания статистических данных, то большой двоичный объект статистики не создается, дата недоступна, а *last_updated* столбец имеет значение null. Это происходит с отфильтрованной статистикой, для которой предикат не возвращает ни одной строки, или с новыми пустыми таблицами.

## <a name="permissions"></a>Разрешения  
 Требуется наличие у пользователя разрешения на выбор столбцов статистики либо то, чтобы пользователь был владельцем таблицы или членом предопределенной роли сервера `sysadmin`, предопределенной роли базы данных `db_owner` или предопределенной роли базы данных `db_ddladmin`.  
  
## <a name="examples"></a>Примеры  

### <a name="a-simple-example"></a>A. Простой пример
Следующий пример возвращает статистику для таблицы `PartitionTable` , описанной в разделе [Создание секционированных таблиц и индексов](../../relational-databases/partitions/create-partitioned-tables-and-indexes.md).

```sql
SELECT * FROM sys.dm_db_incremental_stats_properties (object_id('PartitionTable'), 1);
``` 

Дополнительные предложения по использованию см. в разделе  [sys.dm_db_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md).
  
## <a name="see-also"></a>См. также:  
 [DBCC SHOW_STATISTICS (Transact-SQL)](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [sys.stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md)   
 [Динамические административные представления и функции, связанные с объектом, &#40;языке Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/object-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
 [sys.dm_db_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md)   
 [sys.dm_db_stats_histogram (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md) 
