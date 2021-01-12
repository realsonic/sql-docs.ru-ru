---
description: sys.dm_db_column_store_row_group_operational_stats (Transact-SQL)
title: sys.dm_db_column_store_row_group_operational_stats (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 31b71c68-50a0-4fd8-a7fe-2d2292be1163
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e8ff8ab3db826d483f8f6df09277e1a5510aaf48
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097714"
---
# <a name="sysdm_db_column_store_row_group_operational_stats-transact-sql"></a>sys.dm_db_column_store_row_group_operational_stats (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Возвращает активность ввода-вывода на уровне строк, блокировку и метод доступа для сжатых групп строк в индексе columnstore. Используйте **sys.dm_db_column_store_row_group_operational_stats** , чтобы отслеживать продолжительность времени, в течение которого пользовательский запрос должен ожидать чтения или записи в сжатую группы строк или секцию индекса columnstore, а также определить групп строк, которые сталкиваются с значительными операциями ввода-вывода или горячими областями.  
  
 Индексы columnstore в памяти не отображаются в этом динамическом административном отображении.  
 
 
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Идентификатор таблицы с индексом columnstore.|  
|**index_id**|**int**|Идентификатор индекса columnstore.|  
|**partition_number**|**int**|Номер секции внутри индекса или кучи (нумерация начинается с 1).|  
|**row_group_id**|**int**|Идентификатор группы строк в индексе columnstore. Это значение уникально в пределах секции.|  
|**scan_count**|**int**|Число просмотров с помощью группы строк с момента последнего перезапуска SQL.|  
|**delete_buffer_scan_count**|**int**|Сколько раз буфер удаления использовался для определения удаленных строк в этом группы строк. Это включает доступ к Hashtable в памяти и базовому сбалансированного дерева.|  
|**index_scan_count**|**int**|Количество отсканированных секций индекса columnstore. Это одинаково для всех групп строк в секции.|  
|**rowgroup_lock_count**|**bigint**|Совокупное число запросов на блокировку для этого группы строк с момента последнего перезапуска SQL.|  
|**rowgroup_lock_wait_count**|**bigint**|Совокупное количество раз, когда ядро СУБД ожидало эту блокировку группы строк с момента последнего перезапуска SQL.|  
|**rowgroup_lock_wait_in_ms**|**bigint**|Совокупное количество миллисекунд, затраченное ядром СУБД на эту группы строк блокировку с момента последнего перезапуска SQL.|  
  
## <a name="permissions"></a>Разрешения  
 Необходимы следующие разрешения:  
  
-   Разрешение CONTROL на таблицу, заданную параметром object_id.  
  
-   Разрешение Просмотр состояния базы данных для получения сведений обо всех объектах в базе данных с помощью шаблона объекта @*object_id* = null.  
  
 Предоставление разрешения VIEW DATABASE STATE позволяет всем объектам в базе данных быть возвращаемыми, независимо от любых разрешений CONTROL, запрещенных для определенных объектов.  
  
 Запрет разрешения VIEW DATABASE STATE запрещает всем объектам в базе данных быть возвращаемыми, независимо от любых разрешений CONTROL, предоставленных на определенные объекты. Кроме того, если указан шаблон базы данных @*database_id*= null, то база данных опускается.  
  
 Дополнительные сведения см. в разделе [динамические административные представления и функции &#40;&#41;Transact-SQL ](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Динамические административные представления и функции, связанные с индексами &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Наблюдение и настройка производительности](../../relational-databases/performance/monitor-and-tune-for-performance.md)   
 [sys.dm_db_index_physical_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)   
 [sys.dm_db_index_usage_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-usage-stats-transact-sql.md)   
 [sys.dm_os_latch_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-latch-stats-transact-sql.md)   
 [sys.dm_db_partition_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql.md)   
 [sys.allocation_units (Transact-SQL)](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)   
 [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)  
  
  

