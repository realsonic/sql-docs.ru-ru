---
description: sys.dm_os_dispatcher_pools (Transact-SQL)
title: sys.dm_os_dispatcher_pools (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_dispatcher_pools_TSQL
- dm_os_dispatcher_pools
- sys.dm_os_dispatcher_pools
- sys.dm_os_dispatcher_pools_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- extended events [SQL Server], views
- sys.dm_os_dispatcher_pools DMV
ms.assetid: b9edbc83-c6bc-4753-9bb5-a454cfe7d6bf
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 04dbc2fcedb889b54d8a95fa5357b23857cd062a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095130"
---
# <a name="sysdm_os_dispatcher_pools-transact-sql"></a>sys.dm_os_dispatcher_pools (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает сведения о пулах диспетчера сеансов. Пулы диспетчеров представляют собой пулы потоков, используемые системными компонентами для фоновой обработки.  
  
> [!NOTE]  
>  Чтобы вызвать эту функцию из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте имя **sys.dm_pdw_nodes_os_dispatcher_pools**.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|dispatcher_pool_address|**varbinary(8)**|Адрес пула диспетчеров. dispatcher_pool_address является уникальным. Не допускает значение NULL.|  
|тип|**nvarchar(256)**|Тип пула диспетчеров. Не допускает значение NULL. Существует два типа пулов диспетчеров:<br /><br /> DISP_POOL_XE_ENGINE;<br /><br /> DISP_POOL_XE_SESSION.<br /><br /> Запрос к динамическому административному представлению для полного списка|  
|name|**nvarchar(256)**|Имя пула диспетчеров. Не допускает значение NULL.|  
|dispatcher_count|**int**|Число активных потоков диспетчеров. Не допускает значение NULL.|  
|dispatcher_ideal_count|**int**|Число потоков диспетчеров, которые могут быть задействованы по мере роста пула диспетчеров. Не допускает значение NULL.|  
|dispatcher_timeout_ms|**int**|Время в миллисекундах, в течение которого диспетчер будет ожидать нового задания, прежде чем завершить работу. Не допускает значение NULL.|  
|dispatcher_waiting_count|**int**|Число бездействующих потоков диспетчеров. Не допускает значение NULL.|  
|queue_length|**int**|Число рабочих элементов, ожидающих обработки пулом диспетчеров. Не допускает значение NULL.|  
|pdw_node_id|**int**|**Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение.|  
  
## <a name="permissions"></a>Разрешения

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах `Server admin` `Azure Active Directory admin` требуется учетная запись или. Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   

## <a name="see-also"></a>См. также  
  
  


