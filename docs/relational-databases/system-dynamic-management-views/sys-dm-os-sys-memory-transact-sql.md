---
description: sys.dm_os_sys_memory (Transact-SQL)
title: sys.dm_os_sys_memory (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_sys_memory
- sys.dm_os_sys_memory_TSQL
- sys.dm_os_sys_memory
- dm_os_sys_memory_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_sys_memory dynamic management view
ms.assetid: 1ca58814-1caa-44c1-b307-ff0bdcbbef62
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d3bfa7176eea75cc3a494097d3813c6b7198e364
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094042"
---
# <a name="sysdm_os_sys_memory-transact-sql"></a>sys.dm_os_sys_memory (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Возвращает информацию о распределении памяти операционной системы.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ограничен внешним распределением памяти на уровне операционной системы, а также физическими возможностями располагаемого оборудования. Определение общего состояния системы является важной частью процесса распределения памяти в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Чтобы вызвать эту функцию из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте имя **sys.dm_pdw_nodes_os_sys_memory**.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**total_physical_memory_kb**|**bigint**|Общий объем физической памяти, доступной операционной системе, в килобайтах (КБ).|  
|**available_physical_memory_kb**|**bigint**|Объем доступной физической памяти, в КБ.|  
|**total_page_file_kb**|**bigint**|Максимальный объем памяти, выделяемый операционной системой, в килобайтах (КБ).|  
|**available_page_file_kb**|**bigint**|Общий объем файла подкачки, который не используется, в КБ.|  
|**system_cache_kb**|**bigint**|Общий объем памяти системного кэша, в КБ.|  
|**kernel_paged_pool_kb**|**bigint**|Общий объем пула ядра, разбитого на страницы, в КБ.|  
|**kernel_nonpaged_pool_kb**|**bigint**|Общий объем пула ядра, не разбитого на страницы, в КБ.|  
|**system_high_memory_signal_state**|**bit**|Состояние уведомления о достаточном объеме системной памяти. Значение 1 указывает на то, что сигнал о достаточном объеме памяти был задан Windows. Дополнительные сведения см. в разделе [креатемемориресаурценотификатион](/windows/win32/api/memoryapi/nf-memoryapi-creatememoryresourcenotification) в библиотеке MSDN.|  
|**system_low_memory_signal_state**|**bit**|Состояние уведомления о недостаточном объеме системной памяти. Значение 1 указывает на то, что сигнал памяти о недостаточном объеме памяти был задан Windows. Дополнительные сведения см. в разделе [креатемемориресаурценотификатион](/windows/win32/api/memoryapi/nf-memoryapi-creatememoryresourcenotification) в библиотеке MSDN.|  
|**system_memory_state_desc**|**nvarchar(256)**|Описание состояния памяти. См. таблицу ниже.|  
|**pdw_node_id**|**int**|**Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение.|  
  
|Условие|Значение|  
|---------------|-----------|  
|system_high_memory_signal_state = 1<br /><br /> и<br /><br /> system_low_memory_signal_state = 0|Достаточный объем доступной физической памяти|  
|system_high_memory_signal_state = 0<br /><br /> и<br /><br /> system_low_memory_signal_state = 1|недостаточный объем доступной физической памяти|  
|system_high_memory_signal_state = 0<br /><br /> и<br /><br /> system_low_memory_signal_state = 0|Загрузка физической памяти постоянна|  
|system_high_memory_signal_state = 1<br /><br /> и<br /><br /> system_low_memory_signal_state = 1|Загрузка физической памяти непостоянна<br /><br /> Сигналы о достаточном и недостаточном объеме памяти не могут появляться одновременно. Однако быстрые изменения на уровне операционной системы могут вызвать одновременное появление обоих сигналов. При появлении обоих сигналов расценивается как сообщение о переменном состоянии.|  
  
## <a name="permissions"></a>Разрешения  
 необходимо разрешение VIEW SERVER STATE на сервере.  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [SQL Server динамические административные представления, связанные с операционной системой &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
