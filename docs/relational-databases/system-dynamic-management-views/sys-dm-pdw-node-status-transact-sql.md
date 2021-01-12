---
description: sys.dm_pdw_node_status (Transact-SQL)
title: sys.dm_pdw_node_status (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 8e263b65-81d0-49d0-8873-62ef424369d6
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: 520ee2be8bfce8c6913be55be8f3a479e90ac448
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092741"
---
# <a name="sysdm_pdw_node_status-transact-sql"></a>sys.dm_pdw_node_status (Transact-SQL)

[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  Содержит дополнительные сведения (более [sys.dm_pdw_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-transact-sql.md)) о производительности и состоянии всех узлов устройств. В нем отображается одна строка для каждого узла в устройстве.  
  
|Имя столбца|Тип данных|Description|Диапазон|  
|-----------------|---------------|-----------------|-----------|  
|pdw_node_id|**int**|Уникальный числовой идентификатор, связанный с узлом.<br /><br /> Ключ для этого представления.|Уникальным для устройства, независимо от типа.|  
|process_id|**int**|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]||  
|process_name|**nvarchar(255)**|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]||  
|allocated_memory|**bigint**|Общая выделенная память на этом узле.||  
|available_memory|**bigint**|Общая объем доступной памяти на этом узле.||  
|process_cpu_usage|**bigint**|Общее использование ЦП процесса в тактах.||  
|total_cpu_usage|**bigint**|Общее использование ЦП в тактах.||  
|thread_count|**bigint**|Общее число потоков, используемых на этом узле.||  
|handle_count|**bigint**|Общее число используемых дескрипторов на этом узле.||  
|total_elapsed_time|**bigint**|Общее время, прошедшее с момента запуска или перезапуска системы.|Общее время, прошедшее с момента запуска или перезапуска системы. Если total_elapsed_time превышает максимальное значение для целого числа (24,8 дней в миллисекундах), это приведет к сбою материализации из-за переполнения.<br /><br /> Максимальное значение в миллисекундах эквивалентно 24,8 дням.|  
|is_available|**bit**|Флаг, указывающий, доступен ли этот узел.||  
|sent_time|**datetime**|Время последней отправки сетевого пакета этим узлом.||  
|received_time|**datetime**|Время последнего получения сетевого пакета этим узлом.||  
|error_id|**nvarchar (36)**|Уникальный идентификатор последней ошибки, произошедшей на этом узле.||  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления Azure синапсе Analytics и Параллельное хранилище данных &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
