---
description: sys.dm_xtp_gc_queue_stats (Transact-SQL)
title: sys.dm_xtp_gc_queue_stats (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_xtp_gc_stats
- dm_xtp_gc_stats_TSQL
- sys.dm_xtp_gc_stats_TSQL
- sys.dm_xtp_gc_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_xtp_gc_stats dynamic management view
ms.assetid: addef774-318d-46a7-85df-f93168a800cb
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017
ms.openlocfilehash: 98d27fff17b897c7dfaba34b7afd2e21181a5cef
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096465"
---
# <a name="sysdm_xtp_gc_queue_stats-transact-sql"></a>sys.dm_xtp_gc_queue_stats (Transact-SQL)

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Выводит сведения о каждой рабочей очереди подсистемы сборки мусора на сервере и различные статистические данные о них. Имеется только одна очередь на каждый логический ЦП.  
  
 Основной поток сборки мусора (бездействующий поток) отслеживает обновленные, удаленные и вставленные строки для всех транзакций, выполненных с момента последнего вызова основного потока сборки мусора. Когда поток сборки мусора начинает работать, он определяет, изменилась ли метка времени самой старой активной транзакции. Если самая старая транзакция изменилась, то бездействующий поток ставит в очередь элементы работы (фрагменты данных по 16 строк) для транзакций, наборы записи которых больше не нужны. Например, если вы удаляете 1024 строки, то спустя некоторое время в очереди появятся 64 элемента работы сборки мусора, каждый из которых будет содержать 16 удаленных строк.  Пользовательская транзакция после фиксации выбирает все поставленные в очередь элементы в своем планировщике. Если в планировщике нет помещенных в очередь элементов, пользовательская транзакция выполнит поиск во всех очередях в текущем узле NUMA.  
  
 Для определения того, высвобождает ли сборка мусора память для удаленных строк, выполните sys.dm_xtp_gc_queue_stats, чтобы видеть, обрабатывается ли поставленная в очередь работа. Если записи в current_queue_depth не обрабатываются или в current_queue_depth не добавляются новые рабочие элементы, это означает, что сборка мусора не освобождает память. Например, сборка мусора не может быть выполнена при длительной транзакции.  
  
 Дополнительные сведения см. в разделе [In-Memory OLTP (оптимизация в памяти)](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  

|Имя столбца|Тип|Описание|  
|-----------------|----------|-----------------|  
|queue_id|**int**|Уникальный идентификатор очереди.|  
|total_enqueues|**bigint**|Общее число рабочих элементов подсистемы сборки мусора, поставленных в эту очередь с момента запуска сервера.|  
|total_dequeues|**bigint**|Общее число рабочих элементов подсистемы сборки мусора, выведенных из этой очереди с момента запуска сервера.|  
|current_queue_depth|**bigint**|Текущее количество рабочих элементов подсистемы сборки мусора в этой очереди. Этот элемент может привести к сборке мусора одного или нескольких элементов.|  
|maximum_queue_depth|**bigint**|Максимальная глубина этой очереди.|  
|last_service_ticks|**bigint**|Метки времени ЦП на момент последнего обслуживания очереди.|  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение VIEW SERVER STATE.  
  
## <a name="user-scenario"></a>Пользовательский сценарий  
 Этот выход показывает, что [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполняется на 4 ядрах либо что экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] был сопоставлен с 4 ядрами.  
  
 Этот выход показывает, что в очередях нет рабочих элементов, которые необходимо обработать. Для очереди 0 общее число рабочих элементов, удаленных из очереди с момента запуска SQL, составляет 15625, а максимальная глубина очереди — 15625.  
  
```  
queue_id total_enqueues total_dequeues current_queue_depth  maximum_queue_depth  last_service_ticks  
----------------------------------------------------------------------------------------------------  
0        15625                15625    0                    15625                1233573168347  
1        15625                15625    0                    15625                1234123295566  
2        15625                15625    0                    15625                1233569418146  
3        15625                15625    0                    15625                1233571605761  
```  
  
## <a name="see-also"></a>См. также:  
 [Оптимизированные для памяти динамические административные представления таблиц &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
