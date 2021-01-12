---
description: sys.dm_xtp_system_memory_consumers (Transact-SQL)
title: sys.dm_xtp_system_memory_consumers (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_xtp_system_memory_consumers
- sys.dm_xtp_system_memory_consumers_TSQL
- sys.dm_xtp_system_memory_consumers
- dm_xtp_system_memory_consumers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_xtp_system_memory_consumers dynamic management view
ms.assetid: 9eb0dd82-7920-42e0-9e50-7ce6e7ecee8b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 20edce0accbe4549b5c109b8d6958fd742cd4ede
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096411"
---
# <a name="sysdm_xtp_system_memory_consumers-transact-sql"></a>sys.dm_xtp_system_memory_consumers (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Сообщает о потребителях памяти системного уровня в [!INCLUDE[hek_2](../../includes/hek-2-md.md)]. Память для этих потребителей получена либо из пула по умолчанию (если выделение находится в контексте пользовательского потока), либо из внутреннего пула (если выделение находится в контексте системного потока).  
  
```  
-- system memory consumers @ instance  
select * from sys.dm_xtp_system_memory_consumers  
```  
  
 Дополнительные сведения см. в разделе [In-Memory OLTP (оптимизация в памяти)](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
|Имя столбца|Тип|Описание|  
|-----------------|----------|-----------------|  
|memory_consumer_id|**bigint**|Внутренний идентификатор потребителя памяти.|  
|memory_consumer_type|**int**|Целое число, представляющее тип потребителя памяти с одним из следующих значений:<br /><br /> 0 — не должно отображаться. Суммирует использование памяти для двух или более потребителей.<br /><br /> 1 — РЕЗЕРВный: отслеживает потребление памяти для резерва системы.<br /><br /> 2 — VARHEAP: отслеживает потребление памяти для кучи переменной длины.<br /><br /> 4. Пул страниц ввода-вывода: отслеживает потребление памяти для пула системных страниц, используемого для операций ввода-вывода.|  
|memory_consumer_type_desc|**nvarchar (16)**|Описание типа потребителя памяти.<br /><br /> 0 — не должно отображаться.<br /><br /> 1 — РЕЗЕРВНЫЙ<br /><br /> 2 — VARHEAP<br /><br /> 4 — PGPOOL|  
|memory_consumer_desc|**nvarchar (64)**|Описание экземпляра потребителя памяти.<br /><br /> VARHEAP <br />Системная куча. Общее назначение. В настоящее время используется только для выделения рабочих элементов сборки мусора.<br />-или-<br />Куча резервного блока памяти. Используется резервными блоками памяти, когда количество элементов в списке достигает определенного предела (обычно около 5000 элементов).<br /><br /> PGPOOL. для пулов систем ввода-вывода существует три разных размера: пул системных страниц в 4 КБ, Пул страниц 64 K и пул страниц 256 K.|  
|lookaside_id|**bigint**|Идентификатор поставщика конкретных потоков локальной памяти резервного блока.|  
|pagepool_id|**bigint**|Идентификатор поставщика конкретных потоков локальной памяти пула страниц.|  
|allocated_bytes|**bigint**|Число байтов, зарезервированных для этого потребителя памяти.|  
|used_bytes|**bigint**|Число байтов, используемых этим потребителем. Применимо только для потребителей памяти varheap.|  
|allocation_count|**int**|Количество выделений.|  
|partition_count|**int**|Только для внутреннего использования.|  
|sizeclass_count|**int**|Только для внутреннего использования.|  
|min_sizeclass|**int**|Только для внутреннего использования.|  
|max_sizeclass|**int**|Только для внутреннего использования.|  
|memory_consumer_address|**varbinary**|Внутренний адрес потребителя памяти.|  
  
## <a name="permissions"></a>Разрешения  
 Необходимы разрешения VIEW SERVER STATE на сервере.  
  
## <a name="user-scenario"></a>Пользовательский сценарий  
  
```  
-- system memory consumers @ instance  
selectmemory_consumer_type_desc,   
allocated_bytes/1024 as allocated_bytes_kb,   
used_bytes/1024 as used_bytes_kb, allocation_count  
from sys.dm_xtp_system_memory_consumers  
```  
  
 Вывод показывает всех потребителей памяти на уровне системы. Например, имеются потребители для резервных блоков памяти транзакции.  
  
```  
memory_consumer_type_name           memory_consumer_desc                           allocated_bytes_kb   used_bytes_kb        allocation_count  
-------------------------------          ---------------------                          -------------------  --------------        ----------------  
VARHEAP                                  Lookaside heap                                 0                    0                    0  
VARHEAP                                  System heap                                    768                  0                    2  
LOOKASIDE                                GC transaction map entry                       64                   64                   910  
LOOKASIDE                                Redo transaction map entry                     128                  128                  1260  
LOOKASIDE                                Recovery table cache entry                     448                  448                  8192  
LOOKASIDE                                Transaction recent rows                        3264                 3264                 4444  
LOOKASIDE                                Range cursor                                   0                    0                    0  
LOOKASIDE                                Hash cursor                                    3200                 3200                 11070  
LOOKASIDE                                Transaction save-point set entry               0                    0                    0  
LOOKASIDE                                Transaction partially-inserted rows set        704                  704                  1287  
LOOKASIDE                                Transaction constraint set                     576                  576                  1940  
LOOKASIDE                                Transaction save-point set                     0                    0                    0  
LOOKASIDE                                Transaction write set                          704                  704                  672  
LOOKASIDE                                Transaction scan set                           320                  320                  156  
LOOKASIDE                                Transaction read set                           704                  704                  343  
LOOKASIDE                                Transaction                                    4288                 4288                 1459  
PGPOOL                                   System 256K page pool                          5120                 5120                 20  
PGPOOL                                   System 64K page pool                           0                    0                    0  
PGPOOL                                   System 4K page pool                            24                   24                   6  
```  
  
 Чтобы определить общий объем памяти, потребляемой блоками распределения системы, выполните следующие действия.  
  
```  
select sum(allocated_bytes)/(1024*1024) as total_allocated_MB, sum(used_bytes)/(1024*1024) as total_used_MB   
from sys.dm_xtp_system_memory_consumers  
  
total_allocated_MB   total_used_MB  
-------------------- --------------------  
2                    2  
```  
  
## <a name="see-also"></a>См. также:  
 [Оптимизированные для памяти динамические административные представления таблиц &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
