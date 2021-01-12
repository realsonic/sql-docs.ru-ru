---
description: sys.dm_xtp_gc_stats (Transact-SQL)
title: sys.dm_xtp_gc_stats (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
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
ms.assetid: 8287d611-50e3-43e1-ba8d-3e3793d3ba0e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a25b5e02c4a89bce85ae65575f80a588163074f7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096414"
---
# <a name="sysdm_xtp_gc_stats-transact-sql"></a>sys.dm_xtp_gc_stats (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Предоставляет сведения (общую статистику) о текущем поведении процесса сборки мусора [!INCLUDE[hek_2](../../includes/hek-2-md.md)].  
  
 Уборка строк производится либо в рамках стандартной обработки транзакций, либо основным потоком сборки мусора, который называется простаивающим исполнителем. Когда пользовательская транзакция фиксируется, она вымещает один рабочий элемент из очереди сборки мусора ([sys.dm_xtp_gc_queue_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xtp-gc-queue-stats-transact-sql.md)). Все строки, которые могли быть удалены, но не были затронуты основной пользовательской транзакцией, обрабатываются простаивающим исполнителем в рамках сканирования «пыльных углов» (сканирования областей индекса, которые реже используются).  
  
 Дополнительные сведения см. в разделе [In-Memory OLTP (оптимизация в памяти)](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
|Имя столбца|Тип|Описание|  
|-----------------|----------|-----------------|  
|rows_examined|**bigint**|Число строк, просмотренных подсистемой сборки мусора после запуска сервера.|  
|rows_no_sweep_needed|**bigint**|Количество строк, которые были удалены без сканирования «пыльных углов».|  
|rows_first_in_bucket|**bigint**|Число строк, просмотренных подсистемой сборки мусора, которые были первой строкой в хэш-контейнере.|  
|rows_first_in_bucket_removed|**bigint**|Число строк, просмотренных подсистемой сборки мусора, которые были первой строкой в удаленном хэш-контейнере.|  
|rows_marked_for_unlink|**bigint**|Число строк, просмотренных подсистемой сборки мусора, которые уже были отмечены как разорвавшие связь в индексах с ref_count =0.|  
|parallel_assist_count|**bigint**|Число строк, обработанных пользовательскими транзакциями.|  
|idle_worker_count|**bigint**|Число строк мусора, обработанных простаивающим исполнителем.|  
|sweep_scans_started|**bigint**|Количество сканирований «пыльных углов», выполненных подсистемой сборки мусора.|  
|sweep_scans_retries|**bigint**|Количество сканирований «пыльных углов», выполненных подсистемой сборки мусора.|  
|sweep_rows_touched|**bigint**|Строки, прочитанные при обработке «пыльных углов».|  
|sweep_rows_expiring|**bigint**|Строки, срок действия которых истекает, прочитанные при обработке «пыльных углов».|  
|sweep_rows_expired|**bigint**|Строки, срок действия которых истек, прочитанные при обработке «пыльных углов».|  
|sweep_rows_expired_removed|**bigint**|Строки, срок действия которых истек, удаленные при обработке «пыльных углов».|  
  
## <a name="permissions"></a>Разрешения  
 Требует разрешения VIEW SERVER STATE на экземпляр.  
  
## <a name="usage-scenario"></a>Сценарии использования  
 Ниже приведен образец выходных данных.  
  
```  
rows_examined        rows_no_sweep_needed rows_first_in_bucket rows_first_in_bucket_removed  
280085               209512               69905  
rows_first_in_bucket_removed rows_marked_for_unlink parallel_assist_count idle_worker_count  
69905                        0                      8953  
  
idle_worker_count    sweep_scans_started  sweep_scan_retries   sweep_rows_touched  
10306473             670                  0                    1343  
  
sweep_rows_expiring  sweep_rows_expired   sweep_rows_expired_removed  
               0                 673673  
```  
  
## <a name="see-also"></a>См. также:  
 [Оптимизированные для памяти динамические административные представления таблиц &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
