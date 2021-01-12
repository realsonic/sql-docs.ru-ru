---
description: sys.dm_broker_queue_monitors (Transact-SQL)
title: sys.dm_broker_queue_monitors (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_broker_queue_monitors
- sys.dm_broker_queue_monitors_TSQL
- dm_broker_queue_monitors_TSQL
- sys.dm_broker_queue_monitors
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_broker_queue_monitors dynamic management view
ms.assetid: 401207dc-ef4a-4a3f-879c-76dcbb52d6bc
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a553a293d5423b4731f0f2547a6b5d9d4d5c60ac
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099971"
---
# <a name="sysdm_broker_queue_monitors-transact-sql"></a>sys.dm_broker_queue_monitors (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает по одной строке для каждого монитора очереди в экземпляре. Монитор очереди управляет активацией очереди.  
  

|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|Идентификатор объекта для базы данных, содержащей очередь, за которой следит монитор. Допускает значение NULL.|  
|**queue_id**|**int**|Идентификатор объекта для очереди, за которой следит монитор. Допускает значение NULL.|  
|**state**|**nvarchar(32)**|Состояние монитора. Допускает значение NULL. Возможны следующие варианты.<br /><br /> **Активный**<br /><br /> **NOTIFIED**<br /><br /> **RECEIVES_OCCURRING**|  
|**last_empty_rowset_time**|**datetime**|Последний раз, когда инструкция RECEIVE из очереди возвратила пустой результат. Допускает значение NULL.|  
|**last_activated_time**|**datetime**|Последний раз, когда монитор очереди активировал хранимую процедуру. Допускает значение NULL.|  
|**tasks_waiting**|**int**|Количество сеансов, ожидающих в инструкции RECEIVE для этой очереди. Допускает значение NULL.<br /><br /> Примечание. это число включает в себя любой сеанс, в котором выполняется инструкция RECEIVE, независимо от того, начал ли монитор очереди сеанс. Это происходит, если вместе с RECEIVE используется инструкция WAITFOR. В основном, эти задачи ожидают прибытия сообщений в очередь.|  
  
## <a name="permissions"></a>Разрешения  
 необходимо разрешение VIEW SERVER STATE на сервере.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-current-status-queue-monitor"></a>A. Монитор текущего состояния очереди  
 Этот сценарий предоставляет сведения о текущем состоянии всех очередей сообщений.  
  
```  
SELECT t1.name AS [Service_Name],  t3.name AS [Schema_Name],  t2.name AS [Queue_Name],    
CASE WHEN t4.state IS NULL THEN 'Not available'   
ELSE t4.state   
END AS [Queue_State],    
CASE WHEN t4.tasks_waiting IS NULL THEN '--'   
ELSE CONVERT(VARCHAR, t4.tasks_waiting)   
END AS tasks_waiting,   
CASE WHEN t4.last_activated_time IS NULL THEN '--'   
ELSE CONVERT(varchar, t4.last_activated_time)   
END AS last_activated_time ,    
CASE WHEN t4.last_empty_rowset_time IS NULL THEN '--'   
ELSE CONVERT(varchar,t4.last_empty_rowset_time)   
END AS last_empty_rowset_time,   
(   
SELECT COUNT(*)   
FROM sys.transmission_queue t6   
WHERE (t6.from_service_name = t1.name) ) AS [Tran_Message_Count]   
FROM sys.services t1    INNER JOIN sys.service_queues t2   
ON ( t1.service_queue_id = t2.object_id )     
INNER JOIN sys.schemas t3 ON ( t2.schema_id = t3.schema_id )    
LEFT OUTER JOIN sys.dm_broker_queue_monitors t4   
ON ( t2.object_id = t4.queue_id  AND t4.database_id = DB_ID() )    
INNER JOIN sys.databases t5 ON ( t5.database_id = DB_ID() );  
```  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Динамические административные представления, связанные с компонентом Service Broker (Transact-SQL)](../../relational-databases/system-dynamic-management-views/service-broker-related-dynamic-management-views-transact-sql.md)  
  
  

