---
description: sys.dm_exec_session_wait_stats (Transact-SQL)
title: sys.dm_exec_session_wait_stats (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 04/24/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_exec_session_wait_stats
- sys.dm_exec_session_wait_stats_tsql
- dm_exec_session_wait_stats
- dm_exec_session_wait_stats_tsql
helpviewer_keywords:
- sys.dm_exec_session_wait_stats
ms.assetid: df84842a-71eb-4fda-b448-5953cf9985dc
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d0c91c8af4b0264c6cb418e858e18e0c252eda15
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171426"
---
# <a name="sysdm_exec_session_wait_stats-transact-sql"></a>sys.dm_exec_session_wait_stats (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Возвращает сведения обо всех ожиданиях, обнаруженных потоками, которые выполнялись для каждого сеанса. Это представление можно использовать для диагностики проблем с производительностью [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] сеанса, а также с конкретными запросами и пакетами.  Это представление возвращает сеанс с теми же сведениями, которые объединены для [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md) но также предоставляет номер **session_id** .  
  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] и выше).  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|session_id|**smallint**|Идентификатор сеанса.|  
|wait_type|**nvarchar(60)**|Имя типа ожидания. Дополнительные сведения см. в разделе [sys.dm_os_wait_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md).|  
|waiting_tasks_count|**bigint**|Число ожиданий данного типа. Этот счетчик наращивается каждый раз при начале ожидания.|  
|wait_time_ms|**bigint**|Общее время ожидания данного типа в миллисекундах. Это время включает в себя время signal_wait_time_ms.|  
|max_wait_time_ms|**bigint**|Максимальное время ожидания данного типа.|  
|signal_wait_time_ms|**bigint**|Разница между временем сигнализации ожидающего потока и временем начала его выполнения.|  
  
## <a name="remarks"></a>Примечания  
 Это динамическое административное представление сбрасывает сведения для сеанса при открытии сеанса или при сбросе сеанса (если пул соединений).  
  
 Дополнительные сведения о типах ожидания см. в разделе [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md).  
  
## <a name="permissions"></a>Разрешения  
 Если пользователь имеет разрешение **View Server State** на сервере, пользователь увидит все выполняющиеся сеансы на экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ; в противном случае пользователь увидит только текущий сеанс.  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [SQL Server динамические административные представления, связанные с операционной системой &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [sys.dm_os_wait_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md)  
 
