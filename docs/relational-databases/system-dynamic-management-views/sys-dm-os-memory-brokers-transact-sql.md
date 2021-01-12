---
description: sys.dm_os_memory_brokers (Transact-SQL)
title: sys.dm_os_memory_brokers (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_os_memory_brokers
- dm_os_memory_brokers_TSQL
- sys.dm_os_memory_brokers_TSQL
- dm_os_memory_brokers
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_brokers dynamic management view
ms.assetid: 48dd6ad9-0d36-4370-8a12-4921d0df4b86
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 63944b66dc5b36d5fcdd09dba293aeaf6eab989e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097622"
---
# <a name="sysdm_os_memory_brokers-transact-sql"></a>sys.dm_os_memory_brokers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Внутреннее распределение памяти в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] осуществляется с помощью диспетчера памяти [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Отслеживание различий между счетчиками памяти процесса от **sys.dm_os_process_memory** и внутренних счетчиков может указывать на использование памяти из внешних компонентов в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] пространстве памяти.  
  
 Брокеры памяти должным образом распределяют выделяемые объемы памяти между различными компонентами [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], основываясь на текущем и прогнозируемом использовании. Брокеры памяти не осуществляют выделение памяти. Ими выполняется только отслеживание операций выделения памяти с целью вычисления необходимого распределения.  
  
 Сведения о брокерах памяти приведены в следующей таблице.  
  
> [!NOTE]  
>  Чтобы вызвать эту функцию из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте имя **sys.dm_pdw_nodes_os_memory_brokers**.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**pool_id**|**int**|Идентификатор пула ресурсов, если он связан с пулом регулятора ресурсов.|  
|**memory_broker_type**|**nvarchar(60)**|Тип брокера памяти. Сейчас в перечислены три типа брокеров памяти в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , приведенных ниже.<br /><br /> **MEMORYBROKER_FOR_CACHE** : память, выделенная для использования кэшированными объектами (не кэшем буферного пула).<br /><br /> **MEMORYBROKER_FOR_STEAL** : память, украденная из буферного пула. Эта память недоступна для повторного использования другими компонентами до тех пор, пока она не будет освобождена текущим владельцем.<br /><br /> **MEMORYBROKER_FOR_RESERVE** : память зарезервирована для будущего использования текущими выполняемыми запросами.|  
|**allocations_kb**|**bigint**|Объем памяти, в килобайтах (КБ), выделенный для этого типа брокера.|  
|**allocations_kb_per_sec**|**bigint**|Интенсивность операций выделения памяти, в килобайтах (КБ) в секунду. Это значение может быть отрицательным для операций освобождения выделенной памяти.|  
|**predicted_allocations_kb**|**bigint**|Прогнозируемый объем выделенной памяти для брокера. Основывается на закономерности использования памяти.|  
|**target_allocations_kb**|**bigint**|Рекомендуемый объем выделенной памяти, в килобайтах (КБ), основанный на текущих параметрах и закономерностях использования памяти. Этот брокер должен быть приведен к указанному объему путем сокращения или увеличения используемого им объема выделенной памяти.|  
|**future_allocations_kb**|**bigint**|Прогнозируемое количество памяти, в килобайтах (КБ), которое будет выделено в течение следующих нескольких секунд.|  
|**overall_limit_kb**|**bigint**|Максимальный объем памяти в килобайтах (КБ), который может быть выделен брокером.|  
|**last_notification**|**nvarchar(60)**|Рекомендация по использованию памяти, основанная на текущих параметрах и закономерностях использования памяти. Допустимы следующие значения:<br /><br /> grow<br /><br /> shrink<br /><br /> стабильная|  
|**pdw_node_id**|**int**|**Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение.|  
  
## <a name="permissions"></a>Разрешения  

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах `Server admin` `Azure Active Directory admin` требуется учетная запись или. Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   
  
## <a name="see-also"></a>См. также:  

  [SQL Server динамические административные представления, связанные с операционной системой &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  


